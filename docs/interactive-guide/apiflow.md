
# API Flow

## Pre-requisites

### Connectivity

The DDP services are accessed through the public Internet. DDP accepts communication only via the HTTPS channel.  Custom HTTP headers are also used carry additional information in each request.

| Environment       | Host                           | Base Path |
| ----------------  | ------------------------------ | --------- |
| Integration Test  | https://int.api.firstdata.com  | /ddp      |
| Pre-Production    | https://cat.api.firstdata.com  | /ddp      |
| Production        | https://prod.api.firstdata.com | /ddp      |

### Header Description

The header of Each API call will contain several parameters. It is important that each parameter contain the specified values to have a successful API Call. Any changes to the values will be noted through out the guide.

#### HTTP Headers

| Header Name | Required | Description |
| --- | --- | --- |
| Api-Key | Yes | Merchant API key |
| Timestamp | Yes | Request initiation UTC timestamp, formatted as Epoch time. The value is in milliseconds. Sample value format is 1499961987232 |
| Authorization | Yes | Authorization header is required to have the "HMAC" string capitalized and followed by one space followed by the calculated hmac signature. For request generated through Server: Authorization: - HMAC <signature>.   "HMAC " followed by the Encrypted payload as signature, shown below  |
| Client-Request-Id | Yes | Contains a unique ID generated by the client that is used for enforcing idempotence on POST actions. |
| Content-Type | Yes | application/json |
| access\_token | Conditionally | Required when vaulting payment information. Utilizes the tokenId returned from the /tokens api. |

#### Sample Header

```JSON
"Content-Type":"application/json"
"Client-Request-Id":"4da61bfb-6852-4f2a-9462-733276f633d7"
"Api-Key": "YMgw8VSrYMG6WTIUnoUUGv7hF9Aqh3EO"
"Authorization" : "HMAC W5X9NAlPgSNsfQX55fXbXrk3arzL6KxcCTA6SrnxL+U="
"Timestamp": "1607368688646"
```

#### How to generate HMAC Signature

##### Description

HMAC signature is used in all calls made to our API and is necessary to receive a successful response from the system.

##### High Level Flow

- Get and save the current time
- Identify and save the request method
- Take the environment key and append a colon along with the current time
- Save this as the current raw signature (key:time)
- Get and save the payload from the request
- If the request method is not POST, move directly to step 7.
- Encrypt the request payload using SHA256 and save it
- Take this encrypted payload, make it a string and then encrypt it using Base64.
- Take the raw signature from step 4, append a colon and the Base64 encrypted payload to it and save it.
- Take the raw signature and HMAC encrypt it using SHA256 against the environment secret.
- Finally take this encrypted raw signature, make it a string and then encrypt it using Base64 to produce the signature for the header.

```javaScript

var key = postman.getEnvironmentVariable('clientKey');
var secret = postman.getEnvironmentVariable('clientSecret');
var time = new Date().getTime();
var method = request.method;
var rawSignature = key + ":" + time;
var requestBody = request.data;

if (method != 'GET' && method != 'DELETE') {
    var payload_digest = CryptoJS.SHA256(requestBody);
    var b64BodyContent = CryptoJS.enc.Base64.stringify(payload_digest);
    rawSignature = rawSignature + ":" + b64BodyContent;
}

var signature = CryptoJS.HmacSHA256(rawSignature, secret);
postman.setEnvironmentVariable('time', time);
postman.setEnvironmentVariable('signature', CryptoJS.enc.Base64.stringify(signature));

```

## Step 1: Create a Recipient

**Description**

Recipient is a end user who can gets the disbursement money in their accounts provided.

We have two types of recipients.

1. Consumer

2. Business

This request will create a recipient for card vaulting or payment purposes. The recipient can be either business or consumer depends on the merchant use case.
Create recipient is always the first step that needs to be taken to for any transactions or calls on the system. In this step we will register the recipient in our system with the unique identifier (merchantCustomerId ) i.e. the merchant unique random number for the customer.

<!-- theme: success -->
> **Special Considerations**
>| Parameter               | Note                                                                                                                                                                                   |
>| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
>| **merchantCustomerId**  | field should be **your** (*ie the merchant’s*) unique identifier for this customer and should be generated by you.                                                                       |
>| **recipientId**         | Generated at this step and changes based upon the **email address** of the customer. The same payment method cannot be saved to the same `recipientId`                                  |
>| **recipientIdentifier** | Accepted Values are: *`[Last_4_of_SSN, SPECIAL_CODE, ACCOUNT_NUMBER, DATE_OF_BIRTH, PHONE_NUMBER]`*. This is used in conjunction with other information to uniquely identify the consumer. |

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/recipients)

### Optional/Info: Update Recipient Info

**Description**

This request will update the previously created recipient with the necessary information. This recipient is identified by the passed in `merchantCustomerId` in the url. Updates the specified recipient by setting the values of the parameters passed. Any parameters not provided will be left unchanged.
The minimal request can be any single entity that requires updating, any of the objects or entities in the parameters of the create recipient schema will work here.

