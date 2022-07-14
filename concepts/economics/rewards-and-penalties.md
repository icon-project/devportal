# Rewards & Penalties

### Rewards

There are 4 types of rewards for earning ICX based on the ICON community's current economic values. These rewards are as follows:

* Block producer rewards
* Delegator / voter rewards
* Contribution Proposal System / Ecosystem Expansion rewards&#x20;
* Cross-chain message relay rewards

#### Reward supply

The supply for rewards are equivalent to the inflation of the [ICX](icx.md) currency on the ICON blockchain [Main Network](../../icon-stack/icon-networks/main-network.md). Inflation is currently set to 3 million ICX per month. The reward types can be thought of as the destinations for currency minted during the inflation process. Newly minted currency can be used to make decisions on how the value of the ICON ecosystem should be directed. In this sense, recipients of newly minted ICX may consider some responsibility associated with receiving rewards so that the ICON ecosystem grows or maintains its value rather than being diluted by the increased currency supply.

#### How are rewards split up?

As mentioned above, the four reward types are: block producer, delegator / voter, Contribution Proposal System / Ecosystem Expansion, and cross-chain message relay

* Block producer rewards are split between the block producers [pro-rata](https://www.investopedia.com/terms/p/pro-rata.asp) based on the percent of bonded delegation
* Delegator / voter rewards are split between delegators / voters pro-rata based on votes given versus total votes
* [Contribution Proposal System / Ecosystem Expansion](https://cps.icon.community) rewards are sent to the singular contract for the Contribution Proposal System / Ecosystem Expansion Fund
* Cross-chain message relay rewards are not currently set (May 26, 2022), as the [ICON Interoperability](../../projects/btp-and-icon-bridge.md) products are not yet launched

| Reward destination                                 | Percentage (%) | Amount (ICX) |
| -------------------------------------------------- | -------------- | ------------ |
| Block producer                                     | 13             | 390,000      |
| Delegator / voter                                  | 77             | 2,310,000    |
| Contribution Proposal System / Ecosystem Expansion | 10             | 300,000      |
| Cross-chain message relay                          | 0 (TBD)        | 0            |

_The above variables may be changed by a_ [_Network Proposal_](../governance/network-proposals.md) _that has been successfully passed upon a vote by the_ [_Delegates_](../governance/delegates.md) _at the time of the vote._

_The rewards variables are made available by the following smart contract and methods._

> Smart contract location: [https://tracker.icon.foundation/contract/cx0000000000000000000000000000000000000000#readcontract](https://tracker.icon.foundation/contract/cx0000000000000000000000000000000000000000#readcontract)
>
> Get all network variables: `getNetworkInfo()`
>
> Get only economics-related variables: `getIISSInfo()`

### Penalties

_Note: Penalties are currently turned off for the ICON blockchain Main Network_

There are 3 types of penalties that result in forfeiting ICX or the opportunity to earn ICX based on participation patterns and community agreements within the ICON ecosystem. Delegates are the only community members who are penalized in the following ways based on their performance:

* Validation penalty
* Low productivity penalty
* Disqualification penalty

#### Validation penalty

This occurs when a specific delegate fails to validate blocks successively for 660 blocks

The penalty is to exclude such delegates from block production during the term. Block production and validation opportunities are forfeited until the next term, creating an opportunity cost.

#### Low productivity penalty

This applies when the Productivity Ratio of a specific delegate has dropped below 85%. The Productivity Ratio is the ratio of actual blocks produced and validated  divided by the number of opportunities to produce and validate a block. Delegates will not be subject to this penalty for their first 86,240 blocks as a block-producing delegate.

The penalty is to disqualify the delegate and burn 6% of the ICX staked to them.

#### Disqualification penalty

This applies when a specific delegate has been disqualified via a _Delegate Disqualification Proposal._

The penalty is to disqualify the delegate in question and burn 6% of the ICX staked to them.
