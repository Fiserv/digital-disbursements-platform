#  SANCTION HOLD REVIEW

## Introduction
The Sanctions Hold is applicable to all international transactions, regardless of the payment method used. It introduces new screening rules for cross-border payments designed specifically for non-US merchants.

All cross-border transactions will undergo OFAC (Office of Foreign Assets Control) screening. DDP will send the recipient's information to the OFAC system. Based on their rules, OFAC will respond back with a result (pass/fail). If any transaction fails in OFAC, it will be held for further review (Sanction Hold Review). After investigating the failed transaction, the OFAC team will respond with one of three outcomes (NOT_ENOUGH_INFO, MATCH, NO_MATCH).

## Use Case Example(s)
- The merchant should opt for debit cross-border disbursement.
- Applicable for API and portal merchants.
- Disburse the payment using a debit cross-border card.
    - In case the OFAC response is NOT_ENOUGH_INFO, the payment will be canceled.
    - If the OFAC response is MATCH, the payment will be placed on hold.
    - When the OFAC response is NO_MATCH, the payment will proceed for disbursement.