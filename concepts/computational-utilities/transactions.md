# Transactions

### What is a transaction?

As defined by the [ICON glossary](https://icon.community/glossary/transaction/):

> Cryptographically signed instructions from accounts

A transaction will contain instructions on how to initiate the process of updating the state of the blockchain. A simple example of a transaction would be to send ICX from one account to another.

On ICON, a transaction has an associated identity value, also referred to as a _transaction hash_.

### Performing a transaction

The three main categories of ways to perform a transaction are as follows: through the [Hana wallet](https://chrome.google.com/webstore/detail/hana/jfdlamikmbghhapbgfoogdffldioobgl/related), through a [decentralized application (dApp)](../../projects/decentralized-applications-dapps.md), or via the [ICON JSON-RPC API](../../icon-stack/client-apis/).

To use Hana wallet, you must first [create an account](accounts.md#creating-an-account). Then, open the wallet via web browser and perform a transaction.

In the case of using a dApp, each application will have different instructions for performing a transaction. Typically they will make the information on how to do so available either on their main application interface or through their own documentations.

To use the JSON-RPC API, see the `icx_sendTransaction` function in the API specifications.

### Querying a transaction

There are two main ways to query a transaction: through the [Tracker](https://https/tracker.icon.community) or through the JSON-RPC API available from an [API Endpoint](../network/api-endpoints.md).

To query a transaction through the tracker, click the search icon and paste in the transaction hash.

For users that intend to perform transaction queries in a programmatic way through an API Endpoint, you can follow the instructions below to validate and confirm the transaction.

**Check the Transaction Validation using the JSON-RPC API**

If you get the transaction hash as a response to sending a transaction to the ICON network, this means that your transaction has passed the validation. ICON network returns the transaction hash only when a transaction is valid. For specific information about this process, see the JSON-RPC APIs "icx\_sendTransaction" section of the [ICON JSON-RPC API v3 Specification](../../icon-stack/client-apis/json-rpc-api/) documentation.

**Check the Transaction Confirmation using the JSON-RPC API**

You can check if a transaction is confirmed or pending using "icx\_getTransactionResult" JSON-RPC API. "Pending" means that a transaction is in the transaction pool or block generation stage (except in the case of consensus failure on the block, it can be temporarily marked as "pending"). For specific information about this process, see the JSON-RPC APIs "icx\_getTransactionResult" section of the [ICON JSON-RPC API v3 Specification](../../icon-stack/client-apis/json-rpc-api/) documentation.

### The transaction process

**Create Transaction**

Create and propagate a transaction to the ICON network. Propagated transactions proceed with basic syntax and signature validation, and is put into the transaction pool to be recorded to a block if they are valid. At the same time, the ICON network returns the transaction hash data. If a transaction is invalid, that transaction is immediately rejected and the ICON network returns an error instead of a transaction hash. The word "rejected" refers to the transaction being removed because it can not be propagated on the ICON network.

**Transaction Pool**

In this stage, a transaction is waiting before being put into the block.

**Block Generation**

In this stage, a transaction is put into a generated block. If consensus is reached regarding the generated block (after the consensus process), that block is connected to the blockchain, and the transactions in the block are recorded permanently. If the consensus on the block fails, all transactions in that block are removed after a certain period of time. The word "removed" refers to ceasing to exist on the ICON network entirely. Therefore, the user who created and propagated the transaction needs to track the transaction until it is recorded on the blockchain (i.e. confirmed).

**Confirmed**

The block which contains a transaction is connected to the blockchain after the consensus process. This state of the transaction is called "confirmed". A transaction in a confirmed block is recorded permanently regardless of the transaction processing result (success or failure).

### Transaction fees

Fees are incurred when executing a smart contract on the ICON network, and the fees are determined based on the total amount of resources used to execute the transaction. [STEP](../economics/step.md) is the unit of the resource-usage measurement.

There are three categories that charge transaction fees:

1. Smart Contract Function Usage
2. Blockchain Database Usage
3. Amount of Transaction Data
