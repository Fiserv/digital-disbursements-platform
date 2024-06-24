# Data Commerce

## Introduction

Data Commerce provides the functionality to execute account validation prior to the execution of any ACH pay-outs. By integrating this with DDP, can perform EWS (Early Warning System) validation, reducing the number of returns and ensure compliance with NACHA guidelines. This functionality is available for both Portal and Direct Disbursement merchants.

## Use Case Example(s)

Validating ACH Transactions:
    &emsp;A customer initiates an ACH transaction. DDP sends a request to Data Commerce (EWS) with transaction details.
    &emsp;Data Commerce (EWS) validates the transaction and responds with an approval or denial status.
    &emsp;If approved, DDP proceeds with further validation and disbursement.
    &emsp;If denied, the transaction is canceled.
    &emsp;If a transaction receives an "Inconclusive" status from Data Commerce (EWS), DDP checks the merchant configuration:
        &emsp;&emsp;- If configured as 'Pass', the transaction proceeds.
        &emsp;&emsp;- If configured as 'Fail', the transaction is declined.
