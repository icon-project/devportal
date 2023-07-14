# Message lifecycle

### Simplified message lifecycle

The simplified lifecycle of an xCall message, only including what is relevant to most dApp developers, is as follows:

1. EOA initiates a cross-chain call by invoking the `sendCallMessage` method on the source chain's xCall contract.
2. Once the call is relayed to the destination chain, a `CallMessage` event is triggered on the destination chain's xCall contract.
3. EOA then respond to this event by invoking the `executeCall` method in the xCall Smart Contract on the destination chain.
4. The execution of this method triggers the target dApp's smart contract method.
5. If the call executes successfully, a `CallExecutedEvent` is emitted.
6. If an error occurs during execution, and rollback data was included in the original call, an error handling process is initiated, sending a `ResponseMessage` Event back to the source chain for state rollback operations. For more information, see [Troubleshooting](../troubleshooting.md).

In this process, the dApp developer handles [initiating the cross-chain call](../quickstart/sending-a-message-with-xcall.md), [responding to events](../guides/receiving-a-message.md), and [errors](../troubleshooting.md).

`sendCallMessage` is a `Payable` method and requires a fee. For more information, see [Handling Fees](../guides/handling-fees.md).

### Full message lifecycle

For the full message lifecycle, please see the [xCall specification](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md).

