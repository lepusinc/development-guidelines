# Issues

This document defines the Jira custom field "System Impact Scope."

For issue type **definitions and selection criteria** — what Epic / Story / Task / Bug / Sub-task mean and when to use each, plus the purpose, workflow usage, approval flow, and decision-recording policy for Jira issues — see the "Jira 課題運用ガイド" page in the Dev Playbook (Confluence), which is the source of truth for that context:
<https://lepus.atlassian.net/wiki/spaces/DEVPB/pages/41615364/Jira>

For issue and PR **templates** (which template to use per type), see [Pull Requests](./PULL_REQUEST.md).

## 1. System Impact Scope

For the Jira custom field "System Impact Scope," use the following common options and select all applicable items.

| No. | Item | Description |
| --- | --- | --- |
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
