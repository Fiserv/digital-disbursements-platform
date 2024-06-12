# ANI (VISA ACCOUNT NAME INQUIRY)                            

## Introduction
Account Name Inquiry (ANI) is a feature that allows clients to verify that the recipient's name matches the name on card they are using to disburse funds through a debit card (Visa/Master). In simpler terms, it helps ensure that the name on the card being used for a transaction matches the name of the person who will be receiving the funds.


## Use Case Examples

Based on the user's preference, we can configure the system to either disburse or not disburse the funds. There are three categories that can be used with the ANI feature: Full Match (FM), Partial Match (PM), and No Match (NM).

For instance, if a customer only wants to disburse funds when there is a Full Match (FM) between the recipient's name and the name on card, we can configure the system to decline any payments that have a Partial Match (PM) or No Match (NM). On the other hand, if the customer wants to disburse funds even with a Partial Match (PM), we can adjust the configuration accordingly.

In summary, the system's decision to disburse or decline funds can be customized based on the user's preferences and the categorization of the ANI feature, allowing for flexibility in managing transactions.

ANI Rule Details

| S.NO | Account Owner Given Name | Target First Name | First Name Match Decision | Account Owner Middle Name | Target Middle Name | Middle Name Match Decision | Account Owner Last Name | Target Last Name | Last Name Match Decision | Account Name Match Decision |
|------|--------------------------|-------------------|---------------------------|---------------------------|--------------------|----------------------------|-------------------------|------------------|--------------------------|-----------------------------|
| 1    | ACCONTE                  | ACCOUNT           | FULL MATCH NAME           | ACCOUNT                   | ACCOUNT            | FULL MATCH INQUIRY         | ACCOUNT                 | ACCOUNT          | FULL MATCH FULL MATCH    | FULL MATCH                  |
| 2    | ACCOUNT                  | ACCONTE           | NO MATCH NAME             | ACCOUNT                   | ACCOUNT            | NO MATCH INQUIRY           | ACCOUNT                 | ACCOUNT          | FULL MATCH FULL MATCH    | FULL MATCH                  |
| 3    | ACCOUNT                  | ACCOUNT           | FULL MATCH NAME           | NAME                      | NAME               | FULL MATCH INQUIRY         | NAME                    | NAME             | FULL MATCH FULL MATCH    | FULL MATCH                  |
| 4    | ACCONT                   | ACCOUNT           | PARTIAL MATCH             | NAME                      | NAME               | FULL MATCH INQUIRY         | INQUIRY                 | INQUIRY          | FULL MATCH FULL MATCH    |                             |
| 5    | ACCOUNT                  | ACCOUNT           | FULL MATCH NAMEE          | NAME                      | PARTIAL MATCH      | INQUIRY                    | INQUIRY                 | INQUIRY          | FULL MATCH FULL MATCH    |                             |
| 6    | ACCOUNT                  | A                 | NO MATCH NAME             | NAME                      | NAME               | NO MATCH INQUIRY           | INQUIRY                 | INQUIRY          | NO MATCH INQUIRY         | INQUIRY INQUIRY INQUIRY     |
| 7    | ACCOUNT                  | A                 | FULL MATCH NAME           | NAME                      | NAME               | FULL MATCH INQUIRY         | INQUIRY                 | INQUIRY          | FULL MATCH INQUIRY       | FULL MATCH FULL MATCH       |
| 8    | ACCOUNT                  | A                 | PARTIAL MATCH             | NAME                      | NAME               | PARTIAL MATCH              | INQUIRY                 | INQUIRY          | PARTIAL MATCH            | FULL MATCH FULL MATCH       |
| 9    | ACCOUNT                  | ACCOUNT           | FULL MATCH                | NAME                      |                    | INQUIRY * Paypal           | * Paypal                | NO MATCH         | NO MATCH NO MATCH        |                             |



## API Changes 
NA

## Troubleshoooting
NA

