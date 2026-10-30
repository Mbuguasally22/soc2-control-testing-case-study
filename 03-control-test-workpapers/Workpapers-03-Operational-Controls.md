# Control Test Workpapers: Operational Controls (CP-09 to CP-12)

**Doc ID:** CP-GRC-003, Part 3
**Prepared by:** GRC Analyst, Cedarpoint Analytics
**Reviewed by:** Compliance Lead for WP-09, WP-10 and WP-12; Head of Security for WP-11 (the Compliance Lead owns control CP-11 and cannot review a test of their own control), 2026-10-30
**Test plan:** CP-GRC-001, version 1.1 (folder 01)
**Evidence requests:** folder 02, referenced below as ER-xx
**Test work performed:** 2026-10-20 to 2026-10-28

> **Fictional case study.** Cedarpoint Analytics is an invented company. All results, names of items and counts in these workpapers are made up for practice.

**Note on the test plan version.** Test plan version 1.1 was approved on 2026-10-30, after this test work was done. The analyst checked every rating in this part against both version 1.0 and version 1.1. No rating differs between the two versions.

**Note on late evidence.** Test plan section 8 says missing evidence counts as an exception unless the owner produces it by the evidence deadline. This applies where the evidence is the proof that a control operated (see WP-11). It does not apply to population lists the analyst needs only to draw a sample. Those are recorded as timeliness notes (see ER-15 in WP-08).

---

## Summary of results

| Workpaper | Control | Items tested | Exceptions | Conclusion | Deficiency |
|---|---|---|---|---|---|
| WP-09 | CP-09 Incident response plan and incident handling | 1 of 1 tabletop test, 3 of 3 incidents | 0 | Effective | None |
| WP-10 | CP-10 Backups and restore tests | 50 of 183 daily jobs (25, then 25 more), 1 of 1 restore test | 1 | Effective with exception | D-09 |
| WP-11 | CP-11 Critical vendor risk reviews | 9 of 9 critical vendors | 2 | Not effective, operating | D-10, D-11 |
| WP-12 | CP-12 Security awareness training | 25 of 38 new hires, 25 of 94 employees (annual) | 2 | Not effective, operating | D-12 |

Severity ratings for D-09 to D-12 are set in the deficiency log (folder 04).

---

## WP-09: Incident response plan tested and incidents handled (CP-09)

| Item | Detail |
|---|---|
| Criteria | CC7.3, CC7.4 |
| Control owner | Head of Security |
| Frequency | Annual and per event |
| Test date | 2026-10-20 to 2026-10-21 |
| Evidence used | ER-17, ER-18, ER-47 |

**Control as written:** The incident response plan is reviewed and tested by a tabletop exercise at least once a year, and actions from the test are tracked. Every incident is recorded in a ticket with a severity, a named incident commander, a timeline and a closure date. Incidents of medium severity or higher get a post-incident review within 10 business days of closure. Affected customers are told within 72 hours of Cedarpoint confirming an incident that affected their data or service.

**Objective:** Incidents are handled the same way every time, and the company learns from them.

**Design assessment:** Adequate with observation. The plan names one incident commander and no backup (see observation).

**Population and completeness:** The incident ticket list (ER-18) shows 3 incidents from 2026-04-01 to 2026-09-30. The analyst compared it with the alerts the logging platform escalated to incident tickets (ER-47): 3 escalations, matching the same 3 ticket IDs. Population accepted. All 3 were tested because the population is under 25.

**Limit of the completeness check:** Both lists show only incidents that were detected and declared. An event nobody noticed would not appear in either. The 71-hour MFA lapse found in WP-04 was not declared as an incident. The plan defines an incident as an event that results in unauthorized access to, or loss, disclosure or unavailability of, company or customer data or systems. No unauthorized access was found in the lapse (WP-04), so it does not meet that definition. The detection gap is covered in D-04 and the alert rule gap in WP-08.

**Procedures performed:** Inspection.
1. Inspected the plan's review record, the tabletop exercise record and the action tracker.
2. For each incident, inspected the ticket for severity, incident commander, timeline, closure date, post-incident review date and customer notice where required.

**Results: annual test**

| Check | Result |
|---|---|
| Plan reviewed and approved in the period | Yes, version 3.2 approved 2026-04-28 |
| Tabletop exercise held in the 12 months to 2026-09-30 | Yes, 2026-05-20, ransomware scenario, 11 attendees including the Head of Security, Engineering Lead, IT Manager, Compliance Lead and People Operations Lead |
| Lessons-learned actions tracked | 7 raised, 6 closed by 2026-09-30, 1 open and due 2026-12-15 |

