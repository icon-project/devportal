# Sending a message

Sending a call message requires your Smart Contract to call the **pre-deployed** xCall Smart Contract's `sendCallMessage` method.

More detailed explanation can be found [here](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md#sendcallmessage). Rollback is covered in the [Troubleshooting ](../troubleshooting.md)section.

#### JVM

```
/**
 * Sends a call message to the contract on the destination chain.
 *
 * @param _to The BTP address of the callee on the destination chain
 * @param _data The calldata specific to the target contract
 * @param _rollback (Optional) The data for restoring the caller state when an error occurred
 * @return The serial number of the request
 */
@Payable
@External
BigInteger sendCallMessage(String _to, byte[] _data, @Optional byte[] _rollback);
```

Which is part of [CallService.java](https://www.javadoc.io/doc/foundation.icon/btp2-xcall/latest/foundation/icon/btp/xcall/CallService.html) interface.

#### EVM

```
/**
   @notice Sends a call message to the contract on the destination chain.
   @param _to The BTP address of the callee on the destination chain
   @param _data The calldata specific to the target contract
   @param _rollback (Optional) The data for restoring the caller state when an error occurred
   @return The serial number of the request
 */
function sendCallMessage(
    string memory _to,
    bytes memory _data,
    bytes memory _rollback
) external payable returns (
    uint256
);
```

Which is part of [ICallService.sol](https://github.com/icon-project/btp2-solidity/blob/276a7d7b004bcc918b6c4bf656439c335a3960b5/library/contracts/interfaces/ICallService.sol) interface.

### Examples

The following is a simple example of a Java contract being used as a proxy contract for the xCall contract in the Berlin testnet.

```java
package app;

import foundation.icon.score.client.ScoreClient;
import score.Address;
import score.Context;
import score.annotation.External;
import score.annotation.Payable;


@ScoreClient
public class  Example {

    private BigInteger _sendCallMessage(String _to, byte[] _data, byte[] _rollback) {
        // address of xcall contract on berlin
        Address xcallSourceAddress = "cxf4958b242a264fc11d7d8d95f79035e35b21c1bb";
        return Context.call(BigInteger.class, Context.getValue(), xcallSourceAddress, "sendCallMessage", _to, _data, _rollback);
    }

    @Payable
    @External
    public void sendMessage(String _to, byte[] _data, byte[] _rollback) {
        BigInteger id = _sendCallMessage(_to, _data, _rollback);
        Context.println("sendCallMessage Response:" + id);
    }
}
```

In this example, any EOA can call the `sendMessage` method of the Example contract, this contract internally invokes the `sendCallMessage` method of the xCall contract.

The following Javascript sample showcase how to call the `sendMessage` method of the previous contract deployed on the Berlin testnet.

```javascript
const IconService = require("icon-sdk-js");

const {
  IconBuilder,
  IconConverter,
  SignedTransaction,
  HttpProvider,
  IconWallet
} = IconService.default;

const { CallTransactionBuilder, CallBuilder } = IconBuilder;

// RPC node for the ICON network
const ICON_RPC_URL = "https://berlin.net.solidwallet.io/api/v3/icon_dex";
// Network NID
const NID = "0x7";
// Private key for the wallet
const PK_BERLIN = "0x0123...";
// xCall contract address on origin chain. In this example the origin is Berlin
const XCALL_ORIGIN = "cxf4958b242a264fc11d7d8d95f79035e35b21c1bb";
// contract address of the dapp on origin chain
const CONTRACT_ORIGIN = "cx123..";
// network label of the destination chain. In this example the destination is Sepolia
const NETWORK_LABEL_DESTINATION = "0xaa36a7.eth2";
// address of the dapp contract on the destination chain
const CONTRACT_DESTINATION = "0x123...";

const HTTP_PROVIDER = new HttpProvider(ICON_RPC_URL);
const ICON_SERVICE = new IconService.default(HTTP_PROVIDER);
const ICON_WALLET = IconWallet.loadPrivateKey(PK_BERLIN);

/*
 * sendMessage - calls the dapp contract method
 * @param {string} _to - btp address of the recipient
 * @param {string} _data - the data to send
 * @param {boolean} useRollback - whether to use rollback
 * @returns {object} - the transaction receipt
 * @throws {Error} - if there is an error calling the dapp contract method
 */
async function sendMessage(_to, _data) {
  try {
    const fee = await getFee();

    const params = {
      _to: _to,
      _data: _data
    };
    const txObj = new CallTransactionBuilder()
      .from(ICON_WALLET.getAddress())
      .to(CONTRACT_ORIGIN)
      .method("sendMessage")
      .params(params)
      .stepLimit(IconConverter.toBigNumber(20000000))
      .nid(IconConverter.toBigNumber(NID))
      .nonce(IconConverter.toBigNumber(1))
      .version(IconConverter.toBigNumber(3))
      .timestamp(new Date().getTime() * 1000)
      .value(fee)
      .build();

    const signedTx = new SignedTransaction(txObj, ICON_WALLET);
    return await ICON_SERVICE.sendTransaction(signedTx).execute();
  } catch (e) {
    console.log(e);
    throw new Error("Error calling contract method");
  }
}

/*
 * getFee - calls the getFee method of the xcall contract
 * @param {boolean} useRollback - whether to use rollback
 * @returns {object} - the transaction receipt
 * @throws {Error} - if there is an error getting the fee
 */
async function getFee(useRollback = false) {
  try {
    const params = {
      _net: NETWORK_LABEL_DESTINATION,
      _rollback: useRollback ? "0x1" : "0x0"
    };

    const txObj = new CallBuilder()
      .to(XCALL_ORIGIN)
      .method("getFee")
      .params(params)
      .build();

    return await ICON_SERVICE.call(txObj).execute();
  } catch (e) {
    console.log("error getting fee", e);
    throw new Error("Error getting fee");
  }
}

/*
 * strToHex - converts a string to hex
 * @param {string} str - the string to convert
 * @returns {string} - the hex string
 * @throws {Error} - if there is an error converting the string
 */
function strToHex(str) {
  let hex = "";
  for (let i = 0; i < str.length; i++) {
    hex += "" + str.charCodeAt(i).toString(16);
  }
  return hex;
}

async function main() {
  const _to = `btp://${NETWORK_LABEL_DESTINATION}/${CONTRACT_DESTINATION}`;
  const _data = strToHex("Hello World");

  const receipt = await sendMessage(_to, _data);
  console.log(receipt);
}

main();
```

