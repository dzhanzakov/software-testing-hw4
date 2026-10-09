# Test Plan — Money Transfer Between Own Accounts

| | |
|---|---|
| **Document** | Test plan section (ISO/IEC/IEEE 29119-3 "Test plan") |
| **Feature** | Mobile banking app — Transfers → Between my accounts |
| **Version / date** | 1.0 · 9 October 2026 |
| **Author** | _[your name, group]_ |
| **Related documents** | `2_test_cases.md` (`TR-*`), `3_traceability_matrix.md`, `4_sms_release_checklist.md`, `5_defect_reports.md` (`DEF-*`), `6_ai_appendix.md` |

## 1. Scope

**In scope** (each item traces to a requirement):

| Req. | Requirement | Test focus |
|---|---|---|
| REQ-3.1 | Transfer amount must be 100 – 500,000 KZT | Equivalence partitions + boundary values (99, 100 / 500,000, 500,001) |
| REQ-3.2 | Daily transfer limit 1,000,000 KZT | Decision table: amount × daily total so far × limit |
| REQ-3.3 | SMS confirmation code required above 100,000 KZT | State-transition testing: code sent → entered → confirmed / expired / cancelled; boundary at 100,000 vs 100,001 |
| REQ-3.4 | Transfer rejected when balance is insufficient | Boundary: amount = balance, balance + 1. *Note: this condition was missing from the week 3 decision table; it is included here so it is not left uncovered.* |

**Out of scope** (explicitly, to prevent argument later):

- Transfers to other customers, other banks, and international transfers
- Currency conversion and fees
- Performance, load and penetration testing (separate plans)
- Web banking channel and tablet layouts
- Transfer history/statement formatting beyond "an entry is (not) created"
- SMS gateway internals (the telecom operator's delivery time is treated as a dependency, not tested)
- Daily total reset at the day boundary: needs clock control in the test environment and the rule is not defined in the requirements (see DEF-002); listed as an uncovered item in the traceability matrix

## 2. Approach

- **Levels and types:** system-level functional testing through the UI of the mobile app; confirmation testing of every fixed defect; regression of the SMS flow and daily-limit calculation before release.
- **Techniques:** equivalence partitioning and boundary value analysis (REQ-3.1, REQ-3.4), decision table (REQ-3.2), state-transition testing (REQ-3.3). One short exploratory session (30 min) on double-tap and app-interruption behaviour, with notes turned into defect reports.
- **Test cases:** 12 documented cases (`TR-AMT-*`, `TR-DAY-*`, `TR-OTP-*`, `TR-BAL-*`), each with exact preconditions, concrete data and a checkable expected result. The SMS code flow is also covered by a release checklist of at most 12 items.
- **Automation:** none in this release; test cases are written so they can be automated in week 5 (independent, concrete data).
- **Environment and data:** test environment 2; iOS 18.2 and Android 8 and 14 on physical devices; app build to be recorded on every defect. Test customer **TC-014** (balance 1,000,000 KZT, daily total 0) plus test customer **TC-021** (balance 50,000 KZT, daily total 0) for REQ-3.4. Both customers have a registered test phone that receives the SMS code. Test accounts must be reset between runs. Lead time for new test accounts: assume 2 working days.
- **Defect handling:** every defect gets a report with environment, preconditions, steps, expected/actual result, reproducibility, evidence and a trace to requirement and test case. Severity is proposed by the tester; priority is set by the product owner. Retest is mandatory before closing.
- **People and schedule:** one tester executes (author); product owner triages and accepts residual risk. Execution window: 2 working days after entry criteria are met, then 1 day for retest and regression.

## 3. Entry criteria (testing starts when all are true)

1. Build deployed to test environment 2, and the build number is recorded.
2. Smoke test passes: login → open Transfers → reach the amount screen.
3. Test customers TC-014 and the 50,000 KZT customer exist and have been reset to their starting state.
4. Requirements REQ-3.1 – REQ-3.4 are written down with testable acceptance criteria.
5. Test cases `TR-*` are written and reviewed against the requirements.

## 4. Exit criteria (testing stops when all are true, or the gap is accepted in writing)

1. All 12 planned test cases executed; none left "not run".
2. Zero open defects of critical or high severity.
3. Requirement coverage: 4 of 4 requirements (REQ-3.1 – REQ-3.4) have at least one passed test.
4. Boundary tests on amount (100 / 500,000), daily limit (1,000,000) and SMS threshold (100,000 / 100,001) all passed.
5. SMS release checklist fully ticked; regression of REQ-3.2 and REQ-3.3 cases passed on the final build.
6. Every defect fixed during the cycle has a passed confirmation test.
7. Residual risks listed in the completion report.

**If a criterion is not met:** the gap, its risk, and the decision to ship are written down and accepted by the product owner by name and date. Example: _"Shipped with one open medium defect DEF-0xx, accepted by the product owner on <date>."_

## 5. Top three product risks

| # | Risk (what could go wrong) | Impact | Likelihood | Mitigation in testing |
|---|---|---|---|---|
| 1 | **Double debit**: tapping Confirm twice, or a retry after a slow network response, debits the account twice | High — direct financial loss and regulatory exposure | Medium | `TR-OTP` double-tap case; exploratory session on interruption and retry; check balance and statement entry after each |
| 2 | **Daily limit miscalculated**: limit not updated after confirmation, wrong at the boundary (exactly 1,000,000), or not reset at the day boundary | High — customer can exceed the limit, or is wrongly blocked | Medium | Decision-table cases `TR-DAY`; daily total landing exactly on 1,000,000 (accepted) and on 1,000,001 (rejected); verify daily total after each confirmation |
| 3 | **SMS code weaknesses**: expired or reused code accepted, third wrong code does not cancel the transfer, or code not required at 100,001 | High — fraud and authorisation bypass | Low–medium | State-transition cases `TR-OTP`; boundary at 100,000 vs 100,001; checklist items for expiry (120 s) and wrong-code limit |

**Top project risks** (could stop testing): test data not reset between runs; test environment 2 unavailable; SMS gateway slow in the test environment, which blocks `TR-OTP` cases.

## 6. Assumptions

- Requirements REQ-3.1 – REQ-3.4 are the complete requirement set for this feature.
- The SMS code is valid for 120 seconds and three wrong codes cancel the transfer (as in the release checklist).
- This plan records the decisions made now and will be updated if scope, environment or requirements change.
