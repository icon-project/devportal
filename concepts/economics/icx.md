# ICX

### What is a cryptocurrency? <a href="#what-is-a-cryptocurrency" id="what-is-a-cryptocurrency"></a>

A description of _cryptocurrency_ is summarized nicely by the [Ethereum Developer Documentations](https://ethereum.org/en/developers/docs/intro-to-ether/):

> A cryptocurrency is a medium of exchange secured by a blockchain-based ledger.
>
> A medium of exchange is anything widely accepted as payment for goods and services, and a ledger is a data store that keeps track of transactions. Blockchain technology allows users to make transactions on the ledger without reliance upon a trusted third party to maintain the ledger.
>
> The first cryptocurrency was Bitcoin, created by Satoshi Nakamoto. Since Bitcoin's release in 2009, people have made thousands of cryptocurrencies across many different blockchains

### What is ICX? <a href="#what-is-ether" id="what-is-ether"></a>

**ICX** is the basic currency unit of the ICON Network. The fundamental economic activities that take place on the ICON Network are based on the functionality of ICX. To use the language from the description of cryptocurrency above, ICX is a medium of exchange for users on the ICON Network. The three ways that ICX is exchanged are "Minting", "Staking", and "Transferring".

The price of ICX is correlated with the value of computational power on the ICON Network. The actual _cost to use_ the computational power on the ICON Network is called [_STEP_](https://app.gitbook.com/o/-McMmnDTovEDTOOjq4sG/s/-McMmuxKCCgDfbGouV8t-887967055/\~/changes/jerz7SXXKg0b6WUC1rjf/concepts/economics/step), and is described in its own section of the documentation.

The smallest denomination of ICX is loop. 1 ICX = 1,000,000,000,000,000,000 loop (10^18).

### Minting ICX <a href="#minting-ether" id="minting-ether"></a>

Minting is the process in which by which new ICX is added to the ICON ledger. ICX is minted with every block produced. The current inflation rate is set to 36 million new ICX every year. Newly minted ICX is sent to the public treasury, where it can be claimed by the following users and public funds:

* Validator nodes
* Stakers
* [Contribution Proposal System / Ecosystem Fund](https://icon.community/glossary/contribution-proposal-system/)

### Staking ICX

[Staking](https://icon.community/glossary/staking/), in the general context of cryptocurrency, is defined as follows by the ICON Community Glossary:

> Locking up tokens in contribution to a blockchain entity. Typically staking yields rewards

In the context of the ICON Network, staking is an action taken by both general users and delegates.

General users -- stake tokens to vote for delegates.

Delegates -- stake tokens to prove that a new block should be considered valid and thus should be added to the blockchain.

### Transferring ICX <a href="#transferring-ether" id="transferring-ether"></a>

Transactions on ICON Network are denominated in ICX, and they send from the "sender" address to the "receiver" address.

The recipient address can also be a [smart contract](../../icon-stack/smart-contracts/).

[More on transactions](../computational-utilities/transactions.md)

### Querying ICX <a href="#querying-ether" id="querying-ether"></a>

Users can query the ICX balance of any [account](../computational-utilities/accounts.md) by inspecting the "balances" database at the address of the account.
