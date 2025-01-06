# Portal Flow

## Introduction

The Digital Disbursement (DDP) recipient portal offers a ready to use website where recipients can accept and manage their disbursements. The DDP Recipient Portal offers three paths a user can follow:

- **Guest Path**
  - This is the path every first time user will follow. After completing the first payout the guest user will be offered the ability to create an account. Additionally, a merchant has the option to enable  "Guest Flow Only" where users will not be prompted to create an account after completing a payout. Typically, the decision is based on the number of times that a consumer/business is expected to receive a payment.

- **Registered User Path**
  - If a recipient already has a registered account, they will be directed to a login screen. Once logged in recipients will be able to view, accept, reject, and track their payouts. see an example of a registered recipients dashboard for viewing and managing payouts below.

- **Multiple Recipient Payments**
  - If this option is being used, then the first recipient to enter his/her disbursement information will receive all of the funds.  The remaining recipients will have to approve the disbursement to the recipient receiving the funds prior to the completion of the disbursement.  If the payment expires before all recipients have approved or a recipient rejects the payment, then the payment is cancelled, funds are released in the DFA, and recipients are notified of the expiration.

![Portal Dashboard image](../../assets/images/accept-payment-reg-user.png "Portal Dashboard image")

## Step 1: Initiate Payment

<!-- Theme="Success -->
> Ensure you understand how to set up API calls with our [Environment Set-up Guide](environment.md)

### Option 1a: Initiate Single Consumer Payment

Merchant will want to initiate a payment for a single consumer recipient through the merchant portal and send email to recipient for the payment disbursement.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)

### Option 1b: Initiate Single Company Payment

Merchant will want to initiate a payment for a single company recipient through the merchant portal and send email to recipient for the payment disbursement.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)

### Option 1c: Initiate Multi Consumer Payment

Merchant will want to initiate a payment for a Multi consumer recipient through the merchant portal and send email to recipient for the payment disbursement.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)

### Option 1d: Initiate Multi Company Payment

Merchant will want to initiate a payment for a Multi company recipient through the merchant portal and send email to recipient for the payment disbursement.

[![Try it out](../../../../assets/images/button.png)](../api/?type=post&path=/ddp/v1/payments)

### Option 1e: Cancel a Payment

Merchant can only cancel payments that are in a “Pending” status. Once a disbursement method has been selected by the recipient, the payment is no longer able to be cancelled. After disbursement cancellation only applicable to ACH, Coinbase, E-check, Venmo and PayPal.

Clients are able to initiate a payment cancellation at any time during the process, unless the disbursement is being processed. When a cancellation is successful:

• The Transaction and Payment statuses are updated.

• The hold is removed from the funds in the DFA.

• Notification is sent to the recipient(s).

Using `merchantCustomerId`: This request can be used when you want to view all the transactions for that merchant customer id  

[![Try it out](../../../../assets/images/button.png)](../api/?type=patch&path=/ddp/v1/payments/{merchantTransactionId}/cancel)

Using `merchantTransactionId`: This request to be used when you want to view the details of a particular transactions

[![Try it out](../../../../assets/images/button.png)](../api/?type=patch&path=/ddp/v1/payments/{transactionId}/cancel)

## Step 2: Optional/Info: Get Merchant Info

This request will get the details of the onboard merchant with no request parameter only the headers are required. With this call you can get the full information of the merchant like ledger balance, Available balance, DFA Account Number, Merchant Name & Address details etc.

[![Try it out](../../../../assets/images/button.png)](../api/?type=get&path=/ddp/v1/merchantInfo)

## See Also

- [Please refer our FAQ's](?path=docs/faq/faq.md)
