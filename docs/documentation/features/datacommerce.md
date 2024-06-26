# Data Commerce Bank Account Verification

## Introduction

Data Commerce enables account validation prior to processing any ACH payouts. By integrating this feature into the Direct Disbursement Platform (DDP), it can conduct EWS (Early Warning System) validation, reducing return occurrences and ensuring compliance with NACHA guidelines. This functionality is accessible to both Portal and Direct Disbursement merchants.

## Use Case Example(s)

Validating ACH Transactions:
- A customer initiates an ACH transaction. DDP sends a request to Data Commerce (EWS) containing transaction details.
- Data Commerce (EWS) validates the transaction and responds with either an approval or denial status.
- Upon approval, DDP proceeds with additional validation and disbursement.
- In case of denial, the transaction is cancelled.
- If a transaction receives an "Inconclusive" status from Data Commerce (EWS), DDP reviews the merchant configuration:
  * If configured as 'Pass', the transaction proceeds.
  * If configured as 'Fail', the transaction is declined.
