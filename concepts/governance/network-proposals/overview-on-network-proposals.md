# Overview on Network Proposals

## What is a network proposal?

A _network proposal_ is a mechanism for governance where [Delegates](../delegates.md) can submit ideas for changes that they would like to see implemented in the ICON Network. These ideas are recorded and voted upon _on-chain_, which means that the proposal document and the voting results can be seen publicly by all.&#x20;

### Common proposal types

There are four different types of Network Proposals that greatly affect the system; _Malicious Smart Contract Proposal_, _Delegate Disqualification Proposal_, _STEP Price Proposal_ and _Text Proposal_, defined as follows:

* Malicious smart contract proposal : a proposal to shut down a specific smart contract
* Delegate disqualification proposal : proposal to remove a Delegate from the network
* STEP price proposal : proposal to adjust base transaction fees
* Text proposal : a proposal to vote on an idea. No action is actually taken

### Examples

See the [community forum section on Governance](https://forum.icon.community/c/governance/22) for examples of Network Proposals. Three examples are briefly highlighted below. Note that some of the highlighted proposals use the deprecated term _P-Rep_ instead of _Delegate_:

#### Example 1: Cap P-Reps’ ICX delegation share to 2.5% of the total delegated ICX

[Forum Link](https://forum.icon.community/t/cap-p-reps-icx-delegation-share-to-2-5-of-the-total-delegated-icx/2333)

Proposer: Nym

> The goal of this post is to start a conversation with the community and P-Reps. It is not a finalized proposal, but I hope that a fully specified IIP will be written following the output of this discussion.
>
> This is not a proposal from the ICON Foundation. All ideas shared below are the result of aggregating fragmented short conversations with the community and P-Reps, as well as our own understanding of the incentives imbalance currently in place in the network.
>
> Special thanks to [@Benny\_Options](https://forum.icon.community/u/benny\_options) for drafting this text and laying down these ideas in clear.
>
> #### Introduction <a href="#introduction-1" id="introduction-1"></a>
>
> Decentralization is one of the main goals of the ICON network. Unfortunately today the network is suffering from a strong vote stagnancy, and voting is a core concept of a DPoS consensus.
>
> This is not a good thing for the network because:
>
> * It discourages new P-Rep candidates to participate and compete and so the network does not have many nodes securing it
> * While every nodes have very similar costs and produce similar amount of work (specifically, producing/validating blocks and storing a copy of the blockchain), their reward is vastly unequal
> * It concentrates most of the delegated ICX into a few nodes at the top, making some nodes unprofitable while others excessively profitable
>
> #### Proposed idea <a href="#proposed-idea-2" id="proposed-idea-2"></a>
>
> Impose a maximum percentage delegation per P-Rep node of 2.5%. `setDelegate` function will first check the percentage of votes held by the P-Reps receiving delegation. If the P-Rep is above 2.5% of all delegation, setDelegate will revert.
>
> In English terms, it means that a P-Reps cannot receive additional ICX delegation, if its current delegation represents more than 2.5% of the total delegated ICX on the network.
>
> #### Goals <a href="#goals-3" id="goals-3"></a>
>
> #### More quality nodes <a href="#more-quality-nodes-4" id="more-quality-nodes-4"></a>
>
> By limiting the max delegation to 2.5%, it redirects newly delegated ICX to look for another node. Therefore, sub and candidates P-Reps will receive more delegation. This encourages more ICONists to run a P-Rep node as it’s easier to compete with top nodes. There would be a minimum of 40 quality nodes securing the network as a result (2.5% \* 40 = 100%). It also indirectly encourages ICONists to select their P-Rep in a more attentive manner, rather than choosing the top nodes by default.
>
> #### Better vote spreading <a href="#better-vote-spreading-5" id="better-vote-spreading-5"></a>
>
> Delegated ICX will be spread more evenly across different nodes. Therefore it increased decentralization (see the next section for a counter argument).
>
> #### Fair reward between P-Reps <a href="#fair-reward-between-p-reps-6" id="fair-reward-between-p-reps-6"></a>
>
> There is no economic, business or security purpose to having one node earning significantly more money than another node for the same amount of work and expenses. All P-Reps’ costs and work are very similar when it comes to running a node, therefore the reward for doing so should also be similar.
>
> Upon reaching the cap, a P-Rep operator could deploy another node, incurring additional cost to access additional revenue. P-Reps who are looking for additional income could also build products or services on top of the network for this purpose.
>
> It also makes ROI much easier to predict over a long period of time. P-Reps (and those interested in deploying a node) would compare the income generated by reaching the delegation cap to the cost of running the infrastructure.
>
> #### Potential issues <a href="#potential-issues-7" id="potential-issues-7"></a>
>
> #### Service disruption <a href="#service-disruption-8" id="service-disruption-8"></a>
>
> * sICX is now a service that delegates to p-rep nodes. This service would need to be adjusted.
> * Wallet UX will have to be updated to provide a smooth voting experience without voting failure.
>
> #### Sybil incentives <a href="#sybil-incentives-9" id="sybil-incentives-9"></a>
>
> * Top nodes will deploy multiple nodes in order to continue maximizing their reward, which could become a threat to decentralization if the top 22 P-Reps (main) are controlled by a few entities.
>
> \===============
>
> Please share your feedback on this topic.\
> Thank your for reading.

#### Example 2: ICON 2 - Monthly Reward Fund Setting Proposal

[Forum Link](https://forum.icon.community/t/icon-2-monthly-reward-fund-setting-proposal/2431)

Proposer: Nym (on behalf of ICON Foundation)

> The ICON Foundation submits a Network Proposal to set monthly reward fund to 3000000 ICX
>
> [https://tracker.icon.foundation/proposal/0x5fb632c4378adf1a52815af5a5f6294563d9d1c2403b86cc62d2414e1ec048fd 6](https://tracker.icon.foundation/proposal/0x5fb632c4378adf1a52815af5a5f6294563d9d1c2403b86cc62d2414e1ec048fd)
>
> Feel free to discuss this and cast your vote when you’re ready. Thank you.

#### Example 3: ICON 2.0 Penalty System

[Forum Link](https://forum.icon.community/t/icon-2-0-penalty-system/1296)

Proposer: _Benny\_Options_

> On ICON 1.0 we never implemented any sort of burning penalty, however, we have plans to implement a burning penalty on the bond of the P-Rep. Please see the details below and let us know if you have any strong feedback you would like us to consider:
>
> **Validation Penalty** - This is currently live on ICON 1.0 and it will be the same on ICON 2.0. After missing 660 blocks, a P-Rep will be removed from block production until the next term. There is no burn associated with this and there will be no burn associated with this.
>
> **Accumulated Validation Penalty** - This will be a new penalty. If a P-Rep receives 5 Validation Penalties within their last 30 chances, where a chance is defined as serving 1 term as a block producer, the P-Rep will be removed from block production and 20% of their bond will be burned. This penalty will be enforced on every validation penalty from 5 or more within the last 30 chances. In other words:
>
> If a P-Rep get’s 5 validation penalties in the last 30 chances: burn 20% of current bond\
> If a P-Rep get’s 6 validation penalties in the last 30 chances: burn 20% of current bond\
> If a P-Rep get’s 7 validation penalties in the last 30 chances: burn 20% of current bond\
> etc. etc. etc.
>
> The goal here is to remove a down node from block production since ranking is based on the amount of bond posted. If you are a Main P-Rep suffering this penalty over and over again, eventually you will drop to Sub P-Rep status as your bond continues to be slashed.
>
> Overall, it is important for network stability to remove downed nodes from the block production rotation, and slashing the bond until the node drops out of rotation is a less harsh way of doing this versus complete disqualification.
>
> **Governance Slashing** - this will stay as we agreed in IISS 3.0, minus the slashing associated with the Contribution Proposal System since the CPS management is no longer mandatory as described [here 12](https://forum.icon.community/t/minor-modification-to-the-contribution-proposal-system/1279).

### Resources:

* [https://github.com/icon-project/governance2](https://github.com/icon-project/governance2#registerproposal)
