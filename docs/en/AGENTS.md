# Instructions for AI Agents - Lepus Development Guidelines

`AGENTS.md` and the markdown files under `docs/` are created in accordance with the AAIF (Agentic AI Foundation) guidelines.

This repository contains the engineering team's documentation standards, review policies, and PR templates. It is **documentation-only**—no code or build/test automation is present.

## Document Index

| Document | When to read |
| --- | --- |
| [Review Policy](./REVIEW_POLICY.md) | Review levels, RACI roles, PR size guidelines, CodeRabbit severity mapping |
| [Pull Request Guidelines](./PULL_REQUEST.md) | PR/issue template selection, title & checklist rules |
| [Git Workflow Guidelines](./GIT_WORKFLOW.md) | Branch strategy, commit message conventions |
| [Issues](./ISSUE.md) | Issue types, system impact scope labeling |
| [Dependencies Guidelines](./DEPENDENCIES_GUIDELINES.md) | Evaluating and selecting third-party packages |
| [Laravel Guidelines](./laravel/AGENTS.md) | Laravel-specific rules — only when the target project uses Laravel |
| [Testing Guidelines](./testing/AGENTS.md) | Unit/feature test scope, validation testing responsibility |

## Key Knowledge for AI Agents

### 1. Repository Purpose & Scope

- **Internal use only**: PRs from outside Lepus Group are not accepted.
- All content is for documentation, not code implementation.
- Main files: `README.md`, `README.ja.md`, `CONTRIBUTING.md`, `docs/en/REVIEW_POLICY.md`, `docs/ja/REVIEW_POLICY.md`, `docs/en/ISSUE.md`, `docs/ja/ISSUE.md`, `docs/en/PULL_REQUEST.md`, `docs/ja/PULL_REQUEST.md`, `docs/en/GIT_WORKFLOW.md`, `docs/ja/GIT_WORKFLOW.md`, PR templates in lepusinc/.github: <https://github.com/lepusinc/.github/tree/main/.github/PULL_REQUEST_TEMPLATE>.

### 2. AI Agent Task Delegation

- **[Recommended]** Delegate research to a research subagent.
- **[Recommended]** Delegate design to a design subagent.
- **[Recommended]** Delegate implementation to an implementation subagent.
- **[Recommended]** Delegate writing test code to a test-code subagent.
- **[Recommended]** Delegate review to a review subagent.
- **[Required]** Keep planning (finalizing what to build and getting user buy-in) with the main agent — it requires an interactive back-and-forth with the user that a background subagent cannot conduct.

### 3. AI Agent Accuracy Practices

- **[Required]** Ask instead of guessing: when a requirement is ambiguous or information is missing, ask the user rather than silently assuming.
- **[Required]** Verify against the current codebase: before acting on a claim about a function, file, or existing behavior, confirm it against the current code — do not rely on memory or documentation that may be stale.
- **[Recommended]** Use DeepWiki/Devin MCP for spec research: when researching a repository's current specification, query DeepWiki MCP if the repository is indexed there; for a private repository DeepWiki does not cover, query Devin MCP instead. Also use DeepWiki MCP when researching the specifications of OSS/third-party dependencies.
- **[Recommended]** Adversarially verify review/research findings: before reporting a review or research finding as confirmed, challenge it from an independent perspective (e.g., a separate refutation pass) to reduce false positives.
- **[Required]** Compare multiple candidates for high-risk (L2) designs: for L2-level design decisions, produce multiple independent candidate designs and score them before selecting one to implement, rather than committing to a single first draft.
- **[Recommended]** Prefer IDE MCP tools for code edits: when an IDE (e.g., a JetBrains product) exposes MCP tooling, use it for structural edits such as renames. IDE-driven refactoring updates all call sites automatically, so the agent does not need to manually search for every reference.
- **[Recommended]** Run the code formatter after edits: in projects with a configured code formatter, run it after editing code.

### 4. Patterns & Conventions

- **Single Responsibility Principle**: Bug fixes and refactoring should be separate PRs (except minimal, behavior-preserving refactoring).
- **RACI matrix**: Developer (implementation/tests), Reviewer/Tech Lead (quality/design/risk), QA/PM (AC definition/validation).
- **Test-first for bug fixes**: Reproduction test → fix → regression check.

### 5. Rule Severity Labels

Each rule in the guidelines is classified with one of the following severity labels.

| Label | Description |
| --- | --- |
| **[Mandatory]** | No exceptions. Must be fixed within this PR. |
| **[Required]** | Must fix. Can be deferred by creating a ticket (ticket URL or key must be specified). |
| **[Conditional]** | Allowed with valid justification. |
| **[Recommended]** | Not required, but improvement is desired. |

### 6. Language & Documentation

- All documentation is available in both English (`docs/en/`) and Japanese (`docs/ja/`).
- Reference the appropriate language version for PRs and reviews.
- When bilingual text is provided, list English first, then Japanese.

---

For questions or unclear conventions, consult `README.md` or open an issue for clarification.
