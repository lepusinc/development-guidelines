# Git Workflow Guidelines

This document defines Git operation guidelines for the Lepus Engineering Team.
If a repository has its own defined Git workflow or branch operation guidelines, follow those repository-specific definitions first.

## 1. Branch Strategy

- `main`: Stable branch for production-delivered changes
- `develop`: Integration branch for ongoing development (mirrored to testing environment)
- `feature/*`: Feature development branches
- `bugfix/*`: Bug fix branches
- `hotfix/*`: Emergency release branches
- `release/*`: Release candidate branches
- `docs/*`: Documentation update branches
- `refactor/*`: Behavior-preserving structural change branches
- `test/*`: Test addition/update branches
- `chore/*`: Maintenance branches (tooling, config, cleanup)
- `ops/*`: Operational change branches (CI workflow, environment setup)

## 2. Development Workflow

1. Create a working branch (`feature/*`, etc.) from `develop`.
2. Implement changes on the working branch.
3. Open a `feature -> develop` Pull Request.
4. Squash merge after review approval.

### 2.1 Conflict Resolution

- For feature branch synchronization, use `git rebase origin/develop` as the default, instead of merging `develop` into `feature`.
- If conflict resolution in a `feature -> develop` PR introduces dependency on another feature, add a `dependency` label and include `depends-on: <PR number or ticket key>` in the PR description.

## 3. Release Workflow

1. Create `release/<topic>` from `main`.
2. `cherry-pick` release-target commits from `develop` (as a rule, 1 branch = 1 squash commit).
3. Open a release PR from `release/<topic>` to `main`.
4. Squash merge after review approval.
5. If needed, sync `main` changes back into `develop` to keep branch differences aligned.

## 4. PR Operations

- Use a PR template (`.github/PULL_REQUEST_TEMPLATE/with_ticket.md` or `.github/PULL_REQUEST_TEMPLATE/without_ticket.md`).
- Use `with_ticket.md` when a ticket exists, and `without_ticket.md` when no ticket exists.
- Use `release_or_ops.md` for release/operational coordination changes (release merge, CI workflow changes, local dev setup updates, documentation updates).
- Use `with_ticket.md` or `without_ticket.md` for production build/deploy changes.
- Keep the PR title format and checklist aligned with the selected template.

## 5. Branch Naming

Format:

`<type>/<ticket-or-topic>-<short-description>`

Examples:

- `feature/PROJ-123-add-review-level-checklist`
- `bugfix/PROJ-456-correct-pr-template-link`
- `hotfix/PROJ-789-patch-critical-release-issue`
- `docs/update-readme-document-index`
- `release/2026-02-docs-sync`

Use branch types defined in `1. Branch Strategy` for `type`.

Naming constraints:

- If a ticket exists, use the ticket key first (e.g., `PROJ-123`).
- If no ticket exists, use a clear topic phrase.
- Use lowercase kebab-case for `ticket-or-topic` and `short-description`.
- Do not use spaces, underscores, or special characters.
- Keep branch names under 60 characters when possible.

## 6. Commit Messages

- `test:`: Changes for reproduction/regression tests
- `fix:`: Bug fixes
- `refactor:`: Behavior-preserving refactoring
- When a ticket exists, include both the ticket key and ticket title in the commit message.  
  Format: ``<type>: [<ticket-key>] <ticket-title>``  
  Example: ``fix: [PROJ-123] Show error message on login failure``
- Large formatting or mass rename changes should be split into a dedicated or preparatory PR.
