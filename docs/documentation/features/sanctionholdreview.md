# Sanction Hold Review

## Introduction

All cross-border transactions will undergo Office of Foreign Assets Control (OFAC) screening. Currently, this is applicable for debit (Visa/Mastercard) cross-border transactions. The Digital Disbursement Platform (DDP) will send the recipient's information to the OFAC system. Based on their rules, OFAC will respond with a result (pass/fail). If any transaction fails OFAC screening, it will be held for further review (Sanction Hold Review). After investigating the failed transaction, the OFAC team will respond with one of three outcomes (NOT_ENOUGH_INFO, MATCH, NO_MATCH).


## Prerequisites 
 
-The merchant should opt for debit cross-border disbursement.
-Applicable for both API and portal merchants.
-Disburse the payment using a debit cross-border card.


## Use Case Example(s)

The merchant initiates the payment, and while doing so, we have the recipient details. We will share these recipient details with the OFAC system incase of crossborder payment. OFAC will respond based on the recipient's information. The following responses can be received from OFAC based on their review.

    - In case the OFAC response is NOT_ENOUGH_INFO, the payment will be canceled(PC).
    - If the OFAC response is MATCH, the payment will be placed on hold(PH).
    - When the OFAC response is NO_MATCH, the payment will proceed for disbursement(DI).
	

| Description 	| Transaction Status	   | Payment Status 	|
|---------------|--------------------------|--------------------|
| Transaction is under review process - Pending Review | IP – In Process    | PR – Pending Review    |
| Transaction being a close match to an individual/entity listed on the OFAC Specially Designated Nationals (SDN) list | TC – Transaction Complete    | PH – Payment Hold     |

The merchant will not get the funds returned if the payment is on hold (PH). The duration of the Payment Hold is based on the OFAC response.