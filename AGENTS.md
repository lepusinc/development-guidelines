# Instructions for AI Agents - Lepus Development Guidelines

## What this repository is

This is **documentation only** — the Lepus Engineering Team's development guidelines (review policy, Git workflow, issue conventions, dependency selection, testing, and framework-specific rules). There is no application code, no build step, and no test suite. "Working in this repo" means editing Markdown.

Internal use only: external PRs (from outside the Lepus Group) are not accepted. Branch from `main` and open a PR against `main`.

## Structure & the bilingual rule

All guidelines exist in two parallel trees:

- `docs/en/` — English, authored **for AI agents** (this is the primary source AI tooling consults).
- `docs/ja/` — Japanese, authored for human engineers.

This document is available in the following languages:

- [English](docs/en/AGENTS.md)
- [Japanese](docs/ja/AGENTS.md)

When you add or change a guideline, **keep `docs/en/` and `docs/ja/` in sync** — a change to one almost always needs the matching change in the other. Note the trees are not yet symmetric: `docs/ja/` currently has testing docs (`TESTING_GUIDELINES.md`, `UNIT_TEST_GUIDELINES.md`, `FUTURE_TEST_GUIDELINES.md`) that have no English counterpart. Check both sides before assuming a file exists.

When writing bilingual text inline, list English first, then Japanese.

### `AGENTS.md` is layered

Each directory level has its own `AGENTS.md` that scopes the rules below it:

- Root `AGENTS.md` → repository-wide agent instructions (repo purpose, bilingual rule, conventions, CI) plus language switcher links to the trees below.
- `docs/en/AGENTS.md` (and `ja`) → repository-wide agent conventions: review levels, severity labels, PR workflow.
- `docs/en/laravel/AGENTS.md` (and `ja`) → **Laravel-specific rules that take precedence over the general `docs/en/` rules when the target project uses Laravel.** Ignore the `laravel/` directory entirely for non-Laravel projects.

Read the relevant `AGENTS.md` before editing files in a subtree — it defines the conventions for that subtree.

## Conventions that constrain edits

**Severity labels** — every rule in a guideline is tagged with one of: `[Mandatory]`/`[厳守]`, `[Required]`/`[要修正]`, `[Conditional]`/`[条件付き]`, `[Recommended]`/`[推奨]`. New rules must carry one.

**Review levels** referenced throughout: L0 (minor), L1 (feature add/change, requires acceptance criteria + evidence), L2 (high-risk: schema/security/external integration, requires pair review + manual verification).

**Commit prefixes**: `docs:`, `feat:`, `fix:`, `test:`, `refactor:`. Large renames/reformatting go in a separate PR from substantive changes.

**PR template**: this repo uses `release_or_ops.md` from `lepusinc/.github` (templates are centralized there, not in this repo).

## Formatting & CI

Markdown is linted and auto-fixed by **markdownlint** on save (see `.vscode/settings.json`); Prettier is explicitly disabled for Markdown. Match existing formatting — preserve prose wrapping (no reflow).

CI (`.github/workflows/ci.yml`) only verifies that required files exist (`README.md`, `README.ja.md`, `CONTRIBUTING.md`, `docs/en/`, `docs/ja/`) and that at least one `.md` file is tracked. There is no content/lint check in CI, so correctness of guideline content is on the author and reviewer.
