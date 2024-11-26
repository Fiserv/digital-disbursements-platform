# Idempotency & Timeout Handling
> Only applicable to Direct Disbursemnt Merchants

### Conclusive & Inconclusive Responses
 
- When client makes disburse payment request, the response can be either conclusive or inconclusive
- A conclusive response, is the one where client is sure of transaction status, Even if its declined. It includes,
- `HTTP 4XX` Declined due to error in client input
- `HTTP 2XX`  Approved (Except IP transaction status)
- An inconclusive response, is the one where client is not sure of transaction status, Even if client gets 2xx response. It includes,
- `HTTP 5XX` Server Errors
- `HTTP 2XX`with In Progress Status
- `Timeouts`

### Timeouts

- Timeouts usually happens, when a (system/program) tired to connect to server, but it didn't receive a response in expected time frame. This could be due to
  - Network issues (or)
  - Server overload (or)
  - Intermediate node failures (or)
  - Various other reasons
- Timeout can happen anywhere between source and destination, Ex
  - Between Merchant and Merchant's proxy
  - Between Merchant and DDP
  - Between DDP and Payment Providers (VISA or Master)
  - Payment Provider (VISA or Master) and Issuer Bank
- Timeouts are inevitable, but they are very infrequent.
- When a timeout occurs, The client cannot determine if the request reached the server or not.

> To recover from inconclusive responses and avoid the risk of duplicate payments, Merchant should adopt one of two below practices,

### Idempotency

- With Idempotency, DDP guarantees a single fund transfer per payment, even with duplicate requests
- In order for a payment to qualify for idempotency, It should have same
  - MTID (Merchant Transaction Id)
  - MCID (Merchant Customer Id)
  - Payment Amount
- In case of Inconclusive response, Merchant should wait for 5 minutes, then retry the same payment
- If DDP identifies the payment has a duplicate, DDP will return the current status
- Otherwise, DDP will process the payment, business as usual
- Merchants are permitted to retry the same payment up to 5 times
- Merchants are permitted to retry the same payment with in 24 hours of initial request

![image](assets/images/idempotency.png)

### Status Checks

- In case of Inconclusive response, Merchant should retrieve current payment status after 5 minutes of initial request
- If payment status is conclusive, Merchant should take appropriate action
- If payment status is inconclusive, Merchant should retry status check api again
- Merchants are permitted to retry status check for maximum of 5 times with 5 minutes gap

![image](assets/images/status_check.png)


### Merchant Guidelines
| Transaction Status | Payment Status | Description | Best Practices (Direct Disbursement Merchants) |  Best Practices (Portal Merchants) |
| ------------------ | -------------- | ----------- | ---------------------------------------------- | ---------------------------------- |
| `IP` In Process | `IP` In Process | DDP has presented payout request to payment endpoint; however, the outcome is currently unknown.  Merchant will be provided updated information via web hook (push) or DDP's Status Check API (pull), depending upon Merchant's preference | DDP will return a 2XX message.  To avoid the risk of duplicate payments, Merchant should adopt one of two practices:    1) Merchant should wait 5 minutes, then send DDP Status request; DDP will return current status.  Or    2) Merchant should wait 5 minutes; then retry transaction with same (MTID, MCID & Amount).  If DDP identifies the transaction has a duplicate, DDP will return the current status; otherwise, DDP will process the transaction, business as usual.  Merchants are permitted to retry the same transaction up to 5 times.  Recommended that merchant retries at 2 minutes intervals. | Not applicable |
| `TV` Transaction Cancelled  | `SE` System Error | Subsequent to responding with IP/IP, DDP determines that transaction did not reach payment endpoint. | Merchants can receive notification of a system error via web hook (push); or when calling for a Status Check or retrying a prior request. | Not applicable |
| `TC` Transaction Completed | `ED` Declined by Payment Endpoint | DDP has successfully processed the transaction to the payment endpoint; but the endpoint has declined the request.  | DDP will return a 4XX message.  Merchant can send a new transaction with the same Recipient account information; however, the same response is likely. | Not applicable |
| *`HTTP 5XX`* | *`HTTP 5XX`* |  DDP failed to process payment and the outcome is currently unknown.  Merchant will be provided updated information via web hook (push) or DDP's Status Check API (pull), depending upon Merchant's preference | To avoid the risk of duplicate payments, Merchant should adopt one of two practices:    1) Merchant should wait 5 minutes, then send DDP Status request; DDP will return current status.  Or    2) Merchant should wait 5 minutes; then retry transaction with same (MTID, MCID & Amount).  If DDP identifies the transaction has a duplicate, DDP will return the current status; otherwise, DDP will process the transaction, business as usual.  Merchants are permitted to retry the same transaction up to 5 times.  Recommended that merchant retries at 2 minutes intervals. | Not applicable |
| *`Client Timeout`* | *`Client Timeout`*  | Merchant's request timed out at merchant's application  Reasons may include but not limited to Merchant side network issues Latent responses from DDP ... | To avoid the risk of duplicate payments, Merchant should adopt one of two practices:    1) Merchant should wait 5 minutes, then send DDP Status request; DDP will return current status.  Or    2) Merchant should wait 5 minutes; then retry transaction with same (MTID, MCID & Amount).  If DDP identifies the transaction has a duplicate, DDP will return the current status; otherwise, DDP will process the transaction, business as usual.  Merchants are permitted to retry the same transaction up to 5 times.  Recommended that merchant retries at 2 minutes intervals. | Not applicable |

