# Unit Test Guidelines

## Purpose & Scope

- Targets the smallest units of logic — functions, methods, classes, etc.
- Purpose: verify logic branches, boundary values, and error paths quickly and exhaustively.
- **[Recommended]** Minimize dependencies on out-of-process resources (DB, HTTP, external APIs). Replace unavoidable dependencies with mocks or stubs.
- **[Required]** Because unit tests run fast and cheaply verify large numbers of input combinations, branch and pattern coverage is unit tests' responsibility.

## Topic Guidelines

- [Validation Testing](./VALIDATION.md)

## Format & Parsing Tests

- **[Recommended]** When testing formats such as CSV or API responses, prepare a Fixture and assert that the output matches it.
- **[Recommended]** When testing parsing, also prepare a Fixture and use it as the input.
