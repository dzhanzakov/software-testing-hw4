# Defect Reports — Money Transfer Between Own Accounts

Three defect reports **designed from gaps in the requirements** (allowed by the assignment: "you can mock them or design from lack of requirements"). They are requirement defects found by reviewing REQ-3.1 – REQ-3.4, not failures observed in a running app. Each can be reproduced by reading the requirements document, with no app needed.

> **Before submitting:** open your real week 3 transfer brief and confirm each "Actual result" below is true of it. If the brief already defines the item, delete or replace that report. Severity and priority are **proposals**; the product owner decides priority.

| ID | Title | Severity (proposed) | Priority (proposed) | Status |
|---|---|---|---|---|
| DEF-001 | REQ-3.3 SMS code: lifetime, wrong-attempt limit and resend rules are not defined | High | High | New |
| DEF-002 | REQ-3.2 Daily limit: day boundary and which transfers count are not defined | High | Medium | New |
| DEF-003 | REQ-3.3 / REQ-3.4: order of SMS code and balance check is not defined for amounts above 100,000 KZT with insufficient balance | Medium | Medium | New |

---

## DEF-001 · REQ-3.3 SMS code: lifetime, wrong-attempt limit and resend rules are not defined

| Field | Content |
|---|---|
| **Title** | REQ-3.3 SMS code: lifetime, wrong-attempt limit and resend rules are not defined |
| **Environment** | Document review. Source: requirements REQ-3.1 – REQ-3.4 for the money transfer feature (week 3 transfer brief); test plan v1.0 and test cases `TR-OTP-*` in this pack. No app build involved. |
| **Preconditions** | You have the requirements document open at REQ-3.3 ("SMS code required for transfers above 100,000 KZT"). |
| **Steps to reproduce** | 1. Open the requirements document and find REQ-3.3.<br>2. Read the full text of REQ-3.3 and any note attached to it.<br>3. Search the whole document for the words "expire", "attempts", "wrong code", "resend".<br>4. Open `4_sms_release_checklist.md` items 3, 4, 5, 6 and 10 and try to find, in the requirements, the value each item checks. |
| **Expected result** | REQ-3.3 (or a linked requirement) states: how long a code is valid, how many wrong codes are allowed before the transfer is cancelled, whether a new code can be requested, and whether an old code stays valid. |
| **Actual result** | REQ-3.3 states only the threshold (code required above 100,000 KZT). Steps 3 and 4 find none of the four rules. The values used in the checklist (120 seconds, three attempts) and in `TR-OTP-003` are assumptions taken from the course examples. |
| **Reproducibility** | Deterministic — a document gap, visible on every reading (confirm by re-reading once yourself before submitting) |
| **Severity / priority** | Severity **High** (proposed) — the SMS code is the authorisation step for large transfers, and a wrong rule means fraud exposure or locked-out customers. Priority **High** (proposed) — `TR-OTP-003` and four checklist items cannot be given a verified expected result until it is answered. |
| **Evidence** | Quote of REQ-3.3 as written in the brief + screenshot of the document search for "expire" and "attempts" with zero results. *(attach before submitting)* |
| **Traces to** | REQ-3.3 / TR-OTP-003, checklist items 3, 4, 5, 6, 10 |
| **Reported by / date** | _[your name]_ · 9 October 2026 |

---

## DEF-002 · REQ-3.2 Daily limit: day boundary and which transfers count are not defined

| Field | Content |
|---|---|
| **Title** | REQ-3.2 Daily limit: day boundary and which transfers count are not defined |
| **Environment** | Document review. Source: requirements REQ-3.1 – REQ-3.4 for the money transfer feature (week 3 transfer brief); test plan v1.0 and test cases `TR-DAY-*` in this pack. No app build involved. |
| **Preconditions** | You have the requirements document open at REQ-3.2 ("Daily limit 1,000,000 KZT"). |
| **Steps to reproduce** | 1. Open the requirements document and find REQ-3.2.<br>2. Read the full text of REQ-3.2 and any note attached to it.<br>3. Look for the time at which the daily total returns to 0 and the time zone it uses.<br>4. Look for whether a transfer that was rejected, cancelled at the SMS screen, or failed counts towards the daily total.<br>5. Look for whether the total is updated when the customer taps Confirm or when the code is accepted. |
| **Expected result** | REQ-3.2 states the reset moment (for example, midnight in a named time zone, or a rolling 24 hours) and states that only completed transfers count, or says exactly which others do. |
| **Actual result** | REQ-3.2 states only the limit value (1,000,000 KZT per day). Steps 3, 4 and 5 find no rule. A customer transferring at 23:59 and again at 00:01 cannot be told, from the requirements, whether the second transfer is allowed. |
| **Reproducibility** | Deterministic — a document gap, visible on every reading (confirm by re-reading once yourself before submitting) |
| **Severity / priority** | Severity **High** (proposed) — a wrong limit rule lets customers exceed the limit or blocks valid transfers. Priority **Medium** (proposed) — `TR-DAY-001` and `TR-DAY-002` can run now because they do not depend on the missing rules; the day-boundary case cannot. |
| **Evidence** | Quote of REQ-3.2 as written in the brief + marked-up copy showing the three missing rules. *(attach before submitting)* |
| **Traces to** | REQ-3.2 / TR-DAY-001, TR-DAY-002; traceability matrix items REQ-3.2a and REQ-3.2b |
| **Reported by / date** | _[your name]_ · 9 October 2026 |

---

## DEF-003 · REQ-3.3 / REQ-3.4: order of SMS code and balance check is not defined for amounts above 100,000 KZT with insufficient balance

| Field | Content |
|---|---|
| **Title** | REQ-3.3 / REQ-3.4: order of SMS code and balance check is not defined for amounts above 100,000 KZT with insufficient balance |
| **Environment** | Document review. Source: requirements REQ-3.1 – REQ-3.4 for the money transfer feature (week 3 transfer brief); test plan v1.0 and test cases `TR-BAL-*` in this pack. No app build involved. |
| **Preconditions** | You have the requirements document open at REQ-3.3 and REQ-3.4. Take the case: customer TC-021, balance 50,000 KZT, transfer amount 100,001 KZT (both rules apply: code required, balance insufficient). |
| **Steps to reproduce** | 1. Open REQ-3.3 (code required above 100,000 KZT).<br>2. Open REQ-3.4 (transfer rejected when balance is insufficient).<br>3. Apply both to the case above.<br>4. Look in the requirements for what the customer sees first: the code screen, or the insufficient-balance error.<br>5. Look for the exact error message text for REQ-3.1, REQ-3.2 and REQ-3.4. |
| **Expected result** | The requirements state which check runs first (a balance check before the SMS code is the safer order, so no code is sent for a transfer that cannot happen) and give the message text for each rejection. |
| **Actual result** | Neither requirement states the order, and no message text is given for any rejection. Two testers could write opposite expected results for the case above and both be consistent with the requirements. The week 3 decision table also did not include insufficient balance as a condition. |
| **Reproducibility** | Deterministic — a document gap, visible on every reading (confirm by re-reading once yourself before submitting) |
| **Severity / priority** | Severity **Medium** (proposed) — affects customer experience and cost of SMS, not money. Priority **Medium** (proposed) — needed before the combined case can be added to the test set. |
| **Evidence** | Quote of REQ-3.3 and REQ-3.4 side by side with the example case worked through. *(attach before submitting)* |
| **Traces to** | REQ-3.3, REQ-3.4 / TR-BAL-002; traceability matrix item "REQ-3.3 × REQ-3.4" |
| **Reported by / date** | _[your name]_ · 9 October 2026 |
