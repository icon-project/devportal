# Blocks

### What is a block?

From the ICON glossary, a [block](https://icon.community/glossary/block/) is defined as follows:

> An addition to the ICON distributed ledger that may be validated. Consists of transactions

Blocks are an efficient way to maintain a synchronized state between multiple machines. Instead of synchronizing every transaction individually, multiple transactions are synchronized in a single commit. This scheme of synchronization gives [validators](../network/validator-nodes.md) more time to form a consensus. This reduces the potential bottleneck that would be caused by network latency if each transaction were sent to all validator machines in individual messages.

### Block structure

The ICON blockchain is based off of a [merkle tree](https://icon.community/glossary/merkle-tree/) ([wikipedia](https://en.wikipedia.org/wiki/Merkle\_tree)). It is also a forward-only-moving global state machine. Each block represents one step forward of the machine. This means updates to the decentralized database containing financial or non-financial information. To fulfill two of the major promises of the blockchain, the global state must be entirely trusted and reproducible at each step.

As such, each block contains information in two categories: metadata about the blockchain itself and information about the updates in this step forward of the global state of the blockchain. The metadata is the version of the blockchain being run. The state-update-data is as follows: current time, block ID, block producer / validator information, transactions in this step, and log information.

### Block storage

The data storage layer is typically one of the most generic, low-level layers in any application. This layer is a building block for less abstract parts of an application to place their data into without worry of formatting or the specifics of data retrieval. The ICON blockchain uses RocksDB, which is a common choice among blockchain [clients](../network/clients.md). The main data structure that we use to store blockchain data is called a [_trie_](https://en.wikipedia.org/wiki/Trie), which is short for _retrieval_. Tries make it simple to both insert new data and to check whether important pieces of data like blocks, transactions, or accounts exist in our database.

### Creation of the next block

Block creation is the responsibility of the validators. Validators take turns creating the next block on the blockchain. On their turn, they submit the block they've created to their peer validators so that they can [form a consensus](consensus-mechanism.md) on whether the block is valid or not
