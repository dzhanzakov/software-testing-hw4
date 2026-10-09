# SMS Code — Release Checklist

**Feature:** Money transfer above 100,000 KZT (REQ-3.3)
**Run before every release.** Build: ____________  Tester: ____________  Date: ____________

A checklist says what to verify, not how. Use test customer TC-014 (balance 1,000,000 KZT, daily total 0), recipient deposit ending 4417, on test environment 2.

| # | Check | Traces to |
|---|---|---|
| 1 | [ ] No code is requested for 100,000 KZT; a code is requested for 100,001 KZT | REQ-3.3 · TR-OTP-001, 002 |
| 2 | [ ] Code arrives on the registered phone within 30 seconds | REQ-3.3 |
| 3 | [ ] Code expires after 120 seconds | REQ-3.3a (assumed value) |
| 4 | [ ] Code entered after expiry is rejected and no money moves | REQ-3.3a |
| 5 | [ ] Two wrong codes followed by the correct code completes the transfer | REQ-3.3 |
| 6 | [ ] Third wrong code cancels the transfer; the correct code afterwards is not accepted | REQ-3.3 · TR-OTP-003 |
| 7 | [ ] Confirm tapped twice debits once | REQ-3.3 · TR-OTP-004 |
| 8 | [ ] Balance and statement are unchanged until the code is confirmed | REQ-3.3 |
| 9 | [ ] Cancelling on the code screen leaves balance and daily total unchanged | REQ-3.3 |
| 10 | [ ] A code from a previous transfer is rejected for a new transfer | REQ-3.3 |
| 11 | [ ] SMS text is correct in KZ, RU and EN | REQ-3.3 |
| 12 | [ ] Daily total updates after confirmation | REQ-3.2 · TR-OTP-002 |

Twelve items. Every one traces back to a boundary, a state or a transition in the test design.
