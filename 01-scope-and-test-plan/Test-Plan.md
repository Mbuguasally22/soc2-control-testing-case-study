# Test Plan: SOC 2 Readiness Control Testing

**Doc ID:** CP-GRC-001
**Version:** 1.0
**Prepared by:** GRC Analyst, Cedarpoint Analytics
**Approved by:** Head of Security
**Approval date:** 2026-09-21

> **Fictional case study.** Cedarpoint Analytics is an invented company. The populations, dates and numbers in this plan are made up for practice. Nothing here is real client work or a real SOC 2 report.

---

## 1. Purpose

This plan sets out how the GRC analyst will test Cedarpoint's controls before the SOC 2 Type II audit begins. It is a readiness exercise run by the company itself. It does not replace the auditor's work, and the auditor will set its own tests and sample sizes.

The goal is to find gaps while there is still time to fix them, and to give leadership a clear view of what is and is not ready.

## 2. Engagement summary

| Item | Detail |
|---|---|
| Company | Cedarpoint Analytics (fictional), Canadian B2B SaaS, about 120 employees |
| Report type being prepared for | SOC 2 Type II |
| Trust Services Criteria in scope | Security (Common Criteria), Availability, Confidentiality |
| Readiness testing period | 2026-04-01 to 2026-09-30 (six months of past activity) |
| Planned audit observation window | 2027-01-01 to 2027-06-30 |
| Testing fieldwork | 2026-10-13 to 2026-10-30 |
| Criteria used | AICPA Trust Services Criteria (2017, with 2022 revised points of focus) |

Why test past activity: if the readiness test finds that a control did not operate during April to September, the company has until the audit window opens to fix the process and start producing clean evidence.

## 3. Systems in scope

| System | Purpose | Owner |
|---|---|---|
| Production cloud environment | Hosts the customer-facing application and customer data | Engineering Lead |
| Identity provider | Single sign-on and multi-factor authentication for workforce and production access | IT Manager |
| Source code repository and CI/CD pipeline | Code storage, peer review and deployment to production | Engineering Lead |
| Ticketing system | Access requests, change tickets, incident tickets | IT Manager |
| HR information system | Hire and termination records, background check status | People Operations Lead |
| Mobile device management tool | Laptop encryption, endpoint protection and patch status | IT Manager |
| Vulnerability scanner | Weekly scans of production and corporate systems | Head of Security |
| Logging and alerting platform | Security event collection and alerts | Head of Security |
| Learning platform | Security awareness training records | People Operations Lead |
| Vendor register | List of third parties and their risk reviews | Compliance Lead |

**Out of scope:** the marketing website, the internal wiki, personal devices not enrolled in device management, and any customer-owned systems.

## 4. Roles

| Role | Responsibility in this plan |
|---|---|
| GRC Analyst | Writes the plan, issues evidence requests, performs tests, records results and drafts the deficiency log and readiness summary |
| Head of Security | Approves the plan, reviews deficiencies, signs off severity ratings |
| Control owners | Supply evidence on time, confirm facts, own remediation of their gaps |
| Compliance Lead | Reviews the workpapers for completeness and keeps the evidence index |
| Leadership team | Receives the readiness summary and decides on resourcing for fixes |

## 5. Testing approach

### 5.1 Two questions for every control

1. **Design:** If the control runs as written, would it meet its objective? A control that cannot work as written fails here, even if people follow it.
2. **Operating effectiveness:** Did the control actually run, as written, every time it should have during the testing period?

### 5.2 Test procedures used

| Procedure | What it means | Example |
|---|---|---|
| Inquiry | Ask the control owner how the control works | Interview the IT Manager about the quarterly access review |
| Observation | Watch the control being performed | Watch a restore from backup |
| Inspection | Examine records, configurations or documents | Open a change ticket and check for an approval and a reviewer |
| Reperformance | Run the control again independently and compare results | Recompute a list of terminated users still holding active accounts |

Inquiry alone is never enough to conclude a control works. Every test uses inspection or reperformance as well.

### 5.3 Evidence standards

- Evidence comes from the system where possible (an export, a report, a configuration screen), not from a summary written by the control owner.
- Every item records the source, the date it was pulled and who pulled it.
- Screenshots must show a date and the system name.
- **Population completeness:** before sampling, the analyst checks that the list of items to sample from is complete. For example, the change population is compared against deployment counts in the pipeline, and the terminations list is compared against the HR system. A sample from an incomplete list proves nothing.
- Evidence is stored with a reference number that matches the evidence request list (folder 02).

## 6. Controls in scope

Expected populations are estimates and will be confirmed when evidence arrives. The sample column follows the sampling method in section 7.

