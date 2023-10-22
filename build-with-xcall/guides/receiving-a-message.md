# Receiving a message

There are two steps that the EOA on the destination chain must do in order to receive and execute a message.

1. Listen for the `CallMessage` event emitted by the xCall contract on the destination chain that has the same serial number as the request from the source.
2. Invoke the `executeCall` method on the xCall contract on the destination chain. Pass the request id and calldata from the `CallMessage` event as arguments.

#### Listen for CallMessage event

When the xCall contract on the destination chain receives the call request, it emits the following event for notifying the user.

```
/**
 * Notifies the user that a new call message has arrived.
 *
 * @param _from The BTP address of the caller on the source chain
 * @param _to A string representation of the callee address
 * @param _sn The serial number of the request from the source
 * @param _reqId The request id of the destination chain
 * @param _data The calldata
 */
@EventLog(indexed=3)
void CallMessage(String _from, String _to, BigInteger _sn, BigInteger _reqId, byte[] _data);
```

To minimize the gas cost, the calldata payload delivered from the source chain are exported to event `_data` field, instead of storing it in the state db. The \_data payload should be repopulated by the user (or client) when calling the following `executeCall` method. Then `xCall` compares it with the saved hash value to validate its integrity.

### Invoke executeCall

After `CallMessage` Event is observed by EOA, he invokes the following method on `xCall` with the given `_reqId` and `_data`.

```
/**
 * Executes the requested call message.
 *
 * @param _reqId The request id
 * @param _data The calldata
 */
@External
void executeCall(BigInteger _reqId, byte[] _data);
```

When the EOA calls `executeCall` method, the `xCall` contract invokes the following predefined method in the target dApp with the calldata associated in `_reqId`.

```
/**
 * Handles the call message received from the source chain.
 * Only called from the Call Message Service.
 *
 * @param _from The BTP address of the caller on the source chain
 * @param _data The calldata delivered from the caller
 */
@External
void handleCallMessage(String _from, byte[] _data);
```

