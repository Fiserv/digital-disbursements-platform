# Bank Account Verification

## Introduction

The Direct Disbursement Platform (DDP) includes a feature that validates bank accounts before processing any ACH payouts. It conducts Early Warning System (EWS) validation, which reduces return occurrences and ensures compliance with NACHA guidelines. This functionality is accessible to both Portal and Direct Disbursement merchants.

## Prerequisites

- The merchant should select payment method as ACH.
- Merchant ID (MID) level configuration should be done to enable account validation.

## Use Case Example(s)

**Validating ACH Transactions:**

A customer initiates an ACH transaction. The Direct Disbursement Platform (DDP) sends a request containing the account number, routing number, payee first name, and payee last name to the Early Warning System (EWS), which validates the transaction details.

Merchant ID (MID) Level Settings determine whether an MID participates in this feature (default: turned off) and specify whether a validation check will be performed on credit ACH payouts only, debit ACH payouts only, or all ACH payouts.

1. ***For credit payouts:*** Perform checks for Ownership, Combined, and Status at the MID level (merchant's choice).

2. ***For debit payouts:*** Perform checks for Status only.

Early Warning System (EWS) will provide a Pass, Fail or Inconclusive response.

1. ***Pass Response:*** Allow the payout to proceed.

2. ***Fail Response:*** Prevent payout to the account and prompt the payee to choose an alternative method.

3. ***Inconclusive Response:***

   - ***For credit payouts:*** Determine if "Inconclusive" should be treated like a 'Fail' or 'Pass' response (default: 'Pass').
   - ***For debit payouts:*** Treat "Inconclusive" as a 'Pass' response.
