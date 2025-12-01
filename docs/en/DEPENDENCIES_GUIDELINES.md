# Third-Party Package Selection Guidelines (Application Level)

This document defines guidelines for selecting third-party packages used by applications to reduce the risks of external dependencies, including supply chain attacks.

## Objectives

- Adopt only trustworthy packages to ensure maintainability and safety
- Maintain a secure ecosystem that assumes ongoing vulnerability management
- Avoid person-dependent decisions and apply unified criteria across projects

---

# 1. Trustworthiness (Popularity & Ecosystem Trust)

## MUST

- Demonstrates sufficient usage among similar packages  
  Roughly top 3-5 by downloads
- Has a healthy download volume  
  100k+ downloads
- Updated within the last six months
- Follows SemVer-based versioning

## SHOULD

- At least 1-2 years since initial release  
  (allow exceptions for official SDKs from major vendors)
- Does not have unnecessary dependency bloat
- Does not introduce frequent breaking changes

---

# 2. Security & Compliance

## MUST

- No unresolved critical/high vulnerabilities
  - GitHub Security Advisories
  - NVD (National Vulnerability Database)
  - Packagist Security Advisories, etc.
- Maintainer team is not exclusively anonymous accounts

## SHOULD

- Supports generating an SBOM (Software Bill of Materials)
- Security incidents are handled promptly
- Compatible with automated security updates (Dependabot / Renovate, etc.)
- Release tags are signed (or protected)

---

# 3. Code Quality

## MUST

- Test code exists and CI is running
- Basic documentation (README, usage examples, etc.) is available

## SHOULD

- Supports static analysis tools (PHPStan / Psalm / ESLint / MyPy, etc.)
- Code is readable and not excessively complex
- Architecture principles are clear and APIs are consistent

---

# 4. Project Sustainability

## MUST

- Commits or responses to issues within the last six months
- Issues/PRs are not left unaddressed

## SHOULD

- Multiple maintainers (bus factor is not 1)
- Supported by sponsors, companies, or a community

---

# 5. License Compliance

## MUST

- License permits commercial use  
  - MIT, Apache-2.0, BSD variants, etc.
- Do not adopt licenses that conflict with the application (e.g., GPL)

## SHOULD

- License compatibility is confirmed for dependencies as well  
  (verifiable via SBOM, etc.)

---

# 6. Evaluation Process

1. All MUST items must be satisfied
2. Meeting 70% or more of the SHOULD items is recommended
3. If a package fails the criteria but you still want to adopt it, submit an **exception request**
4. Exceptions require a team review or architect approval

---

# 7. Ongoing Review

- Periodically inventory existing packages
  - Vulnerability checks
  - Maintenance status review
  - Consider replacing deprecated packages
- Keep Dependabot / Renovate enabled for continuous updates

---

# 8. Exception Rules

In the following cases, packages that do not meet the usual MUST/SHOULD criteria may be allowed.

## 8.1 Official SDKs / Official Clients

- Packages officially provided and maintained by vendors such as AWS, GCP, Stripe, PayPal, Slack
- Short time since initial release or low download counts are exempt from the criteria
- If responses to security advisories are slow, evaluate alternatives

## 8.2 Domain-Specific Libraries for Business Requirements

- No alternatives exist to meet specific business requirements
- Usage is internal and impact is limited
- Perform additional safety evaluations (e.g., code reviews) for the package

## 8.3 Emerging but Promising Libraries

- Community is growing quickly and could become a future standard
- Maintainers are reliable and active (e.g., well-known OSS developers)
- Introduce as a "trial adoption" and monitor in production for six months

## 8.4 Assuming Internal Forks/Modifications

- OSS version has issues, but we can fork and maintain it internally
- License must permit forking
- Once forked, dependency is detached from the external supply chain, so criteria may be relaxed

## 8.5 Packages Required by Infrastructure or Security Products

- APM (New Relic, etc.), security agents, observability agents
- Required for platform compatibility
- Follow the vendor's support policies

---

<!-- # Appendix: Evaluation Sheet (Template)

| Item                          | MUST | SHOULD | Evaluation |
| ----------------------------- | ---- | ------ | ---------- |
| Download volume & popularity  | ✔    |        |            |
| Maintenance (updated in 6mo)  | ✔    |        |            |
| Critical vulnerabilities      | ✔    |        |            |
| Test code and CI              | ✔    |        |            |
| License compatibility         | ✔    |        |            |
| Community activity            |      | ✔      |            |
| Multiple maintainers          |      | ✔      |            |
| Documentation readiness       |      | ✔      |            |

*All MUST items must be satisfied; adoption is allowed when a reasonable share of SHOULD items are met.*  
*For exceptions, record the "exception reason" and "approver."*

--- -->
