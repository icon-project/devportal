# Examples

This document describes how to interact with `ICON Network` using JavaScript SDK. This document contains SDK installation, API usage guide, and code examples.

This document is focused on serving the following groups.

* Web Developer using JavaScript as the main language
* Web Developer who wants to make dApp using ICON Network

This document is focused on how to use `icon-sdk-js` properly. If you would like to know about the details of `icon-sdk-js` API, see official `icon-sdk-js` API Reference document.

Get different types of examples as follows. Complete source code is found on Github at [https://github.com/icon-project/icon-sdk-js/tree/master/quickstart](https://github.com/icon-project/icon-sdk-js/tree/master/quickstart)

| Example                                                            | Description                                                                                    |
| ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| [Wallet](examples.md#wallet)                                       | An example of creating and loading a keywallet.                                                |
| [ICX Transfer](examples.md#icx-transfer)                           | An example of transferring ICX and confirming the result.                                      |
| [Token Deploy and Transfer](examples.md#token-deploy-and-transfer) | An example of deploying an IRC token, transferring the token and confirming the result.        |
| [Sync Block](examples.md#sync-block)                               | An example of checking block confirmation and printing the ICX and token transfer information. |

### Installation

#### Usage in Node.js

Install `icon-sdk-js` module using `npm`.

```bash
npm install --save icon-sdk-js
```

Import `icon-sdk-js` module.

```javascript
const IconService = require('icon-sdk-js');
```

#### Usage in browser

Install `icon-sdk-js` module using `npm`,

```bash
npm install --save icon-sdk-js
```

or using CDN.

```markup
<script src="https://cdn.jsdelivr.net/gh/icon-project/icon-sdk-js@latest/build/icon-sdk-js.web.min.js"></script>
```

Then, import `icon-sdk-js` module.

```javascript
import IconService from 'icon-sdk-js';
```

#### Usage in react-native environment

Install `icon-sdk-js` module using `npm`,

```bash
npm install --save icon-sdk-js
```

Then, import `icon-sdk-js/build/icon-sdk-js.web.min.js` module.

```javascript
import IconService from 'icon-sdk-js/build/icon-sdk-js.web.min.js';
```

### Using the SDK

#### Generate IconService

Generate `IconService` to communicate with the nodes.

The `IconService` class contains a set of API methods. It allows you to send transaction, check the result and get block information, etc. It accepts the `HttpProvider` which serves the purpose of connecting to HTTP and HTTPS based JSON-RPC servers. The `HttpProvider` takes the full URI where the server can be found. Here is a simple example how to initialize `IconService` class.

```javascript
/*  HttpProvider is used to communicate with http. 
    In this example, we use Yeouido node as provider. */
const provider = new HttpProvider('https://bicon.net.solidwallet.io/api/v3');

/* Create IconService instance */
const iconService = new IconService(provider);
```

#### Queries

All query methods of `IconService` returns a `HttpCall` instance. To execute the request and get the result value, you need to run `execute()` function of `HttpCall` instance. All requests will be executed **asynchronously**. Synchronous request is not available. For more information, check the [API reference documentation](../README.md).

```javascript
const { CallBuilder } = IconService.IconBuilder;

/* Get block information by block height */
const block = await iconService.getBlockByHeight(1000).execute();

/* Get block information by block hash */
const block = await iconService.getBlockByHash('0xdb3···d95').execute();

/* Get latest block information */
const block = await iconService.getLastBlock().execute();

/* Returns the balance of a EOA address */
const balance = await iconService.getBalance('hx9d8···b57').execute();

/* Returns the SCORE API list */
const apiList = await iconService.getScoreApi('cx000···001').execute();

/* Returns the total number of issued ICX. */
const totalSupply = await iconService.getTotalSupply().execute();

/* Returns information about a transaction requested by transaction hash */
const txObj = await iconService.getTransaction('0xb90···238').execute();

/* Returns the result of a transaction by transaction hash */
const txObj = await iconService.getTransactionResult('0xb90···238').execute();

/* Generates a call instance using the CallBuilder */
const callObj = new CallBuilder()
    .to('cxc24···6c7')
    .method('balanceOf')
    .params({ _owner: 'hx87a···d6a' })
    .build()

/* Executes a call method to call a read-only API method on the SCORE immediately without creating a transaction on Loopchain */
const result = await iconService.call(callObj).execute();
```

#### Transactions

To invoke SCORE API for changing states, you need to send a transaction. Before sending a transaction, the transaction should be signed. It can be done using the `Wallet` object.

```javascript
const { IconWallet } = IconService;

/* Creates an instance of Wallet. */
const wallet = IconWallet.create();

/* Load Wallet object by private key */
const wallet = IconWallet.loadPrivateKey('2ab···e4c');

/* Load Wallet object by keystore object */
const ks = { "version": 3 ··· "coinType": "icx" };
const pw = 'qwer1234!';
const wallet = IconWallet.loadKeystore(ks, pw);
```

Next, you need to build the transaction object using builder class.

```javascript
/* Build `IcxTransaction` instance for sending ICX. */
const txObj = new IcxTransactionBuilder()
    .from('hx462···bfd')
    .to('hx87a···d6a')
    .value(IconAmount.of(7, IconAmount.Unit.ICX).toLoop())
    .stepLimit(IconConverter.toBigNumber(100000))
    .nid(IconConverter.toBigNumber(3))
    .nonce(IconConverter.toBigNumber(1))
    .version(IconConverter.toBigNumber(3))
    .timestamp((new Date()).getTime() * 1000)
    .build()

/* Build `MessageTransaction` instance for sending data. */
const txObj = new MessageTransactionBuilder()
    .from('hx462···bfd')
    .to('hx87a···d6a')
    .stepLimit(IconConverter.toBigNumber(100000))
    .nid(IconConverter.toBigNumber(3))
    .nonce(IconConverter.toBigNumber(1))
    .version(IconConverter.toBigNumber(3))
    .timestamp((new Date()).getTime() * 1000)
    .data(IconConverter.fromUtf8('Hello'))
    .build()

/* Build `DeployTransaction` instance for deploying SCORE. */
const txObj = new DeployTransactionBuilder()
    .from('hx462···bfd')
    .to('cx000···000')
    .stepLimit(IconConverter.toBigNumber(2500000))
    .nid(IconConverter.toBigNumber(3))
    .nonce(IconConverter.toBigNumber(1))
    .version(IconConverter.toBigNumber(3))
    .timestamp((new Date()).getTime() * 1000)
    .contentType('application/zip')
    .content('0x504···000')
    .params({
        initialSupply: IconConverter.toHex('100000000000'),
        decimals: IconConverter.toHex(18),
        name: 'StandardToken',
        symbol: 'ST',
    })
    .build()

/* Build `CallTransaction` instance for executing SCORE function. */
const txObj = new CallTransactionBuilder()
    .from('hx902···3b5')
    .to('cx350···48d')
    .stepLimit(IconConverter.toBigNumber('2000000'))
    .nid(IconConverter.toBigNumber('3'))
    .nonce(IconConverter.toBigNumber('1'))
    .version(IconConverter.toBigNumber('3'))
    .timestamp((new Date()).getTime() * 1000)
    .method('transfer')
    .params({
        _to: 'hxd00···497',
        _value: IconConverter.toHex(IconAmount.of(1, IconAmount.Unit.ICX).toLoop()),
    })
    .build()
```

Using `SignedTransaction` object, you can make signature for transaction object. By passing `SignedTransaction` instance to `IconService.sendTransaction()` method, it will automatically generate transaction object including signature, and send to ICON node.

`sendTransaction` method returns a `HttpCall` instance. To execute the request and get the result value, you need to run `execute()` function of `HttpCall` instance. All requests will be executed **asynchronously**. Synchronous request is not available.

```javascript
/* Create SignedTransaction instance */
const signedTransaction = new SignedTransaction(txObj, wallet)

/* Send transaction. It returns transaction hash. */
const txHash = await iconService.sendTransaction(signedTransaction).execute();
```

### Code Examples

#### Install Dependency

Please go to `quickstart` directory and install dependency to use `icon-sdk-js`.

```
npm install   // install dependencies for executing the quickstart project (including icon-sdk-js package)
```

#### Run example file

Run example file.

```
npm start   // open http://localhost:3000/ in browser
```

If you want to rebuild icon-sdk-js library and run quickstart project, go to icon-sdk-js root directory and run `npm run quickstart:rebuild` command.

```
npm run quickstart:rebuild   // open http://localhost:3000/ in browser
```

#### Set Node URL

If you want to use custom ICON node URL, change the value of `NODE_URL` variable in `./mockData/index.js`. Default value of `NODE_URL` is `https://bicon.net.solidwallet.io/api/v3`

_For more information on the testnet, see_ [_the documentation_](https://github.com/icon-project/icon-project.github.io/blob/master/docs/icon\_network.md) _for the ICON network._

```javascript
const NODE_URL = 'https://bicon.net.solidwallet.io/api/v3';
```

####

#### Wallet

This example shows how to create a new `Wallet` and load wallet with privateKey or Keystore file.

**Create a wallet**

Create new EOA by calling `create` function. After creation, the address and private Key can be looked up.

```javascript
const wallet = IconWallet.create(); //Wallet Creation
console.log("Address: " + wallet.getAddress()); // Address Check
console.log("PrivateKey: " + wallet.getPrivateKey()); // PrivateKey Check

// Output
Address: hx4d37a7013c14bedeedbe131c72e97ab337aea159
PrivateKey: 00e1d6541bfd8be7d88be0d24516556a34ab477788022fa07b4a6c1d862c4de516
```

**Load a wallet**

You can call existing EOA by calling `loadKeystore` and `loadPrivateKey` function.

After creation, address and private Key can be looked up.

```javascript
// Load Wallet with private key
const walletLoadedByPrivateKey = IconWallet.loadPrivateKey('38f792b95a5202ab431bfc799f7e1e5c74ec0b9ede5c6142ee7364f2c84d72f6');
console.log(walletLoadedByPrivateKey.getAddress());
// Output: hx902ecb51c109183ace539f247b4ea1347fbf23b5
console.log(walletLoadedByPrivateKey.getPrivateKey());
// Output: 38f792b95a5202ab431bfc799f7e1e5c74ec0b9ede5c6142ee7364f2c84d72f6);

// Get keystore object from wallet
const keystoreFile = walletLoadedByPrivateKey.store('qwer1234!');
// Load wallet with keystore file
const walletLoadedByKeyStore = IconWallet.loadKeystore(keystoreFile, 'qwer1234!');
console.log(walletLoadedByKeyStore.getAddress());
// Output: hx902ecb51c109183ace539f247b4ea1347fbf23b5
console.log(walletLoadedByKeyStore.getPrivateKey());
// Output: 38f792b95a5202ab431bfc799f7e1e5c74ec0b9ede5c6142ee7364f2c84d72f6);
```

**Store the wallet**

After `Wallet` object creation, Keystore file can be stored by calling `store` function.

After calling `store`, Keystore JSON object can be looked up with the returned value.

```javascript
const privateKey = '38f792b95a5202ab431bfc799f7e1e5c74ec0b9ede5c6142ee7364f2c84d72f6'
const wallet = IconWallet.loadPrivateKey(privateKey);
console.log(wallet.store('qwer1234!'));
// Output: 
// {
//     "version": 3,
//     "id": "e00e113c-1e45-47e4-b732-10f3d1903d75",
//     "address": "hx7d3d4c743bb82b927ea8a0551a3b9288e722ac84",
//     "crypto": {
//         "ciphertext": "d5df37230528bbfc0015e93c61e60041a31fb63266f61ffec60a31f474d4d7d0",
//         "cipherparams": {
//             "iv": "feaf0cc19678e4b78369904a99ba411e"
//         },
//         "cipher": "aes-128-ctr",
//         "kdf": "scrypt",
//         "kdfparams": {
//             "dklen": 32,
//             "salt": "e2e3666919161044f7b369d6ad4296380d4a13b9b5e844301c64a502ea3da240",
//             "n": 16384,
//             "r": 8,
//             "p": 1
//         },
//         "mac": "43789e78de4744d06c14cf966b9609fadbcd815b5380caf3f778797f9824d9d7"
//     }
// }
```

#### ICX Transfer

This example shows how to transfer ICX and check the result.

_For the Wallet and IconService creation, please refer to the information above._

**ICX transfer transaction**

In this example, you can create a Wallet with `MockData.PRIVATE_KEY_1` and transfer 1 ICX to `MockData.WALLET_ADDRESS_2`.

```javascript
const wallet = IconWallet.loadPrivateKey(MockData.PRIVATE_KEY_1);
const walletAddress = wallet.getAddress();
// 1 ICX -> 1000000000000000000 conversion
const value = IconAmount.of(1, IconAmount.Unit.ICX).toLoop();
console.log(value);
// Output: BigNumber {s: 1, e: 18, c: Array(1)}
```

You can get a step cost for transferring icx as follows.

```javascript
async getDefaultStepCost() {
    const { CallBuilder } = IconBuilder;
    // Get governance score api list
    const governanceApi = await this.iconService.getScoreApi(MockData.GOVERNANCE_ADDRESS).execute();
    console.log(governanceApi)
    const methodName = 'getStepCosts';
    // Check input and output parameters of api if you need
    const getStepCostsApi = governanceApi.getMethod(methodName);
    const getStepCostsApiInputs = getStepCostsApi.inputs.length > 0 ? JSON.stringify(getStepCostsApi.inputs) : 'none';
    const getStepCostsApiOutputs = getStepCostsApi.outputs.length > 0 ? JSON.stringify(getStepCostsApi.outputs) : 'none';
    console.log(`[getStepCosts]\n inputs: ${getStepCostsApiInputs} \n outputs: ${getStepCostsApiOutputs}`);

    // Get step costs by iconService.call
    const callBuilder = new CallBuilder();
    const call = callBuilder
        .to(MockData.GOVERNANCE_ADDRESS)
        .method(methodName)
        .build();
    const stepCosts = await this.iconService.call(call).execute();
    return stepCosts.default
}
```

Generate transaction using the values above.

```javascript
// networkId of node 1:mainnet, 2~:etc
const networkId = new BigNumber("3"); // input node’s networkld
const version = new BigNumber("3"); // version

// Recommended icx transfer step limit :
// use 'default' step cost in the response of getStepCosts API
const stepLimit = await this.getDefaultStepCost(); // Please refer to the above description.

// Timestamp is used to prevent the identical transactions. Only current time is required (Standard unit : us)
// If the timestamp is considerably different from the current time, the transaction will be rejected.
const timestamp = (new Date()).getTime() * 1000;

//Enter transaction information
const icxTransactionBuilder = new IcxTransactionBuilder();
const transaction = icxTransactionBuilder
      .nid(networkId)
      .from(walletAddress)
      .to(MockData.WALLET_ADDRESS_2)
      .value(value)
      .version(version)
      .stepLimit(stepLimit)
      .timestamp(timestamp)
      .build();
```

Generate SignedTransaction to add the signature of the transaction.

```javascript
// Create signature of the transaction
const signedTransaction = new SignedTransaction(transaction, wallet);
// Read params to transfer to nodes
console.log(signedTransaction.getProperties());
// Output: 
// {
//     from: "hx902ecb51c109183ace539f247b4ea1347fbf23b5",
//     nid: "0x3",
//     nonce: "0x1",
//     signature: "OE/yJbKn+3kXiC/5x1mvOCpdbTiCAvdlZDgSH31//alTe4kTEVRCETXsQx+Jkbfwa6Qel1PUddoowdkQJDLPrgE=",
//     stepLimit: "0x186a0",
//     timestamp: "0x578ed370a3780",
//     to: "hxd008c05cbc0e689f04a5bb729a66b42377a9a497",
//     value: "0xde0b6b3a7640000",
//     version: "0x3",
// }
```

After calling sendTransaction from `IconService`, you can send a transaction and check the transaction’s hash value. ICX transfer is now sent.

```javascript
// Send transaction
const txHash = await iconService.sendTransaction(signedTransaction).execute();
// Print transaction hash
console.log(txHash);
// Output
// 0x69c07ff23e2eafb068ec026f1a116082f0d869b3964531e43088f6638bcfe0f7
```

**Check the transaction result**

After the transaction is sent, the result can be looked up with the returned hash value.

In this example, you can check your transaction result every 2 seconds because of the block confirmation time. Checking the result is as follows:

```javascript
// Check the result with the transaction hash
const transactionResult = await iconService.getTransactionResult(txHash).execute();
console.log("transaction status(1:success, 0:failure): "+transactionResult.status);
// Output
// transaction status(1:success, 0:failure): 1
```

You can check the following information using the TransactionResult.

* status : 1 (success), 0 (failure)
* to : transaction’s receiving address
* failure : Only exists if status is 0(failure). code(str), message(str) property included
* txHash : transaction hash
* txIndex : transaction index in a block
* blockHeight : Block height of the transaction
* blockHash : Block hash of the transaction
* cumulativeStepUsed : Accumulated amount of consumed step’s until the transaction is executed in block
* stepUsed : Consumed step amount to send the transaction
* stepPrice : Consumed step price to send the transaction
* scoreAddress : SCORE address if the transaction generated SCORE (optional)
* eventLogs : List of EventLogs written during the execution of the transaction.
* logsBloom : Bloom Filter of the indexed data of the Eventlogs.

**Check the ICX balance**

In this example, you can check the ICX balance by looking up the transaction before and after the transaction.

ICX balance can be confirmed by calling getBalance function from `IconService`

```javascript
// create or load wallet
const wallet = IconWallet.loadPrivateKey(MockData.PRIVATE_KEY_2);
// Check the wallet balance
const balance = await iconService.getBalance(wallet.getAddress()).execute();
console.log(balance);

// Output: 
// 100432143214321432143
```

#### Token Deploy and Transfer

This example shows how to deploy IRC token and transfer deployed token.

_For the Wallet and IconService generation, please refer to the information above._

**Token deploy transaction**

You need the SCORE Project to deploy token.

In this example, you will use ‘test.zi’ from the ‘resources’ folder.

\*test.zi : SampleToken SCORE Project Zip file.

Generate a wallet using `MockData.PRIVATE_KEY_1`, then read the binary data from ‘test.zi’

```javascript
const { Wallet } = this.iconService;
this.wallet = IconWallet.loadPrivateKey(MockData.PRIVATE_KEY_1);

this.content = '';
// Read test.zi from ‘resources’ folder.
```

Enter the basic information of the token you want to deploy.

```javascript
const initialSupply = IconConverter.toBigNumber("100000000000");
const decimals = IconConverter.toBigNumber("18");
const tokenName = "StandardToken";
const tokenSymbol = "ST";
```

You can get the maximum step limit value as follows.

```javascript
// Get apis that provides Governance SCORE
// GOVERNANCE_ADDRESS : cx0000000000000000000000000000000000000001
async getMaxStepLimit() {
    const { CallBuilder } = IconBuilder;

    const governanceApi = await this.iconService.getScoreApi(MockData.GOVERNANCE_ADDRESS).execute();
    // "getMaxStepLimit" : the maximum step limit value that any SCORE execution should be bounded by.
    const methodName = 'getMaxStepLimit';
    // Check input and output parameters of api if you need
    const getMaxStepLimitApi = governanceApi.getMethod(methodName);

    const params = {};
    params[getMaxStepLimitApi.inputs[0].name] = 'invoke';

    // Get max step limit by iconService.call
    const callBuilder = new CallBuilder();
    const call = callBuilder
        .to(MockData.GOVERNANCE_ADDRESS)
        .method(methodName)
        .params(params)
        .build();
    const maxStepLimit = await this.iconService.call(call).execute();
    return IconConverter.toBigNumber(maxStepLimit)
}
```

Generate transaction with the given values above.

```javascript
async buildDeployTransaction() {
    const { DeployTransactionBuilder } = IconBuilder;

    const initialSupply = IconConverter.toBigNumber("100000000000");
    const decimals = IconConverter.toBigNumber("18");
    const tokenName = "StandardToken";
    const tokenSymbol = "ST";
    const contentType = "application/zip";
    // Enter token information
    // key name ("initialSupply", "decimals", "name", "symbol")
    // You must enter the given values. Otherwise, your transaction will be rejected.
    const params = {
        initialSupply: IconConverter.toHex(initialSupply),
        decimals: IconConverter.toHex(decimals),
        name: tokenName,
        symbol: tokenSymbol
    }
    const installScore = MockData.SCORE_INSTALL_ADDRESS;
    const stepLimit = await this.getMaxStepLimit();
    const walletAddress = this.wallet.getAddress();
    // networkId of node 1:mainnet, 2~:etc
    const networkId = IconConverter.toBigNumber(3);
    const version = IconConverter.toBigNumber(3);
    // Timestamp is used to prevent the identical transactions. Only current time is required (Standard unit : us)
    // If the timestamp is considerably different from the current time, the transaction will be rejected.
    const timestamp = (new Date()).getTime() * 1000;

    //Enter transaction information
    const deployTransactionBuilder = new DeployTransactionBuilder();
    const transaction = deployTransactionBuilder
        .nid(networkId)
        .from(walletAddress)
        .to(installScore)
        .stepLimit(stepLimit)
        .timestamp(timestamp)
        .contentType(contentType)
        .content(`0x${this.content}`)
        .params(params)
        .version(version)
        .build();        
    return transaction; 
}
```

Generate SignedTransaction to add signature to the transaction.

```javascript
// Create signature of the transaction
const signedTransaction = new SignedTransaction(transaction, this.wallet);
// Read params to transfer to nodes
const signedTransactionProperties = JSON.stringify(signedTransaction.getProperties()).split(",").join(", \n");
```

You can check the transaction hash value by calling sendTransaction from ‘IconService\`. Deployment is now completed.

```javascript
this.deployTxHash = await this.iconService.sendTransaction(signedTransaction).execute();
```

**Check the deploy transaction result**

After the transaction is sent, the result can be looked up with the returned hash value. You can also check the ST Token score address that you deployed.

Checking the result is as follows:

```javascript
// Check the result with the transaction hash
const transactionResult = await iconService.getTransactionResult(this.deployTxHash).execute();

console.log("transaction status(1:success, 0:failure): "+transactionResult.status);
console.log("Your score address: " + transactionResult.scoreAddress);

// Output
// transaction status(1:success, 0:failure): 1
// Your score address: cx19584dcfacd0d7cc5e0562a53959069213d7adca
```

_For the 'TransactionResult', please refer to the `IcxTransactionExample`._

**Token transfer transaction**

You can send the ST token that you deployed right before.

You can generate Wallet using `MockData.PRIVATE_KEY_1` just like in the case of `IcxTransactionExample`, then send 1 ST Token to `MockData.WALLET_ADDRESS_2`

You need the token address to send your token.

```javascript
const wallet = IconWallet.loadPrivateKey(MockData.PRIVATE_KEY_1);
const toAddress = MockData.WALLET_ADDRESS_2;
const tokenAddress = this.scoreAddress; //ST Token Address that you deployed
const tokenDecimals = 18; // token decimal
// 1 ICX -> 1000000000000000000 conversion
const value = IconAmount.of(1, IconAmount.Unit.ICX).toLoop();
```

You can get a step cost to send token as follows.

```javascript
async getDefaultStepCost() {
    const { CallBuilder } = IconBuilder;

    // Get apis that provides Governance SCORE
    // GOVERNANCE_ADDRESS : cx0000000000000000000000000000000000000001
    const governanceApi = await this.iconService.getScoreApi(MockData.GOVERNANCE_ADDRESS).execute();
    console.log(governanceApi)
    const methodName = 'getStepCosts';

    // Get step costs by iconService.call
    const callBuilder = new CallBuilder();
    const call = callBuilder
        .to(MockData.GOVERNANCE_ADDRESS)
        .method(methodName)
        .build();
    const stepCosts = await this.iconService.call(call).execute();
    // For sending token, it is about twice the default value.
    return IconConverter.toBigNumber(stepCosts.default).times(2)
}
```

Generate Transaction with the given parameters above. You have to add receiving address and value to param object to send token.

```javascript
async buildTokenTransaction() {
    const { CallTransactionBuilder } = IconBuilder;

    const walletAddress = this.wallet.getAddress();
    // You can use "governance score apis" to get step costs.
    const value = IconAmount.of(1, IconAmount.Unit.ICX).toLoop();
    const stepLimit = await this.getDefaultStepCost();
    // networkId of node 1:mainnet, 2~:etc
    const networkId = IconConverter.toBigNumber(3);
    const version = IconConverter.toBigNumber(3);
    // Timestamp is used to prevent the identical transactions. Only current time is required (Standard unit : us)
    // If the timestamp is considerably different from the current time, the transaction will be rejected.
    const timestamp = (new Date()).getTime() * 1000;
    // SCORE name that send transaction is “transfer”.
    const methodName = "transfer";
    // Enter receiving address and the token value.
    // You must enter the given key name("_to", "_value"). Otherwise, the transaction will be rejected.
    const params = {
        _to: MockData.WALLET_ADDRESS_2,
        _value: IconConverter.toHex(value)
    }

    //Enter transaction information
    const tokenTransactionBuilder = new CallTransactionBuilder();
    const transaction = tokenTransactionBuilder
        .nid(networkId)
        .from(walletAddress)
        .to(this.scoreAddress)
        .stepLimit(stepLimit)
        .timestamp(timestamp)
        .method(methodName)
        .params(params)
        .version(version)
        .build();        
    return transaction;
}
```

Generate SignedTransaction to add signature to your transaction.

```javascript
// Generate transaction signature.
const signedTokenTransfer = new SignedTransaction(transaction, wallet);
// Read params to send to nodes.
console.log(signedTokenTransfer.getProperties());
```

Call sendTransaction from ‘IconService’ to check the transaction hash. Token transaction is now sent.

```javascript
// Send transaction
this.transactionTxHash = await iconService.sendTransaction(signedTokenTransfer).execute();
// Print transaction hash
console.log(this.transactionTxHash);
// Output
// 0x6b17886de346655d96373f2e0de494cb8d7f36ce9086cb15a57d3dcf24523c8f
```

**Check the transfer transaction result**

You can check the result with the returned hash value of your transaction.

In this example, you can check your transaction result in every 2 seconds because of the block confirmation time. Checking the result is as follows:

```javascript
// Check the result with the transaction hash
const transactionResult = await iconService.getTransactionResult(this.transactionTxHash).execute();
console.log("transaction status(1:success, 0:failure): "+transactionResult.status);
// Output
// transaction status(1:success, 0:failure):1
```

_For the TransactionResult, please refer to the `IcxTransactionExample`._

#### Check the token balance

In this example, you can check the token balance before and after the transaction.

You can check the token balance by calling ‘balanceOf’ from the token SCORE.

```javascript
const { CallBuilder } = IconBuilder;
const tokenAddress = this.scoreAddress;
// Method name to check the balance
const methodName = "balanceOf";
// You must enter the given key name (“_owner”).
const params = {
    _owner: address
}
const callBuilder = new CallBuilder();
const call = callBuilder
    .to(tokenAddress)
    .method(methodName)
    .params(params)
    .build();
// Check the wallet balance
const balance = await this.iconService.call(call).execute();
```

#### Sync Block

This example shows how to read block information and print the transaction result for every block creation.

_Please refer to above for Wallet and IconService creation._

**Read the block information**

In this example, 'getLastBlock' is called periodically in order to check the new blocks,

by updating the transaction information for every block creation.

```javascript
// Check the recent blocks
const block = await iconService.getLastBlock().execute();
console.log(block.height);
// Output
// 237845
```

If a new block has been created, get the transaction list.

```javascript
const txList = block.getTransactions();
```

You can check the following information using the ConfirmedTransaction:

* version : json rpc server version
* to : Receiving address of transaction
* value: The amount of ICX coins to transfer to the address. If omitted, the value is assumed to be 0
* timestamp: timestamp of the transmitting transaction (unit: microseconds)
* nid : network ID
* signature: digital signature data of the transaction
* txHash : transaction hash
* dataType: A value indicating the type of the data item (call, deploy, message)
* data: Various types of data are included according to dataType.

**Transaction output**

```javascript
async syncBlock(block) {
    // the transaction list of blocks
    console.log(block)
    const txList = block.getTransactions();

    Promise.all(
        txList.map(async transaction => {
            const txResult = await this.iconService.getTransactionResult(transaction.txHash).execute();

            // Print icx transaction
            if ((transaction.value !== undefined) && transaction.value > 0) {
                document.getElementById("S03-2").innerHTML += `<li>${block.height} - [ICX] status: ${txResult.status === 1 ? 'success' : 'failure'}  |  amount: ${transaction.value}</li>`
            }

            // Print token transaction
            if (transaction.dataType !== undefined &&
                transaction.dataType === "call") {
                const method = transaction.data.method;

                if (method !== null && method === "transfer") {
                    const params = transaction.data.params;
                    const value = IconConverter.toBigNumber(params["_value"]); // value
                    const toAddr = params["_to"];

                    const tokenName = await this.getTokenName(transaction.to);
                    const symbol = await this.getTokenSymbol(transaction.to);

                    document.getElementById("S03-2").innerHTML += `<li>${block.height} - [${tokenName} - ${symbol}] status: ${txResult.status === 1 ? 'success' : 'failure'}  |  amount: ${value}</li>`;
                }
            }
        })
    )

    this.prevHeight = block.height;
}
```

**Check the token name & symbol**

You can check the token SCORE by calling the `name` and `symbol` functions.

```javascript
async getTokenName(to) {
    const { CallBuilder } = IconBuilder;
    const tokenAddress = to; // token address
    const callBuilder = new CallBuilder();
    const call = callBuilder
        .to(tokenAddress)
        .method("name")
        .build();
    const result = await this.iconService.call(call).execute();
    return result;
}
```

```javascript
async getTokenSymbol(to) {
    const { CallBuilder } = IconBuilder;
    const tokenAddress = to; // token address
    const callBuilder = new CallBuilder();
    const call = callBuilder
        .to(tokenAddress)
        .method("symbol")
        .build();
    const result = await this.iconService.call(call).execute();
    return result;
}
```

### Changelog

* [https://github.com/icon-project/icon-sdk-js/blob/master/CHANGELOG.md](https://github.com/icon-project/icon-sdk-js/blob/master/CHANGELOG.md)

### References

* [API Reference](./README.md)
* [ICON JSON-RPC API v3](../json-rpc-api/README.md)
* [ICON Network](../../../README.md)

### Licenses

This project follows the Apache 2.0 License. Please refer to [LICENSE](https://www.apache.org/licenses/LICENSE-2.0) for details.
