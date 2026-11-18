# Domain Model (Entities)

This document defines framework-independent guidelines for designing entities (domain objects that hold state). Rules specific to Laravel's Eloquent models are in [Eloquent Model](./laravel/ELOQUENT.md).

## 1. State & Lifecycle

State transitions are the application's control flow itself. If state can be overwritten from anywhere, impossible states and inconsistent accompanying values appear. Therefore, the entity itself protects its state.

- **[Mandatory]** An entity's state (status, result, active/inactive flags, and other values that drive application control) must be changeable only through the entity's dedicated methods. Do not set state values directly from outside the entity, such as from services, controllers, or UI handling.
- **[Mandatory]** Values that accompany a state change (requested-at, finalized-at, reason, etc.) must be accepted only as arguments of the dedicated method that changes the state. Do not allow accompanying values to be set through separate setters or mass assignment.
- **[Mandatory]** Check whether the state change is allowed (whether the transition from the current state is valid and its preconditions are met) inside the dedicated method. Throw an exception when the change is not allowed.
- **[Required]** When a state change requires the state of child entities to be kept in sync, sync them inside the parent's dedicated method. Do not allow the child's state alone to be changed from outside.
- **[Required]** Even when bulk updates are used for persistence reasons, encapsulate them inside dedicated methods. Do not bulk-update state columns directly from outside the entity.

```text
// Bad: state and accompanying values are set directly from outside
batch.status = IN_PROGRESS
batch.requestedAt = now()

// Good: the dedicated method checks the transition and records the accompanying value together
batch.markRequested(requestedAt = now())
```
