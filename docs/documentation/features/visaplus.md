# Visa Plus

## Introduction

The Visa Plus feature allows clients to enable Visa+ as a payout method on DDP for both Portal and Direct Disbursement merchants. This enables the Visa+ B2C use case, allowing participating payout merchants to offer consumers the option to add their Visa+ payname to receive payments.

## Use Case Example(s)

The merchant initiates a payment to the recipient. The recipient accepts the payment via the Visa+ payment method by providing their payname. The DDP must confirm the payname with Visa and resolve any discrepancies.

Visa provides the 16-digit payment account reference linked to the recipient’s Visa+ payname, along with customer data and an expiration date. The DDP then sends a push funds request to Visa.

The DDP responds with the status of the funds transfer to the recipient. If successful, funds settlement occurs between the merchant sponsor and the recipient wallets through regular Visa settlement.
