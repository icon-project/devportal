# Storage

### Decentralized storage

One of the main considerations of a blockchain-based business should be how to store your data. Of course, some of that can be stored on the blockchain itself. However, this can be extremely inefficient for large datasets. Developers should carefully consider their different data sources and the various options available to them so that their applications can be efficient and their storage considerations can be taken care of.

### Persistence mechanism

#### Blockchain-based

Blockchain-based storage is where data is stored directly on the blockchain. This is what happens in ICON. It works really well for smaller sets of data that must be referenced frequently and benefit the most from the guarantees of the blockchain. It is typically more costly than other forms of persistent storage, as [nodes](../concepts/network/validator-nodes.md) must be paid for holding data.

#### Contract-based

Contract-based data storage is persistent, but does not necessarily contain the same guarantees of the blockchain. For example, data on the blockchain is stored forever. Contract-based data may be stored somewhere else and deleted once the contract agreement proves that storage of a certain set of data is no longer necessary or useful.

Here are some platforms that do contract-based persistence:

* [Filecoin](https://docs.filecoin.io/about-filecoin/what-is-filecoin/)
* [Skynet](https://siasky.net)
* [Storj](https://storj.io)
* [0Chain](https://0chain.net)

#### Additional decentralized options

Sometimes it is not necessary to use the blockchain directly or at all in order to utilize decentralized data storage tools. Consider using one of these services directly if you would like to have an interface layer between you and the blockchain for decentralized data storage or if you would like to create your own storage service with a set of guarantees that you decide upon:

* [IPFS](https://docs.ipfs.io/concepts/what-is-ipfs/)
* [Pinata](https://www.pinata.cloud) _(IPFS pinning service)_
* [web3.storage](https://web3.storage) _(IPFS/Filecoin pinning service)_
* [Infura](https://infura.io/product/ipfs) _(IPFS pinning service)_
