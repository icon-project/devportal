# Staking

### What is staking?

As defined by the [ICON glossary](https://icon.community/glossary/staking/):

> Locking up tokens in contribution to a blockchain entity. Typically staking yields rewards

On ICON, staking has two different meanings, depending on the context. The first context is of a community member staking to a [delegate](../governance/delegates.md). The second context is of a delegate staking to validate a new block.

#### Staking to a Delegate

For a community member, staking refers to the act of supporting a delegate or delegate candidate by designating some amount of locked up [ICX](icx.md) for that candidate.

#### Staking for block validation

For a delegate, staking refers to the act of locking up some ICX during the process of validating the next block to be added to the blockchain.

### What are the benefits of staking?

Staking is a way to support the ICON ecosystem as well as to earn ICX. Staking to a delegate increases diversity in the ICON ecosystem. The delegate candidates with the most ICX staked to them are voted in as delegates, so the more users that stake, the more representative the delegates are of the community. Additionally, there is a system of [financial rewards](rewards-and-penalties.md) associated with staking. This is a way to encourage user participation.

### How to stake?

Staking to a delegate can be done through the [Hana wallet](https://chrome.google.com/webstore/detail/hana/jfdlamikmbghhapbgfoogdffldioobgl/related) or through the ICON JSON-RPC API directly using the [`setStake`](../../icon-stack/client-apis/json-rpc-api/#setstake) and [`setDelegation`](../../icon-stack/client-apis/json-rpc-api/#setdelegation) functions.
