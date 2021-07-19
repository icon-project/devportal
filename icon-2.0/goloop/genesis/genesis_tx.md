# Transaction

## Introduction

This document specifies the genesis file format.

## Value Types

| Value type | Description | Example |
| :--- | :--- | :--- |
| T\_ADDR\_EOA | "hx" + 40-digit HEX string | `"hxbe258ceb872e08851f1f59694dac2558708ece11"` |
| T\_ADDR\_SCORE | "cx" + 40-digit HEX string | `"cxb0776ee37f5b45bfaea8cff1d8232fbb6122ec32"` |
| T\_HASH | "0x" + 64-digit HEX string | `"0xc71303ef8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"` |
| T\_INT | "0x" + lowercase HEX string | `"0xa"` |
| T\_ARRAY | json arrays | `[ “0x1234567890”, “0x2345678990” ]` |
| T\_BOOLEAN | "0x1" or "0x0" | `"0x1"` |
| T\_STRING | json string | `"test string"` |
| T\_BYTES | "0x" + lowercase HEX string | `"0x112233445566..."` |
| T\_DICT | json dictionary | `{ "supply": "0x1" }` |

## Parameters

* `accounts` \(required, T\_ARRAY\)   It contains EOA \(Externally Owned Account\) or CA \(Contract Account\) information. Determines who starts out with how much balance or which application is pre-installed when the blockchain starts.
  * account \(T\_DICT\)
    * `name` \(T\_STRING, default=`null`\)   It is a name of the account. It is required to define a special account such as `god`, `treasury` or `governance`, otherwise it is optional only for account aliases.
    * `address` \(T\_ADDR\_EOA or T\_ADDR\_SCORE\)   The address of the account.
    * `balance` \(T\_INT, default=`"0x0"`\)   Initial balance of the account.
    * `score` \(T\_DICT, default=`null`\)   Required if the account \(CA\) has code to be pre-installed.
      * `owner` \(T\_ADDR\_EOA\)   The owner address of the contract.
      * `contentType` \(T\_STRING\)   MIME type of the content. `application/zip` is for user Python SCORE and `application/java` is for user Java SCORE, while `application/x.score.system` is used for system SCORE.
      * `contentId` \(T\_STRING, replace `content`\)   
         The content URI.

        | Prefix | Description | Sample |
        | :--- | :--- | :--- |
        | `hash:` | Used for hashable SCORE | `"hash:0x1234567890abcdef...e"` |
        | `cid:` | Used for system SCORE | `"cid:multisig/r1"` |

      * `content` \(T\_BYTES, replace `contentId`\)   Hex string contains bytes of compressed codes.
      * `params` \(T\_DICT, default=`null`\)   Parameters that will be passed to the SCORE initialization method \(`on_install()` in Python, `<init>()` in Java\).
