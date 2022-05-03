# ICON execution environments

The ICON execution environments are the implementations of the ICON protocol and subsequently accepted [Improvement Proposals](https://github.com/icon-project/IIPs) for the purpose of maintaining the state of the blockchain continuously and robustly, while also allowing for users to interact with it safely.

The execution environments are responsible for building the one true state of the ICON blockchain that all [nodes](../../concepts/network/clients.md) must have access to in order to operate correctly.

### State

State in ICON is split into two parts. The first part is the [_Merkle Patricia Tree_](https://eth.wiki/en/fundamentals/patricia-tree) data structure, which, like in many other blockchains, stores information about accounts and transactions as they evolve over time. The second part is a data structure called an [_Object Graph_](https://en.wikipedia.org/wiki/Object\_graph), which stores some data related to the blockchain as well.

### Transactions

[Transactions](../../concepts/computational-utilities/transactions.md) are defined by the ICON glossary as follows:

> Cryptographically signed instructions from accounts.

They are the actions that result in updates to the blockchain. Some transactions can cause new smart contracts to be created, while others can cause smart contracts to be called and the state of data managed by that contract to be updated.

### Environments

[Java](../client-apis/java-sdk.md)

[Python (deprecated)](../client-apis/python-sdk/)
