# P2P

## Introduction

The Person-to-Person (P2P) feature facilitates direct fund transfers between clients within the DDP ecosystem. The Direct Disbursement merchants have access to this convenient functionality. Enabling Person-to-Person (P2P) payments offers advantages such as convenience, speed, cost-effectiveness, accessibility, enhanced user experience, and increased efficiency in fund transfers.

## Use Case Example(s)

Customer would like to make a payment to a recipient using the P2P method. The customer is required to send the sender details along with the request API payload. The DDP will process the request and route it to the relevant payment channels, such as Visa/Mastercard, to utilize the sender information and disburse the funds accordingly to the recipient.

Initially, the P2P payment process supports transactions in USD only. However, the DDP aims to expand its services to support cross-border transactions with non-US issuers in the future.

## API 
An update has been made to the Payment API to incorporate this feature. Please review the API specifications provided below.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)