#  SANCTION HOLD REVIEW

## Introduction
The Sanctions Hold applies to any cross-border transaction, regardless of the payment method. Sanctions Hold screening introduces new rules for cross-border payments that are specifically intended for non-US merchants.

## Use Case Example(s)

- The merchant must be enabled for cross-border payments.
- Create a recipient and initiate a payment that includes recipient details found on the sanctions list.
- Disburse the payment using a DMATCH, then use a border card.
    - If the Sanctions list matches 'NOT_ENOUGH_INFO', the payment will be cancelled.
    - If the Sanctions list matches 'MATCH', the payment will proceed to disbursement.
    - If the Sanctions list matches 'NO_MATCH', the payment will proceed to disbursement

## API Changes
NA
<!-- Parameter Details Try Out - link to new request -->

## Troubleshoooting
NA
<!--Error Codes, FAQs / Common Issues -->