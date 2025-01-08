# Cross Currency

## Introduction

A cross-currency payment involves the entire transaction chain where an account is debited in one currency and credited in another. Initially, this will apply to Visa and MasterCard, with potential future expansion to other payment methods. Direct Disbursement Platform (DDP) will enable merchants to choose the currency for transaction submission, regardless of their Disbursement Funding Account (DFA) currency. This solution includes:

- Allowing merchants with DFA in USD to present transactions in other currencies.
- Offering customers, the option to be paid in DFA Amount or Destination Amount based on the destination card billing currency, with DDP handling the Foreign eXchange (FX) conversion and adding a margin for revenue.

## Prerequisites

- Merchants should enable Cross Border payments.
- Applicable for API and portal merchants.
- The allowed currencies should be configured at the bank level, if not already done.
- The cross-currency type should be either Merchant Initiated Cross Currency (MICC) or Recipient Initiated Cross Currency (RICC). DDP is currently supporting only MICC.

## Use Case Example(s)

MICC-Cross currency transaction:

- When the merchant sends a payment initiation request in a currency other than the DFA currency, the system checks if the configuration to accept such requests is enabled. If enabled, the request is accepted; otherwise, it is declined.
- If the merchant is enabled for MICC but the provided currency is not within the bank’s allowed currencies, the request is declined.
- The system checks if the fxEnquiryId is present in the request. If it is, the system retrieves rate details from FX_RATES and checks the expiry time. If the rate has expired, the request is declined.
- If the fxEnquiryId is not provided by the merchant, the system calls the OpenFX adapter to get the rate details and proceeds.
- The system performs a BIN check for the provided recipient card to determine the card brand/provider. It then retrieves the rate details for the provider from OpenFX.
- The system requests the conversion amount from OpenFX. If the amount is not convertible, the request is declined. If successful, the OpenFX response details are captured in the table FX_RATES.
- The system verifies funds availability with the converted amount. If funds are available, the request proceeds; otherwise, it is declined. Posting to the DFA will always occur in the DFA currency.
- The payout amount captured in the recipient notifications will be the amount and currency submitted by the merchant.
- The system creates the Transaction and Payment Transaction records and sends the response, including the FX rate details.
- The recipient notification is sent in the submitted currency.

## Error Code

| Error code |  Scenario| Error message|
|------------|------------------------------------|--------------------|
| 500401 | When we are unable to get response from downstream API | Timeout occurred while retrieving response from openFx API.|
| 400405 | When RateType is not configured for the mid | Rate type not configured |
| 400407| When cross currency is not enabled for the mid   | Cross currency not enabled  |
| 400408| When cross currency type i.e, MICC or RICC is not set for the given mid   | Cross currency type not set |
| 400409| When fromCurrencyCode and toCurrencyCode is same  | FromCurrencyCode and toCurrencyCode are same. Please try with different card |
| 400410| When Transaction and Card currencies are not same   | Card Currency is different from Transaction Currency |

## API

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)
