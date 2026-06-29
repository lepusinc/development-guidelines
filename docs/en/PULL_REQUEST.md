# Pull Requests

This document defines the **operational rules** for GitHub templates and pull requests: which PR template and issue template to use, and how to keep PR titles and checklists aligned.

Templates are maintained in lepusinc/.github (centralized across all repositories):

- PR templates: <https://github.com/lepusinc/.github/tree/main/.github/PULL_REQUEST_TEMPLATE>
- Issue templates: <https://github.com/lepusinc/.github/tree/main/.github/ISSUE_TEMPLATE>

## 1. PR Templates

- Use a PR template (`with_ticket.md` or `without_ticket.md`) from <https://github.com/lepusinc/.github/tree/main/.github/PULL_REQUEST_TEMPLATE>.
- Use `with_ticket.md` when a ticket exists, and `without_ticket.md` when no ticket exists.
- Use `release_or_ops.md` for release/operational coordination changes (release merge, CI workflow changes, local dev setup updates, documentation updates). Template: <https://github.com/lepusinc/.github/blob/main/.github/PULL_REQUEST_TEMPLATE/release_or_ops.md>.
- Use `with_ticket.md` or `without_ticket.md` for production build/deploy changes.
- Keep the PR title format and checklist aligned with the selected template.

## 2. Issue Templates

Select the template that matches the issue type:

| Type | Template |
| --- | --- |
| Epic | [epic.md](https://github.com/lepusinc/.github/blob/main/.github/ISSUE_TEMPLATE/epic.md) |
| Story | [story.md](https://github.com/lepusinc/.github/blob/main/.github/ISSUE_TEMPLATE/story.md) |
| Task | [task.md](https://github.com/lepusinc/.github/blob/main/.github/ISSUE_TEMPLATE/task.md) |
| Bug | [bug.md](https://github.com/lepusinc/.github/blob/main/.github/ISSUE_TEMPLATE/bug.md) |
| Sub-task | [sub_task.md](https://github.com/lepusinc/.github/blob/main/.github/ISSUE_TEMPLATE/sub_task.md) |

For issue type **definitions and selection criteria** (what each type means and when to use it), see the "Jira 課題運用ガイド" page in the Dev Playbook (Confluence): <https://lepus.atlassian.net/wiki/spaces/DEVPB/pages/41615364/Jira>. For the Jira custom field "System Impact Scope," see [Issues](./ISSUE.md).
