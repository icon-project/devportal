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

**I**f execution fails on the destination chain, `handleCallMessage` may also need to be defined in the source chain Smart Contract A in order to handle the [executeRollback](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md#executerollback) invocation properly. For more information, see [Error handling](error-handling.md).

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

