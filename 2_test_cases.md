# Test Cases — Money Transfer Between Own Accounts

Twelve test cases, each traced to a requirement, written so a colleague can run them without asking the author anything.

## Conventions used in every case

- **Environment:** test environment 2; mobile app on iOS 18.2 (record the app build number in the run log). Repeat the whole set on Android if time allows.
- **Starting state:** every case begins from a freshly reset customer (run the test-data reset script first). No case depends on another case having run.
- **Test customers:** **TC-014** — balance 1,000,000 KZT, daily total 0. **TC-021** — balance 50,000 KZT, daily total 0. Both have a registered test phone that receives SMS codes.
- **Recipient:** the customer's own deposit account ending **4417** in every case.
- **Where to read results:** balance on the Accounts screen; daily total on the Limits screen (Profile → Limits → "Used today"); statement on the account's Transactions list.
- **Common steps (S):**
  - **S1** Log in as the customer named in the preconditions.
  - **S2** Open Transfers → Between my accounts.
  - **S3** Select the deposit ending 4417 as recipient.
  - **S4** Enter the amount given in the test data.
  - **S5** Tap Continue.
- **Amounts up to and including 100,000 KZT** complete without an SMS code; if the app shows a confirmation summary, tap Confirm **once**.
- **Wording of error messages** is not defined in the requirements. Where a case says "an error is shown", record the exact text you see in the run log.

## Summary

| ID | Title | Traces to | Technique |
|---|---|---|---|
| TR-AMT-001 | Amount 99 is rejected (just below minimum) | REQ-3.1 | Boundary value |
| TR-AMT-002 | Amount 100 is accepted (minimum) | REQ-3.1 | Boundary value |
| TR-AMT-003 | Amount 500,000 is accepted with SMS code (maximum) | REQ-3.1, REQ-3.3 | Boundary value |
| TR-AMT-004 | Amount 500,001 is rejected (just above maximum) | REQ-3.1 | Boundary value |
| TR-DAY-001 | Transfer bringing daily total to exactly 1,000,000 is accepted | REQ-3.2 | Decision table |
| TR-DAY-002 | Transfer bringing daily total to 1,000,001 is rejected | REQ-3.2 | Decision table |
| TR-OTP-001 | Amount 100,000 needs no SMS code | REQ-3.3 | Boundary value |
| TR-OTP-002 | Amount 100,001 needs SMS code; correct code completes transfer | REQ-3.3 | State transition |
| TR-OTP-003 | Third wrong SMS code cancels the transfer | REQ-3.3 | State transition |
| TR-OTP-004 | Confirm tapped twice debits only once | REQ-3.3 | State transition / risk-based |
| TR-BAL-001 | Amount equal to balance is accepted | REQ-3.4 | Boundary value |
| TR-BAL-002 | Amount one above balance is rejected | REQ-3.4 | Boundary value |

---

## REQ-3.1 — Amount between 100 and 500,000 KZT

### TR-AMT-001 · Amount 99 is rejected (just below minimum)

**Traces to:** REQ-3.1

**Preconditions:** TC-014, balance 1,000,000 KZT, daily total 0.

**Test data:** amount = **99** KZT.

**Steps:**

1. S1 — log in as TC-014.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **99** as the amount.
5. Tap Continue.

**Expected result:** an error states that the amount is below the minimum (100 KZT); the app stays on the amount screen; no SMS code screen appears; balance still 1,000,000; daily total still 0; no statement entry.

### TR-AMT-002 · Amount 100 is accepted (minimum)

**Traces to:** REQ-3.1

**Preconditions:** TC-014, balance 1,000,000 KZT, daily total 0.

**Test data:** amount = **100** KZT.

**Steps:**

1. S1 — log in as TC-014.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **100** as the amount.
5. Tap Continue (and tap Confirm once if a summary screen appears).

**Expected result:** a "transfer completed" message appears; no SMS code screen appears; balance is **999,900**; daily total is **100**; the statement shows one entry of 100 KZT.

### TR-AMT-003 · Amount 500,000 is accepted with SMS code (maximum)

**Traces to:** REQ-3.1, REQ-3.3

**Preconditions:** TC-014, balance 1,000,000 KZT, daily total 0; the test phone is switched on and receives SMS.

**Test data:** amount = **500,000** KZT; the SMS code that arrives on the test phone.

**Steps:**

1. S1 — log in as TC-014.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **500,000** as the amount.
5. Tap Continue.
6. Read the SMS code on the test phone and enter it within 120 seconds.
7. Tap Confirm once.

**Expected result:** the SMS code screen appears within 3 seconds of step 5; after step 7 a "transfer completed" message appears; balance is **500,000**; daily total is **500,000**; the statement shows one entry of 500,000 KZT.

### TR-AMT-004 · Amount 500,001 is rejected (just above maximum)

**Traces to:** REQ-3.1

**Preconditions:** TC-014, balance 1,000,000 KZT, daily total 0.

**Test data:** amount = **500,001** KZT.

**Steps:**

1. S1 — log in as TC-014.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **500,001** as the amount.
5. Tap Continue.

**Expected result:** an error states that the amount is above the maximum (500,000 KZT); the app stays on the amount screen; no SMS code is sent; balance still 1,000,000; daily total still 0; no statement entry.

---

## REQ-3.2 — Daily limit 1,000,000 KZT

### TR-DAY-001 · Transfer bringing daily total to exactly 1,000,000 is accepted

**Traces to:** REQ-3.2

**Preconditions:** TC-014 reset to balance 1,000,000 KZT with daily total **999,900** (set by the reset script, not by earlier tests).

**Test data:** amount = **100** KZT (999,900 + 100 = 1,000,000, exactly the limit).

**Steps:**