| ID | Control | Criteria | Frequency | Owner | Expected population | Sample |
|---|---|---|---|---|---|---|
| CP-01 | Quarterly review of user access to production and key systems | CC6.2, CC6.3 | Quarterly | IT Manager | 2 quarters | 2 (all) |
| CP-02 | New user access is requested and approved before it is granted | CC6.2 | Per event | IT Manager | about 38 new accounts | 25 |
| CP-03 | Access is removed within one business day of termination | CC6.2, CC6.3 | Per event | IT Manager | about 16 terminations | 16 (all) |
| CP-04 | Multi-factor authentication is enforced for workforce and production access | CC6.1 | Continuous | IT Manager | All users, by system report | Full population report plus configuration check |
| CP-05 | Customer data is encrypted at rest and in transit | CC6.1, CC6.7 | Continuous | Engineering Lead | About 6 data stores | 6 (all) |
| CP-06 | Production changes are peer reviewed and approved before release | CC8.1 | Per event | Engineering Lead | About 320 changes | 40 |
| CP-07 | Vulnerabilities are scanned weekly and fixed within set deadlines | CC7.1 | Weekly | Head of Security | 26 scan runs, about 140 critical or high findings | 5 scans and 25 findings |
| CP-08 | Security alerts are triaged and documented | CC7.2, CC7.3 | Per event | Head of Security | About 600 alerts | 40 |
| CP-09 | Incident response plan is tested yearly and incidents are handled per the plan | CC7.3, CC7.4 | Annual and per event | Head of Security | 1 test, about 3 incidents | 1 test and 3 incidents (all) |
| CP-10 | Production backups run daily and restores are tested | A1.2, A1.3 | Daily and annual | Engineering Lead | About 183 daily jobs, 1 restore test | 25 jobs and 1 restore test |
| CP-11 | Critical vendors are risk reviewed before onboarding and yearly | CC9.2 | Annual | Compliance Lead | About 9 critical vendors | 9 (all) |
| CP-12 | Security awareness training is completed by new hires within 30 days and by all staff yearly | CC1.4, CC2.2 | Per event and annual | People Operations Lead | About 38 new hires, about 120 staff | 25 new hires and 25 staff |
| CP-13 | Background checks are completed before the start date | CC1.4 | Per event | People Operations Lead | About 38 new hires | 25 |
| CP-14 | A formal risk assessment is performed and approved yearly | CC3.2 | Annual | Head of Security | 1 | 1 |
| CP-15 | Laptops are encrypted, patched and protected by endpoint security | CC6.7, CC6.8 | Continuous | IT Manager | About 130 laptops | 25 |
| CP-16 | Confidential data is deleted on schedule and retired storage is wiped | C1.2 | Quarterly | Engineering Lead | 2 quarters | 2 (all) |

## 7. Sampling method

This is the plan's own sampling guide. It follows common audit practice but is not an AICPA rule, and the external auditor will apply its own method.

| How often the control runs | Items to test |
|---|---|
| Once a year | 1 |
| Quarterly | 2 (both quarters in the period) |
| Monthly | 3 |
| Weekly | 5 |
| Daily | 25 |
| Many times a day or per event, population under 250 | 25 |
| Many times a day or per event, population 250 or more | 40 |
| Any population under 25 | Test every item |

**How items are picked.**
- Items are selected at random from the complete, checked population, using a random number list saved with the workpaper.
- For time-based controls, the sample is spread across the whole period so that no month is missed.
- The analyst may add targeted items on top of the random sample (for example, all changes tagged emergency). Targeted items are reported separately and do not count toward the random sample result.

## 8. Exceptions and conclusions

An **exception** is any tested item that does not meet the control criteria. Missing evidence counts as an exception unless the owner can produce it within the evidence deadline.

| Situation | What the analyst does |
|---|---|
| One-time or annual control fails | Rated Not effective |
| Sample of 5 or fewer has any exception | Rated Not effective |
| Larger sample has 1 exception | Extend the sample by another 25 items |
| Extended sample has no further exception | Rated Effective with exception; the single exception is logged |
| Extended sample has another exception | Rated Not effective |
| Larger sample has 2 or more exceptions | Rated Not effective |

For each exception the analyst also records the root cause and whether it looks like a one-off slip or a repeating pattern, because a pattern is a more serious finding than a single miss.

**Conclusion ratings used in the workpapers**

| Rating | Meaning |
|---|---|
| Effective | Designed well and operated without exception |
| Effective with exception | Designed well, operated with a limited, explained exception |
| Not effective, design | Control as written cannot meet its objective |
| Not effective, operating | Control is well designed but was not performed reliably |
| Not tested | Could not be tested; reason recorded (for example, evidence not provided) |

## 9. Deficiency severity

Every exception or design gap goes into the deficiency log (folder 04) with a severity.

| Severity | Definition | Remediation target |
|---|---|---|
| Critical | Customer data or production systems could be exposed or changed without detection, and no other control would catch it | Fix before 2026-12-11 and retest |
| High | Control failure likely to produce a qualified opinion if present during the audit window | Fix before 2026-12-11 and retest |
| Medium | Control gap with a partial compensating control in place | Fix before audit window opens, or agree a dated plan with the Head of Security |
| Low | Documentation or timing issue with no effect on the control objective | Fix within the next quarter |

Severity is rated by the analyst and confirmed by the Head of Security. When two ratings seem possible, the analyst uses the higher one until the Head of Security decides.

## 10. Timeline

| Date | Milestone |
|---|---|
| 2026-09-21 | Test plan approved |
| 2026-09-28 | Evidence requests issued to control owners |
| 2026-10-12 | Evidence due |
| 2026-10-13 to 2026-10-30 | Testing and workpapers |
| 2026-11-02 to 2026-11-06 | Deficiencies reviewed with control owners |
| 2026-11-10 | Readiness summary delivered to leadership |
| 2026-12-11 | Remediation deadline for Critical and High deficiencies |
| 2026-12-14 to 2026-12-23 | Retesting of fixed controls |
| 2027-01-01 | Audit observation window opens |

## 11. Assumptions and limits

- Populations and counts are estimates for this fictional exercise and are confirmed when real evidence arrives.
- The analyst depends on control owners for evidence and has no direct write access to in-scope systems.
- A clean readiness result does not guarantee a clean audit. The external auditor decides its own scope, sampling and conclusions.
- Changes to scope or sample sizes after approval are recorded in section 12 and approved by the Head of Security.

## 12. Document history

| Version | Date | Change | Approved by |
|---|---|---|---|
| 1.0 | 2026-09-21 | First approved version | Head of Security |
