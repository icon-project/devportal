# xCall

xCall is a standard interface to make permissionless calls between different blockchain networks. Each network that ICON is interoperable with has an xCall contract address that dApps can call to transfer data across chains.

Currently, permissionless calls are done using the blockchain transmission protocol (BTP). The testnet contract addresses for xCall can be found in [the BTP section](blockchain-transmission-protocol-btp.md#btp-testnet-network-information).

The ICON project plans to extend the interoperability protocols that work with xCall. For example, there is active development for [new integrations](https://github.com/icon-project/IBC-Integration) with the Cosmos inter-blockchain communication protocol (IBC) as smart contracts on ICON.

### Sending a message

dApps need to invoke the following method of the `xcall` contract on the source chain to send a call message to the destination chain.

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

When the `xcall` contract on the source chain receives the call request message, it sends the `_data` to BTP address `_to` on the destination chain.

The `_rollback` parameter is for handling error cases. If the `_rollback` parameter is not null, then the source chain will receive a response message back regardless of whether the destination chain execution of the message request fails.  For more information, see [Error Handling](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md#error-handling).

The `_data` parameter is an arbitrary payload defined by the DApp.

The `_to` parameter is the [BTP address](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md#btp-address) of the callee contract which receives the `_data` payload on the destination chain.

This method returns the serial number of the request. The serial number is the unique identifier of the message request on the source chain.

This method is payable, and dApps need to call this method with proper fees. dApps that want to make a call to `sendCallMessage` can query the total fee amount for the destination network via the `getFee` method, and then enclose the appropriate fees in the method call. For more information on fees, see [Fees Handling](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md#fees-handling).

```
/**
 * Gets the fee for delivering a message to the _net.
 * If the sender is going to provide rollback data, the _rollback param should set as true.
 * The returned fee is the sum of the protocol fee and the relay fee.
 *
 * @param _net The network address
 * @param _rollback Indicates whether it provides rollback data
 * @return the sum of the protocol fee and the relay fee
 */
@External(readonly=true)
BigInteger getFee(String _net, boolean _rollback);
```

The `_net` parameter is the [network address](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md#network-address) of the destination chain to query the fee for sending a message to.

The `_rollback` parameter indicates whether the fee calculation should include the cost of sending a rollback message from the destination chain to the source chain. The source chain is the chain that this `xcall` contract is on.

These fees are for the delivery of messages and do not include the cost to execute the call message or rollback message.

### Receiving a message

When the `xcall` contract on the destination chain receives the call request, it emits the following event.

```
/**
 * Notifies the user that a new call message has arrived.
 *
 * @param _from The BTP address of the caller on the source chain
 * @param _to A string representation of the callee address
 * @param _sn The serial number of the request from the source
 * @param _reqId The request id of the destination chain
 */
@EventLog(indexed=3)
void CallMessage(String _from, String _to, BigInteger _sn, BigInteger _reqId);
```

The user on the destination chain must invoke the following method on `xcall` with the given `_reqId` in order for the call to be executed.

```
/**
 * Executes the requested call message.
 *
 * @param _reqId The request Id
 */
@External
void executeCall(BigInteger _reqId);
```

The `_reqId` is the unique identifier of the message request on the destination chain.

The dApp on the destination chain must implement `handleCallMessage`. When the user calls the `executeCall` method, the `xcall` contract invokes the following predefined method in the target DApp with the calldata associated in the CallMessage `_reqId`.

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

### Source Chain Response Messages

If the `_rollback` parameter is not null, then the source chain will receive a response message back regardless of whether the destination chain execution of the message request fails.

```
/**
 * Notifies that a response message has arrived for the `_sn` if the request was a two-way message.
 *
 * @param _sn The serial number of the previous request
 * @param _code The response code
 *              (0: Success, -1: Unknown generic failure, >=1: User defined error code)
 * @param _msg The result message if any
 */
@EventLog(indexed=1)
void ResponseMessage(BigInteger _sn, int _code, String _msg);
```

The `_sn` parameter is the unique identifier of the message request on the source chain.

The `_code` parameter the error code of the exception raised when the execution failed on the destination chain.

### Error handling

dApps can optionally rollback execution on the source chain if execution of the call request has failed on the destination chain. When an error occurs on the destination chain and the `_rollback` parameter is not null, the `xcall` contract on the source chain emits the following event for notifying the user that an additional rollback operation is required.

```
/**
 * Notifies the user that a rollback operation is required for the request '_sn'.
 *
 * @param _sn The serial number of the previous request
 */
@EventLog(indexed=1)
void RollbackMessage(BigInteger _sn);
```

**xCall ABI docs and tutorials to be released soon.**

### Resources

* End-to-end demonstration of BTP containing xCall: [https://github.com/icon-project/btp2/tree/main/e2edemo](https://github.com/icon-project/btp2/tree/main/e2edemo)
* xCall Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md)
* IBC on ICON: [https://github.com/icon-project/IBC-Integration](https://github.com/icon-project/IBC-Integration)
* xCall testnet contracts: [#btp-testnet-network-information](blockchain-transmission-protocol-btp.md#btp-testnet-network-information "mention")
