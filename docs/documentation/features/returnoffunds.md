# Return Of Funds                            

## Introduction

The Return of Funds is a feature that facilitates a specific requirement for merchants. This requirement states that a merchant's funds, if the payouts processed during that time do not exhaust the full deposited amount, the system must reimburse the remaining funds to the merchant.


## Use Case Example(s)

DDP will need to create a process to calculate whether the funds deposited by the Merchant should be returned, following the FIFO (First In First Out) model. At the end of each reporting day, localized for Merchant processing time, DDP will run a job to determine if funds should be returned on that day. If a return is necessary, DDP will initiate the refund using one of the following methods:

By creating a payout to the debit card provided by the merchant.
Through a manual return process via settlement ops by crediting an account back to the merchant.
The Merchant is expected to return the funds within 3 calendar days, with the day of deposit considered as Day 1 and the return being due on Day 4.
