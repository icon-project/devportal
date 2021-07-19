# Invoking SCORE Functions

In the previous document, [Deploying your SCORE](deploying-your-score.md), we deployed the SampleToken. Now we will invoke functions of SampleToken using T-Bears.

### Prerequisite

We assume that you already have installed T-Bears. If you have not yet installed it, please read [SCORE Quickstart](../quickstart/) or [T-Bears Installation](../../tbears/installation.md) first.

* [SCORE Quickstart](../quickstart/)
* [Token & Crowdsale](../sample-scores/token-and-crowdsale.md)
* [Deploying your SCORE](deploying-your-score.md) 
* [T-Bears CLI Commands](../../tbears/cli-commands.md) 

### Purpose

Understand how to invoke SCORE functions.

### Invoking SCORE methods

#### Invoking read-only methods

SCORE methods can be executed by calling [JSON RPC APIs](../../references/reference-manuals/icon-json-rpc-api-v3-specification.md) to ICON nodes. You need to generate a JSON file which contains information about the calling method and its parameters. `icx_call` JSON-RPC API will be used to invoke a read-only method of SCORE.

* Read-only method does not make state transition, therefore, the request message does not need to be signed. 
* You can send the request using curl, T-Bears CLI, or SDKs \([Java SDK](../../icon-sdks/java-sdk/), [JavaScript SDK](../../icon-sdks/javascript/), [Python SDK](../../icon-sdks/python-sdk/), [Swift SDK](../../icon-sdks/swift-sdk/)\)

**Getting total supply**

Make a JSON file for calling `totalSupply` in SampleToken as follows.

```javascript
# totalsupply.json
{
    "jsonrpc": "2.0",
    "method": "icx_call",
    "id": 1,
    "params": {
        "to": "cx0841205d73b93aec1062877d8d4d5ea54c6665bb",
        "dataType": "call",
        "data": {
            "method": "totalSupply"
        }
    }
}
```

You can use T-Bears `call` command to send the request. `tbears call` commands implements the `icx_call` JSON-RPC protocol.

```bash
$ tbears call totalsupply.json
response : {
    "jsonrpc": "2.0",
    "result": "0x3635c9adc5dea00000",
    "id": 1
}
```

**Getting balance of specific address**

Make another JSON file for calling `balanceOf` in SampleToken. In this case, you need to specify `_owner` parameter for the `balanceOf` method.

```javascript
# balanceof.json
{
    "jsonrpc": "2.0",
    "method": "icx_call",
    "id": 1,
    "params": {
        "to": "cx0841205d73b93aec1062877d8d4d5ea54c6665bb",
        "dataType": "call",
        "data": {
            "method": "balanceOf",
            "params": {
                "_owner": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6"
            }
        }
    }
}
```

Again, you can use T-Bears `call` command to send the request.

```bash
$ tbears call balanceof.json
response : {
    "jsonrpc": "2.0",
    "result": "0x0",
    "id": 1
}
```

The result of calling `balanceOf` is 0, that means the balance of address `hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6` is 0.

#### Invoking writable methods

The JSON RPC method, `icx_sendTransaction`, is used for invoking writable SCORE methods.

The writable methods can change the state of the smart contract. So the signature should be presented in the JSON RPC request to prove the transaction was originated by the `from` account. `to` is the address of the smart contract.

* Writable methods cause state transition, therefore, the request message must be signed.
* It is very difficult to invoke the writable function without using T-Bears CLI or SDK, because you need to sign the message. T-Bears CLI and SDKs will sign the message using your keystore file.  Use the SDK you are familiar with to send the transaction. \([Java SDK](../../icon-sdks/java-sdk/), [JavaScript SDK](../../icon-sdks/javascript/), [Python SDK](../../icon-sdks/python-sdk/), [Swift SDK](../../icon-sdks/swift-sdk/), [T-Bears CLI](../../tbears/cli-commands.md)\)

The following example request is transferring 1 token to `hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6` from `hxe7af5fcfd8dfc67530a01a0e403882687528dfcb`.

