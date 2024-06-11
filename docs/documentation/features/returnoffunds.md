# Return Of Funds                            

## Introduction

Return Of Funds is a feature that fecilitates specific Singapore requirement that states a merchants funds once deposited, must be returned within 3 days if the payouts that processed over those 3 days do not deplete the full deposited amount.


## Use Case Example(s)

DDP will need to create a process to calculate if deposited funds by Merchant need to be returned based on FIFO(First In First Out) Model. DDP would run a job at the end of each reporting day localized for Singapore processing time to calculate if funds need to be returned that day. If funds need to be returned, DDP will return the funds in one of 2 ways.
    1) Via creating a payout to debit card that the merchant provides 
    2) manual return process through settlement ops Crediting an account back to the merchant.

The Return parameter for Singapore is 3 calendar days, where the day of deposit is Day 1 and the Return is due on Day 4.

So, for funds deposited on Monday, the expected Return day would be Thursday, unless Thursday is a bank holiday (BH), in which case the funds would be returned on the next banking day. 

The Return parameter should be configured at the sponsor bank level. So that it will be applicable to all the merchants onboarded with this bank.

We will use Bank table country config column for the configuration. These are additional attributes in the same object.

The holiday calendar of Singapore has to be configured. We have to make use of the existing WorkingDayCalculator of US holiday calendar from disbursement-utils and make changes to accommodate Singapore holiday calendar.

The Return of Funds in DDP is a separate process which will be running daily EOD. There is no change in the existing payout process.


## API Changes 
NA
<!-- Parameter Details Try Out - link  to new request  -->


## Troubleshoooting
NA
<!--Error Codes, 
FAQs / Common Issues -->
