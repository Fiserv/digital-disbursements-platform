# How we can help you?

**Here are few common questions that may help you while you are exploring our API.**

## General FAQs

<details>
<summary><b>What are the recipient types available for digital disbursement payments?</b></summary>

DDP supports 2 recipient types – Consumer and Company.

Consumer is an individual or a person. Company is usually a business or business user.

For more information on recipient please refer a recipient section [API Flow] (../docs/?path=docs/interactive-guide/apiflow.md)

</details>

<details>
<summary><b>What should a customer do if the amount of the payment issued to them is incorrect? </b></summary>

This can be handled in 2 ways:

1) The portal allows the customer to reject a payout due to incorrect amount. Once rejected, the customer can enter the reason for the rejection—i.e. Incorrect amount—and a notification will be sent to the client. The client can issue a new payment for the correct amount.

2) The client can cancel the payment and void it prior to the customer accepting. Once the transaction is canceled, the client can issue a new payment for the correct amount.

</details>

<details>
<summary><b>How often can I send my customers payments? </b></summary>

There is no limit on how many payments a client can send their customer through this system. The customer, however, may have limits on how much they can accept on their debit card on daily or monthly basis.

</details>

<details>
<summary><b>Can I stop a payment once it has been sent to a customer? </b></summary>

If the payment has been sent but not accepted, the client can perform a cancellation through the Digital Payouts platform. If the payment has been accepted by the customer, the client would need to perform a reversal. A reversal can only be completed if the payment was accepted to a bank account via Bank Transfer. A payment accepted to a debit card cannot be reversed.

</details>

<details>
<summary><b>What is TIN? Also, which use case involves passing value for TIN?</b></summary>

A Tax Identification Number (TIN) is internationally used as an identification number for tax purposes. This is mandatory field when we are creating a  business type recipients.
For more information on recipient please refer a recipient section [API Flow] (../docs/?path=docs/interactive-guide/apiflow.md)

</details>

<details>
 <summary><b>Is the mobile number mandatory for create recipient?</b></summary>

No, the mobile number is not the mandatory parameters for creating the recipient. Its an optional parameter while creating the recipient either for consumer or business.

For more information on recipient please refer a recipient section [API Flow] (../docs/?path=docs/interactive-guide/apiflow.md)

</details>

<details>
<summary><b>Can we make a disbursement using credit or pre-paid cards?</b></summary>

We don’t support credit and pre-paid cards for disbursements, we supports only debit cards.
There are the following payment methods we support for the disbursements :
Debit
ACH
Coinbase
Money Network
PayPal
Venmo
E-Check
TA Token

For more information please refer to the payment section [API Flow] (../docs/?path=docs/interactive-guide/apiflow.md)

</details>

<details>
<summary><b>Do we expect an API response and the webhook notification at the same time ?</b></summary>

HTTP Responses are mostly synchronous and immediate. Notifications are configured based on the event and can be immediate or time dependent. For eg: if the notification is configured for the payment request, we will receive both at the same time. For more information on Notification please refer notification [section].

</details>

<details>
<summary><b>Can you elaborate on how a retry is initiated? What is the maximum retry count? </b></summary>

When recipient provides wrong payment information (Payment will be declined by backend as payment info is wrong) for fixed number of times then payment is cancelled when max retry attempt is reached.

How many times a recipient can attempt to disburse a payment with wrong payment information can be configured by using key ENABLE_PAYMENT_RETRY_COUNT.

</details>

<details>
<summary><b> Does “TV – Transaction Cancelled” or “DNF - Delivery Notification Failed” is a final state and we have to VOID this payment? </b></summary>

This is the final state and can be considered as a payment cancelled. There is no need to VOID the payment or any other action required.
For more information please refer [Payment Status] (../docs/?path=docs/troubleshooting/Transaction-payment-status.md)

</details>

<details>
<summary><b>How do we setup to get webhook notifications back from Fiserv in our lower environments? </b></summary>

   To enable the webhook notification we have to raise a firewall request from the Fiserv side and it requires a 10 days SLA to get the required approvals from the security team and enable it.
   The webhook URLs for non-prod and prod environments should  be given during onboarding request.

