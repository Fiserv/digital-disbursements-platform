# Error Codes

Listed below are the error codes being returned during DDP operations. The codes are separated into sections based on what was being done in the software at the time the error occurred.

## Payment Services (General)

| API                   | Error code | Scenario                                                                                     | Error message                                                                                  |
|-----------------------|------------|----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| /payments             | 400021     | recipientProfileInfo missing in the payload                                                  | Invalid or Missing recipientProfileInfo.                                                       |
| /payments             | 400021     | paymentsInfo missing in the payload                                                          | Invalid or Missing paymentsInfo.                                                               |
| /payments             | 400022     | Merchant id missing/length issue/doesn’t exists in Merchant table                            | Invalid or Missing originationID (merchantID).                                                 |
| /payments             | 400022     | MerchantCustomer id missing/length issue/mapping doesn’t exists in Merchants_recipient table | Invalid or Missing vendorID / merchantCustomerID.                                              |
| /payments             | 400022     | Merchant transaction id length validation                                                    | Invalid merchantTransactionId                                                                  |
| /payments             | 400022     | Merchant status is not active in the Merchant table                                          | Merchant is not Active                                                                         |
| /payments             | 400023     | Recipient id missing/length issue/mapping doesn’t exists in Merchants_recipient table        | Invalid or Missing recipientID”"                                                               |
| /payments             | 400023     | Recipient is not active                                                                      | Recipient Profile is not Active.                                                               |
| /payments             | 400024     | Total disbursement amount is missing/incorrect format                                        | Invalid or Missing totalDisbursementAmount.                                                    |
| /payments             | 400024     | Currency code is not valid                                                                   | Invalid or Missing currencyCode.                                                               |
| /payments             | 400024     | Description length is more than allowed                                                      | Invalid description.                                                                           |
| /payments             | 400024     | Payment amount is missing/incorrect format                                                   | Invalid or Missing payment.                                                                    |
| /payments             | 400024     | Total disbursements amount and payment amount does not match in the payload                  | Transaction total and Payment total does not match.                                            |
| /payments             | 400024     | Currency code is not valid                                                                   | Invalid or Missing currencyCode.                                                               |
| /payments             | 400024     | Payment Type missing in payload                                                              | Invalid or Missing paymentType.                                                                |
| /payments             | 400024     | Payment Type doesn’t exists in table                                                         | Payment Type not allowed.                                                                      |
| /payments             | 400025     | DFA balance is insufficient                                                                  | Not Sufficient Fund. Please contact Program Manager.                                           |
| /payments             | 400025     | Daily transaction limit exceeds                                                              | Transaction exceeds allowed daily transaction limit. Please contact Program Manager.           |
| /payments             | 400025     | Daily transaction count limit exceeds                                                        | Transaction exceeds allowed daily transaction count. Please contact Program Manager.           |
| /payments             | 400025     | Monthly transaction limit exceeds                                                            | Transaction exceeds allowed monthly transaction limit. Please contact Program Manager.         |
| /payments             | 400025     | Monthly  transaction count limit exceeds                                                     | Transaction exceeds allowed monthly transaction count. Please contact Program Manager.         |
| /payments             | 400025     | Single transaction amount limit exceeds                                                      | Transaction exceeds allowed limit for a single transaction. Please contact Program Manager.    |
| /payments             | 400025     | Single payout limit exceeds                                                                  | Transaction exceeds allowed limit for a single payment/payout. Please contact Program Manager. |
| /payments             | 400024     | Invalid requestor value                                                                      | Invalid or Missing Requestor allowed values are [MERCHANT, RECIPIENT].                         |                                                                    
| /payments             | 400024     | Invalid transaction id                                                                       | Invalid Transaction ID.                                                                        |
| /payments             | 400024     | Payload is missing                                                                           | Payload cannot be null.                                                                        |
| /payments             | 400024     | Invalid payment transaction id                                                               | "Invalid payment transaction ID.                                                               |
| /payments             | 400024     | Payment id provided is already cancelled or not matching the cancellation criteria           | Payment cannot be cancelled.                                                                   |
| /payments             | 400024     | Transaction id provided is already cancelled or not matching the cancellation criteria       | Transaction cannot be cancelled.                                                               |
| /payments/{id}/cancel | 400029     | Cancel Request receives invalid transaction/payment ID                                       | Invalid request format/data.                                                                   |
| /payments/{id}/cancel | 400034     | Do not allow merchants to cancel ACH transactions after 5 days of the transaction.           | ACH cancellation is not possible after 5 days of payout.                                       |
| /payments             | 400021     | If source and token is different in disbursement request.                                    | Source/Token mismatch                                                                          |

