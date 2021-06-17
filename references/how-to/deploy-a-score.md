# Deploy a SCORE

In this document, we will explain various methods of deploying a SCORE onto the mainnet, and how to estimate the step limit for the deploy transaction.

### Prerequisite

* Have an EOA account and the matching keystore file.
* ICX balance in your wallet.
* Understand the lifecycle of SCORE and the audit process. If you are not familiar with the concepts, please read [SCORE Audit: Deploy Guideline](doc:deploy-guideline). 

### Using T-Bears

To deploy a SCORE using T-Bears CLI, please follow the [deploy guideline](deploy-guideline#section-tbears-deploy-install).

### Using Python SDK

[Token Deploy Code Example](python-sdk#section-token-deploy-transaction) on Python SDK.

### Using Java SDK

[Token Deploy Code Example](java-sdk#section-token-deploy-transaction) on Java SDK.

### Using JavaScript SDK

[Token Deploy Code Example](javascript-sdk#section-token-deploy-transaction) on JavaScript SDK.

### On the CSS \(Contract Support System\)

TBA

### Step Estimation for SCORE Deploy

#### The formula

Required steps for SCORE deploy is as follows.

* Installing SCORE = DEFAULT + INPUT + CONTRACT\_SET + CONTRACT\_CREATE
* Updating SCORE = DEFAULT + INPUT + CONTRACT\_SET + CONTRACT\_UPDATE

Where each term in the right side of the equation is calculated as;

* DEFAULT = 100\_000 
* CONTRACT\_CREATE = 1\_000\_000\_000 
* CONTRACT\_UPDATE = 1\_600\_000\_000 
* CONTRACT\_SET = 30\_000 \* bytes of the SCORE zip file
* INPUT = 200 \* bytes of the `params.data` field in the JSON RPC [icx\_sendTransaction](icon-json-rpc-v3#section-icx_sendtransaction) request message

```javascript
// Example INPUT data
"data": {
     "contentType": "application/zip",
     "content": "0x1867291283973610982301923812873419826abcdef91827319263187263a7326e...", // compressed SCORE data
     "params": {  // parameters to be passed to on_install()
          "name": "ABCToken",
          "symbol": "abc",
          "decimals": "0x12"
      }
}
```

**Example**

Suppose we have a SCORE zip file of 1,528 bytes in size, and the INPUT data size is 1,546 bytes. Required steps for installing the SCORE is;

```text
Steps = DEFAULT + INPUT + CONTRACT_SET + CONTRACT_CREATE
   = 100,000 + (1,546 * 200) + (1,528 * 30,000)  + 1,000,000,000
   = 100,000 + 309,200 + 45,840,000 + 1,000,000,000
   = 1,046,249,200 
   = 0x3e5c7ef0
```

#### Step estimation using the JSON RPC API

[debug\_estimateStep](icon-json-rpc-v3#section-debug_estimatestep) API will estimate the required steps of the given transaction. You can create the transaction data without `stepLimit` and `signature`, and pass it to the API endpoint `<scheme>://<host>/api/debug/v3`. The transaction is not added to the blockchain but simply returns the estimated steps. Sample request messages will look like the bellows.

* SCORE install

```javascript
// Request message without stepLimit and signature
{
    "jsonrpc": "2.0",
    "method": "debug_estimateStep",
    "id": 1234,
    "params": {
        "version": "0x3",
        "from": "hxbe258ceb872e08851f1f59694dac2558708ece11",
        "to": "cx0000000000000000000000000000000000000000", // address 0 means SCORE install
        "timestamp": "0x563a6cf330136",
        "nid": "0x3",
        "nonce": "0x1",
        "dataType": "deploy",
        "data": {
            "contentType": "application/zip",
            "content": "0x1867291283973610982301923812873419826abcdef91827319263187263a7326e...", // compressed SCORE data
            "params": {  // parameters to be passed to on_install()
                "name": "ABCToken",
                "symbol": "abc",
                "decimals": "0x12"
            }
        }
    }
}
```

* SCORE update

```javascript
// Request message without stepLimit and signature
{
    "jsonrpc": "2.0",
    "method": "debug_estimateStep",
    "id": 1234,
    "params": {
        "version": "0x3",
        "from": "hxbe258ceb872e08851f1f59694dac2558708ece11",
        "to": "cxb0776ee37f5b45bfaea8cff1d8232fbb6122ec32", // SCORE address to be updated
        "timestamp": "0x563a6cf330136",
        "nid": "0x3",
        "nonce": "0x1",
        "dataType": "deploy",
        "data": {
            "contentType": "application/zip",
            "content": "0x1867291283973610982301923812873419826abcdef91827319263187263a7326e...", // compressed SCORE data
            "params": {  // parameters to be passed to on_update()
                "amount": "0x1234"
            }
        }
    }
}
```