[Refer Idempotency & Timeout Handling](?path=docs/troubleshooting/Transaction-payment-status.md) for more info



### Inconclusive Response Examples 

``` json
// 2XX In Progress for Disburse Payment

HTTP 200
{
    "transactionId": "TID-1d95aca5-3cf2-4e62-a242-2619c2778dc0",
    "created": 1731516734,
    "merchantTransactionId": "01JCK7X4VDVT8CKX6BZEJATR06",
    "applyDate": "11/13/2024",
    "transactionDate": "11/13/2024",
    "recipient": [
        {
            "merchantCustomerId": "01JCK7XKR0KEDSQFVYVV8EVQJC",
            "recipientId": "de4587a2-8f30-4fc3-9610-c211d912c79f",
            "payments": {
                "paymentId": "b0dd9917-ec48-4f3b-bf50-f1b8473b596f",
                "amount": {
                    "total": 2.5,
                    "currency": "USD"
                },
                "fee": {
                    "total": 0.00,
                    "currency": "USD"
                },
                "paymentStatus": "IP"
            },
            "source": "DEBIT"
        }
    ],
    "transactionStatus": "IP",
    "totalTransactionAmount": {
        "total": 2.5,
        "currency": "USD"
    }
}
```

``` json
// 2XX In Progress for Status Check

HTTP 200
{
    "transactions": {

        "transactionStatus": "IP",
        "totalTransactionAmount": {
            "total": 2.5,
            "currency": "USD"
        },
        "transactionId": "TID-1d95aca5-3cf2-4e62-a242-2619c2778dc0",
        "created": 1731517065,
        "merchantTransactionId": "01JCK7XKR0KEDSQFVYVV8EVQJC",
        "applyDate": "11/13/2024",
        "transactionDate": "11/13/2024",
        "updateDate": "11/13/2024",
        "recipient": [
            {
                "merchantCustomerId": "01JCK7X4VDVT8CKX6BZEJATR06",
                "recipientId": "de4587a2-8f30-4fc3-9610-c211d912c79f",
                "accountId": "bc674eb2-21ee-41ec-9d2a-4ee7612af681",
                "payments": {
                    "paymentId": "b0dd9917-ec48-4f3b-bf50-f1b8473b596f",
                    "amount": {
                        "total": 2.5,
                        "currency": "USD"
                    },
                    "fee": {
                        "total": 0,
                        "currency": "USD"
                    },
                    "paymentType": "Claims",
                    "paymentStatus": "IP",
                    "paymentDate": "11/13/2024"
                },
                "source": "DEBIT",
                "alias": "5017",
                "doingBusinessAs": "Fiserv",
                "firstName": "SWATHI",
                "lastName": "NAGABUSHANAM",
                "groupName": "FISERV"
            }
        ]
    }
}
```

``` json
// 5XX for Disburse Payment or Status Check

HTTP 500
{
  "code": "269901",
  "message": "Unable to process your request, Please try again later, if problem persist, contact sys admin.",
  "category": "common",
  "developerInfo": {
    "developerMessage": "Unable to process your request, Please try again later, if problem persist, contact sys admin."
  }
}

```



### Certification

> TBA
