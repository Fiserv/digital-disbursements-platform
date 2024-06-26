# Data Commerce Bank Account Verification

## Introduction

Data Commerce offers the capability to perform account validation before processing any ACH payouts. By incorporating this feature into the DDP, it can conduct EWS (Early Warning System) validation, thereby decreasing the occurrence of returns and ensuring adherence to NACHA guidelines. This functionality is available to both Portal and Direct Disbursement merchants.

## Use Case Example(s)

Validating ACH Transactions:
- A customer initiates an ACH transaction. DDP sends a request to Data Commerce (EWS) containing transaction details.
- Data Commerce (EWS) validates the transaction and responds with either an approval or denial status.
- Upon approval, DDP proceeds with additional validation and disbursement.
- In case of denial, the transaction is cancelled.
- If a transaction receives an "Inconclusive" status from Data Commerce (EWS), DDP reviews the merchant configuration:
  * For 'Pass' configuration, the transaction proceeds.
  * For 'Fail' configuration, the transaction is declined.
