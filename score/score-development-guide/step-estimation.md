# Step Estimation



In your transaction request, you must specify the `stepLimit`, the maximum amount of steps you are willing to pay for the transaction. `stepLimit` must be greater than the actual `steps` required and less than the maximum value, `0x9502f900`.  
In this document, we will explain how you choose the right `stepLimit` for your transaction.

### Pre-reading

* [ICON Key Concepts: Transaction Fees](doc:transaction-fees) 

### Transaction Fee

Transaction fee is calculated as `usedStep * stepPrice`, where `stepPrice` is the ICX exchange rate, and `usedStep` is the sum of `stepCost` for each action processed in the transaction.

* If `usedStep` reaches the `stepLimit` before finishing the execution, the transaction will fail with `out of step` error, but the amount of `stepLimit` is deducted from your balance.  
* Before executing your transaction, your account must hold at least `stepLimit * stepPrice` amount of ICX. If you do not have sufficient ICX, your transaction will fail immediately.
* Therefore, it is important to set appropriate `stepLimit`, neither too large nor too small, to guarantee the successful execution of the transaction without wasting your ICX.

You can query the `stepCost` of each action, the maximum possible `stepLimit` value you can set, and the `stepPrice` to the Governance SCORE.

* SCORE Address - cx0000000000000000000000000000000000000001
* [getStepCost](https://github.com/icon-project/governance/blob/master/README.md#getstepcosts)
* [getMaxStepLimit](https://github.com/icon-project/governance/blob/master/README.md#getmaxsteplimit)
* [getStepPrice](https://github.com/icon-project/governance/blob/master/README.md#getstepprice)

```bash
$ cat stepcost.json 
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "icx_call",
    "params": {
        "to": "cx0000000000000000000000000000000000000001",
        "dataType": "call",
        "data": {
            "method": "getStepCosts"
        }
    }
}

$ curl -H "Content-Type: application/json" -d @stepcost.json https://bicon.net.solidwallet.io/api/v3 
{
    "jsonrpc": "2.0",
    "result": {
        "default": "0x186a0",
        "contractCall": "0x61a8",
        "contractCreate": "0x3b9aca00",
        "contractUpdate": "0x5f5e1000",
        "contractDestruct": "-0x11170",
        "contractSet": "0x7530",
        "get": "0x0",
        "set": "0x140",
        "replace": "0x50",
        "delete": "-0xf0",
        "input": "0xc8",
        "eventLog": "0x64",
        "apiCall": "0x0"
    },
    "id": 1
}
```

```bash
$ cat maxsteplimit.json 
{
    "jsonrpc": "2.0",
    "id": 2,
    "method": "icx_call",
    "params": {
        "to": "cx0000000000000000000000000000000000000001",
        "dataType": "call",
        "data": {
            "method": "getMaxStepLimit",
            "params": {
                "contextType": "invoke"
            }
        }
    }
}

$ curl -H "Content-Type: application/json" -d @maxsteplimit.json https://bicon.net.solidwallet.io/api/v3 
{
    "jsonrpc": "2.0",
    "result": "0x9502f900",
    "id": 2
}
```

```bash
$ cat stepprice.json 
{
    "jsonrpc": "2.0",
    "id": 3,
    "method": "icx_call",
    "params": {
        "to": "cx0000000000000000000000000000000000000001",
        "dataType": "call",
        "data": {
            "method": "getStepPrice"
            }
        }
    }
}

$ curl -H "Content-Type: application/json" -d @stepprice.json https://bicon.net.solidwallet.io/api/v3 
{
    "jsonrpc": "2.0",
    "result": "0x2540be400",
    "id": 3
}
```

### Step Estimation using the JSON RPC API

[debug\_estimateStep](icon-json-rpc-v3#section-debug_estimatestep) API will estimate the required steps of the given transaction. You can create the transaction data without `stepLimit` and `signature`, and pass it to the API endpoint `<scheme>://<host>/api/debug/v3`. The transaction is not added to the blockchain but simply returns the estimated steps. Sample request messages will look like the bellows.

```javascript
// transaction request - send ICX
{
    "jsonrpc": "2.0",
    "method": "icx_sendTransaction",
    "id": 1234,
    "params": {
        "version": "0x3",
        "from": "hxbe258ceb872e08851f1f59694dac2558708ece11",
        "to": "hx5bfdb090f43a808005ffc27c25b213145e80b7cd",
        "value": "0xde0b6b3a7640000",
        "timestamp": "0x563a6cf330136",
        "nid": "0x3",
        "nonce": "0x1",
        "stepLimit": "0x30d40",
        "signature": "rBaH0U6y85y1CWp/JbalUbzLVGjtGYG+hut/G5o30vBxhWoxPYtSYBQu6X0Tak1SdcnlZSCJL7DeOeKmI4y+5wE="
    }
}

// debug.json for step estimation request - send ICX
{
    "jsonrpc": "2.0",
    "method": "debug_estimateStep",
    "id": 1234,
    "params": {
        "version": "0x3",
        "from": "hxbe258ceb872e08851f1f59694dac2558708ece11",
        "to": "hx5bfdb090f43a808005ffc27c25b213145e80b7cd",
        "value": "0xde0b6b3a7640000",
        "timestamp": "0x563a6cf330136",
        "nid": "0x3",
        "nonce": "0x1"
    }
}
```

Request debug\_estimateStep using curl from CLI

```text
$ curl -H "Content-Type: application/json" -d @debug.json https://bicon.net.solidwallet.io/api/debug/v3
{"jsonrpc": "2.0", "result": "0x186a0", "id": 1234}
```

### Reference

* [How to estimate required step](doc:how-to-estimate-required-step) 
* [ICON JSON-RPC v3 Specification](doc:icon-json-rpc-v3)

