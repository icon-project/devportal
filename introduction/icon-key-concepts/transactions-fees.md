---
description: >-
  Yellow paper:
  https://www.slideshare.net/helloiconworld/yellowpaper-icon-transaction-fee-and-score-operation-policyenv10
---

# Transactions Fees

* ICON network offers a reasonable yet sophisticated fee system taking the complexity of each transaction into consideration.
* Fees are incurred when executing a smart contract on the ICON network, and the fees are determined based on the total amount of resources used to execute the transaction. **Step** is the unit of the resource-usage measurement.
* The SCORE transaction fee on the ICON mainnet is governed by three principles: [Transaction fee policy](transactions-fees.md#transaction-fee-policy), [Virtual Step,](transactions-fees.md#virtual-step) and [SCORE termination policy](transactions-fees.md#score-termination-policy). 
* Blockchain transaction fee hindered the mass service adoption. ICON proposes a new Transaction fee payment policy to alleviate the usability issue. 

### Transaction Fee Policy

#### Step

* Step is the unit of measurement for transaction fees on the ICON network. The amount of Step required is measured by the computational resources required to execute the transaction. 
* Initial setup: 1 ICX = 100,000,000 Step
* If the ICX price goes too high or too low, the Step price becomes fluctuating as well, this may demotivate the ICONist to use ICON network; therefore, in order to have stable Step price, the exchange rate between ICX and Step can be determined and updated by Representatives.

#### Items requiring transaction fees

* There are three categories that charge transaction fees:
  1. Smart Contract Function Usage
  2. Blockchain Database Usage
  3. Amount of Transaction Data

#### Step calculation formula

* **Step = max \( ∑ βi Si + C, C \)**
  * S refers to the amount of usage of the respective variable, β denotes the weight, and C denotes a constant minimum fee. Constant value C prevents the total sum of transaction fee from being negative due to the variables with negative weight \(e.g., ScontractDestruct and Sdelete\) . Any transaction with a `stepLimit` value lower than C will not be processed. 

\[Table 1\] Transaction Fee Variables

| Variable | Description |
| :--- | :--- |
| ScontractCall | Number of times to call the smart contract function |
| ScontractCreate | Number of times to call the smart contract code generation function |
| ScontractUpdate | Number of times to call the smart contract code update function |
| ScontractDestruct | Number of times to call the smart contract code delete function |
| ScontractSet | Size of generated/updated smart contract code \(Bytes\) |
| Sset | Size of data newly set in the state database \(Bytes\) |
| Sreplace | Size of data to be updated in the state database \(Bytes\) |
| Sdelete | Size of data to be deleted in the state database \(Bytes\) |
| Sinput | Size of input data included in transaction \(Bytes\) |
| SeventLog | Size of event log as a result of transaction \(Bytes\) |
| C | Default value that is charged each time transaction is executed |

\[Table 2\] Transaction Fee Weight

In ICON 2, the fee policy will be updated. Please refere to the following post for reference [https://forum.icon.community/t/icon-2-0-score-fee-policy-update-draft-proposal/2194/5](https://forum.icon.community/t/icon-2-0-score-fee-policy-update-draft-proposal/2194/5).

| Weight | Description |
| :--- | :--- |
| βcontractCall | 25,000 |
| βcontractCreate | 1,000,000,000 |
| βcontractUpdate | 1,600,000,000 |
| βcontractDestruct | -70,000 |
| βcontractSet | 30,000 |
| βset | 320 |
| βreplace | 80 |
| βdelete | -240 |
| βinput | 200 |
| βeventLog | 100 |
| C | 100,000 |

#### Step usage limit

To ensure stable operation of the ICON network, the maximum available amount of Step that can be used for a single transaction is limited to 2.5 billion Step. If the transaction does not close at the time when the maximum Step limit is reached, the transaction is suspended and the fees are exhausted. The current value of the maximum Step limit is as follows, and it can be updated when the policy of SCORE execution environment changes.

\[Table 3\] Maximum Step Limit

| Type | Max Step Limit |
| :--- | :--- |
| Invoke \(Transaction\) | 2,500,000,000 |

#### Transaction fee payment policy

There are certain applications that are awkward to charge end-users the fees for their activity on the applications. In such cases, the transaction fee should be charged to the SCORE owner.

* Entity Responsible for Fees
  * By default, the user \(EOA who initiates the transaction\) pays the transaction fees. However, transaction fees can be partly paid by the SCORE owner \(the service provider\) if the SCORE owner decides to do so to attract more users.
* Fee Sharing Ratio
  * The fee sharing ratio allows SCORE owners to attract more customers by paying some or all of the customers' transaction fees. The owner can set the fee payment ratio between the owner and the customer at the time of smart contract registration. 
  * The ratio can be set from 0% to 100%. If the owner's paying ratio is more than 0%, the owner must deposit a certain amount of ICX in the corresponding SCORE, and when a transaction occurs, the fee will be deducted from the [Virtual Step](transactions-fees.md#virtual-step) associated to the SCORE or from the ICX deposited to the SCORE. 

\[Table 4\] Transaction Fee sharing Ratio Example

| Case | Owner | Customer | Sum |
| :--- | :--- | :--- | :--- |
| 1 | 100% | 0% | 100% |
| 2 | 50% | 50% | 100% |
| 3 | 0% | 100% | 100% |

### Virtual Step

#### \* In ICON 2, virtual step bonus has been removed. SCORE can still enable fee sharing but only the deposited ICX will be used. There is no extra virtual step generated anymore.

* Virtual Step is the new fee system of the ICON network for the SCORE owner.  
* Virtual Step is generated every month in proportion to the quantity of ICX deposited and the duration of the deposit period determined. The SCORE owner can pay fees with the generated Virtual Step. 
* This system can offset the fee burden of SCORE owners and motivate owners to pay 100% transaction fee and attracts more customers to use their SCORE.

#### Virtual Step features

* Function 
  * Transaction fees for running SCORE can be paid with Virtual Step.
  * Virtual Step and Step are converted with a 1:1 ratio.
  * As soon as the ICX deposit agreement is executed, Virtual Step is generated as detailed in \[Table 5\]. 
  * Virtual Step is generated every 1,296,000 blocks, which is 1 month, and the unused Virtual Step will be expired. 
* Calculation 
  * When depositing ICX in a SCORE, set the deposit amount and the deposit duration.
  * When depositing a certain amount of ICX in a smart contract for m month block period,  fm rate of Virtual Step is generated for every month \( 1M=1,296,000 block\). fm is defined as the monthly receive rate of ICX deposited as shown in \[Table 5\]\) 
  * The deposit period can be set from a minimum of 1 month to a maximum of 24 months. 
  * The deposit amount can be set from a minimum of 5,000 ICX to a maximum of 100,000 ICX. 
  * The minimum and maximum deposit duration, deposit amount, and Virtual Step production rate can be adjusted through consensus among the Representatives.
* Policy 
  * Generated Virtual Step cannot be transferred or traded. 
  * Virtual Step that has not been consumed within the month are extinguished. 
  * If Virtual Step is exhausted, the required fee is deducted from the deposited ICX.
* Amendment to the Agreement 
  * Multiple deposits in one SCORE can be made, but the existed deposit amount and period cannot be changed. 
  * Making a new deposit agreement is available even if the existing agreement still contains residual Virtual Step, and the amount of Virtual Step produced by each deposit is calculated independently.

#### Virtual Step calculation

* **fm = fm-1**_**\(rbonus\)**_**\(1 + rlongterm bonus\) ....... m &gt; 1**  
  * Where,

    **f1 = 0.08**

    **rbonus = 0.01**        

    **rlongterm bonus = 0.2**,  if m = 6,12,18, or 24,  otherwise 0

The SCORE operator can pay the transaction fees using the Virtual Step generated by depositing a certain amount of ICX for a certain period of time. The monthly rate of Virtual Step generated is designed to have a positive correlation with the ICX deposit period \(refer to \[Table 5\]\).

The amount of Virtual step that corresponds to the monthly receive rate of the deposit will be given every month, which is every 1,296,000 block. The deposit amount can range from 5,00 ICX to 100,000 ICX, and the deposit period can range from 1,296,000 blocks \(approximately 1 month\) to 31,104,000 blocks \(approximately 24 months\) with each interval being 1,296,000 blocks.

For examples,

* Deposited 10,000 ICX and signed for 1 month, receive 80,000,000,000 Virtual Step per month, which is equivalent to 8.00% of the deposit amount 10,000, which is 800 ICX. 
* Deposited 10,000 ICX and signed for 24 months, receive 202,400,000,000 Virtual Step per month, which is equivalent to 20.24% of the deposit, or  2,024 ICX. 

\[Table 5\] Virtual Step Calculation Ratio according to ICX Deposit Period \(Unit : %, 1M = 1,296,000 blocks\)

| Deposit Period \(M\) | Monthly Receive Rate |
| :--- | :--- |
| 1 | **8.00%** |
| 2 | 8.08% |
| 3 | 8.16% |
| 4 | 8.24% |
| 5 | 8.32% |
| 6 | **10.09%** |
| 7 | 10.19% |
| 8 | 10.29% |
| 9 | 10.40% |
| 10 | 10.50% |
| 11 | 10.60% |
| 12 | **12.73%** |
| 13 | 12.85% |
| 14 | 12.98% |
| 15 | 13.11% |
| 16 | 13.24% |
| 17 | 13.37% |
| 18 | **16.05%** |
| 19 | 16.21% |
| 20 | 16.37% |
| 21 | 16.54% |
| 22 | 16.70% |
| 23 | 16.87% |
| 24 | **20.24%** |

#### SCORE termination policy

Termination of Deposit for Early Withdrawal

* SCORE operators will incur a penalty if they need to withdraw the deposited ICX prior to the end of the predetermined period. This fee can exceed the Virtual Step generated and result in losing some of the deposit. The penalty is the sum of Virtual Steps used during the given contract period. 

### Reference

* [How to estimate required step](../../references/how-to/estimate-required-step.md) 
* [Fee Sharing and Virtual Step]()