## Paysecure Adapter for Debit Disbursement Method

| API       | Error code | Scenario                                                                                | Error message                                                                                                     |
|-----------|------------|-----------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| /payments | 400041     | Paysecure request is not correctly formatted                                            | Invalid request format/data.                                                                                      |
| /payments | 400042     | Unable to connect to Paysecure services                                                 | Service has a timeout and will be back shortly                                                                    |
| /payments | 400043     |                                                                                         | Network authorization failed                                                                                      |
| /payments | 400044     | BIN validation failure for Debit card on Paysecure network                              | Failed BIN is not eligible for Credit.                                                                            |
| /payments | 400045     | Card Verification failure on Paysecure network. Payment declined                        | Card Verification Failed.                                                                                         |
| /payments | 400046     | Credit operation failed on Paysecure network. Payment declined                          | Pinless Credit Failed                                                                                             |
| /payments | 400047     | Credit operation failed on Paysecure network due to missing parameter. Payment declined | Missing Parameter                                                                                                 |
| /payments | 500000     | Unable to connect to Paysecure services                                                 | This service is down at the moment please try again later                                                         | 
| /payments | 500002     | Unable to connect to Paysecure services                                                 | "Unable to process your request Please try again later if the problem persist contact system admin.(SERVER_ERROR) |  

## BlueCheck Adapter for ACH Disbursement Method

| API       | Error code | Scenario                                                        | Error message                                                                                                     |
|-----------|------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| /payments | 400049     | BluePay adapter parameters are missing or incorrectly formatted | Invalid request format/data (ach type, account, routing number invalid)                                           |
| /payments | 400050     | BluePay services timing out. Unable to connect.                 | Service has a timeout and will be back shortly.                                                                   |
| /payments | 400051     | Parameters missing for Bluepay services. Payment Declined       | Missing Parameter                                                                                                 |
| /payments | 400052     |                                                                 | Record Not Found.                                                                                                 |
| /payments | 500000     | Unable to connect to BluePay services                           | This service is down at the moment please try again later                                                         |
| /payments | 500002     | Unable to connect to BluePay services                           | Unable to process your request  Please try again later if the problem persist contact system admin.(SERVER_ERROR) |  

## Payment Decline Codes

| API       | Error code | Scenario                                 |
|-----------|------------|------------------------------------------|
| /payments | 400201     | D2D payment declined by PaySecure        |
| /payments | 400202     | ACH payment declined by BluePay          |
| /payments | 400203     | Prepaid payment declined by MoneyNetwork |
| /payments | 400204     | Venmo payment declined by Paypal Inc     |
| /payments | 400205     | Reserved for future methods              |
| /payments | 400206     | Reserved for future methods              |
| /payments | 400207     | Reserved for future methods              |
| /payments | 400208     | Reserved for future methods              |
| /payments | 400209     | Reserved for future methods              |
| /payments | 400210     | Reserved for future methods              |
| /payments | 400211     | Reserved for future methods              |
| /payments | 400212     | Reserved for future methods              |
| /payments | 400213     | Reserved for future methods              |
| /payments | 400214     | Reserved for future methods              |
| /payments | 400215     | Reserved for future methods              |
| /payments | 400216     | Reserved for future methods              |
| /payments | 400217     | Reserved for future methods              |
| /payments | 400218     | Reserved for future methods              |
| /payments | 400219     | Reserved for future methods              |
| /payments | 400220     | Reserved for future methods              |

## Other Codes from Payment Core

