# P2P

## Introduction

The Person-to-Person (P2P) feature facilitates direct fund transfers between merchants and recipients. Direct Disbursement merchants have access to this convenient functionality. Enabling Person-to-Person (P2P) payments offers advantages such as convenience, speed, cost-effectiveness, accessibility, an enhanced user experience, and increased efficiency in fund transfers.

## Prerequisites 

-Merchants should enable P2P payments.
-Applicable only for API merchants.
-P2P requires sender details in the request payload.
-Generate a TA Token for the card (Visa or Mastercard) for both the sender and recipient using North Backend or Commerce Hub.

## Use Case Example(s)

The customer would like to make a payment to a recipient using the P2P method. The customer is required to send the sender's details along with the request API payload. The DDP will process the request and route it to the relevant payment channels, such as Visa/Mastercard, to utilize the sender's information and disburse the funds accordingly to the recipient.

The following required fields must be included in the sender's request.

| Description 		   	 | 
|---------------------	 |
| firstName				 | 
| lastName				 | 
| dateOfBirth			 | 
| nationality			 | 
| birthCountry			 | 
| occupation	 		 | 
| emailAddress 			 | 
| phoneNumber 			 | 
| address	 			 | 
| fundingAccount		 | 
| transactionIdentifier	 | 
| transactionPurpose	 | 

If you need to obtain the data type, please refer to the API explorer.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)

P2P transactions can be domestic or cross-border. However, the sender and merchant should be domestic, while the recipient can be either domestic or cross-border.

Example: A US sender can send money to other countries like Canada, India, etc.

## Error Code

| Error code |  Scenario 							| Error message		  	|
|------------|------------------------------------	|--------------------|
| 400136	 |  Wrong card or card not supported   | BIN Validation Failed  |
| 400028 	 |  If you provide the wrong payload for the sender   | Invalid request format/data |


## API 
An update has been made to the Payment API to incorporate this feature. Please review the API specifications provided below.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)