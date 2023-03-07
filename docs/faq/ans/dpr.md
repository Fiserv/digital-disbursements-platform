## How should we differentiate between a declined payment response?

You can differentiate based on the error codes, but the best way to get the payment status is using our [GET API] (../api?type=get&path=/ddp/v1/transactions/{transactionId}) request for transactions
