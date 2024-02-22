# Introduction

This guide documents the Batch as used by the Digital Payout product. The Batch allows to client initiate and manage large numbers of payments through an easy-to-use through csv files. Merchant places files in Fiserv hosted SFTP Location. DDP polls the SFTP locations for further processing.

• Merchant should upload files to SFTP location with below file name pattern. Each segment is separated by dot (.)

• FLXPI{FLEX_MID}.{TYPE}.{EPOCH}.{NAME}.{VERSION}.csv; Sample File name : FLXPINMM.CI.1654537809.BAT97.1.csv

The below table identifies the parameters of File format.

| Variable   | Description                | Ex    |
| --------  | ----------------------------------------------------------------------| ------- |
| `FLXPI`   | Constant                |      |
| `FLEX_MID` | Alpha Numeric Flex MID provided to merchant at the time of onboarding. max-length = 3  | SF,NMM  |
| `TYPE`   | 2 letter record type  CI Create Recipient & Initiate Payment, IP Initiate Payment, CP Cancel Payment, CR Create Recipient, UR Update Recipient                 | CI   |
| `EPOCH`  | number of seconds that have elapsed since January 1, 1970 (midnight UTC/GMT) | 1654537809 |
| `NAME`  | merchant preferred alpha-numeric (label or name) max-length : 20   | STLMNT220510 batch93 |
| `VERSION`  | file version as integer.             | 1    |
| `EXT`   | file extension              | CI   |

**How Merchant Sends files to SFTP ?**

INBOUND FDFG moves files from MERCHANT's mail-box to SFTP '/available/' directory with file-name <INBOUND_JOB_ID><FILE_NAME>

OUTBOUND FDFG moves files from SFTP '/' directory with file-name <OUTBOUND_JOB_ID><FILE_NAME> to MERCHANT mail-box with <OUTBOUND_JOB_ID><FILE_NAME>

Both JOB_IDs should be different; Ex : INBOUD FLXPI<MID>   OUTBOUND FLXPO<MID>

## Inbound file

CI Create Recipient & Initiate Payment file should have comma separated fields in specific order.

