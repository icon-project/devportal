# Smart contracts

### What is a smart contract? <a href="#what-is-a-smart-contract" id="what-is-a-smart-contract"></a>

A [_smart contract_](https://icon.community/glossary/smart-contract/) is defined as follows by the ICON glossary:

> Blockchain-integrated code for maintaining and interacting with data in a trusted, distributed database

They are represented by a specific type of ICON [account](https://icon.community/glossary/smart-contract/), and can be distinguished by the prefix to the account address of `cx`. Smart contract accounts are deployed to the network and run in a deterministic manner based on their code. As such, they are not controlled by a user and a user cannot conduct manual actions that would override the smart contract logic.

[Wikipedia](https://en.wikipedia.org/wiki/Smart\_contract) notes that they can be thought of as a regular legal contract with rules that are automatically executed, and thus the need for external enforcement or mediation is reduced.

### Permissionless <a href="#permissionless" id="permissionless"></a>

Smart contracts can be written and deployed by anyone as long as they have an ICON account, the funds to deploy the contract, and the ability to produce a contract coded in a valid [programming language](smart-contract-languages.md) for ICON.

Currently, smart contracts can be written in [Java](https://en.wikipedia.org/wiki/Java\_\(programming\_language\)). Support may be available for other [JVM languages](https://en.wikipedia.org/wiki/List\_of\_JVM\_languages) as well, but this is highly untested.

### Composability <a href="#composability" id="composability"></a>

Smart contracts are available to be called from other contracts by design. They act as open APIs so that one smart contract can make use of the already available and auditable sets of rules and actions reachable from the ICON ecosystem.

Learn more about [composability](composability.md) in the corresponding section of the documentation.

### Limitations <a href="#limitations" id="limitations"></a>

As mentioned in the sections on the ICON execution environments (e.g. [Java](../icon-execution-environments/java.md)), there are certain features of a runtime environment that are used and expected by many programmers that are not available in ICON. This is by design, so that the blockchain can be deterministic and a consensus can be formed about the global state. Some limitations are as follows:

* No external networking
* No randomness

Networking and external information can be made accessible through the use of [oracles](https://ethereum.org/en/glossary/#oracle) or external relays that aggregate proofs and communicate across blockchains.

Randomness on the blockchain is a work-in-progress. Some implementations are available, such as that from [Chainlink](https://chain.link/chainlink-vrf).
