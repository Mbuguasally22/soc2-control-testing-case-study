# Control Test Workpapers: Technical Controls (CP-05 to CP-08)

**Doc ID:** CP-GRC-003, Part 2
**Prepared by:** GRC Analyst, Cedarpoint Analytics
**Reviewed by:** Compliance Lead, 2026-10-30
**Test plan:** CP-GRC-001, version 1.1 (folder 01)
**Evidence requests:** folder 02, referenced below as ER-xx
**Test work performed:** 2026-10-15 to 2026-10-22

> **Fictional case study.** Cedarpoint Analytics is an invented company. All results, names of items and counts in these workpapers are made up for practice.

**Note on the test plan version.** Test plan version 1.1 was approved on 2026-10-30, after this test work was done. The analyst checked every rating in this part against both version 1.0 and version 1.1. No rating differs between the two versions.

---

## Summary of results

| Workpaper | Control | Items tested | Exceptions | Conclusion | Deficiency |
|---|---|---|---|---|---|
| WP-05 | CP-05 Encryption at rest and in transit | 6 of 6 data stores, 4 of 4 endpoints | 0 | Effective | None |
| WP-06 | CP-06 Changes reviewed before release | 40 of 324 (random), plus 9 emergency changes (targeted) | 2 random, 1 targeted | Not effective, operating | D-05, D-06 |
| WP-07 | CP-07 Vulnerability scanning and fixing | 5 of 26 scans, 25 of 138 findings | 3 | Not effective, operating | D-07 |
| WP-08 | CP-08 Alert triage | 40 of 604, extended by 25 | 1 | Effective with exception | D-08 |

Severity ratings for D-05 to D-08 are set in the deficiency log (folder 04).

---

## WP-05: Encryption at rest and in transit (CP-05)

| Item | Detail |
|---|---|
| Criteria | CC6.1, CC6.7 |
| Control owner | Engineering Lead |
| Frequency | Continuous |
| Test date | 2026-10-15 to 2026-10-16 |
| Evidence used | ER-09, ER-10, ER-39, ER-42 |

**Control as written:** Every data store holding customer data is encrypted at rest. Every customer-facing endpoint accepts only encrypted connections using TLS 1.2 or higher with a valid certificate.

**Objective:** Customer data cannot be read by someone who gets hold of storage media or intercepts network traffic.

**Design assessment:** Adequate. Encryption is a platform setting, not a choice made by each engineer. Certificate renewal is manual (see observation).

**Population and completeness:** The list of data stores holding customer data (ER-09) shows 6. The analyst compared it with the production inventory of data stores tagged as holding customer data (ER-42): 6 in both. There are 4 customer-facing endpoints in the same inventory. Population accepted. Both populations are under 25, so every item was tested.

**Procedures performed:** Inspection.
1. Inspected the encryption setting on each of the 6 data stores.
2. Inspected the transport security settings and certificate details for each of the 4 endpoints.
3. Reviewed the configuration change history (ER-39) for the whole period for any change that turned encryption off or lowered the minimum TLS version. A snapshot shows the state on one date only, so the change history is needed to show the control held throughout the period.

**Results: data stores**

| Data store | Encrypted at rest | Result |
|---|---|---|
| Primary customer database | Yes | Pass |
| Database read replica | Yes | Pass |
| Customer upload object storage | Yes | Pass |
| Backup storage | Yes | Pass |
| Analytics warehouse | Yes | Pass |
| Search index | Yes | Pass |

**Results: endpoints**

| Endpoint | Minimum TLS version | Plain HTTP blocked or redirected | Certificate days remaining at 2026-09-30 | Result |
|---|---|---|---|---|
| Web application | 1.2 | Yes | 74 | Pass |
| Public API | 1.2 | Yes | 74 | Pass |
| Webhook receiver | 1.2 | Yes | 61 | Pass |
| Customer file portal | 1.2 | Yes | 9 | Pass, with observation |

**Change history:** 11 configuration changes were made to these data stores and endpoints during the period. None turned off encryption or lowered the minimum TLS version. No lapse found.

**Observation:** The customer file portal certificate had 9 days left on 2026-09-30. Expiry dates are tracked in a spreadsheet and renewal is done by hand. An expired certificate would not expose data, but it would interrupt customer access (an Availability risk). Recommend automatic renewal or an expiry alert 30 days ahead. No deficiency is raised.

**Conclusion:** Effective. No exceptions in 6 of 6 data stores and 4 of 4 endpoints, and no lapse in the change history.
**Deficiency reference:** None

---

## WP-06: Production changes reviewed and approved before release (CP-06)

| Item | Detail |
|---|---|
| Criteria | CC8.1 |
| Control owner | Engineering Lead |
| Frequency | Per event |
| Test date | 2026-10-19 to 2026-10-21 |
| Evidence used | ER-11, ER-12, ER-40, ER-41 |

**Control as written:** Every change to production code is made through a pull request that needs approval from at least one reviewer who is not the author, before it is merged and deployed automatically. An emergency change may be released with the Engineering Lead's approval and must be peer reviewed within 2 business days.

