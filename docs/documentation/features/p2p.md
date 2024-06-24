# P2P

## Introduction

The Person-to-Person (P2P) feature facilitates direct fund transfers between clients within the DDP ecosystem. This functionality is available for both Portal and Direct Disbursement merchants. By enabling P2P transactions, participating users experience faster and more efficient payment processing.

## Use Case Example(s)

P2P Payment Process:
- Step 1: AFT Debit from Sender:
  When a person initiates a P2P payment, the sender’s account is debited through an Automated Funds Transfer (AFT). This step ensures that the funds are available for the transaction.
- Step 2: OCT Credit to Recipient:
   Once the sender’s account is debited, the recipient receives an Online Credit Transfer (OCT). This credit is reflected in the recipient’s account, completing the P2P transaction.

The DDP does not facilitate the AFT directly; instead, it relies on the merchant to perform the AFT.
The DDP's role is to forward sender details to VISA and Mastercard (VISA MC) for processing.
Both the sender and recipient are expected to provide card data within a TransArmor (TA) Token.

Initially, only USD (United States Dollar) is supported as the transaction currency.
However, the DDP should eventually support cross-border transactions with non-US issuers.