</details>

<details>
<summary><b>Do we have a DDP web console or monitoring portal to check the transaction history for the disbursements ? </b></summary>

 At the moment its not available for the non-prod environment. But for production we do have the CLX reporting tool, where we can check the transaction details.
 Please refer the CLX section for more details.

</details>

<details>
<summary><b>Can you provide us all the possible code values for each expected payment status “DH” and “DD” ? </b></summary>

| Error Code | Status | message |
| :--------: | :----: | ------- |
| 400063     | DH     | Fraud Failed |
| 400201     | DD     | D2D payment declined by PaySecure. |

For more info please refer [Error Codes] (../docs/?path=docs/troubleshooting/error-codes.md) and
[Payment Status] (../docs/?path=docs/troubleshooting/Transaction-payment-status.md)

</details>

<details>
<summary><b>What are the IPs needs to be whitelisted to enable the webhooks by client?</b></summary>

204.194.141.0/24
204.194.143.0/24

Subnet(/24) will cover a total of 256 ips (204.194.143.0 - 204.194.143.255).

Note : If any difficulties whitelisting the above IPS please use below ones

204.194.141.29 204.194.141.30 204.194.143.29 204.194.143.30

</details>

## API Flow FAQs

<details>
<summary><b>What should be supplied in a tokenId field to the payment API where the  tokenProvider is ENROLMENT_VAULT?</b></summary>

Enrollment Vault Id should be supplied as part of the payment request, which we have received during account vaulting.
 For more info please refer [API Flow] (../docs/?path=docs/interactive-guide/api-flow/apiflow.md)

</details>

<details>
<summary><b>Can we have both email Id and phone number when we are creating a recipient and make only one of them mandatory? </b></summary>

No email id is the mandatory field and for your use case : The scenario when you don’t have the user’s email ID , you can generate random email-ID (which should be unique) to fill those values to create a successful recipient.
For more info please refer [API Flow] (../docs/?path=docs/interactive-guide/api-flow/apiflow.md)

</details>

<details>
  <summary><b>Why is Consumer recipient GUEST value is passed as True? Why is this required when posting the recipient?</b></summary>

If Guest is false for Recipient of type Consumer, then payment cannot be initialized for the recipient as it will be considered as an INACTIVE recipient.
For more information on recipient please refer a recipient section [API Flow] (../docs/?path=docs/interactive-guide/apiflow.md)

</details>

## Portal Flow FAQs

<details>
<summary><b>Does the user experience in the portal has a difference once the payment notification goes out between a consumer and business?</b></summary>

No, the consumer and the business have a user experience.
For more info please refer [Portal Flow] (../docs/?path=docs/interactive-guide/portal-flow/portalflow.md)

</details>

<details>
<summary><b> Can a merchant able to cancel (“TV – Transaction Cancelled”, “PC- Payment Cancelled”) a payment from the portal? </b></summary>

Portal is only for recipients and Merchant cannot cancel a payment using portal.  

For more information please refer [Portal Flow] (../docs/?path=docs/interactive-guide/portalflow.md)

</details>

<details>
<summary><b> What causes a customer to be locked out of the portal and how long will the lockout last?  </b></summary>

When an incorrect One-time Pass code (OTP) is entered 3 times, the customer will be locked out for 30 minutes and their payment will be canceled. An email notification will be sent to the customer stating why the payment was canceled.

</details>

## Batch Flow FAQs

<details>
<summary><b>Whom we need to contact for the MERCHANT_ID?</b></summary>

DDP product team will provide the merchant id.

For more information please refer [Batch Flow] (../docs/?path=docs/interactive-guide/batchflow.md)

</details>

<details>
<summary><b>Share the list of values for the Payment Type field?</b></summary>

These are the following payment type values :
Claims, Wages, Loans, Revenue Share, Gaming

For more information please refer [Batch Flow] (../docs/?path=docs/interactive-guide/batchflow.md)
  
