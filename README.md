# LEPUS Development Guidelines

[![version](https://img.shields.io/github/v/release/lepusinc/development-guidelines?label=version)](https://github.com/lepusinc/development-guidelines/releases/latest) [![CI](https://github.com/lepusinc/development-guidelines/actions/workflows/ci.yml/badge.svg)](https://github.com/lepusinc/development-guidelines/actions/workflows/ci.yml)

🇺🇸 **English** | 🇯🇵 [Japanese](README.ja.md)

This repository contains the **development guidelines and best practices** of the Lepus Engineering Team.  
It defines our principles for code review, pull requests, testing, and collaboration.

---

## 📖 Documentation Index

- [Review Policy](docs/en/REVIEW_POLICY.md)
  - [PR Size Guidelines](docs/en/REVIEW_POLICY.md#pr-size-guidelines)
  - [Review Mindset](docs/en/REVIEW_POLICY.md#-review-mindset)
  - [Quick Checklist](docs/en/REVIEW_POLICY.md#-quick-checklist)
  - [Level Decision Guide](docs/en/REVIEW_POLICY.md#-level-decision-guide)
  - [RACI Matrix](docs/en/REVIEW_POLICY.md#-raci-matrix)
- [Issue Guidelines](docs/en/ISSUE.md)
- [Git Workflow Guidelines](docs/en/GIT_WORKFLOW.md)
- [Third-Party Package Selection Guidelines](docs/en/DEPENDENCIES_GUIDELINES.md)
  - [Objectives](docs/en/DEPENDENCIES_GUIDELINES.md#objectives)
  - [Trustworthiness](docs/en/DEPENDENCIES_GUIDELINES.md#1-trustworthiness-popularity--ecosystem-trust)
  - [Security & Compliance](docs/en/DEPENDENCIES_GUIDELINES.md#2-security--compliance)
  - [Code Quality](docs/en/DEPENDENCIES_GUIDELINES.md#3-code-quality)
  - [Project Sustainability](docs/en/DEPENDENCIES_GUIDELINES.md#4-project-sustainability)
  - [License Compliance](docs/en/DEPENDENCIES_GUIDELINES.md#5-license-compliance)
  - [Evaluation Process](docs/en/DEPENDENCIES_GUIDELINES.md#6-evaluation-process)
  - [Ongoing Review](docs/en/DEPENDENCIES_GUIDELINES.md#7-ongoing-review)
  - [Exception Rules](docs/en/DEPENDENCIES_GUIDELINES.md#8-exception-rules)

### Pull request templates

Pull request templates are maintained in lepusinc/.github:

- <https://github.com/lepusinc/.github/tree/main/.github/PULL_REQUEST_TEMPLATE>
- <https://github.com/lepusinc/.github/blob/main/.github/PULL_REQUEST_TEMPLATE/with_ticket.md>
- <https://github.com/lepusinc/.github/blob/main/.github/PULL_REQUEST_TEMPLATE/without_ticket.md>
- <https://github.com/lepusinc/.github/blob/main/.github/PULL_REQUEST_TEMPLATE/release_or_ops.md>

### Framework-specific Guidelines

- [Laravel Guidelines](docs/en/laravel/README.md)
  - [Migration](docs/en/laravel/MIGRATION.md)
  - [Exception Handling / Logging](docs/en/laravel/EXCEPTION_HANDLING.md)

---

## 🏗️ Scope

These guidelines cover:

- Code review responsibilities and principles
- Pull Request templates and workflows
- Testing and evidence requirements
- Communication mindset for effective collaboration

---

## 📂 Directory Structure

```text
.
├── docs/
│   ├── en/               # English documentation
│   └── ja/               # Japanese documentation
├── LICENSE               # Repository license
├── README.md             # This file
└── README.ja.md          # Japanese README
```

---

## 🤝 Contribution

Suggestions for improvement are always welcome.  
Please open a Pull Request against the `main` branch.

⚠️ **Note:** External contributions (PRs from outside the Lepus Group) are **not accepted**.  
For details, see [CONTRIBUTING.md](CONTRIBUTING.md).

---

## 📜 License

This repository is licensed under the [MIT License](LICENSE).

---

© Lepus Group – Engineering Team