**Objective:** Only reviewed and approved changes reach production.

**Design assessment:** Adequate for code changes, with two gaps.
1. Five repository administrators are on a bypass list that lets them merge without approval (ER-41). No alert fires when the bypass is used.
2. The control and the pipeline cover code deployments only. Direct changes made in the production cloud console, such as the MFA policy change found in WP-04, create no change record and need no approval. This closes the follow-up opened in WP-04.

**Population and completeness:** The deployment list (ER-11) shows 324 production deployments from 2026-04-01 to 2026-09-30. The analyst compared it with the pipeline run history (ER-40): 324 in both. Population accepted.

**Follow-up from WP-04:** The analyst searched the deployment list for 2026-06-03 to 2026-06-06. The MFA policy change is not in it, because it was a console action and not a code deployment. This confirms the second design gap above.

**Sample selection:** 40 of 324 chosen at random on 2026-10-13 (population of 250 or more, so 40 per test plan section 7), spread across all six months. In addition, the analyst tested all 9 emergency changes as a targeted set. Targeted items are reported separately and do not count toward the random sample result.

**Procedures performed:** Inspection. For each item, opened the pull request, confirmed the approver was not the author, and compared the approval time with the merge time.

**Results: random sample of 40**

| Outcome | Count |
|---|---|
| Approved by an independent reviewer before merge | 38 |
| Merged with no approval | 1 |
| Approved after merge | 1 |

**Exception detail**

| Item | Change | Merge date | Issue |
|---|---|---|---|
| 1 | CHG-2231 | Fri 2026-05-22 | Author merged own pull request using the admin bypass; no reviewer |
| 2 | CHG-2698 | Tue 2026-08-11 | Merged at 11:40 using the admin bypass; approval recorded Wed 2026-08-12 at 16:05 |

The analyst inspected both changes after the fact. CHG-2231 was a wording change to an email template. CHG-2698 was a fix to a reporting query. No customer impact was found, but neither change had independent review before release.

**Results: targeted emergency changes (reported separately)**

| Outcome | Count |
|---|---|
| Released with Engineering Lead approval and reviewed within 2 business days | 8 |
| Reviewed late | 1 |

CHG-2841 was released on Tue 2026-09-08 and its review is recorded on Thu 2026-09-17, which is 7 business days later. The designated reviewer was on leave and no backup reviewer was named.

**Root cause:** For both random exceptions, the repository administrator bypass was enabled and unmonitored. For the late emergency review, there is no backup reviewer rule.

**Pattern or one-off:** Pattern for the random exceptions. Both used the same bypass, which is 2 of 40 (5%). The late emergency review has a separate cause.

**Conclusion:** Not effective, operating. A random sample of 40 with 2 exceptions makes the control not effective under section 8 of the test plan. The emergency change exception does not change this conclusion, and it is reported in D-05. The scope gap for console changes is a design finding and is recorded as D-06.
**Deficiency references:** D-05 (operating), D-06 (design: console changes outside change control)

---

## WP-07: Vulnerabilities scanned weekly and fixed within deadlines (CP-07)

| Item | Detail |
|---|---|
| Criteria | CC7.1 |
| Control owner | Head of Security |
| Frequency | Weekly |
| Test date | 2026-10-15 to 2026-10-21 |
| Evidence used | ER-13, ER-14, ER-42, ER-43, ER-46 |

**Control as written:** The vulnerability scanner runs every week against all in-scope production and corporate assets. Critical findings are fixed within 15 days and high findings within 30 days of detection (ER-14).

**Objective:** Known weaknesses are found promptly and closed before they can be exploited.

**Design assessment:** Adequate, with an observation. The policy sets clear deadlines but has no route for formally accepting a risk when a fix cannot be made in time.

**Population and completeness:** The scan history (ER-46) shows 26 weekly runs, one each Monday from 2026-04-06 to 2026-09-28. The period contains 26 Mondays, so the list is complete. The same export lists 138 critical and high findings detected in the period. Population accepted.

**Sample selection:** 5 of 26 scan runs and 25 of 138 findings, chosen at random on 2026-10-13 (test plan section 7). The scan runs sampled were 2026-04-20, 2026-05-25, 2026-06-29, 2026-08-03 and 2026-09-14.

**Procedures performed:** Inspection and reperformance.
1. For each sampled scan, confirmed it ran on the scheduled day and compared the assets scanned with the asset inventory (ER-42).
2. For each sampled finding, recomputed the days between detection and fix and compared the result with the deadline in the policy.
3. For any finding fixed late, checked for a documented risk acceptance (ER-43).

**Results: scan runs**

| Outcome | Count |
|---|---|
| Ran on the scheduled day and covered every asset on the inventory | 5 of 5 |

**Results: findings**

| Outcome | Count |
|---|---|
| Fixed within the deadline | 22 |
| Fixed after the deadline | 3 |
| Fixed late with a documented risk acceptance | 0 |

