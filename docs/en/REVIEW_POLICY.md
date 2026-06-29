# Review Policy

## 🎯 Responsibilities of Review

1. **Alignment with Purpose and Requirements**  
   - Understand the PR’s background and purpose (ticket/AC) and verify that the acceptance criteria are satisfied.  

2. **Verification of Tests and Evidence**  
   - Confirm the presence and validity of unit/integration/E2E tests.  
   - Ensure CI is green.  
   - Check that screenshots, videos, or reproduction steps are provided as evidence.  

3. **Design and Maintainability**  
   - Verify separation of concerns, naming, consistency, complexity, and potential technical debt.  

4. **Security / Authorization / Quality Risks**  
   - Check input validation, authorization, secrets handling, external I/F changes, and performance impacts.  

5. **Refactoring Opportunities (Optional)**  
   - Identify redundancy, duplication, or structural improvements. Prioritize requirement fulfillment over refactoring.  
   - When suggesting improvements, consider not only **code quality** but also **release schedule impact**.  

---

## 📐 Operational Principles

1. **Single Responsibility Principle (SRP)**  
   - Bug fixes and refactoring should be separated in principle.  
   - Exception: only minimal, behavior-preserving refactoring may be included in the same PR (kept small and in separate commits).  

2. **Mandatory AC and Test Evidence**  
   - Acceptance criteria, test results (CI link/screenshots/videos), and reproduction steps are required.  
   - PRs lacking these will not be accepted.  

3. **Review Levels**  
   - **L0: Minor fix** → Light requirement check, CI/test focused.  
   - **L1: Feature addition/modification** → AC and evidence required.  
   - **L2: High-risk** (schema/security/billing logic/external integration) → Pair review + manual verification.  

👉 For detailed criteria and examples, see [Level Decision Guide](#-level-decision-guide).  

4. **Test-First & Quality Improvement**  
   - For bug fixes, follow “Reproduction test → Fix → Regression check.”  
   - When test coverage is insufficient, reviewers must compensate with AC/evidence checks.  
   - In the mid/long term, aim for continuous improvement of coverage starting with priority areas.  

5. **Commit Conventions (Reference)**  
   - See [Git Workflow Guidelines](GIT_WORKFLOW.md#6-commit-messages) for commit message conventions.  

6. **RACI (Responsibility Assignment) Clarification**  
   - Developer → Responsible for implementation & tests **(R)**  
   - Reviewer/Tech Lead → Accountable for quality gate, design & risk check **(A/C)**  
   - QA/PM → Accountable for AC definition & validation **(A/C)**  

👉 For detailed responsibilities, see [RACI Matrix](#-raci-matrix).  

7. **Explicit Impact Scope**  
   - PR must state affected areas (API/DB/auth/UI/external systems) to avoid blind spots.  

8. **PR Size Guidelines**  
   - Keep PRs as small as possible (≤15 min to review).  
   - Ideal: 200–400 LoC.  
   - Smaller PRs → higher bug detection rates.  
   - Requires good design upfront to ensure small, mergeable PRs.  

---

## 🏷️ Review Comment Severity & Handling

Review comments (primarily from CodeRabbit) are triaged by CodeRabbit's **severity** labels. This axis indicates the urgency of an individual finding, and is distinct from two other axes:

- **Review Levels (L0/L1/L2)** above — classify the *PR's* overall risk.
- **Guideline severity labels** (`[Mandatory]` / `[Required]` / `[Conditional]` / `[Recommended]`, defined in [AGENTS.md](AGENTS.md)) — classify how strictly a *guideline rule* must be followed.

Handle findings by CodeRabbit severity as follows:

| CodeRabbit Severity | Handling in this PR | Follow-up issue | Guideline label (≈) |
| --- | --- | --- | --- |
| Critical | Must fix (blocker) | Not allowed | `[Mandatory]` |
| Major | Fix as a rule; defer only with a ticket | Allowed (ticket required) | `[Required]` |
| Minor | Fix now, or create a follow-up issue | Allowed | `[Conditional]` / `[Recommended]` |
| Trivial / Nitpick | Optional | Allowed (or won't-fix) | `[Recommended]` |

For deferring Minor-or-below comments via a follow-up issue (including the required issue links), see the Dev Playbook "Jira 課題運用ガイド" (§7 フォローアップ課題): <https://lepus.atlassian.net/wiki/spaces/DEVPB/pages/41615364/Jira>.

---

## ✅ Quick Checklist

- [ ] PR includes AC and test evidence  
- [ ] CI is green  
- [ ] Requirements are satisfied  
- [ ] Security/auth/performance risks are considered  
- [ ] Impact scope is stated  
- [ ] Refactoring is separated into another PR or minimal & in separate commits  

---

## 💡 Review Mindset

### 1. Humility, Respect, and Trust
- Accept feedback with humility  
- Respect others and give feedback based on trust  
- Aim to improve the team’s assets, not to criticize  

### 2. Clarify the Purpose of Review
- Code review is for **value delivery, quality improvement, and team growth**  
- Not a place for competition or one-upmanship  
- Share the review scope when requesting review  
  - Example: “Please check only the logic consistency. Naming will be fixed later.”  

### 3. Constructive Communication
- Ask questions when code intent is unclear (avoid assumptions)  
- Highlight positives as well as issues  
- Always explain reasons for requested changes logically  
- When AI posts review comments, match the reviewee's language preference  

### 4. Attitude Toward Review
- Authors should accept feedback with humility  
- Reviewers are responsible for approved code  
- Feedback is for **product quality**, not personal critique  

### 5. Efficient Discussion
- Large design-level debates → hold synchronously (meeting or call)  
- Accept that multiple approaches can reach the same goal  

---

## 📊 Level Decision Guide

### ✅ Quick Decision
- **No behavior change at all** → Likely L0  
- **User-facing or external behavior changes** → L1+  
- **Public I/F (API/event/CLI/config semantics) changes** → L1+  
- If unsure → escalate to **higher level (L1)**  

### 🧭 Examples (L0)
- Comment/README changes  
- Removing unused imports, applying formatter  
- Adding logs (no PII, no perf risk)  
- Type hints or minor lint fixes  

### 🧭 Examples (L1)
- Bug fixes or new feature additions  
- API I/O or validation changes  
- Permission/role logic changes  
- Performance-sensitive query or data structure changes  

### ⚠️ Gray Zone Handling
- **CSS/Style**: user-visible → L1  
- **Logs**: safe addition = L0; level change/PII/high volume = L1  
- **Config files**: semantic change = L1; format cleanup = L0  

### 🏷️ Operational Rules
- When in doubt, escalate to **L1**  
- Prefix PR titles with `[L0]` / `[L1]` / `[L2]`  
- If behavior change is found in an L0 PR, reclassify as L1  

---

## 📊 RACI Matrix

| Role           | Responsible (R)             | Accountable (A)        | Consulted (C)                          | Informed (I) |
|----------------|-----------------------------|------------------------|----------------------------------------|--------------|
| **Developer**  | Implementation & tests      | ―                      | Tech Lead, Reviewer                    | QA/PM        |
| **Reviewer**   | Code review & suggestions   | ―                      | Tech Lead (for complex cases)          | QA/PM        |
| **Tech Lead**  | ―                           | Quality gate final A    | PM, QA (as needed)                     | Management   |
| **QA / PM**    | Test spec & AC definition   | Final AC validation    | Tech Lead, Reviewer, Developer         | Management   |