| API                            | Error code | Scenario                                                                           | Error message                                                                                       |
|--------------------------------|------------|------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| /payments                      | 500000     |                                                                                    | "This service is down at the moment  please try again later.                                        |
| /payments                      | 500001     |                                                                                    | "Unable to process your request Please try again later if the problem persist contact system admin. |  
| /payments                      | 400021     |                                                                                    | Invalid request format/data.                                                                        |
| /payments                      | 400022     |                                                                                    | Invalid request format/data.                                                                        |
| /payments                      | 400023     |                                                                                    | Invalid request format/data.                                                                        |
| /payments                      | 400024     |                                                                                    | Invalid request format/data.                                                                        |
| /payments                      | 400001     |                                                                                    | Service has a timeout and will be back shortly.                                                     |
| /payments                      | 404021     |                                                                                    | No data found for the request.                                                                      |
| /payments                      | 409022     |                                                                                    | Conflict with existing record.                                                                      |
| /payments                      | 400025     |                                                                                    | Invalid request format/data.                                                                        |
| /payments                      | 400026     |                                                                                    | Invalid request format/data.                                                                        |
| /payments                      | 400027     |                                                                                    | Invalid request format/data.                                                                        |
| /payments                      | 500004     |                                                                                    | OFAC service is not available. Please try again later.                                              |
| /payments                      | 500004     |                                                                                    | Fraud service is not available.Please try again later.                                              |
| /payments                      | 500004     |                                                                                    | AWS store is not available.Please try again later.                                                  |
| /payments                      | 401021     |                                                                                    | Authentication failed  access  Invalid code)                                                        |
| /payments                      | 400028     |                                                                                    | Invalid request format/data.                                                                        |
| /payments                      | 400029     |                                                                                    | Invalid request format/data.                                                                        |
| /payments                      | 400028     |                                                                                    | Invalid request format/data.                                                                        |
| /payments                      | 400060     |                                                                                    | Invalid Recipient  Recipient  Invalid Recipient.                                                    |
| /payments                      | 400061     |                                                                                    | Invalid Transaction  Notification  Invalid Transaction.                                             |
| /payments                      | 400062     |                                                                                    | Invalid Recipient Id  Recipient  Invalid Recipient Id.                                              |
| /payments                      | 400063     |                                                                                    | Fraud Failed  Fraud  Fraud Failed.                                                                  |
| /payments                      | 400064     |                                                                                    | OFAC Failed  OFAC  OFAC Failed.                                                                     |
| /payments                      | 400065     |                                                                                    | Invalid Transaction Id.  common  Invalid Transaction Id.                                            |
| /payments                      | 400066     |                                                                                    | Invalid Payment Id.  common  Invalid Payment Id.                                                    |
| /payments                      | 400030     |                                                                                    | "This service is down at the moment please try again later.                                         |
| /payments                      | 400031     |                                                                                    | "This service is down at the moment please try again later.                                         | 
| /payments                      | 400044     |                                                                                    | BIN Validation Failed.                                                                              |
| /payments                      | 500005     |                                                                                    | Exception saving notification to db.                                                                |
| /payments                      | 400030     |                                                                                    | Invalid request format/data.                                                                        |
| /payments                      | 400032     |                                                                                    | AWS credentials setup missing.                                                                      |
| /payments                      | 400002     |                                                                                    | Invalid request format/data.                                                                        |
| /payments/{paymentId}/disburse | 400128     | Payment method and Payment method limit validation does not match while disbursing | Either Payment Method is not enabled or Payment Method Limit exceeds allowed limit.                 |

## Transaction Services

| API                           | Error code | Scenario | Error message                                   |
|-------------------------------|------------|----------|-------------------------------------------------|
| /transactions/{transactionId} | 400081     |          | Invalid request format/data.                    |
| /transactions/{transactionId} | 400082     |          | Service has a timeout and will be back shortly. |
| /transactions/{transactionId} | 400083     |          | Missing Parameter                               |
| /transactions/{transactionId} | 400084     |          | No data found for the request.                  |

## Authentication Services

