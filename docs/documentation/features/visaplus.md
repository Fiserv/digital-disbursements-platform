# Visa Plus

## Introduction

The Visa Plus feature enables clients to activate Visa+ as a payout method on the Digital Disbursement Platform (DDP) for Direct Disbursement merchants. Visa Plus is a new payment method that supports the implementation of the Visa+ Person-to-Person (P2P) use case. The Visa+ Service introduces a new consumer-defined payment-specific "username" (referred to as "payname" in Visa+ Service product documentation) that can be used to identify the payment recipient instead of using sensitive personal data such as a Primary Account Number (PAN), email address, or phone number. This eliminates the need for the payment recipient to exchange any sensitive personal data when receiving a payment from a third party.

## Prerequisites

- The recipient has to create a Payname on the participating Visa wallet.
- The merchant should opt for Visa Plus disbursement.
- Applicable for API and portal merchants.

<!-- theme: success -->
> ### Visa+ Payname
>
>A Payname should start with a "+", followed by  to 50 characters. then a dot ".", and the wallet name.
>
>**Example:** +testsbx002.gpay

## Use Case Example(s)

This use case demonstrates the flow of payment initiation, verification, funds transfer, and settlement within the Visa+ payment ecosystem, ensuring a secure and efficient transaction experience for both merchants and recipients. With Visa+, we can receive money but cannot make payments.

![VisaPlus flow image](../../assets/images/VisaPlus.png "VisaPlus flow image")

## Error Code

|Error code|Scenario|Error message|
|---------------|--------------------------|--------------------|
| 400136 |  invalid payname  | Visa Card Verification Failed - Decline |

## API

An update has been made to the Payment API to incorporate this feature. Please review the API specifications provided below.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)