* `chain` \(T\_DICT, default=`null`\)
  * `revision` \(T\_INT, default=`"0x8"`\)   Initial revision.
  * `auditEnabled` \(T\_BOOLEAN, default=`"0x0"`\)   Determines whether audit is required. Default is false.
  * `deployerWhiteListEnabled` \(T\_BOOLEAN, default=`"0x0"`\)   Determines whether only white-listed deployers can deploy SCOREs. Default is false.
  * `fee` \(T\_DICT,default=`null`\)
    * `stepPrice` \(T\_INT, default=`"0x0"`\)   The price of one step. Fee is the product of `stepPrice` and steps used.
    * `stepLimit` \(T\_DICT, default=`null`\)   Maximum step allowance for the request type. If it's not specified, both values are set as zero \(`"0x0"`\)
      * `invoke` \(T\_INT\)
      * `query` \(T\_INT\)
    * `stepCosts` \(T\_DICT, default=`null`\)   The cost of each step type. If it's not specified, all values are set as zero \(`"0x0"`\).
      * `default` \(T\_INT\)
      * `contractCall` \(T\_INT\)
      * `contractCreate` \(T\_INT\)
      * `contractUpdatee` \(T\_INT\)
      * `contractDestruct` \(T\_INT\)
      * `contractSet` \(T\_INT\)
      * `get` \(T\_INT\)
      * `set` \(T\_INT\)
      * `replace` \(T\_INT\)
      * `delete` \(T\_INT\)
      * `input` \(T\_INT\)
      * `eventLog` \(T\_INT\)
      * `apiCall` \(T\_INT\)
      * `defaultGet` \(T\_INT\)
      * `defaultSet` \(T\_INT\)
      * `replaceBase` \(T\_INT\)
      * `defaultDelete` \(T\_INT\)
      * `eventLogBase` \(T\_INT\)
  * `validatorList` \(T\_ARRAY, default=`[]`\)   The list of addresses participating in the consensus. It will not work if it is empty.
    * validator \(T\_ADDR\_EOA\)
  * `memberList` \(T\_ARRAY, default=`[]`\)   The list of addresses participating in the network. If it is empty, the server accepts all network connections.
    * member \(T\_ADDR\_EOA\)
  * `blockInterval` \(T\_INT\)   Block generation interval in msec.
  * `commitTimeout` \(T\_INT\)   
     Commit timeout \(timeout before propose after commit\) in msec.

    If `blockInterval` is specified, then it assumes `commitTimeout` as minimum commit timeout value. If minimum commit timeout value is bigger than `blockInterval`, then it uses value of `blockInterval`. Default value of minimum commit timeout is 200ms.

    Otherwise, it uses `commitTimeout` as block interval and minimum commit timeout. If both of them are not specified, then it uses system default values \(1000ms for block interval, 200ms for minimum commit timeout\).

  * `timestampThreshold` \(T\_INT\)   Allowed timestamp threshold in msec between the block and the transaction. If it's not specified, it uses system default value. System default value can be updated when the node is updated.
  * `minimizeBlockGen` \(T\_BOOL, default=`"0x0"`\)   If it's set as true \(`"0x1"`\), the generation of empty block will be minimized.
  * `roundLimitFactor` \(T\_INT, default=`"0x0"`\)   If it's set as non-zero value, it tries to skip execution of transactions of previous block when consensus round of the height exceeds round limit. Round limit is \(`roundLimitFactor` \* validators + 2 \) / 3.
* `message` \(T\_STRING, default=`null`\)   A message to be recorded in the genesis. It's used to prevent having same network ID from similar configuration.
* `nid` \(T\_INT, default=`null`\)   Network ID for the network. Normally, it shouldn't be set. In that case, it uses calculated network ID from genesis.

## Example

```javascript
{
 "accounts": [
   {
     "name": "god",
     "address": "hxff9221db215ce1a511cbe0a12ff9eb70be4e5764",
     "balance": "0x2961fff8ca4a62327800000"
   },
   {
     "name": "treasury",
     "address": "hx1000000000000000000000000000000000000000",
     "balance": "0x0"
   },
   {
     "name:": "governance",
     "address": "cx0000000000000000000000000000000000000001",
     "score": {
       "owner": "hx609c1c454528bae228514ceccec0c0939637a3fb",
       "contentType": "application/zip",
       "contentId": "hash:0x23cx01af5570f5a1810b7af78caf4bc70a660f0df51e42baf91d4de5b2328de0",
       "params": {
         "governor": "hx11cbe0a213e5a10e7926c4aa5943093f9221db2a"
       }
     }
   },
   {
     "name": "multisig",
     "address": "cx0000000000000000000000000000000000000002",
     "score": {
       "owner": "hx609c1c454528bae228514ceccec0c0939637a3fb",
       "contentType": "application/zip",
       "contentId": "hash:0x810b7af78caf4bc70a660f0df51e42baf91d4de5b2328de0e83dfc56fd70a6cb",
       "params": {
         "maxMember": "0x10"
       }
     }
   }
 ],
 "chain" : {
   "revision": "0x8",
   "auditEnabled": "0x1",
   "deployerWhiteListEnabled": "0x0",
   "fee": {
     "stepPrice": "0x2540be400",
     "stepLimit": {
       "invoke": "0x9502f900",
       "query": "0x2faf080"
     },
     "stepCosts": {
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
       "apiCall": "0x2710"
     }
   },
   "validatorList": [
     "hx4805489d4fd3c07fea9b7e1b210e7926c4aa5943",
     "hx6903484805487fea9b8054c07fea9b7e54c07fef",
     "hx7fea9b7e54c5487fee54c543902f009aab312300",
     "hxdef10990388eeefab3827980e083e028f08f8aaa",
     "hxee01910d0f0a90b00de30999f099db9babd9e255"
   ]
 }
}
```

