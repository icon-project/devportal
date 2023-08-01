# Sending a message with xCall

In the following document we are going to go in detail about the process of sending a cross chain encoded message using xCall.

The following example uses a program written in JavaScript (nodejs) and the repository for it can be found in the following link:

* [https://github.com/icon-community/xcall-sample-dapp](https://github.com/icon-community/xcall-sample-dapp)

We are going to interact with 2 local blockchains, one representing the ICON Network and the other representing any EVM blockchain (Ethereum, BSC, etc) and send a hex encoded string as a message from the ICON chain to the EVM chain.

### Prerequisites

As a prerequisite to follow this tutorial first you have to setup the local environment, which can be done following the steps detailed in this tutorial:

* [BTP Tutorial: Setting up BTP Locally for Testing](https://icon.community/tutorials/btp-tutorial-setting-up-btp-locally-for-testing/).

In order to run the example being shown in here it is necessary that both local networks from the BTP tutorial are up and running and you also need to have the relayer running (the steps for doing this are detailed in the tutorial).

Once your local BTP environment is running a file named `deployments.json` will be generated at the root folder of the `e2edemo` dapp of the `btp2` repository, this file contains the information of all the smart contracts deployed on both networks and other BTP related information, the file will look like this:

```json
{
  "icon0": {
    "network": "0x3.icon",
    "contracts": {
      "bmc": "cx1b8e75486efec00680c62c57e744fd683f5b2858",
      "bmv": "cxe747597d04809feffa4bc369308893f27c9530e8",
      "xcall": "cx17cb94775d2f774277bfbf3477be5f36ca5af37f",
      "dapp": "cxf2b240380ae7bdfd8ef96e6e4d116699149380b8"
    },
    "networkTypeId": "0x1",
    "networkId": "0x2",
    "blockNum": 75891
  },
  "hardhat": {
    "network": "0x539.hardhat",
    "contracts": {
      "bmcm": "0x5FbDB2315678afecb367f032d93F642f64180aa3",
      "bmcs": "0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0",
      "bmc": "0xDc64a140Aa3E981100a9becA4E685f962f0cF6C9",
      "bmv": "0x8A791620dd6260079BF849Dc5567aDC3F2FdC318",
      "xcall": "0x0DCd1Bf9A1b36cE34237eEaFef220932846BCD82",
      "dapp": "0x959922bE3CAee4b8Cd9a407cc3ac1C251C2007B1"
    },
    "blockNum": 1646
  },
  "link": { "src": "icon0", "dst": "hardhat" }
}
```

Copy the `xcall` contract address on both ICON and hardhat networks and the `dapp` contract address in the hardhat network to the `config.js` file of the `xcall-sample-dapp`

```javascript
// IMPORTS
require("dotenv").config();
const IconService = require("icon-sdk-js");
const { ethers } = require("ethers");
const { BigNumber } = ethers;
const Web3Utils = require("web3-utils");
const fs = require("fs");

// CONSTANTS
// CHANGE THESE VALUES TO YOUR OWN

const ICON_WALLET_PK =
  "573b555367d6734ea0fecd0653ba02659fa19f7dc6ee5b93ec781350bda27376";
const EVM_WALLET_PK =
  "0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80";
const ICON_XCALL_ADDRESS = "PLACE_SMART_CONTRACT_ADDRESS_HERE";
const EVM_XCALL_ADDRESS = "PLACE_SMART_CONTRACT_ADDRESS_HERE";
const EVM_DAPP_ADDRESS = "PLACE_SMART_CONTRACT_ADDRESS_HERE";
const EVM_CHAIN_LABEL = "0x539.hardhat";
const ICON_CHAIN_LABEL = "0x3.icon";
```

> Before diving into the process of using xCall to send messages across blockchains is important to notice that due to the nature of this process a moderate level of knowledge about programming with JavaScript and interacting with blockchains is necessary to be able to follow the explanation.
>
> In this document we are going to be doing the following task:
>
> * Signing transactions on the ICON Network and on EVM Chains.
> * Making readonly calls to ICON Network and EVM Chains.
> * Utilize special SDK and libraries designed for blockchains (icon-js-sdk, hardhat, ethers.js, web3js).
>
> Because of this is highly recommended that you are already familiar with these task in order to fully understand the process that is going to be explained.

### Smart contract on the destination chain

Using xCall is an straightforward process, you send message from a source chain by calling the `sendCallMessage` method of the xCall contract and you receive that message on the destination chain, but in order to properly receive that message is necessary to have a smart contract implemented in the destination chain with a method named `handleCallMessage`.

> **`@External void handleCallMessage(String _from, byte[] _data)`**
>
> Handles the call message received from the source chain. Only called from the Call Message Service.
>
> * **Parameters:**
>   * `_from` — The BTP address of the caller on the source chain
>   * `_data` — The calldata delivered from the caller

The `handleCallMessage` method of your smart contract in the destination chain will be the receiver of the message that you are going to be sending from the source chain.

For the purpose of this explanation, a contract as already being deployed on both chains (local ICON and local EVM chain). You can find the address of the smart contract in the `deployments.json` file at the root of the BTP tutorial for setting up the local environment that was described in the Prerequisites section of this document.

### Sending a message

To send a message using xCall you sign a transaction calling the `sendCallMessage` method of the xCall contract on the source chain.

> **`@Payable @External BigInteger sendCallMessage(String _to, byte[] _data, @Optional byte[] _rollback)`**
>
> Sends a call message to the contract on the destination chain.
>
> * **Parameters:**
>   * `_to` — The BTP address of the callee on the destination chain
>   * `_data` — The calldata specific to the target contract
>   * `_rollback` — (Optional) The data for restoring the caller state when an error occurred
> * **Returns:** The serial number of the request

When the `xcall` contract on the source chain receives the call request message, it sends the `_data` to the [BTP address](../../cross-chain-communication/blockchain-transmission-protocol-btp/btp-address.md) `_to` on the destination chain.

* The `_data` param is an arbitrary payload, the xCall interface doesn't care about what you are sending, it leaves the encoding and decoding of this data to the Dapp that you are developing, for example it can be a hex encoded string, or a hex encoded number, etc.
* The `_to` param is a [BTP formatted address](../../cross-chain-communication/blockchain-transmission-protocol-btp/btp-address.md) of the callee contract which receives the `_data` payload on the destination chain.
* The `_rollback` parameter is for handling error cases. If the `_rollback` parameter is not null, then the source chain will receive a response message back regardless of whether the destination chain execution of the message request fails. For more information, see [Error Handling](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md#error-handling).

Using the `icon-sdk-js` we can send the signed transaction invoking the `sendCallMessage` method of the xcall contract as shown in the following example from the `index.js` file of the sample repository given at the intro of this tutorial.

```javascript
// IMPORTS
require("dotenv").config();
const IconService = require("icon-sdk-js");
const { ethers } = require("ethers");
const { BigNumber } = ethers;
const Web3Utils = require("web3-utils");
const fs = require("fs");

// CONSTANTS
// CHANGE THESE VALUES TO YOUR OWN
const ICON_WALLET_PK =
  "573b555367d6734ea0fecd0653ba02659fa19f7dc6ee5b93ec781350bda27376";
const EVM_WALLET_PK =
  "0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80";
const ICON_XCALL_ADDRESS = "cx17cb94775d2f774277bfbf3477be5f36ca5af37f";
const EVM_XCALL_ADDRESS = "0x0DCd1Bf9A1b36cE34237eEaFef220932846BCD82";
const EVM_DAPP_ADDRESS = "0x959922bE3CAee4b8Cd9a407cc3ac1C251C2007B1";
const EVM_CHAIN_LABEL = "0x539.hardhat";
const ICON_CHAIN_LABEL = "0x3.icon";

// SETTINGS
const {
  IconWallet,
  IconBuilder,
  SignedTransaction,
  IconConverter,
  HttpProvider
} = IconService.default;

const { CallTransactionBuilder } = IconBuilder;

const ICON_RPC_URL = process.env.ICON_RPC || "http://localhost:9080/api/v3";
const ICON_RPC_NID = 3;
const EVM_RPC_URL = process.env.EVM_RPC || "http://localhost:8545";

const ICON_HTTP_PROVIDER = new HttpProvider(ICON_RPC_URL);
const ICON_SERVICE = new IconService.default(ICON_HTTP_PROVIDER);
const EVM_PROVIDER = new ethers.providers.JsonRpcProvider(EVM_RPC_URL);
const EVM_SIGNER = new ethers.Wallet(EVM_WALLET_PK, EVM_PROVIDER);
const ICON_SIGNER = IconWallet.loadPrivateKey(ICON_WALLET_PK);

// ICON CHAIN FUNCTIONS
async function sendCallMessage(to, data) {
  try {
    const wallet = ICON_SIGNER;

    const txObj = new CallTransactionBuilder()
      .from(wallet.getAddress())
      .to(ICON_XCALL_ADDRESS)
      .stepLimit(IconConverter.toBigNumber(2000000))
      .nid(IconConverter.toBigNumber(ICON_RPC_NID))
      .nonce(IconConverter.toBigNumber(1))
      .version(IconConverter.toBigNumber(3))
      .timestamp(new Date().getTime() * 1000)
      .method("sendCallMessage")
      .params({
        _to: to,
        _data: data
      })
      .build();

    console.log("# sendCallMessage tx object:");
    console.log(txObj);
    const signedTransaction = new SignedTransaction(txObj, wallet);
    return await ICON_SERVICE.sendTransaction(signedTransaction).execute();
  } catch (e) {
    console.log("Error running sendCallMessage");
    throw new Error(e);
  }
}

// UTILITY FUNCTIONS
function getBtpAddress(network, address) {
  return `btp://${network}/${address}`;
}

async function sleep(time) {
  await new Promise(resolve => {
    setTimeout(resolve, time);
  });
}

function encodeMessage(msg) {
  const encoded = Web3Utils.fromUtf8(msg);
  return encoded;
}

function decodeMessage(msg) {
  const decoded = Web3Utils.hexToString(msg);
  return decoded;
}

// MAIN LOGIC
async function main() {
  try {
    // Encode the message to send
    const dataToSend = encodeMessage("Hello from ICON");
    console.log("\n## Encoded message:", dataToSend);

    // Get the dapp contract btp address on the evm chain
    const btpAddressDestination = getBtpAddress(
      EVM_CHAIN_LABEL,
      EVM_DAPP_ADDRESS
    );
    const btpAddressSource = getBtpAddress(
      ICON_CHAIN_LABEL,
      ICON_SIGNER.getAddress()
    );

    // send the call message to the evm chain
    const callMessageTxHash = await sendCallMessage(
      btpAddressDestination,
      dataToSend
    );
    console.log("\n## Call message tx hash:", callMessageTxHash);
  } catch (e) {
    console.log("error running main function:", e);
  }
}

main();

```

This method returns the serial number of the request. The serial number is the unique identifier of the message request on the source chain.

This method is payable, and dApps need to call this method with proper fees. dApps that want to make a call to `sendCallMessage` can query the total fee amount for the destination network via the `getFee` method, and then enclose the appropriate fees in the method call. For more information on fees, see [Fees Handling](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md#fees-handling).

> **`@External(readonly=true) BigInteger getFee(String _net, boolean _rollback)`**
>
> Gets the fee for delivering a message to the \_net. If the sender is going to provide rollback data, the \_rollback param should set as true. The returned fee is the sum of the protocol fee and the relay fee.
>
> * **Parameters:**
>   * `_net` — The network address
>   * `_rollback` — Indicates whether it provides rollback data
> * **Returns:** the sum of the protocol fee and the relay fee

The `_net` parameter is the [network address](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md#network-address) of the destination chain to query the fee for sending a message to.

### Fetching event in source chain

After sending the message you have to now listen to the `CallMessageSent` event on the source chain.

> **`@EventLog(indexed=3) void CallMessageSent(Address _from, String _to, BigInteger _sn, BigInteger _nsn)`**
>
> Notifies that the requested call message has been sent.
>
> * **Parameters:**
>   * `_from` — The chain-specific address of the caller
>   * `_to` — The BTP address of the callee on the destination chain
>   * `_sn` — The serial number of the request
>   * `_nsn` — The network serial number of the BTP message

To listen to this event you have to make consecutive RPC queries with the `icx_getTransactionResult` method using the hash of the transaction for the `sendCallMessage` call.

```javascript
async function getTransactionResultICON(txHash) {
  for (let i = 0; i < 10; i++) {
    try {
      const txResult = await ICON_SERVICE.getTransactionResult(
        txHash
      ).execute();
      return txResult;
    } catch (e) {
      console.log(`txResult (pass ${i}):`, e);
    }
    await sleep(1000);
  }
}
```

The call will respond with an error status of `Pending` or `Executing` until the transaction have been processed by the network and then you will have an object response like the following:

```json
{
  "status": 1,
  "to": "cx17cb94775d2f774277bfbf3477be5f36ca5af37f",
  "txHash": "0xd2c19a61af825474960e9a5b22e38e90533280924d22ea49bcd191be6f7dd469",
  "txIndex": 1,
  "blockHeight": 394373,
  "blockHash": "0xb96c59c8f8763ce599030d2f6c9d0b96b17205c596a65cb0044b9330de3c7299",
  "cumulativeStepUsed": "1211098",
  "stepUsed": "1211098",
  "stepPrice": "12500000000",
  "eventLogs": [
    {
      "scoreAddress": "cx0000000000000000000000000000000000000000",
      "indexed": ["BTPMessage(int,int)", "0x2", "0x97"],
      "data": []
    },
    {
      "scoreAddress": "cx1b8e75486efec00680c62c57e744fd683f5b2858",
      "indexed": ["BTPEvent(str,int,str,str)", "0x3.icon", "0x8d"],
      "data": ["0x539.hardhat", "SEND"]
    },
    {
      "scoreAddress": "cx17cb94775d2f774277bfbf3477be5f36ca5af37f",
      "indexed": [
        "CallMessageSent(Address,str,int,int)",
        "hxb6b5791be0b5ef67063b3c10b840fb81514db2fd",
        "btp://0x539.hardhat/0x959922bE3CAee4b8Cd9a407cc3ac1C251C2007B1",
        "0x8c"
      ],
      "data": ["0x8d"]
    }
  ],
  "logsBloom": "0x0000000000000000000000000000000000000000000000000000000000000000200001000008200400000810000000480000000000000000000000000000002000004000000000000000000020000000000000000000000000802000000000082800000a000000000000000000000000000000000000000800000000000008000000000000000000000000000000400000000400000008000100000400000020000000000000000000080140000000000000000000000000000000400000000000000000000000000000000000000002002010000840200000000400000000000100000000000000000000000000000000000000000000000000000000000000"
}
```

### Fetching event on destination chain

The `CallMessage` event on the destination chain can be fetched to verify that the xcall interface on the destination chain has a new message that needs to be fetched.

> **`@EventLog(indexed=3) void CallMessage(String _from, String _to, BigInteger _sn, BigInteger _reqId, byte[] _data)`**
>
> Notifies the user that a new call message has arrived.
>
> * **Parameters:**
>   * `_from` — The BTP address of the caller on the source chain
>   * `_to` — A string representation of the callee address
>   * `_sn` — The serial number of the request from the source
>   * `_reqId` — The request id of the destination chain
>   * `_data` — The calldata

Similar as the process of listening to the `CallMessageSent` event on the origin chain, we now wait for the `CallMessage` event on the destination chain.

```javascript
// .. PREVIOUS CODE HERE
function getContractObjectEVM(abi, address) {
  try {
    const contractObject = new ethers.Contract(address, abi, EVM_SIGNER);
    return contractObject;
  } catch (e) {
    console.log(e);
  }
}

function getXcallContractEVM() {
  try {
    const XCALL_ABI = JSON.parse(fs.readFileSync("./xcallAbi.json", "utf8"));
    return getContractObjectEVM(XCALL_ABI.abi, EVM_XCALL_ADDRESS);
  } catch (e) {
    console.log(e);
  }
}

async function waitEventEVM(contract, filterCM) {
  let height = await contract.provider.getBlockNumber();
  let next = height + 1;
  while (true) {
    if (height == next) {
      await sleep(1000);
      next = (await contract.provider.getBlockNumber()) + 1;
      continue;
    }
    for (; height < next; height++) {
      console.log(`waitEventEvmChain: ${height} -> ${next}`);
      const events = await contract.queryFilter(filterCM, height);
      if (events.length > 0) {
        return events;
      }
    }
  }
}

async function main() {
    // ...PREVIOUS CODE HERE //
    // get xcall contract object for evm chain
    const xcallEvmContract = getXcallContractEVM();
    // console.log("xcall contract on evm chain:", xcallEvmContract);

    // get callMessage event on evm chain
    const callMessageFilters = xcallEvmContract.filters.CallMessage(
      btpAddressSource,
      EVM_DAPP_ADDRESS,
      parsedCallMessageSentEvent["_sn"]
    );
    console.log("callMessageFilters:", callMessageFilters);

    console.log("\n ## Waiting for callMessage event on evm chain...");
    const events = await waitEventEVM(xcallEvmContract, callMessageFilters);
    console.log("events:", events);
    console.log("# ReqId:", events[0].args._reqId);
}

```

When the event gets triggered we will have a response like in the following:

```js
[
  {
    blockNumber: 213912,
    blockHash: '0x7fc8fcea0b7d5f55b322a76b5b84e2763b9c1ad757932893a8d10dfcc244ebf3',
    transactionIndex: 0,
    removed: false,
    address: '0x0DCd1Bf9A1b36cE34237eEaFef220932846BCD82',
    data: '0x000000000000000000000000000000000000000000000000000000000000008c',
    topics: [
      '0xe47534aac445893e076f52327e0af29b70dc064167daf94b1e1083ad09c5b789',
      '0x454f0aa696ea4bef449b32595842a5ed0bae3963db8fc88130e3f25f0d76b0bb',
      '0x16f7d9e9e594e72a2e6120902a1339f4be2c2f06b73a09dc24c1956d61880911',
      '0x000000000000000000000000000000000000000000000000000000000000008c'
    ],
    transactionHash: '0x959fc3fb5852ca0fd7e5a64b939c3da39051ba71a39bad289b34dc9652096bbe',
    logIndex: 0,
    removeListener: [Function (anonymous)],
    getBlock: [Function (anonymous)],
    getTransaction: [Function (anonymous)],
    getTransactionReceipt: [Function (anonymous)],
    event: 'CallMessage',
    eventSignature: 'CallMessage(string,string,uint256,uint256)',
    decode: [Function (anonymous)],
    args: [
      [Indexed],
      [Indexed],
      BigNumber { value: "140" },
      BigNumber { value: "140" },
      _from: [Indexed],
      _to: [Indexed],
      _sn: BigNumber { value: "140" },
      _reqId: BigNumber { value: "140" }
    ]
  }
]
```

This event will give us the `_reqId` param that is necessary for us to use as a param in the next step to process and make the xCall contract send that message to our DApp smart contract on the destination chain.

### Execute call on destination chain

Once you have verified that the event has been raised in the destination chain, you have to now sign a transaction to trigger the process that will allow the xCall contract to send the message to your smart contract with the `handleCallMessage` method.

This transaction is being done by calling the `executeCall` method in the xCall contract on the destination chain:

> **`@External void executeCall(BigInteger _reqId, byte[] _data)`**
>
> Executes the requested call message.
>
> * **Parameters:**
>   * `_reqId` — The request id
>   * `_data` — The calldata

The `_reqId` is the unique identifier of the message request on the destination chain, we get this value in the `callMessage` event on the destination chain (the previous step).

```javascript
async function executeCall(reqId, data) {
  try {
    const contract = getXcallContractEVM();
    return await sendSignedTxEVM(contract, "executeCall", reqId, data);
  } catch (e) {
    console.log("error running executeCall");
    console.log(e);
  }
}

async function sendSignedTxEVM(contractObject, method, ...methodParams) {
  const txParams = { gasLimit: 15000000 };
  const tx = await contractObject[method](...methodParams, txParams);
  const receipt = await tx.wait(1);
  return receipt;
}
```

Like previously stated the DApp on the destination chain must implement `handleCallMessage`. When the user calls the `executeCall` method, the `xcall` contract invokes the following predefined method in the target DApp with the call data associated in the CallMessage `_reqId`.

### Fetching event on destination chain after executeCall

To notify the execution result of DApp `handleCallMessage` method, the following event is emitted after its execution.

> **`@EventLog(indexed=1) void CallExecuted(BigInteger _reqId, int _code, String _msg)`**
>
> Notifies that the call message has been executed.
>
> * **Parameters:**
>   * `_reqId` — The request id for the call message
>   *   `_code` — The execution result code
>
>       (0: Success, -1: Unknown generic failure, >=1: User defined error code)
>   * `_msg` — The result message if any

Once the transaction has been processed in the destination chain, similar to the process in the origin chain we can get the transaction result with the RPC method to fetch this transaction result on this specific chain.

```javascript
function filterEventEVM(contract, filter, receipt) {
  const inf = contract.interface;
  const address = contract.address;
  const topics = filter.topics || [];
  if (receipt.events && typeof topics[0] === "string") {
    const fragment = inf.getEvent(topics[0]);
    return receipt.events
      .filter(event => {
        if (event.address == address) {
          return topics.every((v, i) => {
            if (!v) {
              return true;
            } else if (typeof v === "string") {
              return v === event.topics[i];
            } else {
              return v.includes(event.topics[i]);
            }
          });
        }
        return false;
      })
      .map(event => {
        return {
          args: inf.decodeEventLog(fragment, event.data, event.topics)
        };
      });
  }
  return [];
}

async function checkCallExecuted(receipts, contract) {
  let event;
  const logs = filterEventEVM(
    contract,
    contract.filters.CallExecuted(),
    receipts
  );
  if (logs.length > 0) {
    event = logs[0].args;
  }
  return event;
}

async function main() {
    // ... PREVIOUS CODE HERE //
    // check the CallExecuted event
    console.log("\n ## Waiting for CallExecuted event on evm chain...");
    const callExecutedFilters = xcallEvmContract.filters.CallExecuted();
    const callExecutedEvent = await checkCallExecuted(
      executeCallTxHash,
      xcallEvmContract
    );
    console.log("callExecutedEvent:", callExecutedEvent);
}


```

The result in this case is the following:

```json
{
  "to": "0x0DCd1Bf9A1b36cE34237eEaFef220932846BCD82",
  "from": "0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266",
  "contractAddress": null,
  "transactionIndex": 0,
  "gasUsed": { "type": "BigNumber", "hex": "0x01a9cf" },
  "logsBloom": "0x00000000000000000000000200000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000041000040000000000000000000000000000000000000000000000000000000000000000400000000000000000000020000000000000000000000000000000000000000000000000000000100000000000000000000000000200000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004100000010000000000000000000000020000000000400000000000",
  "blockHash": "0x1c6b8f1c557767ae4482052984361479a73ffe77ff44c6acdc9c08c3bf9fe6fb",
  "transactionHash": "0x70c52142fa4efe06fa2f45eb4ae1c380fee3f9e77b911e9b1b048e4495f4204c",
  "logs": [
    {
      "transactionIndex": 0,
      "blockNumber": 214709,
      "transactionHash": "0x70c52142fa4efe06fa2f45eb4ae1c380fee3f9e77b911e9b1b048e4495f4204c",
      "address": "0x959922bE3CAee4b8Cd9a407cc3ac1C251C2007B1",
      "topics": [
        "0x0d8291c70d2bb268eb9759e50f86de9237a804a0f6f014192af802551d5a2f3f"
      ],
      "data": "0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000396274703a2f2f3078332e69636f6e2f68786236623537393162653062356566363730363362336331306238343066623831353134646232666400000000000000000000000000000000000000000000000000000000000000000000000000001948656c6c6f2074686973206973207843616c6c206c6976652100000000000000",
      "logIndex": 0,
      "blockHash": "0x1c6b8f1c557767ae4482052984361479a73ffe77ff44c6acdc9c08c3bf9fe6fb"
    },
    {
      "transactionIndex": 0,
      "blockNumber": 214709,
      "transactionHash": "0x70c52142fa4efe06fa2f45eb4ae1c380fee3f9e77b911e9b1b048e4495f4204c",
      "address": "0x0DCd1Bf9A1b36cE34237eEaFef220932846BCD82",
      "topics": [
        "0xc7391e04887f8b3c16fa20877e028e8163139a478c8447e7d449eba1905caa51",
        "0x000000000000000000000000000000000000000000000000000000000000008d"
      ],
      "data": "0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000000",
      "logIndex": 1,
      "blockHash": "0x1c6b8f1c557767ae4482052984361479a73ffe77ff44c6acdc9c08c3bf9fe6fb"
    }
  ],
  "blockNumber": 214709,
  "confirmations": 2,
  "cumulativeGasUsed": { "type": "BigNumber", "hex": "0x01a9cf" },
  "effectiveGasPrice": { "type": "BigNumber", "hex": "0x04a817c800" },
  "status": 1,
  "type": 0,
  "byzantium": true,
  "events": [
    {
      "transactionIndex": 0,
      "blockNumber": 214709,
      "transactionHash": "0x70c52142fa4efe06fa2f45eb4ae1c380fee3f9e77b911e9b1b048e4495f4204c",
      "address": "0x959922bE3CAee4b8Cd9a407cc3ac1C251C2007B1",
      "topics": [
        "0x0d8291c70d2bb268eb9759e50f86de9237a804a0f6f014192af802551d5a2f3f"
      ],
      "data": "0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000396274703a2f2f3078332e69636f6e2f68786236623537393162653062356566363730363362336331306238343066623831353134646232666400000000000000000000000000000000000000000000000000000000000000000000000000001948656c6c6f2074686973206973207843616c6c206c6976652100000000000000",
      "logIndex": 0,
      "blockHash": "0x1c6b8f1c557767ae4482052984361479a73ffe77ff44c6acdc9c08c3bf9fe6fb"
    },
    {
      "transactionIndex": 0,
      "blockNumber": 214709,
      "transactionHash": "0x70c52142fa4efe06fa2f45eb4ae1c380fee3f9e77b911e9b1b048e4495f4204c",
      "address": "0x0DCd1Bf9A1b36cE34237eEaFef220932846BCD82",
      "topics": [
        "0xc7391e04887f8b3c16fa20877e028e8163139a478c8447e7d449eba1905caa51",
        "0x000000000000000000000000000000000000000000000000000000000000008d"
      ],
      "data": "0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000000",
      "logIndex": 1,
      "blockHash": "0x1c6b8f1c557767ae4482052984361479a73ffe77ff44c6acdc9c08c3bf9fe6fb",
      "args": [
        { "type": "BigNumber", "hex": "0x8d" },
        { "type": "BigNumber", "hex": "0x00" },
        ""
      ],
      "event": "CallExecuted",
      "eventSignature": "CallExecuted(uint256,int256,string)"
    }
  ]
}
```

Decoding the eventlog will give us the following output in which we can see the event being raised with the `_reqId` identifying our cross chain message:

```
[
  BigNumber { value: "140" },
  BigNumber { value: "0" },
  '',
  _reqId: BigNumber { value: "140" },
  _code: BigNumber { value: "0" },
  _msg: ''
]
```

### Verify received message (Optional)

For this DApp a `MessageReceived` event method has been implemented to verify the message has been received, this is not a requirement but is a recommended step.

> **`@EventLog void MessageReceived( String _from, Bytes _data)`**
>
> Verifies the message has been received.
>
> * **Parameters:**
>   * `_from` — Message sender BTP Address
>   * `_data` — Encoded message

This event method implemented on the DApp smart contract on the destination chain returns to us the encoded message in the `_data` param:

```js
[
  'btp://0x3.icon/hxb6b5791be0b5ef67063b3c10b840fb81514db2fd',
  '0x48656c6c6f2074686973206973207843616c6c206c69766521',
  _from: 'btp://0x3.icon/hxb6b5791be0b5ef67063b3c10b840fb81514db2fd',
  _data: '0x48656c6c6f2074686973206973207843616c6c206c69766521'
]
```

### Decode message sent (Optional)

The `_data` param of the result in the previous step is a hex encoded string, once you decode it we can now see the string message that we originally sent from ICON:

```
## Decoded message sent via xcall
Hello this is xCall live!
```

### References

* End-to-end demonstration of BTP containing xCall: [https://github.com/icon-project/btp2/tree/main/e2edemo](https://github.com/icon-project/btp2/tree/main/e2edemo)
* xCall Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md)
* IBC on ICON: [https://github.com/icon-project/IBC-Integration](https://github.com/icon-project/IBC-Integration)
* [xCall testnet contracts](../../cross-chain-communication/blockchain-transmission-protocol-btp/)
