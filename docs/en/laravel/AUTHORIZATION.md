# Authorization

This document defines guidelines for authorization in Laravel applications.

## Basic Policy

- **[Mandatory]** Centralize all resource access control in Laravel Policies. Do not write authorization logic in controller private methods or Blade template `@php` blocks.
- **[Mandatory]** In controllers, use `$this->authorize()` to invoke Policies. Do not create private methods that call `abort()` directly (e.g., `abortIfXxx()`).
- **[Mandatory]** In Blade templates, use `@can` / `@cannot` directives for display control. Do not compute permission variables in `@php` blocks and branch with `@if`.
- **[Recommended]** Combine with route-level `->middleware('can:action,model')` for defense in depth.

## Policy Design

- **[Recommended]** Name Policy methods after actions (`view`, `create`, `update`, `delete`) or meaningful business operations (`viewContracts`, `updateContracts`).
- **[Recommended]** Combine conditions (feature flags + model attributes, etc.) inside the Policy. Its responsibility is to answer "can this user perform this action on this resource?", which may include configuration-based access control.

## Registering in AuthServiceProvider

```php
protected $policies = [
    Fund::class => FundPolicy::class,
];
```

## Implementation Example

```php
// ✅ Correct: centralize access control in Policy

// app/Policies/FundPolicy.php
public function viewContracts(?Admin $admin, Fund $fund): bool
{
    return !CommonHelper::usageRestricted() && $fund->isElectronicContract();
}

public function updateContracts(Admin $admin, Fund $fund): bool
{
    return $this->viewContracts($admin, $fund)
        && CommonHelper::isContractPDFFeatureEnabled();
}

// Controller
public function show(Fund $fund): View
{
    $this->authorize('viewContracts', $fund);
    // ...
}

public function update(WriteRequest $request, Fund $fund): RedirectResponse
{
    $this->authorize('updateContracts', $fund);
    // ...
}

// Blade template
@can('updateContracts', $fund)
    @include('admin.fund.partials.edit-fund-contracts')
@endcan

@if (Gate::check('viewContracts', $fund) && Gate::denies('updateContracts', $fund))
    <div class="bgc-white p-20 bd mB-20">
        Contract PDF feature is disabled. You cannot upload contracts on this screen.
    </div>
@endif
```

```php
// ❌ Incorrect: controller private method controls access
private function abortIfContractManagementUnavailable(Fund $fund): void
{
    if (CommonHelper::usageRestricted() || !$fund->isElectronicContract()) {
        abort(404);
    }
}
```

```blade
{{-- ❌ Incorrect: computing permission variable in @php block --}}
@php
    $canManagePdfContracts = !CommonHelper::usageRestricted()
        && $fund->isElectronicContract()
        && CommonHelper::isContractPDFFeatureEnabled();
@endphp
@if ($canManagePdfContracts)
    ...
@endif
```
