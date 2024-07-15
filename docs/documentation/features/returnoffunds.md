# Return Of Funds                            

## Introduction

Some countries have regulations that require an entity to return funds to the original owner, if the funds have not been used for the intended purpose within a specified period of time. For Singapore, Direct Disbursement Platform (DDP) will be required to return any unused funds to a merchant within 3 banking days. To accomplish this, DDP will need to keep track of each individual deposit so that it can adhere to the Singapore mandates. The initial launch is for Singapore; however, the design will accommodate any region with similar requirements.

DDP needs to create a process to calculate whether deposited funds by the merchant need to be returned based on the First In, First Out (FIFO) model. DDP will run a job at the end of each reporting day, localized for Singapore processing time, to determine if funds need to be returned that day. If funds need to be returned, DDP will do so in one of two ways: 

1. by creating a payout to the debit card provided by the merchant.
2. through a manual return process handled by settlement operations, crediting the account back to the merchant.

## Prerequisites 

- The configuration should be done at the sponsor bank level so that it applies to all merchants onboarded with this bank. We will utilize the 'Bank' table's 'countries_config' column for this configuration as shown below. These additional attributes are part of the same object.

		{"enableReturnOfFunds":true, "returnOfFundsDays":3}
- This will only be for Direct Disbursement Merchants, does not work with Portal.

## Use Case Example(s)

In Singapore, the return period is 3 calendar days. The day of deposit is considered 'Day 1,' and the return is due on 'Day 4'. If funds are deposited on Monday, the expected return day would be Thursday. However, if Thursday is a bank holiday (BH), the funds will be returned on the next banking day.

Assuming that Merchant A deposited $5000 on March 4th (with a return of funds date set for March 7th) and processed $4000 in payouts on the same day, the remaining amount would be $1000. Subsequently, on March 5th, Merchant A submitted another deposit of $5000 and processed $3000 in payouts. As there were no transactions on March 6th, the amount due to be returned to Merchant A on the scheduled fund return date would be $3000.
