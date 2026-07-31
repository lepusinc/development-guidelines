# Validation Testing

- **[Required]** Cover validation testing in unit tests as a rule; do not cover it exhaustively in feature tests. Unit tests run faster than feature tests and make it easier to exhaustively verify input combinations and boundary values, so branch coverage of validation rules belongs in unit tests. In feature tests, limit verification to representative behavior when a validation error occurs (e.g., response status and error response format) rather than covering every rule branch.
