# Consensus mechanism

### What is blockchain consensus?

From wikipedia, [consensus](https://en.wikipedia.org/wiki/Consensus\_\(computer\_science\)) in the context of computer science is defined as follows:

> A fundamental problem in [distributed computing](https://en.wikipedia.org/wiki/Distributed\_computing) and [multi-agent systems](https://en.wikipedia.org/wiki/Multi-agent\_system) is to achieve overall system reliability in the presence of a number of faulty processes. This often requires coordinating processes to reach **consensus**, or agree on some data value that is needed during computation.

In the more focused context of a [decentralized](https://icon.community/glossary/decentralized/) network, consensus is the process by which a group of machines all agree on the same state of a system. For ICON, the state refers to the global state of the blockchain.

There are many different ways for a decentralized network to form a consensus, including the two most popular mechanisms: [Proof-of-Work](https://en.wikipedia.org/wiki/Proof\_of\_work) and [Proof-of-Stake](https://en.wikipedia.org/wiki/Proof\_of\_stake). The Proof-of-Work mechanism is the much more well-tested of the two. Typically, when either of those two terms are used, they refer to a consensus-based system where anyone can become a validator.

### Delegated Proof of Stake (DPOS)

#### What is delegation?

ICON uses a form of the Proof-of-Stake algorithm developed by [Daniel Larimer](https://en.wikipedia.org/wiki/EOS.IO) called [_Delegated Proof-of-Stake_](https://icon.community/glossary/delegated-proof-of-stake/). The key distinction between the two is that the _delegated_ version does not let any group join the pool of [validator nodes](../network/validator-nodes.md). The only valid validators are voted in by the community and are known as [delegates](../governance/delegates.md). They are users who are voted in by the community because they are seen as responsible and reliable for the purpose of processing transactions and validating blocks to add to the blockchain.

#### Mechanism

Each time a new block is to be validated may be referred to as a _round_. In each round, a validator is chosen from among the delegates. That validator is responsible for submitting a proposal for a new block. This is done in the following steps:

1\) Process the next group of [transactions](transactions.md)

2\) Perform a hash on the resulting information

3\) Store those hashes in a [block](blocks.md)

4\) Pass that block to the other validators in the ICON [network](../network/)

5\) Receiving the votes from the other validators on whether the block is valid or not

If the block is valid, then it is added to the blockchain and the validators are [rewarded](../economics/rewards-and-penalties.md) appropriately. If that block is invalid, then another validator is chosen, and the validators are penalized appropriately. If a bad block is voted as valid, then the validator who staked their ICX by voting to add that block could potentially lose all of their stake if the other validators eventually determine that the block is in fact invalid.
