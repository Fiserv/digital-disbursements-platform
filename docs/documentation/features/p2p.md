# Person-to-Person (P2P)

## Introduction

The Person-to-Person (P2P) feature facilitates direct fund transfers between merchants and recipients. Direct Disbursement merchants have access to this functionality. Enabling Person-to-Person (P2P) payments offers advantages such as convenience, speed, cost-effectiveness, accessibility, enhanced user experience, and increased efficiency in fund transfers.

## Prerequisites

- Merchants should enable P2P payments.
- Applicable only for Direct Debit merchants.
- P2P requires sender details in the request payload.
- Allowed sources of funds for the sender’s account are TA Token, Bank Account, and Card. If using a TA Token, Generate a TA Token for the card (Visa or Mastercard) for both the sender and recipient using North Backend or Commerce Hub. For more Information, refer to [TA Token](../docs/?path=docs/troubleshooting/tokens.md)

## Use Case Example(s)

The customer would like to make a payment to a recipient using the P2P method. The There  is required to send the sender's details along with the request API payload. The DDP will process the request and route it to the relevant payment channels, such as Visa/Mastercard, to utilize the sender's information and disburse the funds accordingly to the recipient. Senders and merchants should be US domestic, while recipients can be either US domestic or cross-border.

The following required fields must be included in the sender's request.

| Fields|
|---------------------|
| firstName|
| lastName|
| dateOfBirth|
| nationality|
| birthCountry|
| occupation|
| emailAddress|
| phoneNumber|
| address|
| fundingAccount|
| transactionIdentifier|
| transactionPurpose|

Learn more in the API Explorer:

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)

## Error Code

| Error code |  Scenario| Error message|
|------------|------------------------------------|--------------------|
| 400136|  Wrong card or card not supported   | BIN Validation Failed  |
| 400028|  If you provide the wrong payload for the sender   | Invalid request format/data |

## API

An update has been made to the Payment API to incorporate this feature. Please review the API specifications provided below.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)