[![Try it out](../../../../assets/images/button.png)](../api/?type=patch&path=/ddp/v1/recipients/{id})

### Optional/Info: Get Recipient Info

**Description**

This request will retrieve the information for a recipient based upon its valid `merchantCustomerId` passed in the URL. With this retrieve call you can get the detailed information of the previously created recipient. 

[![Try it out](../../../../assets/images/button.png)](../api/?type=get&path=/ddp/v1/recipients/{id})

## Step 2: Save a Payment Method

<!-- theme: success -->
> **Note**
>
>PayPal, Venmo, Echeck and TA Token payment methods Account vault is not required, can do the disbursements directly.

**Description**

There are following steps require to save (vault) a payment methond for the recipient.

 1. Create a Public Token
 2. Create the Account Nonce Token (API or Hosted)
 3. Vault the paymment method

### Step 2a: Create a Public Token

**Description**

This request will generate a public key to be used in encrypting PCI data as well as a `tokenId` to be passed to subsequent calls through the header. This `tokenId` and the `publicKey` is valid for 20 minutes and after than its expired, you will use the public Key to perform encryption  on the account details as follow:

**Note:** please replace /ddp with /ucom in the context path.

| Account Type | Items to Encrypt             |
| ------------ | ---------------------------- |
| Debit        | Card Number, Month, and Date |
| ACH          | Account Number                |
| Coinbase     | Account Number                |
| MoneyNetwork | Card Number                  |

<!-- theme: success -->
> **Special considerations**
>| Parameter               | Note                                                                                                                                                                                   |
>| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | 
>| **fdCustomerId**       | How the customer is identified in the ucom app. Best practice would be to use the same value as `merchantCustomerId`.                                                                 |

<!-- TODO: Needs correct link to Get Encryption Key API -->
[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/recipients)

### Step 2b: Create the Account Nonce Token

<!--
type: tab
titles: API, Hosted 
-->

**Description**

In this request you are going to encrypt your PCI (Payment Card Industry) data with the help of `publicKey` and `tokenId` generated in "Create a Public Token" API call in previous step. Once that is done you will pass that data into this call and receive a nonce token to be used in the upcoming step for account vaulting. Keep in mind the encrypted payload for each variable should be surrounded by `ENC[ ]` as shown in the examples.

**<ins> Note: please replace /ddp with /ucom in the context path

<!-- theme: failure -->
> **<ins> Header Change
>| Header Key | Change |
>| ---------- | ------ |
>| **Authorization** |  changes to `Bearer <tokenId>` |
>
><!-- theme: success-->
>> *Example `Autorization: Bearer AGG596cV67WF8DjYLE3kS6nSu36x`*

[![See Examples](../../../../assets/images/button.png)](/product/UniversalCommerce/api/?type=post&path=/v1/account-tokens&branch=develop&version=1.0.0)

[UCOM Account Service](/product/UniversalCommerce/api/?type=post&path=/v1/account-tokens&branch=develop&version=1.0.0)

<!--
type: tab
-->

**Description**

HPP is a web SDK and iFrame solution. DDP is intended to provide merchants with an option to capture the cardholder data in secure manner through HPP. Secure card capture is to allow merchants to embed the HP SDK into their existing website/web view. This SDK solution offered by DDP platform shall load in iFrame and comply the PCI requirements. In addition to the core functionality of HPP, It shall provide a capability to customize the UI to match the merchant's website style.

[Hosted flow Guide](?path=assets/uCom_HostedPages2_Integration_Guide.pdf)

[Hosted flow Guide](?path=docs/interactive-guide/hostedPage.md)


<!-- type: tab-end -->

---

### Step 2c: Vault a Payment Method

**Description**

This request will take our generated nonce token from previous step in the payload as well as the `tokenId` from Step 1 in the headers. Once this step is completed the payment method will be vaulted and an EV (Enrolment Vault) token will be generated. These tokens are specific to payment methods vaulted with Fiserv and are not to be confused with TA (TransArmor) tokens which are generated through other flows. This EV token can be non-expired tokens and can be used multiple times for that recipeient.

<!-- theme: failure -->
>**<ins> Header Change
>| Header Key | Change |
>| ---------- | ------ |
>| Authorization | returns to normal HMAC signature |
>| access_token  | the same `tokenId` used in the Bearer authorization in step 3 (*ex.* `access_token: AGG596cV67WF8DjYLE3kS6nSu36x`) |
><!-- TODO: ADD EXAMPLES HERE-->

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/recipients)

### Get Recipient Accounts

**Description**

This request will retrieve the account information based on the recipientId. This will return ALL associated payment methods with this `recipientId`. Remember the `recipientId` changes based upon email so if multiple merchantCustomerIds share the same email this will pull all the accounts from every merchantCustomerId associated with that email.

[![Try it out](../../../../assets/images/button.png)](../api/?type=get&path=/ddp/v1/recipients/{recipientId}/accounts)

## Step 3: Create a Payment

**Description**

There are following different payment methods for the disbursements, there are detailed descriptions are provided below for each of the payment methods.

 1. Debit
 2. ACH
 3. Coinbase
 4. Money Network
 5. PayPal
 6. Venmo
 7. E-Check
 8. TA Token

