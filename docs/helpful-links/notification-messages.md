# Webhook Notifications

## Description

A webhook is essentially the same as any web page URI.  It's a client-specific URI for incoming HTTP POST notification messages.  Responses should be a standard 2xx HTTP response code and anything else would be considered a failure and subject to a retry POST. In this mechanism, consumer application exposes an endpoint and when the producer application has any updated information, it sends a message to a consumer application over URL. The communication between the two applications is Asynchronous and non-blocking. Webhooks are much like SMS notifications. Say your bank sends you an SMS for any transaction. You already told the bank your phone number, so they knew where to send the message.  

## Endpoint URL

Client must provide the merchant_endpoint and the suggest URL is ```https://<<merchant_endpoint>>/payments/notification```. DDP will be able to support Basic Authorization or HMAC for verifications. The endpoint URL should be provided by client for non-production & production respectively. Automated notifications will be posted by DDP on pre-configured payment status. The list of pre-configured notification events shared with clients as part of the onboarding form to choose

| Webhook Name |Description|
| ------------------ |  ---------- |
| ACH Sale Reversal |  Triggered when the ACH reversal process is successful |
|  ACH Sale Reversal Failed |  Triggered when the ACH reversal process is NOT successful |
|  ACH Sale Reversal Canceled |  Triggered when the ACH transaction is successfully cancelled. |
|  Disbursement Failed |  Triggered when a payment has NOT been successfully disbursed using the recipient's chosen selection |
|  Fraud Hold |  Triggered when a payment is placed on Fraud Hold and is not sent to the recipient. |
|  Payment Canceled |  Triggered when a payment status is successfully changed to cancelled. |
|  Payment Rejected |  Triggered when a recipient rejects the payment. |
|  Payment Expired |  Triggered when a pending payment expires and is no longer able to be disbursed. |
|  Payment Disbursed |  Triggered when a payment is disbursed. |
|  Payment Selected |  Enable web hook to merchant when payment method is selected by recipient |
|  ACH Credit Reject |  Triggered when an ACH Disbursement is rejected.  |
|  Payment Selected  Canceled |  triggered when a payment has been rejected by a recipient |
|  ACH Canceled |  Triggered when an ACH disbursement is successfully canceled. |
|  Email Bounced |  Triggered when an email notification sent to a recipient bounces back.  The notification considered is payment initiation. |
|  DFA Balance Alerts |  Triggered when the DFA balance meets or drops below a set balance |
|  Payment Processing |  Enable web hook to merchant when Venmo/Paypal payment is disbursed. |
|  Venmo Credit Reject |  Enable Webhook to Merchant When payment is rejected by the payer. |
|  Venmo Canceled |  Webhook to merchant for canceled status |
|  Venmo Disbursement Unclaimed |  Webhook to merchant for unclaimed status |
|  Venmo Disbursement Failed |  Webhook to merchant for Failed status |
|  Venmo or Paypal Payment Held |  Enable web hook to merchant when Venmo/Paypal payment is held. |
|  Venmo or Paypal Payment Blocked |  Enable web hook to merchant when Venmo/Paypal payment is blocked |
|  Check Disbursement Printed |  webhook to merchant after receiving printed webhook from checkbook.io |
|  Payment Check Disbursed |  Enable webhook when eCheck payment is disbursed. |
|  Check Reversal Success |  Webhook to merchant for reversal success |
|  Check Reversal Fail |  Webhook to merchant for reversal fail |
|  Batch Acknowledgement |  Webhook to merchant for Batch Acknowledgement |
|  Batch Summary |  Webhook to merchant for Batch Summary |

## Samples

### Payment Disbursed

```javaScript

{
    "merchantId": "526265330880",
    "transactionId": "TID-b7681959-4f1f-4bcc-a808-69ab501e01f0",
    "created": 1651043495095,
    "source": "DEBIT",
    "eventData": {
        "recipientId": "8a7f6589805f4b91018069d8ec6502aa",
        "merchantCustomerId": "TestConsumer1282277113",
        "merchantTransactionId": "2508b1-7490q-922181-2821-a1321412",
        "payments": {
            "paymentId": "d606bc37-b4c9-4b33-8e42-6767f65f8181",
            "amount": {
                "total": 10,
                "currency": "USD"
            },
            "fee": {
                "total": 0,
                "currency": "USD"
            },
            "paymentType": "Revenue Share",
            "paymentStatus": "DI"
        }
    },
    "transactionStatus": "TC"
}

```

### Disbursement Failed

```javaScript

{
  "merchantId": "526261603884",
  "transactionId": "TID-762c006e-bf42-4c6b-9021-044ae0bb32a0",
  "created": 1642303516977,
  "source": "DEBIT",
  "eventData": {
    "recipientId": "8a7f89f57e427e94017e594406941e48",
    "merchantCustomerId": "FR01",
    "payments": {
      "paymentId": "9713df72-386e-4484-9d1f-cf7bf607a7c6",
      "amount": {
        "total": 12.00,
        "currency": "USD"
      },
      "fee": {
        "total": 0,
        "currency": "USD"
      },
      "paymentType": "Wages",
      "paymentStatus": "DD"
    }
  },
  "transactionStatus": "IP"
}

```

### Fraud Hold

```javaScript

{
  "merchantId": "526261603884",
  "transactionId": "TID-1c1ecb29-8f34-402b-ac1b-4d4a455bc33f",
  "created": 1642305656131,
  "source": "DEBIT",
  "eventData": {
    "recipientId": "8a7f89f57e427e94017e594406941e48",
    "merchantCustomerId": "FR01",
    "payments": {
      "paymentId": "1acef796-8662-4c98-b787-82ebb4d1ddbe",
      "amount": {
        "total": 26.00,
        "currency": "USD"
      },
      "fee": {
        "total": 0,
        "currency": "USD"
      },
      "paymentType": "Wages",
      "paymentStatus": "DH"
    }
  },
  "transactionStatus": "TC"
}

```
