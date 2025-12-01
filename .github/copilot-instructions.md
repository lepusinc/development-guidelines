# Copilot Instructions for Lepus Development Guidelines

This repository contains the engineering team's documentation standards, review policies, and PR templates. It is **documentation-only**—no code or build/test automation is present.

## Key Knowledge for AI Agents

### 1. Repository Purpose & Scope
- **Internal use only**: PRs from outside Lepus Group are not accepted.
- All content is for documentation, not code implementation.
- Main files: `README.md`, `README.ja.md`, `CONTRIBUTING.md`, `docs/en/REVIEW_POLICY.md`, `docs/ja/REVIEW_POLICY.md`, PR templates in `docs/en/` and `docs/ja/`.

### 2. Pull Request Workflow
- **Branch from `main`** for changes.
- Use the PR template (`docs/en/PULL_REQUEST_TEMPLATE.md` or `docs/ja/PULL_REQUEST_TEMPLATE.md`).
- PRs must include:
  - Background/purpose (ticket link, rationale)
  - Main changes (bulleted)
  - Acceptance criteria (checklist)
  - Impacted packages/areas
  - Test evidence (screenshots, CI results, reproduction steps)
- **No external PRs**—fork if you wish to adapt.

### 3. Review Policy Highlights
- **Review levels**:
  - L0: Minor fix (light check, focus on CI/tests)
  - L1: Feature addition/modification (AC & evidence required)
  - L2: High-risk (schema/security/external integration) → Pair review + manual verification
- **Mandatory**: Acceptance criteria, test results, and reproduction steps.
- **Commit conventions**: Use `test:`, `fix:`, `refactor:` prefixes. Large renames/formatting → separate PRs.
- **Explicit impact scope**: Always state affected areas (API/DB/auth/UI/external systems).

### 4. Patterns & Conventions
- **Single Responsibility Principle**: Bug fixes and refactoring should be separate PRs (except minimal, behavior-preserving refactoring).
- **RACI matrix**: Developer (implementation/tests), Reviewer/Tech Lead (quality/design/risk), QA/PM (AC definition/validation).
- **Test-first for bug fixes**: Reproduction test → fix → regression check.

### 5. Language & Documentation
- All documentation is available in both English (`docs/en/`) and Japanese (`docs/ja/`).
- Reference the appropriate language version for PRs and reviews.

## Examples
- See `docs/en/REVIEW_POLICY.md` and `docs/ja/REVIEW_POLICY.md` for detailed review criteria and RACI roles.
- See PR templates for required sections and checklists.

---
For questions or unclear conventions, consult `README.md` or open an issue for clarification.
