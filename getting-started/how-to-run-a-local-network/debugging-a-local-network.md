# Debugging a local network

### Enable debug\_getTrace RPC method

The `debug_getTrace` RPC method allows a developer to get a trace call of a transaction execution for debugging purposes, this configuration can be enable once the node is running by executing the following command using the `goloop cli`

```bash
goloop system config rpcIncludeDebug true
```
