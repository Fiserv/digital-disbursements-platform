# Data Commerce

## Introduction

Data Commerce provides the functionality to execute account validation prior to the execution of any ACH pay-outs. By integrating this with DDP, can perform EWS (Early Warning System) validation, reducing the number of returns and ensure compliance with NACHA guidelines. This functionality is available for both Portal and Direct Disbursement merchants.

## Use Case Example(s)

Validating ACH Transactions:
- A customer initiates an ACH transaction. DDP sends a request to Data Commerce (EWS) with transaction details.
- Data Commerce (EWS) validates the transaction and responds with an approval or denial status.
- If approved, DDP proceeds with further validation and disbursement.
- If denied, the transaction is canceled.
- If a transaction receives an "Inconclusive" status from Data Commerce (EWS), DDP checks the merchant configuration:
  * If configured as 'Pass', the transaction proceeds.
  * If configured as 'Fail', the transaction is declined.