</details>

<details>
<summary><b>Can we send a formatted date string in place of epoch in the file name?</b></summary>

Number of seconds that have elapsed since January 1, 1970 (midnight UTC/GMT). Example : 1654537809
  
</details>

<details>
<summary><b>Can we send some other number sequence instead of EPOCH in the file name?</b></summary>

No. We mandate epoch & there is a separate field for version. This is standardized across all clients. We do this for few reasons.

- Created Date for all records in a file is same - So it makes sense to put it on file-name - and we need that info.
- EPOCH by itself is a standard. By default, it enforces GMT and Consumes 10 digits.
- Other Date format with seconds precision takes at least 14 digits - without TZ info.
- EPOCH time is available in all programming languages - from java-script and shell script.
  
</details>

<details>
<summary><b>Who will Share the FTP and PGP Key details?</b></summary>

- FTP - Will it be the same from Outgoing as well as incoming files
- PGP - Used in file export from RK to Fiserv

This FTP and PGP key information is given by our MFT team once the mailboxes are setup. Prior to this we need a File Gateway Onboarding form completed by the client. If there are any questions or concerns with this process the MFT team should be able to provide assistance or point you in the right direction. Once this is filled out we can submit a ticket to our MFT team to get this setup completed. Once this is completed then they should have the FTP and PGP key information readily available to be shared.
  
</details>

## Payment Services FAQs

<details>
<summary><b>How should we differentiate between a declined payment response?</b></summary>

You can differentiate based on the error codes, but the best way to get the payment status is using our [GET API] (../api?type=get&path=/ddp/v1/transactions/{transactionId}) request for transactions
  
</details>

<details>
<summary><b>Will you return the original ISO response codes from issuer/card scheme in the response for a POST /ddp/v1/payments </b></summary>

ISO response + DDP internal response code.

</details>

## TPSP FAQs

<details>
<summary><b>What is TPSP client?</b></summary>

TPSP stands for Third Party Service Providers, A third-party service provider is generally defined as an external person or company who provides a service or technology as part of a contract.

</details>

<details>
<summary><b>What is the process to onboard a TPSP clients?</b></summary>

The onboarding process is similar with our regular merchants, the only difference is we can perform the certification testing in our CA(Client Testing Environment) once the TPSP customers can execute and validated their test cases.

</details>

<details>
<summary><b>What is the process & time duration for certification testing?</b></summary>
Certification testing is usually conducted in a two-hour attended session which can cover the maximum scenario's for the enabled features. Fiserv Testing Center of Excellence (TCoE) and Delivery team  are require to attend the sessions and validate the backend system for the tested scenarios.

</details>

## Security FAQs

<details>
<summary><b>What kind of fraud check mechanism exists?</b></summary>

We have vigilance Fraud check is enabled, which means any suspicious card or account will be declined for disbursements.

</details>

<details>
<summary><b>Do DDP support HMAC in web hook notification?</b></summary>

Currently we don’t support the HMAC in webhook notification, we do support basic authentication in which a key value can be passed as part of header information, we will update the implementation guide accordingly.

</details>

<details>
<summary><b>What is the process for generating and agreeing about the secret key for the HMAC to secure the notifications?</b></summary>

Below are the way which we are using to generate the signature, you may also use the same. var key = postman.getEnvironmentVariable('clientId'); var secret = postman.getEnvironmentVariable('clientSecret');

var time = new Date().getTime(); var method = request.method;

var rawSignature = key + ":" + time; var requestBody = request.data;

if (method != 'GET' && method != 'DELETE') { var payload_digest = CryptoJS.SHA256(requestBody); var b64BodyContent = CryptoJS.enc.Base64.stringify(payload_digest); rawSignature = rawSignature + ":" + b64BodyContent; }

var signature = CryptoJS.HmacSHA256(rawSignature, secret); postman.setEnvironmentVariable('time', time); postman.setEnvironmentVariable('signature', CryptoJS.enc.Base64.stringify(signature));

</details>