```javascript
# sendtoken.json
{
    "jsonrpc": "2.0",
    "method": "icx_sendTransaction",
    "params": {
        "version": "0x3",
        "from": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
        "value": "0x0",
        "stepLimit": "0x3000000",
        "timestamp": "0x573117f1d6568",
        "nid": "0x3",
        "nonce": "0x1",
        "to": "cx0841205d73b93aec1062877d8d4d5ea54c6665bb",
        "signature": "",
        "dataType": "call",
        "data": {
            "method": "transfer",
            "params": {
                "_to": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6",
                "_value": "0xde0b6b3a7640000"
            }
        }
    },
    "id": 1
}
```

You can use T-Bears `sendtx` command for calling writable SCORE methods. You need to input an appropriate password of the keystore file, `keystore_test1`.

```bash
$ tbears sendtx sendtoken.json -k keystore_test1
Input your keystore password: 
Send transaction request successfully.
transaction hash: 0x41bf7b9ada89eb938ee4e36fce02ec86b3e68c1ceefd61decfe1e3dcc7df43a5
```

Getting the transaction hash itself in the response of sending transaction does not mean the successful execution of the transaction. You need to check the actual result of the transaction by calling `icx_getTransactionResult` JSON RPC API. With T-Bears, you can use `txresult` command.

```bash
$ tbears txresult 0x41bf7b9ada89eb938ee4e36fce02ec86b3e68c1ceefd61decfe1e3dcc7df43a5
Transaction result: {
    "jsonrpc": "2.0",
    "result": {
        "txHash": "0x41bf7b9ada89eb938ee4e36fce02ec86b3e68c1ceefd61decfe1e3dcc7df43a5",
        "blockHeight": "0x4c",
        "blockHash": "0x7f53737f9ead2a04afe72d90bef072d49fa74c1f37b4029801e787aa15d37895",
        "txIndex": "0x0",
        "to": "cx0841205d73b93aec1062877d8d4d5ea54c6665bb",
        "stepUsed": "0xfdb2e",
        "stepPrice": "0x0",
        "cumulativeStepUsed": "0xfdb2e",
        "eventLogs": [
            {
                "scoreAddress": "cx0841205d73b93aec1062877d8d4d5ea54c6665bb",
                "indexed": [
                    "Transfer(Address,Address,int,bytes)",
                    "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
                    "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6",
                    "0xde0b6b3a7640000"
                ],
                "data": [
                    "0x4e6f6e65"
                ]
            }
        ],
        "logsBloom": "0x00000000000000100000002000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000100000040000000000000000200000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000000100000002000000000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000080000000000000000000000000000000000000000",
        "status": "0x1"
    },
    "id": 1
}
```

The `status` field in the result indicates whether the transaction was succeeded or not \(0x1 on success, 0x0 on failure\).

### Ref: Request Message Format

Following the above examples, you should be able to successfully invoke SampleToken methods. In this section, we will present the JSON-RPC request message formats in detail. This is a partial copy from the full JSON-RPC specification.

#### Read-Only method

**JSON Format**

| KEY | VALUE type | Description |
| :--- | :--- | :--- |
| from | T\_ADDR\_EOA | Message sender's address. |
| to | T\_ADDR\_SCORE | SCORE address that will handle the message. |
| dataType | T\_DATA\_TYPE | `call` is the only possible data type. |
| data | T\_DICT |  |
| data.method | String | Name of the function. |
| data.params | T\_DICT | Parameters to be passed to the function. |

**Returns**

* Values returned by the executed SCORE function.

**Example**

