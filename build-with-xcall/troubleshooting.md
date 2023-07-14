# Error Handling

If an error occurs during the execution of the call request on the destination chain, and a rollback parameter was provided in the initial `sendCallMessage` method, the xCall contract on the source chain emits a `RollbackMessage` event. This event is triggered when an error occurs on the destination chain and a rollback operation is needed. The `RollbackMessage` event includes the serial number of the original request that needs to be rolled back.

The EOA on the source chain, after observing the `RollbackMessage` event, needs to invoke the `executeRollback` method on the xCall contract. This is done by passing the serial number of the original request that needs to be rolled back as an argument the same way as `executeCall`.

The destination chain does not pass back rollback instructions. These are defined up front in the `_rollback` parameter in the `sendCallMessage` method.

After the rollback operation is executed, the `RollbackExecuted` event is emitted by the xCall contract. This event notifies that the rollback has been executed and includes the serial number for the rollback, the execution result code, and a result message if any.