**Results: incidents**

| Incident | Opened | What happened | Severity | Commander named | Closed | Post-incident review | Customer notice |
|---|---|---|---|---|---|---|---|
| INC-2026-011 | Fri 2026-05-08 | One employee entered credentials on a phishing page | Medium | Yes | Mon 2026-05-11 | Fri 2026-05-15 (4 business days) | Not required, no customer data affected |
| INC-2026-014 | Tue 2026-07-21 | Public API unavailable for 47 minutes after a failed database failover | High | Yes | Wed 2026-07-22 | Fri 2026-07-31 (7 business days) | Sent Wed 2026-07-22, inside 72 hours |
| INC-2026-018 | Tue 2026-09-15 | A customer usage report was emailed to the wrong customer contact | Medium | Yes | Fri 2026-09-18 | Thu 2026-09-24 (4 business days) | Sent Wed 2026-09-16, inside 72 hours |

All checks passed for all 3 incidents.

**Observation:** The plan names one incident commander and no backup. The single open tabletop action (due 2026-12-15) is to name backups, so the gap is known and has an owner and a date. No deficiency is raised.

**Conclusion:** Effective. The annual test was held and tracked, and 3 of 3 incidents met every requirement.
**Deficiency reference:** None

---

## WP-10: Backups run daily and restores are tested (CP-10)

| Item | Detail |
|---|---|
| Criteria | A1.2, A1.3 |
| Control owner | Engineering Lead |
| Frequency | Daily and annual |
| Test date | 2026-10-20 to 2026-10-23 |
| Evidence used | ER-19, ER-20, ER-51 |

**Control as written:** The production databases and customer upload storage are backed up automatically every day. A failed job sends a notification and is re-run within one business day. A restore test from backup is performed at least once a year and its time is compared with the recovery time objective of 4 hours.

**Objective:** Customer data can be recovered after loss or corruption, within the time customers were told to expect.

**Design assessment:** Adequate with two observations. Failure notifications go to a shared mailbox (see root cause). The procedure does not say which data stores a restore test must cover (see observation).

**Population and completeness:** The job log (ER-19) shows 183 daily jobs, one for each calendar day from 2026-04-01 to 2026-09-30, with no date missing and no duplicate. 179 succeeded and 4 failed. Population accepted.

**Sample selection:** 25 of 183 chosen at random on 2026-10-13 (test plan section 7), spread across all six months.

**Procedures performed:** Inspection and reperformance.
1. For each sampled job, read the status in the job log. For any failed job, compared the failure time with the re-run time (ER-20) and counted business days.
2. For the restore test, inspected the test record and compared the restore time with the 4-hour objective.
3. Inspected the notification settings and who receives failure notices (ER-51).

**Results: sample of 25 jobs**

| Outcome | Count |
|---|---|
| Succeeded on the first run | 24 |
| Failed and re-run within one business day | 0 |
| Failed and re-run late | 1 |

**Exception detail:** The job for Tue 2026-08-18 failed and was re-run on Thu 2026-08-20, which is 2 business days later.

**Extension:** Under test plan section 8, one exception in a larger sample means adding 25 more items. The analyst chose 25 more jobs at random from the 158 remaining. All 25 succeeded on the first run. No further exceptions.

**Information only, not part of the sample conclusion:** The analyst also checked all 4 failed jobs in the population.

| Failed job | Re-run | Business days | Result |
|---|---|---|---|
| Thu 2026-05-14 | Fri 2026-05-15 | 1 | On time |
| Fri 2026-06-26 | Mon 2026-06-29 | 1 | On time |
| Tue 2026-08-18 | Thu 2026-08-20 | 2 | Late |
| Wed 2026-09-09 | Thu 2026-09-10 | 1 | On time |

This matches the sample result: the same single failure was late.

**Results: restore test**

| Check | Result |
|---|---|
| Restore test performed in the 12 months to 2026-09-30 | Yes, 2026-06-10 |
| What was restored | Primary customer database, from the backup of 2026-06-09, into a test environment |
| Time against objective | 3 hours 10 minutes against 4 hours |
| Data check | Row counts and checksums on 12 key tables matched |
| Record signed off | Engineering Lead, 2026-06-11 |