**I**f execution fails on the destination chain, `handleCallMessage` may also need to be defined in the source chain Smart Contract A in order to handle the [executeRollback](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md#executerollback) invocation properly. For more information, see [Troubleshooting](../troubleshooting.md).

#### CallExecuted Event

To notify the execution result of dApp's `handleCallMessage` method, the following event is emitted after its execution.

```
/**
 * Notifies that the call message has been executed.
 *
 * @param _reqId The request id for the call message
 * @param _code The execution result code
 *              (0: Success, -1: Unknown generic failure, >=1: User defined error code)
 * @param _msg The result message if any
 */
@EventLog(indexed=1)
void CallExecuted(BigInteger _reqId, int _code, String _msg);
```

### Example

Waiting for the xCall related events is the same process as waiting for any other event in a JVM (ICON) chain or EVM chain.

The following is an example of waiting for the `CallMessageSent` event on a cross-chain message originating on ICON:

```javascript
const IconService = require("icon-sdk-js");

const { HttpProvider } = IconService.default;

// RPC node for the ICON network
const ICON_RPC_URL = "https://berlin.net.solidwallet.io/api/v3/icon_dex";
const HTTP_PROVIDER = new HttpProvider(ICON_RPC_URL);
const ICON_SERVICE = new IconService.default(HTTP_PROVIDER);

/*
 * filterEvent - filters the event logs
 * @param {object} eventlogs - the event logs
 * @param {string} sig - the signature of the event
 * @param {string} address - the address of the contract
 * @returns {object} - the filtered event logs
 * @throws {Error} - if there is an error filtering the event logs
 */
function filterEvent(eventlogs, sig, address) {
  return eventlogs.filter(event => {
    return (
      event.indexed &&
      event.indexed[0] === sig &&
      (!address || address === event.scoreAddress)
    );
  });
}

/*
 * parseCallMessageSentEvent - parses the CallMessageSent event logs
 * @param {object} event - the event logs
 * @returns {object} - the parsed event logs
 * @throws {Error} - if there is an error parsing the event logs
 */
function parseCallMessageSentEvent(event) {
  const indexed = event[0].indexed || [];
  const data = event[0].data || [];
  return {
    _from: indexed[1],
    _to: indexed[2],
    _sn: indexed[3],
    _nsn: data[0]
  };
}

async function main() {
  // hash of the cross chain transaction
  const txHash = "0x123..";
  // signature for the event
  const callMessageSentSignature = "CallMessageSent(Address,str,int,int)";
  // address of the xcall contract
  const xCallContractAddress = "cx123..";

  // get the transaction result
  const txResult = await ICON_SERVICE.getTransactionResult(txHash).execute();

  // filter the CallMessageSent event logs
  const filteredEvent = filterEvent(
    txResult.eventLogs,
    callMessageSentSignature,
    xCallContractAddress
  );

  // parsing the CallMessageSent event logs
  const parsedEvent = parseCallMessageSentEvent(filteredEvent);
  console.log("parsedEvent", parsedEvent);
}

main();
```

The following code sample showcase how to wait and fetch the `CallMessage` event of the destination chain in a cross chain message scenario where the destination chain is an EVM chain.

```javascript
const fs = require("fs");
const { ethers } = require("ethers");

// RPC of the node in the destination chain
const EVM_RPC_URL =
  "https://sepolia.infura.io/v3/ffbf8ebe228f4758ae82e175640275e0";
// Private key of the wallet in the destination chain
const WALLET_PK = "0x0123...";
// btp network label of the origin chain, in this example Berlin Testnet from ICON
const NETWORK_LABEL_ORIGIN = "0x7.icon";
// Address of the dApp contract in the destination chain
const EVM_DAPP_ADDRESS = "0x0123...";
// Address of the dApp contract in the origin chain
const ICON_DAPP_ADDRESS = "cx123...";
// path to the xcall contract abi
const XCALL_ABI_PATH = "./xcallAbi.json";
// address of the xcall contract in the destination chain
const XCALL_ADDRESS_DESTINATION = "0x0123...";

/*
 * sleep - sleeps for the specified amount of time
 * @param {number} ms - the amount of time to sleep in milliseconds
 * @returns {Promise} - the promise to sleep
 */
async function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

/*
 * filterCallMessageEventEvm - filters the CallMessage event logs
 * @param {string} iconDappAddress - the address of the ICON dapp
 * @param {string} evmDappAddress - the address of the EVM dapp
 * @param {string} sn - the serial number cross chain transaction
 * @returns {object} - the filtered event logs
 * @throws {Error} - if there is an error filtering the event logs
 */
function filterCallMessageEventEvm(iconDappAddress, evmDappAddress, sn) {
  const btpAddressSource = `btp://${NETWORK_LABEL_ORIGIN}/${iconDappAddress}`;
  const xcallContract = getXcallContractObject();
  const callMessageFilters = xcallContract.filters.CallMessage(
    btpAddressSource,
    evmDappAddress,
    sn
  );
  return callMessageFilters;
}

/*
 * waitEventEVM - waits for the event to be emitted
 * @param {object} filterCM - the filter for the event
 * @returns {object} - the event logs
 * @throws {Error} - if there is an error waiting for the event
 */
async function waitEventEVM(filterCM) {
  const contract = getXcallContractObject();
  let height = await contract.provider.getBlockNumber();
  let next = height + 1;
  console.log("block height", height);
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

/*
 * getXcallContractObject - gets the xcall contract object
 * @returns {object} - the xcall contract object
 * @throws {Error} - if there is an error getting the xcall contract object
 */
function getXcallContractObject() {
  try {
    const contract = JSON.parse(fs.readFileSync(XCALL_ABI_PATH));
    const abi = contract.abi;
    const provider = new ethers.providers.JsonRpcProvider(EVM_RPC_URL);
    const signer = new ethers.Wallet(WALLET_PK, provider);
    const contractObject = new ethers.Contract(
      XCALL_ADDRESS_DESTINATION,
      abi,
      signer
    );
    return contractObject;
  } catch (e) {
    console.log(e);
    throw new Error("Error getting Xcall contract");
  }
}

async function main() {
  // Serial number of the crosschain message
  const crossChainMessageSN = "0x1";

  // filter call message event evm
  const callMessageEventEvmFilters = filterCallMessageEventEvm(
    ICON_DAPP_ADDRESS,
    EVM_DAPP_ADDRESS,
    crossChainMessageSN
  );
  console.log(
    "\n# call message event evm filters:",
    callMessageEventEvmFilters
  );

  // wait for call message event evm
  const eventsEvm = await waitEventEVM(callMessageEventEvmFilters);
  console.log("\n# events params:");
  console.log(JSON.stringify(eventsEvm[0].args));
}

main();
```