| API  | Error code | Scenario                     | Error message                                            |
|------|------------|------------------------------|----------------------------------------------------------|
| /otp | 401091     | Invalid request format/data  | Invalid or Missing otp                                   |
| /otp | 400092     | Passing invalid recipient Id | Invalid or missing recipient profile                     |
| /otp | 401093     | Invalid request format/data  | OTP has been rejected.                                   |
| /otp | 400091     |                              | Invalid request format/data                              |
| /otp | 400092     |                              | Invalid or missing recipient profile                     |
| /otp | 400093     |                              | User already exists.                                     |
| /otp | 400094     |                              | User does not  exists.                                   |
| /otp | 400095     |                              | New password cannot be the same as the current password. |
| /otp | 400096     |                              | State of flow id out of scope                            |
| /otp | 400097     |                              | Invalid or Missing field                                 |
| /otp | 401098     |                              | Authentication failed                                    |
| /otp | 400099     |                              | JWT generation failed                                    |
| /otp | 4000100    |                              | Invalid request format/data                              |
| /otp | 400101     |                              | The requested resource was not found                     |
| /otp | 400061     |                              | Invalid request format/data                              |

## Account Services

| API                       | Error code | Scenario                                              | Error message                                                                                                 |
|---------------------------|------------|-------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| /recipients/{id}/accounts | 400052     | Enrollment Vault services are not available           | EV get all service unavailable                                                                                |
| /recipients/{id}/accounts | 400053     | Enrollment Vault services are not available           | EV Add Card service unavailable                                                                               |
| /recipients/{id}/accounts | 400054     | Connectivity with UCOM cannot be established          | Ucom service unavailable                                                                                      |
| /recipients/{id}/accounts | 400049     | Account Services requests are not correctly formatted | "Invalid request format/data. (TokenProvider is Invalid. Can be only SINGLE_USE_TOKEN, Missing TokenProvider) |
| /recipients/{id}/accounts | 400051     | Parameter required by EV is missing                   | Missing Parameter                                                                                             |

## Merchant Services

| API        | Error code | Scenario                                | Error message               |
|------------|------------|-----------------------------------------|-----------------------------|
| /merchants | 400101     | Request format / data is incorrect.     | Invalid request format/data |
| /merchants | 400102     | Merchant ID provided is invalid for DDP |  Invalid merchant id        |

## Recipient Services

| API         | Error code | Scenario | Error message                                                                                       |
|-------------|------------|----------|-----------------------------------------------------------------------------------------------------|
| /recipients | 500000     |          | This service is down at the moment. please try again later                                          |
| /recipients | 500001     |          | Unable to process your request. Please try again later if the problem persist contact system admin. |
| /recipients | 400000     |          | Invalid request format/data.                                                                        |
| /recipients | 400001     |          | Service has a timeout and will be back shortly.                                                     |
| /recipients | 404001     |          | No data found for the request.                                                                      |
| /recipients | 409001     |          | Conflict with existing record.                                                                      |
| /recipients | 500002     |          | Recipient cannot be created. Please try again later.                                                |
| /recipients | 500003     |          | Recipient cannot be updated. Please try again later.                                                |
| /recipients | 500004     |          | Recipient cannot be updated. Please try again later.                                                |
| /recipients | 400002     |          | Invalid request format/data.                                                                        |
| /recipients | 400003     |          | Invalid request format/data.                                                                        |
| /recipients | 400004     |          | Invalid request format/data.                                                                        |
| /recipients | 400005     |          | Invalid request format/data.                                                                        |
| /recipients | 400006     |          | Invalid request format/data.                                                                        |
| /recipients | 400007     |          | Invalid request format/data.                                                                        |
| /recipients | 400008     |          | Invalid request format/data.                                                                        |
| /recipients | 400009     |          | Invalid request format/data.                                                                        |
| /recipients | 400010     |          | Invalid request format/data.                                                                        |
| /recipients | 400011     |          | Invalid request format/data.                                                                        |
| /recipients | 400012     |          | Invalid request format/data.                                                                        |
| /recipients | 400013     |          | Invalid request format/data.                                                                        |
| /recipients | 400014     |          | Invalid request format/data.                                                                        |
| /recipients | 400015     |          | Invalid request format/data.                                                                        |
| /recipients | 400016     |          | Invalid request format/data.                                                                        |
| /recipients | 401098     |          | Authentication failed  access  Authentication failed                                                |