**Root cause of the late re-run:** Failure notices go to a shared mailbox that has no assigned reader (ER-51). The three on-time re-runs happened because an engineer happened to look at the backup dashboard that day. Nothing requires that.

**Pattern or one-off:** One-off in the evidence, with a cause (an unmonitored mailbox) that could repeat.

**Observation:** The restore test covered the primary database only. It did not cover customer upload storage or the analytics warehouse. The control as written sets no scope, so this is not an exception, but a yearly test of each type of data store would meet the objective better.

**Conclusion:** Effective with exception. The restore test met its objective, and the single late re-run is logged.
**Deficiency reference:** D-09

---

## WP-11: Critical vendors risk reviewed before onboarding and yearly (CP-11)

| Item | Detail |
|---|---|
| Criteria | CC9.2 |
| Control owner | Compliance Lead |
| Frequency | Annual and at onboarding |
| Test date | 2026-10-21 to 2026-10-26 |
| Evidence used | ER-21, ER-22, ER-48, ER-50 |

**Control as written:** Before a new critical vendor is onboarded, the Compliance Lead completes a risk review covering data access, a current independent security report (such as a SOC 2 report or ISO 27001 certificate), subprocessors and incident history, and the Head of Security approves it. Every critical vendor is reviewed again at least once every 12 months.

**Objective:** Third parties that handle customer data or run key services are checked before use and while in use.

**Design assessment:** Adequate with observation. The vendor register has no "next review due" field, and review dates are tracked in the Compliance Lead's calendar.

**Population and completeness:** The register (ER-21) lists 9 critical vendors. The analyst compared it with the third-party applications connected to the identity provider and production environment (ER-48): 31 connected applications, all on the register, and the 9 critical vendors match. Population accepted. All 9 were tested because the population is under 25.

**Procedures performed:** Inspection.
1. For vendors with a yearly review falling in the period, compared the review date with the prior review date and checked the gap was 12 months or less.
2. For vendors onboarded in the period, compared the review date with the contract start date (ER-50).
3. Checked for Head of Security approval and a current security report on file.
4. Compared the date each review was produced with the evidence deadline of 2026-10-12.

**Results**

| Vendor | Service | What was tested | Review date | Approved | Evidence by deadline | Result |
|---|---|---|---|---|---|---|
| V-01 | Cloud hosting provider | Yearly (prior 2025-05-20) | 2026-05-12 | Yes | Yes | Pass |
| V-02 | Identity provider | Yearly (prior 2025-06-09) | 2026-06-03 | Yes | Yes | Pass |
| V-03 | Email delivery provider | Yearly (prior 2025-04-28) | 2026-04-22 | Yes | Yes | Pass |
| V-04 | Payment processor | Yearly (prior 2025-07-21) | 2026-07-15 | Yes | Yes | Pass |
| V-05 | Customer support platform | Yearly (prior 2025-08-12) | 2026-08-05 | Yes | Yes | Pass |
| V-06 | Logging and alerting platform | Yearly (prior 2025-06-02) | 2026-05-28 | Yes | Yes | Pass |
| V-07 | Data analytics tool | Onboarding (contract start 2026-06-15) | 2026-06-10 | Yes | Yes | Pass |
| V-08 | Source code hosting provider | Yearly (prior 2025-07-08) | 2026-07-02 | Yes | **No, received 2026-10-19** | **Exception** |
| V-09 | Document signing service | Onboarding (contract signed 2026-07-17, start 2026-07-20) | **2026-10-16** | Yes | **No, received 2026-10-19** | **Exception** |

**Exception detail**
- **V-08:** The review was done on time, 359 days after the prior review. The exception is that the record was not produced by the deadline. Test plan section 8 treats missing evidence as an exception.
- **V-09:** No review existed before onboarding. The review document is dated 2026-10-16, four days after the evidence deadline and after the analyst asked for it, so it was prepared in response to the request and is not evidence that a review happened before the contract started. The analyst did not accept it as period evidence. The vendor does hold a current SOC 2 Type II report, which the late review confirms, so the risk looks low, but the control did not operate.

**Root cause:** For V-09, the contract was signed from a business request without going through the vendor review step, and nothing blocks signature until a review is approved. For V-08, the review file was kept outside the register and the owner could not find it by the deadline.

**Pattern or one-off:** Separate causes. Two of 9 (22%).

