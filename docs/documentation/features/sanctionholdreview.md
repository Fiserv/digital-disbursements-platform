#  SANCTION HOLD REVIEW

## Introduction
The Sanctions Hold applies to any cross-border transaction, regardless of the payment method. Sanctions Hold screening introduces new rules for cross-border payments that are specifically intended for non-US merchants.

## Use Case Example(s)

- The merchant should have cross-border payment capability enabled.
- Create a recipient and initiate a payment, ensuring that recipient details are checked against the sanctions list.
- Disburse the payment using a Debit cross-border card.
  - If the sanctions list shows a match of 'NOT_ENOUGH_INFO', the payment will be cancelled.
  - If the sanctions list shows a match of 'MATCH', the payment will proceed to disbursement.
  - If the sanctions list shows a match of 'NO_MATCH', the payment will proceed to disbursement.