**Debit**

This request is the payment call where we will take our `enrolment_vault` token and use it to create a disbursement payment for a recipient. The key variables here are `merchantTransactionId`, this must be unique for every transaction call , as well as paymentType which should correspond to the payment type associated with your environment for eg. Gaming, Claims, Wages etc. The EV token will never expire for the same `merchantCustomerId` and we can perform many number of transactions using same EV token.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments/{id}/disburse)

**ACH**

An ACH is an electronic fund transfer made between banks and credit unions across what is called the Automated Clearing House network. This is an ACH payment request  where we will take our `enrolment_vault`token and use it to create a disbursement payment for a recipient. The key variables here are `merchantTransactionId`, this must be unique for every transaction call , as well as paymentType which should correspond to the payment type associated with your environment.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments/{id}/disburse)

**Coinbase**

This request is the payment call using the Coinbase payment method to create a disbursement payment for a recipient. The key variables here are `merchantTransactionId`, this must be unique for every transaction call , as well as paymentType which should correspond to the payment type associated with your environment for eg. Gaming, Claims, Wages etc. The EV token will never expire for the same `merchantCustomerId` and we can perform many number of transactions using same EV token.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments/{id}/disburse)

**Money Network**

This request is the payment call using the Money Network payment method, where we will take our `enrolment_vault` token and use it to create a disbursement payment for a recipient. The key variables here are `merchantTransactionId`, this must be unique for every transaction call , as well as paymentType which should correspond to the payment type associated with your environment for eg. Gaming, Claims, Wages etc. The EV token will never expire for the same `merchantCustomerId` and we can perform many number of transactions using same EV token.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments/{id}/disburse)

**PayPal**

This request is the payment call using the PayPal payment method, where we can use the `email` or `phoneNumber`  variables to initate the payments. The key variables here are `merchantTransactionId`, this must be unique for every transaction call , as well as paymentType which should correspond to the payment type associated with your environment.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments/{id}/disburse)

**Venmo**

This request is the payment call using the Venmo payment method, where we can use the `phoneNumber`  variables to initate the payments. The key variables here are `merchantTransactionId`, this must be unique for every transaction call , as well as paymentType which should correspond to the payment type associated with your environment.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments/{id}/disburse)

**E-Check**

This request is the payment call using the E-Check payment method to create a disbursement payment for a recipient. The key variables here are `merchantTransactionId`, this must be unique for every transaction call , as well as paymentType which should correspond to the payment type associated with your environment for eg. Gaming, Claims, Wages etc. 

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments/{id}/disburse)

**TA Token**

This request is the payment call using the TA Token payment method to create a disbursement payment for a recipient. The key variables here are `merchantTransactionId`, this must be unique for every transaction call , as well as paymentType which should correspond to the payment type associated with your environment for eg. Gaming, Claims, Wages etc. 

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments/{id}/disburse)

### Step 3a:  Cancel a Payment

**Description**

This feature is to cancel a payment transaction which is in progress or waiting for settlement. This is applicable for any ACH,Coinbase,Venmo(incase of unclaimed) and PayPal(incase of unclaimed) transaction which is waiting for settlement. The prerequisite is to have a transaction initiated or disbursed using the payment API. The parameter required is the transaction id or TID [this is the unique id generated by DDP and is mapped 1:1 against the merchant transaction id.

Using `merchantCustomerId`: This request can be used when you want to view all the transactions for that merchant customer id  

[![Try it out](../../../../assets/images/button.png)](../api/?type=patch&path=/ddp/v1/payments/{merchantTransactionId}/cancel)

Using `merchantTransactionId`: This request to be used when you want to view the details of a particular transactions

[![Try it out](../../../../assets/images/button.png)](../api/?type=patch&path=/ddp/v1/payments/{transactionId}/cancel)

### Step 3b: Get Transaction Status

**Description**

This request will get the details of a transaction and will take either `merchantCustomerId` or 'merchantTransactionId' in the request parameter. The prerequisite is to have a transaction initiated or disbursed using the payment API. Please refer transaction initiate/disburse section in the following link for more information. 

Using `merchantCustomerId`: This request can be used when you want to view all the transactions for that merchant customer id  

[![Try it out](../../../../assets/images/button.png)](../api/?type=get&path=/ddp/v1/transactions/recipients/{merchantCustomerId})

Using `merchantTransactionId`: This request to be used when you want to view the details of a particular transactions

[![Try it out](../../../../assets/images/button.png)](../api/?type=get&path=/ddp/v1/transactions/{merchantTransactionId})

## Step 4: Optional/Info: Get Merchant Info

**Description**

This request will get the details of an onboarded merchant with no request parameter only the headers are required. With this call you can get the full information of the merchant like ledger balance, Available balance, DFA Account Number, Merchant Name & Address details etc.

[![Try it out](../../../../assets/images/button.png)](../api/?type=get&path=/ddp/v1/merchantInfo)

## See Also

- [Please refer our FAQ's](?path=docs/faq/faq.md)