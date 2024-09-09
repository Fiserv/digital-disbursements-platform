# Cross Currency

## Introduction

A cross-currency payment involves the entire transaction chain where an account is debited in one currency and credited in another. Initially, this will apply to Visa and MasterCard, with potential future expansion to other payment methods. DDP will enable merchants to choose the currency for transaction submission, regardless of their DFA currency. This solution includes:

- Allowing merchants with DFA in USD to present transactions in other currencies.
- Offering customers, the option to be paid in DFA Amount or Destination Amount based on the destination card billing currency, with DDP handling the FX conversion and adding a margin for revenue.

## Prerequisites

- Merchants should enable Cross Border payments.
- Applicable for API and portal merchants.
- The allowed currencies list should be added to the CURRENCY_CONFIG column of the Bank table, as bank-level currencies are configured. This configuration is necessary at the bank level to specify multiple allowed currencies.
- The cross-currency type should be either Merchant Initiated Cross Currency (MICC) or Recipient Initiated Cross Currency (RICC). DDP is currently supporting only MICC.

## Use Case Example(s)

MICC-Cross currency transaction:

- The merchant initiates a payment with a currency different from the DFA currency. The recipient receives a notification with the submitted currency and amount.
- The recipient accesses the payout via the portal. The recipient sees the payment in the submitted currency and amount.
- The recipient selects a payment method from the available list. The system provides payment methods based on the MICC configuration (currently, MICC merchant users can use VISA/MASTERCARD).
- The recipient submits the payment request. The system verifies that the transaction currency is not the DFA currency. The system makes the balance adjustment with the DFA currency and amount in the Funds activity.
- The recipient receives a notification with the payment details. If enabled, the merchant receives a webhook with the payment details.

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
