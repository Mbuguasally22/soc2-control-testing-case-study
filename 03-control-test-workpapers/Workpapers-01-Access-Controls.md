# Control Test Workpapers: Access Controls (CP-01 to CP-04)

**Doc ID:** CP-GRC-003, Part 1
**Prepared by:** GRC Analyst, Cedarpoint Analytics
**Reviewed by:** Compliance Lead, 2026-10-27
**Test plan:** CP-GRC-001 (folder 01)
**Evidence requests:** folder 02, referenced below as ER-xx

> **Fictional case study.** Cedarpoint Analytics is an invented company. All results, names of items and counts in these workpapers are made up for practice.

---

## Summary of results

| Workpaper | Control | Items tested | Exceptions | Conclusion | Deficiency |
|---|---|---|---|---|---|
| WP-01 | CP-01 Quarterly access review | 2 of 2 | 1 | Not effective, operating | D-01 |
| WP-02 | CP-02 Access approved before granted | 38 of 38 (25 sampled, then extended) | 1 | Effective with exception | D-02 |
| WP-03 | CP-03 Access removed after termination | 16 of 16 | 2 | Not effective, operating | D-03 |
| WP-04 | CP-04 MFA enforced | 127 of 127 users plus configuration history | 1 | Effective with exception | D-04 |

Severity ratings for D-01 to D-04 are set in the deficiency log (folder 04).

---

## WP-01: Quarterly review of user access (CP-01)

| Item | Detail |
|---|---|
| Criteria | CC6.2, CC6.3 |
| Control owner | IT Manager |
| Frequency | Quarterly |
| Test date | 2026-10-13 |
| Evidence used | ER-01, ER-02 |

**Control as written:** Each quarter the IT Manager reviews the list of users and their access for the identity provider, production cloud environment, source code repository and ticketing system, and removes or changes access that is no longer needed. The review is recorded in a ticket.

**Objective:** Access that is no longer appropriate is found and removed.

**Design assessment:** The control would meet its objective if every in-scope system is covered. The written procedure does not list the systems to cover, which makes it easy to miss one. Design is adequate with an observation (see below).

**Population and sample:** 2 quarterly reviews in the period (Q2 and Q3 2026). Both tested, per the test plan.

**Procedures performed:** Inspection and reperformance.
1. Opened the review ticket for each quarter and recorded the reviewer, date and outcome.
2. Compared the user list exported on each review date from each system (ER-02) against the systems the control requires.
3. Recomputed the number of accounts removed and agreed it to the ticket.

**Results**

| Item | Review date | Reviewer | Systems with user list attached | Accounts removed | Result |
|---|---|---|---|---|---|
| Q2 2026 | 2026-07-14 | IT Manager | 4 of 4 | 6 | Pass |
| Q3 2026 | 2026-10-06 | IT Manager | 3 of 4 (source code repository missing) | 4 | **Exception** |

**Exception detail:** The Q3 ticket states that all systems were reviewed, but no user list was attached for the source code repository and the IT Manager confirmed it was not reviewed. The repository holds production source code and deployment permissions. On 2026-10-14 the analyst exported the current repository user list and compared it to the HR active staff list: no accounts for former employees were found, so no inappropriate access was identified, but the review itself did not cover the system.

**Root cause:** The review checklist lives in the IT Manager's notes and does not name the repository. In Q2 the repository was reviewed from habit.

**Pattern or one-off:** One-off in the evidence, with a cause (no written checklist) that could repeat.

**Observation:** Add a system checklist to the access review procedure so every in-scope system is named.

**Conclusion:** Not effective, operating. With a sample of 5 or fewer, one exception makes the control not effective under section 8 of the test plan.
**Deficiency reference:** D-01

---

## WP-02: New access approved before it is granted (CP-02)

| Item | Detail |
|---|---|
| Criteria | CC6.2 |
| Control owner | IT Manager |
| Frequency | Per event |
| Test date | 2026-10-19 (extension 2026-10-21) |
| Evidence used | ER-03, ER-04 |

**Control as written:** A new account is created only after an access request ticket is approved by the user's manager (and the system owner for production access).

**Objective:** Users get only the access that someone accountable has approved.

**Design assessment:** Adequate. The ticket requires an approver and a role, and the identity provider creates accounts only from the ticket.

**Population and completeness:** The list of accounts created from 2026-04-01 to 2026-09-30 (ER-03) shows 38 accounts. The analyst compared it to the account-creation count in the identity provider's audit log: 38 in both. Population accepted.

**Sample selection:** 25 of 38 chosen at random on 2026-10-13 using a saved random number list (method in test plan section 7). The sample is spread across all six months.

**Procedures performed:** Inspection. For each sampled account, opened the access request ticket (ER-04) and compared the approval timestamp with the account creation timestamp in the identity provider.

**Results of the 25 sampled accounts**

| Outcome | Count |
|---|---|
| Approved by the right person before creation | 24 |
| Approved after creation | 1 |
| No ticket found | 0 |

**Exception detail:** One account, created 2026-06-18 at 14:05 for a new support engineer, was approved by the manager at 09:12 the next morning. The role was standard application access, with no production data access.

