# Sending a message with xCall (with rollback)

### Prerequisites

For the case of showcasing sending a message with rollback please follow the prerequisites and instructions on the [previous tutorial](sending-a-message-with-xcall.md).

### Rollback

The rollback functionality of xCall allows the developer to design specific situations in their smart contract DApp on which the process can fail, examples of this can be not having enough balance of a certain token locked in the destination chain to be able to mint some other token in the case of doing a cross chain transaction. \
\
The developer is free to setup their own fail conditions, and once they are trigger is necessary to call the native 'revert' handle of the execution environment to initiate the rollback process.\
\
For the case of the example shown in the [e2edemo of the btp2 repo](https://github.com/icon-project/btp2/tree/main/e2edemo), we can see that the [smart contract calls the `Context.revert()`](https://github.com/icon-project/btp2-java/blob/9acff597162ec66aef343e2c06cc607e0aa10dfa/dapp-sample/src/main/java/foundation/icon/btp/xcall/sample/DAppProxySample.java#L103) handler when the data in the message is equal to the string literal "revertMessage"\


```java
 @Override
 @External
    public void handleCallMessage(String _from, byte[] _data) {
        onlyCallService();
        Context.println("handleCallMessage: from=" + _from);
        if (callSvcBtpAddr.equals(_from)) {
            // handle rollback data here
            // In this example, just compare it with the stored one.
            RollbackData received = RollbackData.fromBytes(_data);
            var id = received.getId();
            RollbackData stored = rollbacks.get(id);
            Context.require(stored != null, "invalid received id");
            Context.require(received.equals(stored), "rollbackData mismatch");
            rollbacks.set(id, null); // cleanup
            RollbackDataReceived(_from, stored.getSvcSn(), received.getRollback());
        } else {
            // normal message delivery
            String msgData = new String(_data);
            Context.println("handleCallMessage: msgData=" + msgData);
            if ("revertMessage".equals(msgData)) {
                Context.revert("revertFromDApp");
            }
            MessageReceived(_from, _data);
        }
    }
```

calling the revert handler of the specific execution environment stops the execution and refunds any unused gas.

### ResponseMessage and RollbackMessage events

For two way communication using xCall (whenever the \_`rollback` param is not null) there are 2 events associated that can be raised.

A `ResponseMessage` event will be raised in the source chain every time the `_rollback` param is not null regardless of if an error occurred or not.

> **`@EventLog(indexed=1) void`**
>
> **`ResponseMessage(BigInteger _sn, int _code, string _msg)`**
>
> Notifies that a response message has arrived for the `_sn` if the request was a two way message.
>
> * **Parameters:**
>   * `_sn` — The serial number of the request
>   * `_code` — The response code ( 0: Success, -1: Unknown generic failure, >= 1 User defined error code)
>   * `_msg`  — The result message if any

A `RollbackMessage` event will be raised in the source chain when the `_rollback` param is not null and an error has occurred (a revert has been called either with `Context.revert()` on JVM or `revert` on EVM chains.

> **`@EventLog(indexed=1) void`**
>
> **`RollbackMessage(BigInteger _sn)`**
>
> Notifies the user that a rollback operation is required for the request `_sn`.
>
> * **Parameters:**
>   * `_sn` — The serial number of the request

A code example of how to wait for these events on an ICON chain can be seen in the [xcall sample dapp](https://github.com/FidelVe/xcall-sample-dapp) used for this tutorial.

```javascript
async function waitResponseMessageEvent(id, blocksToWait = 5) {
  const sig = "ResponseMessage(int,int,str)";
  const parseId = id.toHexString();
  return await waitEventICON(sig, ICON_XCALL_ADDRESS, parseId, blocksToWait);
}

async function waitRollbackMessageEvent(id, blocksToWait = 20) {
  const sig = "RollbackMessage(int)";
  const parseId = id.toHexString();
  return await waitEventICON(sig, ICON_XCALL_ADDRESS, parseId, blocksToWait);
}

async function waitRollbackExecutedEvent(id, blocksToWait = 20) {
  const sig = "RollbackExecuted(int,int,str)";
  const parseId = id.toHexString();
  return await waitEventICON(sig, ICON_XCALL_ADDRESS, parseId, blocksToWait);
}
async function waitEventICON(sig, address, id, blocksToWait = 5) {
  console.log(`## Waiting for event ${sig} on ${address} with id ${id}`);
  const maxBlocksToCheck = blocksToWait;
  let blocksChecked = 0;
  const latestBlock = await getBlockICON("latest");
  let blockNumber = latestBlock.height - 2;

  while (blocksChecked < maxBlocksToCheck) {
    blockNumber = blockNumber + 1;
    const latestBlock = await getBlockICON("latest");
    if (blockNumber > latestBlock.height) {
      await sleep(1000);
      blockNumber = blockNumber - 1;
      continue;
    }
    console.log(`## Fetching block ${blockNumber} for event`);
    const block = await getBlockICON(blockNumber);
    const txsInBlock = await getTransactionsFromBlock(block);
    for (const tx of txsInBlock) {
      const filteredEvents = filterEventICON(tx.eventLogs, sig, address);
      if (filteredEvents.length > 0) {
        for (const event of filteredEvents) {
          const idNumber = parseInt(id);
          const eventIdNumber = parseInt(event.indexed[1]);
          if (eventIdNumber == idNumber) {
            return event;
          }
        }
      }
      blocksChecked++;
    }
  }

  return null;
}
```

### Calling ExecuteRollback

Once the `RollbackMessage` event has been raised, confirming that a rollback operation is required we can execute the rollback by calling the `executeRollback` method of the xCall contract on the source chain.

> **`@External void`**
>
> **`executeRollback(BigInteger _sn)`**
>
> Rollbacks the caller state of the request `_sn`
>
> * **Parameters:**
>   * `_sn` — The serial number of the request

The following function showcases how this transaction is being signed in our xcall sample dapp.

```javascript
async function executeRollbackICON(id) {
  try {
    const wallet = ICON_SIGNER;
    const params = {
      _sn: id.toHexString()
    };

    const txObj = new CallTransactionBuilder()
      .from(wallet.getAddress())
      .to(ICON_XCALL_ADDRESS)
      .stepLimit(IconConverter.toBigNumber(2000000))
      .nid(IconConverter.toBigNumber(ICON_RPC_NID))
      .nonce(IconConverter.toBigNumber(1))
      .version(IconConverter.toBigNumber(3))
      .timestamp(new Date().getTime() * 1000)
      .method("executeRollback")
      .params(params)
      .build();

    const signedTransaction = new SignedTransaction(txObj, wallet);
    console.log("## executeRollback signed transaction");
    console.log(signedTransaction.getRawTransaction());
    return await ICON_SERVICE.sendTransaction(signedTransaction).execute();
  } catch (e) {
    console.log("Error running executeRollback");
    throw new Error(e);
  }
}
```