**Exception detail**

| Item | Severity | Detected | Deadline | Fixed | Days late |
|---|---|---|---|---|---|
| F-1 | Critical | 2026-06-01 | 2026-06-16 | 2026-06-20 | 4 |
| F-2 | High | 2026-05-04 | 2026-06-03 | 2026-06-12 | 9 |
| F-3 | High | 2026-07-13 | 2026-08-12 | 2026-08-26 | 14 |

**Information only, not part of the sample conclusion:** The analyst also reperformed the deadline check on all 138 findings. 17 (12%) were closed late or were still open past their deadline on 2026-09-30, and 4 of the 17 were Critical. This matches the 3 of 25 (12%) found in the sample.

**Root cause:** All three sampled exceptions are on the legacy reporting service. Its dependency upgrades need downtime, and no maintenance window was scheduled. No risk acceptance was recorded for any of them.

**Pattern or one-off:** Pattern. All 3 exceptions share one service and one cause.

**Observation:** Add a risk acceptance route to the vulnerability policy, with an approver and an expiry date, so a missed deadline is a recorded decision and not a silent overrun.

**Conclusion:** Not effective, operating. Scanning worked in all 5 sampled runs, but a sample of 25 findings with 3 exceptions makes the fixing part of the control not effective under section 8 of the test plan.
**Deficiency reference:** D-07

---

## WP-08: Security alerts triaged and documented (CP-08)

| Item | Detail |
|---|---|
| Criteria | CC7.2, CC7.3 |
| Control owner | Head of Security |
| Frequency | Per event |
| Test date | 2026-10-16 to 2026-10-22 |
| Evidence used | ER-15, ER-16, ER-44, ER-45 |

**Control as written:** Every alert of medium severity or higher is triaged by the security team within one business day (within 4 hours for high and critical), classified as true positive, false positive or duplicate, and closed with a written note or escalated to an incident ticket.

**Objective:** Real security events are noticed, judged and acted on.

**Design assessment:** Adequate with an observation. The alert rules do not include any rule for changes to identity provider or cloud console policies (see below and D-04).

**Population and completeness:** The first export of the alert list (ER-15) was missing alert IDs and arrived 2 days after its due date. The reissued export shows 604 alerts from 2026-04-01 to 2026-09-30. The analyst compared it with the platform's own alert total for the period (ER-44): 604 in both. Population accepted. The late delivery is an evidence timeliness note and not an exception on the control.

**Sample selection:** 40 of 604 chosen at random on 2026-10-15 (population of 250 or more), spread across all six months and all severities.

**Procedures performed:** Inspection. For each sampled alert, compared the alert time with the triage time, and checked for a classification and a written note.

**Results of the 40 sampled alerts**

| Outcome | Count |
|---|---|
| Triaged within the time limit, classified and documented | 39 |
| Closed without a triage note | 1 |

**Exception detail:** One medium severity alert from 2026-05-27 was closed with the single word "dup" and no link to the original alert. The analyst traced it and found it was a true duplicate of an alert triaged 20 minutes earlier, so no event was missed. The record still does not meet the control, because the reason cannot be seen from the alert itself.

**Extension:** Under test plan section 8, one exception in a larger sample means adding 25 more items. The analyst chose 25 more alerts at random from the 564 remaining and obtained the records (ER-45). All 25 were triaged within the time limit, classified and documented. No further exceptions.

**Link to WP-04:** The analyst searched the alert population for 2026-06-03 to 2026-06-06 for any alert about an identity provider or cloud console policy change. None exists, and the platform has no rule for such changes. This supports the root cause of D-04.

**Root cause:** The closing screen accepts any text as a note and has no required field for the original alert when a duplicate is closed.

**Pattern or one-off:** One-off.

**Conclusion:** Effective with exception. The single exception is logged.
**Deficiency reference:** D-08

---

## Reviewer sign-off

| Workpaper | Reviewed by | Review date | Comments |
|---|---|---|---|
| WP-05 | Compliance Lead | 2026-10-30 | Change history review agreed |
| WP-06 | Compliance Lead | 2026-10-30 | Emergency changes reported separately as planned |
| WP-07 | Compliance Lead | 2026-10-30 | Day counts reperformed |
| WP-08 | Compliance Lead | 2026-10-30 | Extension to 65 alerts accepted |

## Corrections after review

| Date | Workpaper | Correction |
|---|---|---|
| 2026-10-30 | WP-06 | Evidence log: ER-40 moved to the first request round so the completeness check came before the sample was selected. No change to the workpaper text. |
| 2026-10-30 | WP-07 | Sample selection date corrected from 2026-10-15 to 2026-10-13, to agree with the evidence request log |
| 2026-10-30 | WP-08 | Sample selection date corrected from 2026-10-13 to 2026-10-15, after the alert population was received and checked. ER-16 request date changed to match. |

No conclusion changed. Compliance Lead re-review of these corrections is pending.

*End of Part 2. Part 3 covers CP-09 to CP-12.*