**Extension:** Under test plan section 8, one exception in a larger sample means adding 25 more items. Only 13 accounts remained in the population of 38, so the analyst tested all 13. The IT Manager provided the 13 tickets on 2026-10-20. All 13 were approved before creation. No further exceptions.

**Root cause:** The new hire started that morning and HR asked IT to create the account first. The manager approved the ticket after the fact.

**Pattern or one-off:** One-off.

**Conclusion:** Effective with exception. The single exception is logged.
**Deficiency reference:** D-02

---

## WP-03: Access removed within one business day of termination (CP-03)

| Item | Detail |
|---|---|
| Criteria | CC6.2, CC6.3 |
| Control owner | IT Manager |
| Frequency | Per event |
| Test date | 2026-10-14 |
| Evidence used | ER-05, ER-06 |

**Control as written:** When an employee leaves, IT disables their identity provider account, which also blocks access to connected systems, within one business day of their last working day.

**Objective:** Former employees cannot access company systems.

**Design assessment:** The control depends on HR telling IT about each departure. The notice is currently sent by email, with no ticket and no deadline. The design is weak at that point (see observation).

**Population and completeness:** The HR termination list (ER-05) shows 16 terminations from 2026-04-01 to 2026-09-30. The analyst compared it to the count of accounts disabled in the identity provider (ER-06): 16 in both, and every name matched. Population accepted. Because the population is under 25, all 16 were tested.

**Procedures performed:** Inspection and reperformance.
1. For each termination, compared the last working day to the identity provider's disable timestamp and counted business days between them.
2. For any late disable, searched sign-in logs for activity after the last working day.

**Results**

| Outcome | Count |
|---|---|
| Disabled within one business day | 14 |
| Disabled late | 2 |

**Exception detail**

| Item | Last working day | Disabled | Business days after | HR notice to IT | Sign-in after last day |
|---|---|---|---|---|---|
| 7 | Fri 2026-07-10 | Wed 2026-07-15 | 3 | Tue 2026-07-14 | None found |
| 12 | Thu 2026-08-20 | Mon 2026-08-24 | 2 | Mon 2026-08-24 | None found |

**Root cause:** In both cases HR's email reached IT after the last working day, and IT disabled the account the same day it learned of the departure. The delay sits in the HR to IT handoff.

**Pattern or one-off:** Pattern. Two of 16 (12.5%) share the same cause.

**Observation:** Replace the email with an automatic ticket created by the HR system on the day a termination is entered, with a due date of one business day after the last working day.

**Conclusion:** Not effective, operating. Two or more exceptions in a population tested in full make the control not effective under section 8 of the test plan. No access after the last working day was found for either account.
**Deficiency reference:** D-03

---

## WP-04: Multi-factor authentication enforced (CP-04)

| Item | Detail |
|---|---|
| Criteria | CC6.1 |
| Control owner | IT Manager |
| Frequency | Continuous |
| Test date | 2026-10-15 |
| Evidence used | ER-07, ER-08 |

**Control as written:** Multi-factor authentication (MFA) is required for every workforce user in the identity provider, and for sign-in to the production cloud console and the source code repository.

**Objective:** A stolen password alone cannot give access to company systems.

**Design assessment:** Adequate. Enforcement is set by policy in each system rather than left to individual users.

**Population and completeness:** The user report (ER-07) lists 127 active accounts as of 2026-09-30. The analyst reconciled it to 118 active employees in the HR system plus 9 contractors on the approved contractor list: 127. Population accepted.

**Procedures performed:** Inspection and reperformance.
1. Counted users with MFA enrolled in the report.
2. Inspected MFA policy settings and their change history in the identity provider, production cloud console and source code repository (ER-08) for the whole period.
3. For any period when a policy was off, reviewed sign-in logs for that window.

**Results**

| Check | Result |
|---|---|
| Users enrolled in MFA | 127 of 127 |
| Identity provider policy on throughout the period | Yes |
| Source code repository policy on throughout the period | Yes |
| Production cloud console policy on throughout the period | **No** |

**Exception detail:** The change history shows the production cloud console MFA requirement was switched off on 2026-06-03 at 10:20 and back on on 2026-06-06 at 09:45, about 71 hours. An engineer turned it off during a migration task. The sign-in logs for that window show 9 console sign-ins by 4 users. Two of those sign-ins used a password only. Both users were enrolled in MFA, and no unusual activity was found in the logs for either session.

**Root cause:** The setting was changed directly in the console with no change ticket, and no alert fires when the policy changes.

**Pattern or one-off:** One-off event. The missing alert means it could happen again without being noticed.

**Follow-up:** The analyst will check whether this change appears in the change population when testing CP-06 (change management).

**Conclusion:** Effective with exception. Enrollment was complete and the control worked except for a limited, explained lapse of about three days.
**Deficiency reference:** D-04

---

## Reviewer sign-off

| Workpaper | Reviewed by | Review date | Comments |
|---|---|---|---|
| WP-01 | Compliance Lead | 2026-10-27 | Evidence references checked |
| WP-02 | Compliance Lead | 2026-10-27 | Extension to all 38 accepted |
| WP-03 | Compliance Lead | 2026-10-27 | Business day counts reperformed |
| WP-04 | Compliance Lead | 2026-10-27 | Sign-in log review agreed |

*End of Part 1. Part 2 covers CP-05 to CP-08.*
