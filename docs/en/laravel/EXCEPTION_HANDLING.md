# Exception Handling / Logging

This document defines guidelines for exception handling and logging in Laravel applications.

## Basic Policy

- **[Mandatory]** Centralize exception responses, command output, and logging in `app/Exceptions/Handler.php` (HTTP via `render()`, console via `renderForConsole()`, logging via `report()`). Do not catch exceptions within application code for the purpose of handling responses or logging.
- **[Mandatory]** Never catch `Throwable` in application-layer code. `Throwable` includes `Error` classes (`TypeError`, `ParseError`, `OutOfMemoryError`, etc.) that represent unrecoverable programming or runtime failures; these must propagate to the global exception handler (`App\Exceptions\Handler`) for centralized logging and response control. Application-layer `try`/`catch` must target `Exception` or a more specific subclass only. (Receiving `Throwable` as a parameter — e.g., the `failed(Event $event, Throwable $exception)` signature defined by Laravel — is not catching and is fine.)
- The following uses of exception catching are permitted:
  - Rolling back transactions
  - Closing files or resources
  - Returning `false` or a fallback value for expected exceptions (business logic branching)
  - Re-throwing with a user-facing message (see "Controller Exception Handling Patterns" below)
- **[Mandatory]** It is absolutely not permitted to catch and swallow exceptions solely for the purpose of changing UX. Response control must be handled in the Handler.
- **[Recommended]** If exception responses exist that are not delegated to the Handler, consider revisiting the responsibility boundaries and exception design.

## Exception Class Design

- **[Mandatory]** Change exception log levels by mapping them in `$levels` in the Handler. Both class names and interface names are accepted as keys.
- **[Recommended]** Create custom exception classes for domain-specific errors and express their meaning through the class name (e.g., `PaymentFailedException`, `ResourceNotFoundException`).

```php
protected $levels = [
    ValidationException::class  => LogLevel::INFO,
    WarningLoggable::class      => LogLevel::WARNING,  // interface names are also accepted
    ExternalApiException::class => LogLevel::ERROR,
];
```

## Log Level Guidelines

| Level | Intended Use |
| --- | --- |
| `INFO` | Expected errors caused by the user, such as validation errors |
| `WARNING` | Business rule violations, retryable external service errors, etc. |
| `ERROR` | System failures, data inconsistencies requiring immediate attention |
| `CRITICAL` | Failures that render the service inoperable |

## Response

- **[Mandatory]** Centralize HTTP response conversion in `Handler`'s `render()` or `renderable()`.
- **[Mandatory]** Do not include stack traces or internal information in production responses.
- **[Required]** Unify the error response format (structure and field names) across the entire application.

## Controller Exception Handling Patterns

Only the following two patterns are permitted when handling exceptions in controllers.

### Pattern 1: Re-throw with a user-facing message

Replace the low-level exception message with a user-safe message and re-throw as a user-facing exception. The Handler identifies this exception type and renders a user-facing error response containing the message (the specific HTTP status code and view are Handler-side concerns). Always pass the original exception as `previous` to preserve the stack trace.

```php
// ✅ Permitted: replace with user-facing message and re-throw
try {
    // ...
} catch (Exception $e) {
    throw new XxxException(__('messages.alerts.try_again_later'), previous: $e);
}

// ❌ Prohibited: controller catches exception and controls redirect
try {
    // ...
} catch (Exception $e) {
    return redirect()->back()->with(['warning' => __('messages.alerts.try_again_later')]);
}
```

### Pattern 2: Service expresses outcome via return value, controller branches (redirect to different page)

When a redirect to a different page is needed based on success or failure, express the outcome as a return value rather than an exception, and have the controller branch on that value. The specific shape of the return value (a Result object, DTO, tuple, enum, `null` vs object, etc.) is an implementation detail — the point is that the outcome is carried as a value rather than thrown.

```php
// ✅ Permitted: service returns a value expressing the outcome; controller branches
$result = $updateService->save($fund, $values);
if ($result->failed()) {
    return redirect()->route('admin.fund.other', $fund->id)
        ->with(['warning' => $result->errorMessage()]);
}

// ❌ Prohibited: controller catches exception and controls redirect target
try {
    $updateService->save($fund, $values);
} catch (Exception $e) {
    return redirect()->route('admin.fund.other', $fund->id)
        ->with(['warning' => __('messages.alerts.try_again_later')]);
}
```

## Event Listener Exception Handling

Laravel's official documentation explicitly prescribes only one pattern (`failed()` for `ShouldQueue` listeners); the rest is a design principle this project adopts: the caller of `event()` should not be aware of whether a listener is synchronous or queued. That is the listener's implementation detail and may change over time (e.g., adding `ShouldQueue` to an existing listener). Listener error handling therefore belongs on the listener side.

- **[Mandatory]** For queued listeners (`ShouldQueue`), let exceptions propagate out of `handle()`. Handle post-retry cleanup via the `InteractsWithQueue` trait and a `failed(Event $event, Throwable $exception)` method.

  Why: Laravel's queue retry mechanism (`$tries`, `backoff`, `retryUntil`) and the `failed()` callback only run if the exception propagates out of `handle()`. Wrapping `handle()` in try/catch disables both. See [Laravel Events — Handling Failed Jobs](https://laravel.com/docs/12.x/events#handling-failed-jobs).

- **[Recommended]** Do not wrap `event()` in try/catch at the dispatch site. Handle failures on the listener side.

  Why: exceptions propagate back to the dispatch site only for synchronous listeners, so the try/catch runs only while every listener is synchronous. The moment any listener adds `implements ShouldQueue`, the catch silently stops being invoked. This means the try/catch implicitly depends on the listener's execution mode (sync vs queued), which is an implementation detail. Laravel's official docs do not prescribe this — it is a design principle of this project.

- **[Recommended]** For synchronous listeners, if the listener is a side-effect whose failure must not break the main flow (e.g., audit logging), catch inside `handle()` and `report()` the exception. If the listener's success is critical to the main flow, move the logic into the caller or a dedicated service.

  Why: Laravel does not prescribe a pattern for synchronous listener exceptions — it is a design judgment. Listeners are intended for loosely coupled side-effects; logic whose success the main flow depends on does not belong in a listener.

```php
// ✅ Correct: synchronous side-effect listener — catch inside handle() to protect the main flow
class LogActionEventListener
{
    public function handle(UpdateFundEvent $event): void
    {
        try {
            $this->addAction->addLoggableAction(...);
        } catch (Exception $e) {
            report($e);
        }
    }
}

// ✅ Correct: queued listener — let exceptions propagate; clean up in failed()
class SendNotificationListener implements ShouldQueue
{
    use InteractsWithQueue;

    public function handle(FundUpdated $event): void
    {
        // No try/catch — let exceptions propagate so the queue can retry.
        $this->notifier->send(...);
    }

    public function failed(FundUpdated $event, Throwable $exception): void
    {
        report($exception);
    }
}

// ❌ Prohibited: queued listener wraps handle() in try/catch
class SendNotificationListener implements ShouldQueue
{
    public function handle(FundUpdated $event): void
    {
        try {
            $this->notifier->send(...);
        } catch (Throwable $e) {
            report($e); // Queue cannot retry; failed() is never called.
        }
    }
}

// ❌ Discouraged: caller wraps event() in try/catch
// Works only while the listener is synchronous. If the listener later adds
// `implements ShouldQueue`, this catch silently stops running — the caller
// becomes coupled to an implementation detail of the listener.
try {
    event(new UpdateFundEvent(...));
} catch (Throwable $e) {
    report($e);
}
```