Each field should be quoted with double-quotes (") for data reliability.  

Refer below table for DDP CSV Fields

### Inbound File Mapping Details

| Field Name          | Max Length/Format  | Required      | Comments                 |
| ------------------- | ------------------ | --------------| --------------------------------------------------------------------   |
| `AMOUNT`            | ###.##             | &#10004;      | Amount to be used to initiate payment. ex : 10.88 , 879.00     |
| `CURRENCY`          | ISO 4217 3-letter currency code | &#10004; | Only USD supported at the moment          |
| `MERCHANT_ID`       |  32        | &#10004;  | Merchant who initiated the request.Can be different in non-prod and prod.  |
| `MERCHANT_CUSTOMER_ID OR VENDOR_ID` |  32  | &#10004; | Merchant generated customer-id. Used as unique customer identifier for merchant.  |
| `PAYMENT_TYPE OR DISBURSEMENT_TYPE` |  20  | &#10004; | free text - Type of disbursement - Wages,Claims,promotions,Loans etc"    |
| `RECIPIENT_TYPE`    | 100           | &#10004;  | recipient type. possible values Consumer/Company         |
| `EMAIL`      |  100       | &#10004;  | recipient email address               |
| `FIRST_NAME`     |  100       | &#10004; if RECIPIENT_TYPE = 'Consumer'  | recipient first name should be sent if the recipient type is "Consumer" |
| `LAST_NAME`     |  100       | &#10004; if RECIPIENT_TYPE = 'Consumer'  | recipient last name should be sent if the recipient type is "Consumer" |
| `DATE_OF_BIRTH`    |       | &#10004; if RECIPIENT_TYPE = 'Consumer'  | date of birth should be sent if the recipient is "Consumer". Recipient's Date of Birth. Preferred format YYYYMMDD. Ex : 19891228 |
| `TIN`      | 10      | &#10004; if RECIPIENT_TYPE = 'Company'  | TIN should be sent if the recipient is "Company"  |
| `PHONE_NUMBER`   | 50     | &#10004; if If merchant enforces 2-factor auth | recipient phone number     |
| `PHONE_NUMBER_EXTENSION` |     |     | recipient phone number extension            |
| `STREET`      | 200     | &#10004;  | Recipient's address - street             |
| `CITY`      | 50     | &#10004;  | Recipient's address - city               |
| `STATE`     | 50      | &#10004;  | Recipient's address - state               |
| `POSTAL_CODE`    | 20     | &#10004;  | Recipient's address - postal code            |
| `COUNTRY`     | 20      | &#10004;  | Recipient's address - country             |
| `DBA`      |      | &#10004; if RECIPIENT_TYPE = 'Company' | DBA should be sent if the recipient Company name  |
| `MERCHANT_TRANSACTION_ID OR REFERENCE_ID` | 100    | &#10004; | unique for each request             |
| `CUSTOM_FIELDS`    |      |     | optional and value will be list of key value pairs(key and value pairs will be delimited by colon( : )) and delimited by comma(,) |

### Sample Input File

<!-- theme: success -->
>"amount.value","amount.currency","merchantId","merchantCustomerId","paymentType","recipient.type","recipient.email","recipient.firstName","recipient.lastName","recipient.dob","recipient.tin","recipient.phone.value","recipient.phone.ext","recipient.address.street","recipient.address.city","recipient.address.state","recipient.address.postalCode","recipient.address.country","recipient.dba","merchantTransactionId","customFields.recipient"
>"1.00","USD","526287175883","vmfs","Claims","Consumer","test.maridu@test.com","Test","Maridu","19890628","","","","2900 Westside Pkwy","Alpharetta","GA","30004","USA","","a298fc671a505445","Custom Recipient 1"
>"1.00","USD","526287175883","pvgc","Claims","Consumer","testvjec@test.com","Test","Maridu","19890628","","","","2900 Westside Pkwy","Alpharetta","GA","30004","USA","","a298fc671a505446","Custom Recipient 2"

[Download Input file](?path=assets/FLXPINMM.CI.1654537809.BAT97.1.csv)
<a href="?path=assets/FLXPINMM.CI.1654537809.BAT97.1.csv" download="batch_input.csv">
  HTML download
</a>

## Outbound file

• DDP exports a batch summary report - containing status of each input file record, success/failure, error-description if failed.

Ex: If merchant submits an inbound file with 100 records then outbound file should have 100 entries and their response info.

• Outbound file name will be in FLXPO{FLEX_MID}.{TYPE}.{EPOCH}.{NAME}.{VERSION}_Summary.csv format.

 Ex : If inbound file is FLXPINMM.CI.1654537809.BAT97.1.csv then. outbound file will be FLXPONMM.CI.1654537809.BAT97.1.csv_Summary.csv

### Outbound file Mapping Details

Response file contains multiple records. Each field in a record is separated by pipe (|) delimiter. find the below record format.

CI|MERCHANT_ID|MERCHANT_CUSTOMER_ID|MERCHANT_TRANSACTION_ID|STATUS_CODE|STATUS_DESCRIPTION

| Field Name     | Comments                |
| ------------------- | ------------------------------------------------------------------- |
| `ACTION_CODE`    | identify the action performed initiate payment, cancel payment, etc.|
| `MERCHANT_ID`    | merchant who initiated the request          |
| `MERCHANT_CUSTOMER_ID OR VENDOR_ID` | merchant recipient mapping id        |
| `MERCHANT_TRANSACTION_ID` | Reference ID sent by merchant during payment initiation   |
| `STATUS_CODE`    | This will be code return via api. For success we'll use 00 but for error we'll pass actual error code received from API eg. 400011|
| `STATUS_DESCRIPTION`| For Success value will be SUCCESS and for error it will be reason of API failure. eg. Invalid email id|

### Sample Output File

#### Sample Success
<!-- theme: success -->
>```
>FH|||client123|
>BH|5
>CI|526287175883|bqcmb051|TXNBQC02a2398fc671a505445|00|SUCCESS|
>CI|526287175883|bqcmb0091|TXNBQC03a298fc671a5053446|00|SUCCESS|
>BT|2|2|0
>FT|1|2|0
>```

[Download Output file](?path=assets/FLXPONMM.CI.1654537809.BAT97.1_Summary)

### Sample Failure
<!-- theme: danger -->
>FH|||client1234|  
>BH|23
>CI|526287175883|bqcflex11|BQCUZJ1500|400024|Payment Type not allowed|
>CI|526287175883|bqcflex12|BQCIJZM276|400024|Invalid Email Address|
>BT|2|0|2
>FT|1|0|2

[Download Output file](?path=assets/FLXPONMM.CI.1654537809.BAT97.2_Summary)

## See Also

- [Please refer our FAQ's](?path=docs/faq/faq.md)
