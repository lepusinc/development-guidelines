# Issues

In software development projects, issues are classified and used by purpose.
This document defines issue types, template references, and the Jira custom field "System Impact Scope."

## 1. Issue Types

### Epic

An Epic is a large-scale feature development or improvement that encompasses multiple stories.
It is used to manage long-term goals that may not be completed in a single sprint or release.

[Template](../issue_templates/epic.md)

### Story

A Story describes a valuable feature or requirement from the user's perspective.
It is small enough for a development team to complete in one sprint and has specific acceptance criteria (conditions for completion).

[Template](../issue_templates/story.md)

### Task

A Task is a specific work item broken down from a story from a technical point of view.
It describes the actual work that developers do, such as "implement an API" or "change the database schema."

[Template](../issue_templates/task.md)

### Bug

A Bug is an issue for reporting software defects or unexpected behavior.
It clearly describes the steps to reproduce, the expected result, and the actual result, and requests investigation and correction.

[Template](../issue_templates/bug.md)

### Sub-task

A Sub-task is a work item further subdivided from a story or task.
It is used for more detailed progress management and to facilitate division of labor among multiple people.

[Template](../issue_templates/sub_task.md)

## 2. System Impact Scope

For the Jira custom field "System Impact Scope," use the following common options and select all applicable items.

| No. | Item | Description |
|---|---|---|
| 1 | UI / UX | Whether there are changes to screen structure, components, copy, user operations, or overall user experience. |
| 2 | API | Whether existing API interfaces (endpoints, parameters, response formats) change, or new APIs are added. |
| 3 | External Systems | Whether integration with other systems or external services/APIs is impacted. |
| 4 | Authorization / Authentication | Whether authentication, authorization, or access control mechanisms are impacted. |
| 5 | Business Logic / Domain Rules | Whether core domain rules, calculation logic, or state transitions change. |
| 6 | Configuration / Feature Flags | Whether environment settings, configuration, or feature flags need to be added or changed. |
| 7 | Data Migration / Existing Data | Whether existing data correction, migration, or bulk updates are required. |
| 8 | DB Schema | Whether schema changes are required, such as table structure, columns, indexes, or constraints. |
| 9 | Batch Jobs / Schedulers | Whether batch processing, scheduled jobs, or scheduler configuration is impacted. |
| 10 | Logging / Monitoring / Alerts | Whether logging output, metrics collection, alert conditions, or notification settings need changes. |
| 11 | CI / CD | Whether build, test, or deployment pipeline definitions or flows are impacted. |
| 12 | Deployment Process / Environments | Whether deployment procedures, environment setup, or release methods for production/staging are impacted. |
| 13 | DevOps Tooling & Infrastructure | Whether CI/deployment tools, monitoring platform, IaC (e.g., Terraform), or infrastructure configuration is impacted. |
| 14 | Local Development Environment / Developer Setup | Whether local development setup steps or developer tooling configuration need updates. |
| 15 | Documentation / Help | Whether specifications, design docs, operational runbooks, user manuals, FAQs, or related documentation need updates. |