```javascript
// Request
{
    "jsonrpc": "2.0",
    "method": "icx_call",
    "id": 1234,
    "params": {
        "from": "hxbe258ceb872e08851f1f59694dac2558708ece11",
        "to": "cxb0776ee37f5b45bfaea8cff1d8232fbb6122ec32",
        "dataType": "call",
        "data": {
            "method": "get_balance",
            "params": {
                "address": "hx1f9a3310f60a03934b917509c86442db703cbd52"
            }
        }
    }
}

// Response - success
{
    "jsonrpc": "2.0",
    "id": 1234,
    "result": "0x2961fff8ca4a62327800000"
}

// Response - error
{
    "jsonrpc": "2.0",
    "id": 1234,
    "error": {
        "code": -32602,
        "message": "Invalid params"
    }
}
```

#### Writable method

**JSON format**

| KEY | [VALUE type](../../references/reference-manuals/icon-json-rpc-api-v3-specification.md#value-types) | Required | Description |
| :--- | :--- | :---: | :--- |
| version | T\_INT | required | Protocol version \(`0x3` for V3\) |
| from | T\_ADDR\_EOA | required | EOA address that created the transaction |
| to | T\_ADDR\_EOA or T\_ADDR\_SCORE | required | EOA address to receive coins, or SCORE address to execute the transaction. |
| value | T\_INT | optional | Amount of ICX coins in loop to transfer. When omitted, assumes 0. \(1 icx = 1 ^ 18 loop\) |
| stepLimit | T\_INT | required | Maximum step allowance that can be used by the transaction. |
| timestamp | T\_INT | required | Transaction creation time. timestamp is in microsecond. |
| nid | T\_INT | required | Network ID \(`0x1` for Mainnet, `0x2` for Testnet, etc\) |
| nonce | T\_INT | optional | An arbitrary number used to prevent transaction hash collision. |
| signature | T\_SIG | required | Signature of the transaction. |
| dataType | T\_DATA\_TYPE | optional | Type of data. \(call, deploy, or message\) |
| data | T\_DICT or String | optional | The content of data varies depending on the dataType. |
| data.method | String | required | Name of the functions to invoke in SCORE |
| data.params | T\_DICT | optional | Function parameters |

**Returns**

* Transaction hash \(T\_HASH\) on success
* Error code and message on failure

**Example**

* **SCORE function call**

```javascript
// Request
{
    "jsonrpc": "2.0",
    "method": "icx_sendTransaction",
    "id": 1234,
    "params": {
        "version": "0x3",
        "from": "hxbe258ceb872e08851f1f59694dac2558708ece11",
        "to": "cxb0776ee37f5b45bfaea8cff1d8232fbb6122ec32",
        "stepLimit": "0x12345",
        "timestamp": "0x563a6cf330136",
        "nid": "0x3",
        "nonce": "0x1",
        "signature": "VAia7YZ2Ji6igKWzjR2YsGa2m53nKPrfK7uXYW78QLE+ATehAVZPC40szvAiA6NEU5gCYB4c4qaQzqDh2ugcHgA=",
        "dataType": "call",
        "data": {
            "method": "transfer",
            "params": {
                "to": "hxab2d8215eab14bc6bdd8bfb2c8151257032ecd8b",
                "value": "0x1"
            }
        }
    }
}
```

* **Responses**

```javascript
// Response - success
{
    "jsonrpc": "2.0",
    "id": 1234,
    "result": "0x4bf74e6aeeb43bde5dc8d5b62537a33ac8eb7605ebbdb51b015c1881b45b3aed" // transaction hash
}

// Response - error
{
    "jsonrpc": "2.0",
    "id": 1234,
    "error": {
        "code": -32600,
        "message": "Invalid signature"
    }
}

// Response - error
{
    "jsonrpc": "2.0",
    "id": 1234,
    "error": {
        "code": -32601,
        "message": "Method not found"
    }
}
```

### References

* [ICON JSON-RPC v3 Specification](../../references/reference-manuals/icon-json-rpc-api-v3-specification.md)
* [T-Bears CLI Reference](../../tbears/cli-commands.md)
* [Java SDK](../../icon-sdks/java-sdk/) 
* [JavaScript SDK](../../icon-sdks/javascript/) 
* [Python SDK](../../icon-sdks/python-sdk/) 
* [Swift SDK](../../icon-sdks/swift-sdk/)

