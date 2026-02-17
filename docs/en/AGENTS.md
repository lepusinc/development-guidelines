# Instructions for AI Agents - Lepus Development Guidelines

`AGENTS.md` and the markdown files under `docs/` are created in accordance with the AAIF (Agentic AI Foundation) guidelines.

This repository contains the engineering team's documentation standards, review policies, and PR templates. It is **documentation-only**—no code or build/test automation is present.

## Key Knowledge for AI Agents

### 1. Repository Purpose & Scope

- **Internal use only**: PRs from outside Lepus Group are not accepted.
- All content is for documentation, not code implementation.
- Main files: `README.md`, `README.ja.md`, `CONTRIBUTING.md`, `docs/en/REVIEW_POLICY.md`, `docs/ja/REVIEW_POLICY.md`, `docs/en/ISSUE.md`, `docs/ja/ISSUE.md`, PR templates in `.github/PULL_REQUEST_TEMPLATE/`.

### 2. Pull Request Workflow

- **Branch from `main`** for changes.
- Use the PR template (`.github/PULL_REQUEST_TEMPLATE/with_ticket.md` or `.github/PULL_REQUEST_TEMPLATE/without_ticket.md`).
- Use `with_ticket.md` when a ticket exists (ticket URL + ticket-driven checklist).
- Use `without_ticket.md` when no ticket exists (background/purpose, outputs, verification steps, system impact, checklist).
- Use `release_or_ops.md` for release/operational coordination changes (release merge, CI workflow changes, local dev setup updates, documentation updates).
- Use `with_ticket.md` or `without_ticket.md` for production build/deploy changes.
- Keep the PR title format and checklist aligned with the selected template.
- **No external PRs**—fork if you wish to adapt.

### 3. Review Policy Highlights

- **Review levels**:
  - L0: Minor fix (light check, focus on CI/tests)
  - L1: Feature addition/modification (AC & evidence required)
  - L2: High-risk (schema/security/external integration) → Pair review + manual verification
- **Mandatory**: required sections/checklist items defined by the selected PR template.
- **Commit conventions**: Use `test:`, `fix:`, `refactor:` prefixes. Large renames/formatting → separate PRs.
- **Explicit system impact**: Select and document impacted areas based on `docs/en/ISSUE.md` and `docs/ja/ISSUE.md`.

### 4. Patterns & Conventions

- **Single Responsibility Principle**: Bug fixes and refactoring should be separate PRs (except minimal, behavior-preserving refactoring).
- **RACI matrix**: Developer (implementation/tests), Reviewer/Tech Lead (quality/design/risk), QA/PM (AC definition/validation).
- **Test-first for bug fixes**: Reproduction test → fix → regression check.

### 5. Language & Documentation

- All documentation is available in both English (`docs/en/`) and Japanese (`docs/ja/`).
- Reference the appropriate language version for PRs and reviews.
- When bilingual text is provided, list English first, then Japanese.

## Examples

- See `docs/en/REVIEW_POLICY.md` and `docs/ja/REVIEW_POLICY.md` for detailed review criteria and RACI roles.
- See PR templates for required sections and checklists.

---

For questions or unclear conventions, consult `README.md` or open an issue for clarification.