1. S1 — log in as TC-014.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **100** as the amount.
5. Tap Continue (and tap Confirm once if a summary screen appears).

**Expected result:** a "transfer completed" message appears; balance is **999,900**; daily total is **1,000,000**; the statement shows one entry of 100 KZT.

### TR-DAY-002 · Transfer bringing daily total to 1,000,001 is rejected

**Traces to:** REQ-3.2

**Preconditions:** TC-014 reset to balance 1,000,000 KZT with daily total **999,900**.

**Test data:** amount = **101** KZT (999,900 + 101 = 1,000,001, one above the limit).

**Steps:**

1. S1 — log in as TC-014.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **101** as the amount.
5. Tap Continue.

**Expected result:** an error states that the daily limit would be exceeded; no transfer is made; balance still 1,000,000; daily total still 999,900; no statement entry.

---

## REQ-3.3 — SMS code required above 100,000 KZT

### TR-OTP-001 · Amount 100,000 needs no SMS code

**Traces to:** REQ-3.3

**Preconditions:** TC-014, balance 1,000,000 KZT, daily total 0.

**Test data:** amount = **100,000** KZT (the highest amount that must not ask for a code).

**Steps:**

1. S1 — log in as TC-014.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **100,000** as the amount.
5. Tap Continue (and tap Confirm once if a summary screen appears).

**Expected result:** no SMS code screen appears and no SMS arrives on the test phone; a "transfer completed" message appears; balance is **900,000**; daily total is **100,000**; the statement shows one entry of 100,000 KZT.

### TR-OTP-002 · Amount 100,001 needs SMS code; correct code completes the transfer

**Traces to:** REQ-3.3

**Preconditions:** TC-014, balance 1,000,000 KZT, daily total 0; the test phone is switched on and receives SMS.

**Test data:** amount = **100,001** KZT; the SMS code that arrives on the test phone.

**Steps:**

1. S1 — log in as TC-014.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **100,001** as the amount.
5. Tap Continue.
6. Enter the SMS code from the test phone within 120 seconds.
7. Tap Confirm once.

**Expected result:** after step 5 the SMS code screen appears within 3 seconds and an SMS arrives; after step 7 a "transfer completed" message appears; balance is **899,999**; daily total is **100,001**; the statement shows one entry of 100,001 KZT.

### TR-OTP-003 · Third wrong SMS code cancels the transfer

**Traces to:** REQ-3.3

**Preconditions:** TC-014, balance 1,000,000 KZT, daily total 0; the test phone is switched on and receives SMS.

**Test data:** amount = **100,001** KZT; wrong code = the real code from the SMS with its last digit changed (use the same wrong code three times).

**Steps:**

1. S1 — log in as TC-014.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **100,001** as the amount.
5. Tap Continue and read the SMS code on the test phone (do not enter it yet).
6. Enter the wrong code and tap Confirm (attempt 1).
7. Enter the wrong code and tap Confirm (attempt 2).
8. Enter the wrong code and tap Confirm (attempt 3).
9. Now enter the **correct** code from the SMS and tap Confirm.

**Expected result:** attempts 1 and 2 show a "wrong code" error and keep the code screen open; after attempt 3 the transfer is cancelled with a message and the app leaves the code screen; at step 9 the correct code is **not** accepted and no transfer is made; balance still 1,000,000; daily total still 0; no statement entry.

### TR-OTP-004 · Confirm tapped twice debits only once

**Traces to:** REQ-3.3 (risk 1 in the test plan — double debit)

**Preconditions:** TC-014, balance 1,000,000 KZT, daily total 0; the test phone is switched on and receives SMS.

**Test data:** amount = **100,001** KZT; the SMS code that arrives on the test phone.

**Steps:**

1. S1 — log in as TC-014.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **100,001** as the amount.
5. Tap Continue.
6. Enter the correct SMS code within 120 seconds.
7. Tap Confirm **twice in quick succession** (two taps within one second).
8. Wait 10 seconds, then open the Accounts screen and the statement.

**Expected result:** exactly one transfer is made; balance is **899,999** (not 799,998); daily total is **100,001**; the statement shows **one** entry of 100,001 KZT.

---

## REQ-3.4 — Transfer rejected when balance is insufficient

### TR-BAL-001 · Amount equal to balance is accepted

**Traces to:** REQ-3.4

**Preconditions:** TC-021, balance **50,000** KZT, daily total 0.

**Test data:** amount = **50,000** KZT (equal to the whole balance; below the SMS threshold).

**Steps:**

1. S1 — log in as TC-021.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **50,000** as the amount.
5. Tap Continue (and tap Confirm once if a summary screen appears).

**Expected result:** a "transfer completed" message appears; no SMS code screen appears; balance is **0**; daily total is **50,000**; the statement shows one entry of 50,000 KZT.

*Assumption:* no minimum balance is required on the account (none is stated in the requirements).

### TR-BAL-002 · Amount one above balance is rejected

**Traces to:** REQ-3.4

**Preconditions:** TC-021, balance **50,000** KZT, daily total 0.

**Test data:** amount = **50,001** KZT.

**Steps:**

1. S1 — log in as TC-021.
2. S2 — open Transfers → Between my accounts.
3. S3 — select the deposit ending 4417.
4. Enter **50,001** as the amount.
5. Tap Continue.

**Expected result:** an error states that the balance is insufficient; no SMS code is requested; balance still 50,000; daily total still 0; no statement entry.

---

## Not covered by these twelve cases (by design)

- Amounts that break two rules at once, for example 100,001 KZT with a 50,000 KZT balance (which check comes first is not defined — see DEF-003).
- Expiry of the SMS code after 120 seconds and resending a code — covered by the release checklist (`4_sms_release_checklist.md`), not by a test case.
- Daily total reset at the day boundary — rule not defined (see DEF-002).
