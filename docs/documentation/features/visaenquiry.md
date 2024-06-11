# ANI (VISA ACCOUNT NAME INQUIRY)                            

## Introduction
Account Name Inquiry (ANI) is a feature that allows clients to verify that the recipient's name matches the nameOnCard they are using to disburse funds through a debit card (Visa/Master). In simpler terms, it helps ensure that the name on the card being used for a transaction matches the name of the person who will be receiving the funds.


## Use Case Examples

Based on the user's preference, we can configure the system to either disburse or not disburse the funds. There are three categories that can be used with the ANI feature: Full Match (FM), Partial Match (PM), and No Match (NM).

For instance, if a customer only wants to disburse funds when there is a Full Match (FM) between the recipient's name and the nameOnCard, we can configure the system to decline any payments that have a Partial Match (PM) or No Match (NM). On the other hand, if the customer wants to disburse funds even with a Partial Match (PM), we can adjust the configuration accordingly.

In summary, the system's decision to disburse or decline funds can be customized based on the user's preferences and the categorization of the ANI feature, allowing for flexibility in managing transactions.

ANI Rule Details

![alt text](../../../../assets/images/anirules.png)


## API Changes 
NA

## Troubleshoooting
NA

