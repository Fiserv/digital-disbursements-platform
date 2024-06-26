# Visa Plus

## Introduction

The Visa Plus feature empowers clients to activate Visa+ as a payout method on DDP for both Portal and Direct Disbursement merchants. This functionality facilitates the implementation of the Visa+ B2C (Bussiness-to-Consumer) use case, enabling participating payout merchants to provide consumers with the choice to link their Visa+ payname to receive payments seamlessly. tilizing Visa+ instead of Visa provides enhanced security, increased convenience, personalized options, global acceptance, mobile integration, and dedicated customer support, surpassing the standard services offered by Visa.

## Use Case Example(s)

This use case demonstrates the flow of payment initiation, verification, funds transfer, and settlement within the Visa+ payment ecosystem, ensuring a secure and efficient transaction experience for both merchants and recipients.

1. *Payment Initiation:*
   - The merchant commences a payment to the recipient.
2. *Recipient Acceptance:*
   - The recipient opts for the Visa+ payment method by providing their payname.
3. *Verification Process:*
   - DDP verifies the payname with Visa, addressing any discrepancies as needed.
4. *Account Details Provision:*
   - Visa furnishes the recipient's 16-digit payment account reference, customer data, and an expiration date associated with the Visa+ payname.
5. *Funds Transfer Request:**
   - DDP forwards a push funds request to Visa for processing.
6. *Transaction Status Update:*
   - DDP informs the recipient of the funds transfer status. If successful, the process moves to settlement.
7. *Settlement Procedure:*
   - Successful transactions lead to funds settlement between the merchant sponsor and recipient wallets following standard Visa settlement protocols.

