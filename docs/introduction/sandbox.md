# Basic Sandbox Information

## Introduction

The Fiserv DDP sandboxes are an extra tool provided to our clients to allow them to begin understanding and implementing the DDP flows in their system prior to formally onboarding. The sandboxes are setup with most of the functionality you will find in your personalized environment after onboarding with us so that any development efforts made on the sandbox can transition seamlessly into your own environment. There are a few caveats to this as not all available functionality is able to be generically setup and is therefore unavailable in the Sandbox environment. These can be made available to you in the personalized environment if those configuration choices are made. These differences will be noted in the sections below.

Since this is a generic environment it is possible that some of the given credentials may not match up with what the personalized environment would be, such as the PaymentType. PaymentType may be “Claims” or “Loans” when really your business is “Refund”, but functionally the requests and responses would be the same so you should still be able to code to this and then simply change the paymentType to match your implementation later.

The examples given in this guide are close to the minimal requirements to execute the call. For additional information on fields, errors, or specific requirements please reference the “Implementation Guide”.
**Your Key, Secret and PaymentType will be sent in a secure email when the environment is ready for your use.**

## Functionalities In-Scope

- Recipient
- Create
- Update
- Retrieve
- Tokenization
- Payment Vaulting
- Payments
- Debit (EV Token)
- ACH
- Paypal (Mock Success)
- Cancel Payment
- Account Lookup
- Default Emails
- Portal Flow

## Functionalities Out-of-Scope

### API & Portal

- Webhooks
- Hosted Pages
- Customized Email Templates
- Debit -TA Token
- CLX Reporting
- CLX Bulk Upload
- Payments
- eCheck
- Venmo (Functionally the same as Paypal and only available in PROD)
- Paypal (Non-mock functionality only available in PROD)
- Coinbase-
- Custom Payment Limits

### Portal Only

- Customized Logo
- Guest only flow
- Forced email verification (No SMS/Voice option)
- Batch payments
- Custom Payment Expiry Limit
