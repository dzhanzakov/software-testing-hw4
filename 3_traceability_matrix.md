# Traceability Matrix — Money Transfer Between Own Accounts

Maps every requirement to the tests that check it, so a missing test becomes visible.

**Status legend:** *Not run* = designed and reviewed, not yet executed. Update this column after each execution round (Passed / Failed + defect ID / Blocked).

## 1. Requirement → tests

| Requirement | Test cases | Technique | Status | Notes |
|---|---|---|---|---|
| REQ-3.1 Amount 100 – 500,000 | TR-AMT-001, 002, 003, 004 | Partitions + boundary values | Not run (4 cases) | Lower boundary 99 / 100 and upper boundary 500,000 / 500,001 covered |
| REQ-3.2 Daily limit 1,000,000 | TR-DAY-001, 002 | Decision table (amount × daily total × limit) | Not run (2 cases) | Only the boundary rules of the table are covered; see gaps below |
| REQ-3.3 SMS code above 100,000 | TR-OTP-001, 002, 003, 004; TR-AMT-003 | Boundary value + state transition | Not run (5 cases) | Boundary 100,000 / 100,001, valid code, 3 wrong codes, double tap |
| REQ-3.4 Insufficient balance | TR-BAL-001, 002 | Boundary value | Not run (2 cases) | Boundary balance / balance + 1 |

## 2. Test case → requirement (reverse view)

| Test case | Requirement(s) |
|---|---|
| TR-AMT-001 | REQ-3.1 |
| TR-AMT-002 | REQ-3.1 |
| TR-AMT-003 | REQ-3.1, REQ-3.3 |
| TR-AMT-004 | REQ-3.1 |
| TR-DAY-001 | REQ-3.2 |
| TR-DAY-002 | REQ-3.2 |
| TR-OTP-001 | REQ-3.3 |
| TR-OTP-002 | REQ-3.3 |
| TR-OTP-003 | REQ-3.3 |
| TR-OTP-004 | REQ-3.3 |
| TR-BAL-001 | REQ-3.4 |
| TR-BAL-002 | REQ-3.4 |

Every one of the 12 test cases traces to at least one requirement. No test case is without a requirement.

## 3. Requirements and behaviours that cannot be covered (and why)

| Item | Test cases | Why it is not covered | Action |
|---|---|---|---|
| REQ-3.2a — daily total resets at the day boundary | — **NOT COVERED** | The rule (time zone, midnight or rolling 24 h) is not defined in the requirements, and the test environment would need clock control | Raised as DEF-002; add a case once the rule is written down |
| REQ-3.2b — whether cancelled or rejected transfers count towards the daily total | — **NOT COVERED** | Not defined in the requirements | Raised as DEF-002 |
| REQ-3.3a — SMS code expiry (120 s), resend, and limit of wrong attempts | Expiry: release checklist items 3 and 4 only. Wrong attempts: TR-OTP-003 and checklist items 5 and 6 | Values are assumptions, not requirements | Raised as DEF-001; promote to test cases once the values are confirmed |
| REQ-3.3 × REQ-3.4 — amount above 100,000 **and** above the balance | — **NOT COVERED** | Order of the two checks is not defined | Raised as DEF-003 |
| Multi-device and Android runs | — | Out of scope for the first cycle (see test plan) | Repeat on Android if time allows |

## 4. Coverage summary

| Measure | Value |
|---|---|
| Requirements with at least one test | 4 of 4 |
| Test cases written | 12 |
| Test cases executed | 0 of 12 (update after execution) |
| Open uncovered items | 4 (all traced to DEF-001, DEF-002 or DEF-003) |
