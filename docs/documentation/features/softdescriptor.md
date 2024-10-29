# Soft Descriptor

## Introduction

The Direct Disbursement Platform (DDP) now allows merchants to store a soft descriptor, which will be displayed as their preferred name on the recipient’s bank or card statement. This descriptor is limited to a maximum of 22 characters and can include alphanumeric characters, spaces, and some special characters (&, *, ', , -, ., _, ~). Currently, DDP supports the statement descriptor only for VISA/MASTERCARD. This feature is available for B2B/B2C and P2P/A2A payments, enhancing transparency and recognition for merchants.

## Prerequisites

- Applicable only for Direct Debit merchants.

## Use Case Example(s)

B2B/B2C payments:

- The merchant initiates a payment request including a statementDescriptor. The DDP sends this as statement_descriptor to MasterCard and cardAcceptor.name to VISA.
- If the statementDescriptor is not included in the initial payment request, the system retrieves the merchant’s configured STATEMENT_DESCRIPTOR and sends it to VISA and MasterCard if configured.
- If neither the statementDescriptor is present in the payment request nor the STATEMENT_DESCRIPTOR is configured for the merchant, the system defaults to sending the Doing Business As (DBA) name to VISA and MasterCard.

P2P/A2A payments:

- The system retrieves the merchant’s configured Abbreviation and sends it to VISA and MasterCard if configured.
- If the Abbreviation is not configured, the system defaults to using the Merchant DBA as the abbreviation.
- The P2P/A2A statement descriptor will follow the pattern: Abbreviation*Sender First Name Sender Last Name.

## API

An update has been made to the Payment API to incorporate this feature. Please review the API specifications provided below.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)
