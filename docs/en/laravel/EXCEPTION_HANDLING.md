# Exception Handling / Logging

This document defines guidelines for exception handling and logging in Laravel applications.

## Basic Policy

- **[Mandatory]** Centralize exception responses, command output, and logging in `app/Exceptions/Handler.php` (HTTP via `render()`, console via `renderForConsole()`, logging via `report()`). Do not catch exceptions within application code for the purpose of handling responses or logging.
- The following uses of exception catching are permitted:
  - Rolling back transactions
  - Closing files or resources
  - Returning `false` or a fallback value for expected exceptions (business logic branching)
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

## Event Listener Exception Handling

- **[Mandatory]** Exceptions that occur inside an event listener must be caught and handled within the listener's own `handle()` method. Callers (controllers, services) must not wrap `event()` in try/catch to handle listener failures.
- If a listener implements `ShouldQueue`, exceptions inside the listener do not propagate to the dispatch site at all, so try/catch in the caller is ineffective. Use the `failed()` method for exception handling in queued listeners.

```php
// ✅ Correct: listener handles its own exceptions
class LogActionEventListener
{
    public function handle(UpdateFundEvent $event): void
    {
        try {
            $this->addAction->addLoggableAction(...);
        } catch (Throwable $e) {
            report($e);
            Log::error('Audit log failed', ['exception' => $e]);
        }
    }
}

// ✅ Correct: queued listener uses failed()
class SendNotificationListener implements ShouldQueue
{
    public function handle(FundUpdated $event): void { ... }

    public function failed(FundUpdated $event, Throwable $exception): void
    {
        report($exception);
        Log::error('Notification job failed', ['exception' => $exception]);
    }
}

// ❌ Incorrect: caller catches listener exceptions
try {
    event(new UpdateFundEvent(...));
} catch (Throwable $e) {
    report($e);
    Log::error('event dispatch failed', ['exception' => $e]);
}
```