**Observation:** Add a "next review due" field to the register, and require an approved review before contract signature.

**Conclusion:** Not effective, operating. This is a population tested in full with more than 5 items, so under section 8 of the test plan, 2 or more exceptions make the control not effective. Only V-09 is a control that did not operate. If V-08 were set aside the result would be Effective with exception, but the plan does not allow the analyst to set it aside. Any change would need an approved plan amendment. The deficiency log rates them separately.
**Deficiency references:** D-10 (V-09, no review before onboarding), D-11 (V-08, evidence not produced by the deadline)

**Same rule applies in Part 4.** ER-29 (the Q3 deletion run record for CP-16) was also received after its deadline and will be treated the same way.

---

## WP-12: Security awareness training completed (CP-12)

| Item | Detail |
|---|---|
| Criteria | CC1.4, CC2.2 |
| Control owner | People Operations Lead |
| Frequency | Per event and annual |
| Test date | 2026-10-23 to 2026-10-28 |
| Evidence used | ER-23, ER-24, ER-49 |

**Control as written:** Every new hire completes security awareness training within 30 days of their start date. Every employee completes the annual training during the yearly campaign (2026-04-06 to 2026-05-15). Completion is recorded by the learning platform.

**Objective:** People know their security responsibilities and how to report problems.

**Design assessment:** Weak at one point. The course is assigned automatically when the HR record is created, and not on the start date. If the record is created late, the new hire starts with less than 30 days left (see root cause).

**Population and completeness:**
- **New hires:** The HR list (ER-24) shows 38 new hires from 2026-04-01 to 2026-09-30. The training export (ER-23) shows 38 new-hire assignments. Names and start dates match.
- **Annual training:** 94 employees were active on both 2026-04-06 and 2026-05-15 (ER-49). The platform shows 94 assigned to the campaign.

Both populations accepted.

**Sample selection:** 25 of 38 new hires and 25 of 94 employees, each chosen at random on 2026-10-23 (test plan section 7), the new hires spread across all six months.

**Procedures performed:** Inspection and reperformance. For each sampled new hire, recomputed the days between the start date and the completion date. For each sampled employee, checked that the completion date fell inside the campaign window.

**Results: new hires (25 sampled)**

| Outcome | Count |
|---|---|
| Completed within 30 days of start | 23 |
| Completed after 30 days | 2 |

**Exception detail**

| Item | Start date | HR record created | Course assigned | Completed | Days after start | Days late |
|---|---|---|---|---|---|---|
| H-1 | Mon 2026-06-01 | Mon 2026-06-15 | 2026-06-15 | Mon 2026-07-13 | 42 | 12 |
| H-2 | Mon 2026-08-10 | Mon 2026-08-24 | 2026-08-24 | Mon 2026-09-28 | 49 | 19 |

**Results: annual training (25 sampled)**

| Outcome | Count |
|---|---|
| Completed inside the campaign window | 25 |
| Not completed or completed late | 0 |

**Information only, not part of the sample conclusion:** The analyst checked all 38 new hires. 5 (13%) completed after 30 days. In all 5, the HR record was created more than 7 days after the start date.

**Root cause:** The course is assigned when the HR record is created. For these new hires the record was entered two weeks after they started, leaving them under 30 days for a course they would normally finish in about four weeks.

**Pattern or one-off:** Pattern. All 5 late completions share one cause, and 2 of 25 (8%) in the sample.

**Observation:** The same HR timing gap appears in CP-03 (termination notices reaching IT late, WP-03). A rule that HR records are entered by the start date or last working day would help both controls.

**Conclusion:** Not effective, operating. The annual training passed, but a sample of 25 new hires with 2 exceptions makes the new-hire part not effective under section 8 of the test plan.
**Deficiency reference:** D-12

---

## Reviewer sign-off

| Workpaper | Reviewed by | Review date | Comments |
|---|---|---|---|
| WP-09 | Compliance Lead | 2026-10-30 | Completeness check against escalated alerts agreed |
| WP-10 | Compliance Lead | 2026-10-30 | Extension to 50 jobs accepted; failed job table reperformed |
| WP-11 | Head of Security | 2026-10-30 | Reviewed in place of the Compliance Lead, who owns CP-11; V-09 late review not accepted as period evidence |
| WP-12 | Compliance Lead | 2026-10-30 | Day counts reperformed |

*End of Part 3. Part 4 covers CP-13 to CP-16.*
