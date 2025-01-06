# Person-to-Person (P2P)

## Introduction

Peer-to-Peer (P2P) payments refer to transactions where a company facilitates payments between two individuals—a sender and a receiver. This feature allows developers to utilize a single API to implement direct fund transfers between to recipients. By enabling P2P payments, developers can offer benefits such as convenience, speed, cost-effectiveness, accessibility, an enhanced user experience, and increased efficiency in fund transfers.

## Prerequisites

- Merchants should enable P2P payments.
- Applicable only for Direct Debit merchants.
- P2P requires sender details in the request payload.
- Allowed sources of funds for the sender’s account are TA Token, Bank Account, Card and Wallet.
  - If using a TA Token, Generate a TA Token for the card (Visa or Mastercard) for both the sender and recipient using North Backend or Commerce Hub. For more Information, refer to [TA Token](../docs/?path=docs/helpful-links/tokens.md)
  - If using a Card or Bank Account, it should be encrypted using public key. For more Information, refer to [Public Token](../docs/?path=docs/interactive-guide/apiflow.md#step-2a-create-a-public-token)

## Use Case Example(s)

The customer would like to make a payment to a recipient using the P2P method. The customer is required to send the sender's details along with the request API payload. The DDP will process the request and route it to the relevant payment channels, such as Visa/Mastercard, to utilize the sender's information and disburse the funds accordingly to the recipient. Senders and merchants should be US domestic, while recipients can be either US domestic or cross-border.

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
