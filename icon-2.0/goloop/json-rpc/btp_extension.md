---
title: Extension for BTP
---

# JSON-RPC v3

## Introduction

Blockchain system requirements of BTP are described in the document

Summarize the document to following items.

* A method to get the receipt with the proof path from the block
  * The block contains the root of receipts
  * API to get receipt including proof
* A method to detect validator change.
  * The block contains the root of next validators.
  * API to get the block
  * API to monitor the blockchain
* A method to get validators related with the block
  * The block contains the root of next validators
  * API to get validators for the root
* A method to get votes for the block
  * API to get votes for the block
* A method to detect events
  * The block contains logsbloom related to events.
  * API to monitor events

## Monitor with Websocket

### Block

`GET /api/v3/:channel/block`

> Request

```javascript
{
  "height": "0x10",
  "eventFilters": [
    {
      "addr": "cx49894fa5aec4d662e49934f297673cf08dd9f382",
      "event": "Event(int,bytes,int,Address)",
      "indexed": [
        null,
        "0xda12"
      ],
      "data": [
        "0x2",
        "hxb51a65420ce5199e538f21fc614eacf4234454fe"
      ]
    }
  ]
}
```

#### Parameters

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| height | T\_INT | true | Start height |
| eventFilters | Array | false | Array of EventFilter\(JSON Object type, see [Events Parameters](btp_extension.md#eventsparameters)\) |

> Success Responses

```javascript
{
  "code": 0
}
```

> Failure Response

```javascript
{
  "code": -32602,
  "message": "Bad params"
}
```

#### Responses

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| code | Number | true | 0 or JSON RPC error code. 0 means success. |
| message | String | false | error message. |

> Example notification

```javascript
{
  "hash": "0xabc...",
  "height": "0x10",
  "indexes": [
    [
      "0x0"
    ]
  ],
  "events": [
    [
      ["0x0"]
    ]
  ]
}
```

#### Notification

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| hash | T\_HASH | true | The hash of the new block |
| height | T\_INT | true | The height of the new block |
| indexes | Array | false | Array of array of [index](btp_extension.md#resultindex)es of the results of filtered events in the block ordered by EventFilter and index |
| events | Array | false | Array of array of [events](btp_extension.md#eventlist), the array of event indexes in the result, ordered by EventFilter and index |

### Events

`GET /api/v3/:channel/event`

> Request

```javascript
{
  "height": "0x10",
  "addr": "cx49894fa5aec4d662e49934f297673cf08dd9f382",
  "event": "Event(int,bytes,int,Address)",
  "indexed": [
      null,
      "0xda12"
  ],
  "data": [
      "0x2",
      "hxb51a65420ce5199e538f21fc614eacf4234454fe"
  ]
}
```

#### [Parameters](btp_extension.md) <a id="eventsparameters"></a>

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| height | T\_INT | true | Start height |
| addr | T\_ADDR | false | SCORE address of Event |
| event | String | true | Event signature |
| [indexed](btp_extension.md) | Array | false | Array of arguments to match with indexed parameters of event. null matches any value. |
| data | Array | false | Array of arguments to match with not indexed parameters of event. null matches any value. If indexed parameters of event are exists, require ['indexed'](btp_extension.md#eventsindexed) parameter |

> Success Responses

```javascript
{
  "code": 0
}
```

> Failure Response

```javascript
{
  "code": -32602,
  "message": "Bad params"
}
```

#### Responses

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| code | Number | true | 0 or JSON RPC error code. 0 means success. |
| message | String | false | error message. |

> Example notification

```javascript
{
  "hash": "0xdbc...",
  "height": "0x11",
  "index": "0x0",
  "events": [ "0x0" ]
}
```

#### Notification

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| hash | T\_HASH | true | Hash of the block including the events |
| height | T\_INT | true | Height of the block including the events |
| [index](btp_extension.md) | T\_INT | true | Index of the result including the events in the block |
| [events](btp_extension.md) | Array | true | List of indexes of the event in the result |

You may use `hash` and `index` to get proof of the result including the events\(`icx_getProofForResult`\). You may use `hash`, `index` and `events` to get proofs of the result and the events\(`icx_getProofForEvents`\).

## Extended JSON-RPC Methods

### icx\_getDataByHash

Get data by hash.

It can be used to retrieve data based on the hash algorithm \(SHA3-256\).

Following data can be retrieved by a hash.

* BlockHeader with the hash of the block
* Validators with BlockHeader.NextValidatorsHash
* Votes with BlockHeader.VotesHash
* etc…

> Request

```javascript
{
  "id": 1001,
  "jsonrpc": "2.0",
  "method": "icx_getDataByHash",
  "params": {
      "hash": "0x1fcf7c34dc875681761bdaa5d75d770e78e8166b5c4f06c226c53300cbe85f57"
  }
}
```

#### Parameters

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| hash | T\_HASH | true | The hash value of the data to retrieve. |

> Example responses

```javascript
{
  "id": 1001,
  "jsonrpc": "2.0",
  "result": ""
}
```

> default Response

```javascript
{
  "id": 1001,
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Something went wrong."
  }
}
```

#### Responses

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | OK | Success | Data : base64 encoded bytes |
| default | Default | JSON-RPC Error | Error Response |

### icx\_getBlockHeaderByHeight

Get block header for specified height.

> Request

```javascript
{
  "id": 1001,
  "jsonrpc": "2.0",
  "method": "icx_getBlockHeaderByHeight",
  "params": {
      "height": "0x10"
  }
}
```

#### Parameters

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| height | T\_INT | true | The height of the block in hex string. |

> Example responses

```javascript
{
  "id": 1001,
  "jsonrpc": "2.0",
  "result": ""
}
```

> default Response

```javascript
{
  "id": 1001,
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Something went wrong."
  }
}
```

#### Responses

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | OK | Success | Data : base64 encoded bytes |
| default | Default | JSON-RPC Error | Error Response |

### icx\_getVotesByHeight

Get votes for the block specified by height.

Normally votes for the block are included in the next. So, even though the block is finalized by votes already, the block including votes may not exist. For that reason, we support this API to get votes as proof for the block.

> Request

```javascript
{
  "id": 1001,
  "jsonrpc": "2.0",
  "method": "icx_getVotesByHeight",
  "params": {
      "height": "0x10"
  }
}
```

#### Parameters

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| height | T\_INT | true | The height of the block for votes. |

> Example responses

```javascript
{
  "id": 1001,
  "jsonrpc": "2.0",
  "result": ""
}
```

> default Response

```javascript
{
  "id": 1001,
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Something went wrong."
  }
}
```

#### Responses

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | OK | Success | Encoded votes |
| default | Default | JSON-RPC Error | Error Response |

### icx\_getProofForResult

Get proof for the receipt. Proof, itself, may include the receipt.

Currently, Core2 uses Merkle Patricia Trie to store receipt, so the last leaf node includes the receipt. Key for the receipt must be the binary representation of the unsigned integer, the index of the receipt.

> Request

```javascript
{
  "id": 1001,
  "jsonrpc": "2.0",
  "method": "icx_getProofForResult",
  "params": {
      "hash": "0xc7fae616bd1d377a92c48a35e33e7a072e5e2be155c000088dbdd42a3e31bb74",
      "index": "0x0"
  }
}
```

#### Parameters

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| hash | T\_HASH | true | The hash value of the block including the result. |
| index | T\_INT | true | Index of the receipt in the block.  0 for the first. |

> Example responses
>
> ```javascript
> {
>    "id": 1001,
>    "jsonrpc": "2.0",
>    "result": [
>      "+QExoJezi7p+Q6Sj6RP1PYooLRlGqEz/UWwr0/VryoVpsmVAoLNM357mKnozSX9XP5PASGC4bq+QLpEY3Sg8xdIE7zD3oKIIH/jyDGldEmFNolkwa+3cxEJdrXWItDra84U66s9NoIfRLI8juWLG+6Z/xGVX1g3+J4yDEeInX0gABWGti5euoJrK+4GdphdGbxY4WsLIrflkcfrcqYGmazXClEtYjQ37oIiIPWzA4QFh/R/W1cuHM46RipVLzsSOdLPaFPOeBajToCXRQQ4GVjQiKXMgCTPPaqs30SMfDTTQpMiu9EWhB7wzoKxysxQEI6koHy7gnF5ob9nZYP7QDF/zV7MHU7Ut6NuRgICAgKDdQ/iagBv+CtzgCh1ju+SjVjZgOVeu6BZfBaAs5hYDo4CAgIA=",
>      "+QIRoJM2lLiv1hugUrj98X/c2Q8IWwOOjY5X5hoXhJWxYt9HoCIc9dReCXYR967Ll8MBSUxzksWDY2BnoQi9Wd/7oEoWoPkCx+uBkmGXMdfppwKUS/jaqLBEcxWj4bVoq/WpxFRzoJBir1eJCOvvqV9urYfxHvZ9E4MTcrb9Or7uLXyOQN78oB9ED5ht8egUlm/SGXX1UlpRFz+VwwgN6EY2TH8LJUT7oKsA5iI9WcteAH3ApzQCwO9BGpSHECr7Od0DEGf9/IxAoOsZFmn1IS2/EGAB97IbYRQGIy3j19DS2Y0jWyNmyT5XoERkVHKeInAzSMZcSm22AIIawXF/ibDdskyEDabbdnO5oCxrQAjl/71HrhhG7jokBsviGC3RYglC34NbtOWzZaoHoJMWXQn5I+cRmWg76pmT8VrDO0DSWGMyv1X3GbkPo8w/oPEBG9Q+RjtCMovVi9K6XG08khJpsPtcHB6YkOlHTLa8oPPEZm2q+9Cssdo5l0YzKH7/+cV1h5pxp8baWeUUUssFoBIHc9BwAGJDsArHrh9kkvS6K8B6xmOzRDR0eKfzC9NcoFHqm63YUFSq9I+9gVJB+VDPGWvp6ZV1AejoXwXS/8rkoJM2lLiv1hugUrj98X/c2Q8IWwOOjY5X5hoXhJWxYt9HoJl4/9qlwu2vrYvpyQ8ayLvfMOd3Tmc3KZT7FTTfJjJ3gA==",
>      "6CCmmADEFQCrJP7Wvhjk9aBRoZai37jZw23jIMQBAMQBAMQBAMQAwMA="
>    ]
> }
> ```
>
> Failure Response
>
> ```javascript
> {
>   "id": 1001,
>   "jsonrpc": "2.0",
>   "error": {
>     "code": -32000,
>     "message": "Something went wrong."
>   }
> }
> ```

#### Responses

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | OK | Success | List of base64 encoded proof including the receipt |
| default | Default | JSON-RPC Error | Error Response |

### icx\_getProofForEvents

Get proof for the receipt and the events in it. The proof may include the data itself.

Currently, Core2 uses Merkle Patricia Trie to store the receipt and the events, so the last leaf node includes the data. Key for the data must be the binary representation of the unsigned integer, the index of the data.

> Request

```javascript
{
  "id": 1001,
  "jsonrpc": "2.0",
  "method": "icx_getProofForEvents",
  "params": {
      "hash": "0xc7fae616bd1d377a92c48a35e33e7a072e5e2be155c000088dbdd42a3e31bb74",
      "index": "0x0",
      "events": [ "0x0", "0x2" ]
  }
}
```

#### Parameters

| Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| hash | T\_HASH | true | The hash value of the block including the result. |
| index | T\_INT | true | Index of the receipt in the block.  0 for the first. |
| events | Array | false | List of indexes of the events in the receipt. |

> Example responses
>
> ```javascript
> {
>   "id": 1001,
>   "jsonrpc": "2.0",
>   "result": [
>     [
>       "4hCgFOFiPi6RyndLHtrYmLXDDRtgcu6qaC/qJwyoqBc0sT0=",
>       "+HGgdzCygOSkliUIwU6RfeYGyP31o0QgJbVPs+1dDk2zezGgD7yIQcAXHMULT5HPmuhhvHiADRaVv57VTbMRO6LcYoGgr2WmucHrUlsFKF9YSb6GiUnbIW0sebGhJPS2IcU8HkuAgICAgICAgICAgICAgA==",
>       "+QE4ILkBNPkBMQCVATdyMP+0paHzA7SIxHGmw8UT/ZonAAAAuPAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACAAAAAAAAAAAAAAACAAIAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKAAAAAAQAAAAACAAAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAgAAAAAAAAAAAAAAD4APgAoMGo6EhTChqu29BsnvJLcJy3AgvxZt0ciwJEQNFqU6/H"
>     ],
>     [
>       "+FCCIAC4S/hJlQE3cjD/tKWh8wO0iMRxpsPFE/2aJ/GYRXZlbnQoQWRkcmVzcyxpbnQsYnl0ZXMplQAxnEr0c2+ow+/Vmv51ZKQ/yTGoPGQBwA=="
>     ]
>   ]
> }
> ```
>
> Failure Response
>
> ```javascript
> {
>   "id": 1001,
>   "jsonrpc": "2.0",
>   "error": {
>     "code": -32000,
>     "message": "Something went wrong."
>   }
> }
> ```

#### Responses

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | OK | Success | List of List of base64 encoded proof including the receipt and the events |
| default | Default | JSON-RPC Error | Error Response |

## Binary format

Core2 uses MsgPack and RLP with Null\(RLPn\) for binary encoding and decoding.

* [MsgPack](https://msgpack.org)
* RLPn is [RLP](https://github.com/ethereum/wiki/wiki/RLP) with Null \(`[0xF8 0x00]`\)

| Type | Msgpack | RLPn | Description |
| :--- | :--- | :--- | :--- |
| B\_LIST | List | List | List of items |
| B\_BYTES | Bytes | Bytes | Raw bytes |
| B\_BIGINT | Bytes | Bytes | N bytes of integer representation. ex\) 0x00 → \[ 0x00 \] 0x80 → \[ 0x00 0x80 \] -0x01 → \[ 0xff \] -0x80 → \[ 0x80 \] |
| B\_INT | Integer | B\_BIGINT | 64bits signed integer |
| B\_ADDRESS | Bytes | Bytes | 1 byte - 0x00 ← EOA - 0x01 ← SCORE 20 bytes : Identifier |
| B\_NULL | Null | Null | B\_BYTES\(N\), B\_ADDRESS\(N\) or B\_LIST\(N\) can be Null |

Suffixed `(N)` means a nullable value.

### Block Header

> B\_LIST of followings

| Field | Type | Description |
| :--- | :--- | :--- |
| Version | B\_INT | 1 ← Version 1 \(legacy\) N ← Version N |
| Height | B\_INT | Height of the block,  0 means genesis block. |
| Timestamp | B\_INT | Micro-seconds after EPOCH |
| Proposer | B\_ADDRESS\(N\) | Proposer of the block |
| PrevID | B\_BYTES\(N\) | 32 bytes hash value |
| VotesHash | B\_BYTES\(N\) | 32 bytes hash value |
| NextValidatorsHash | B\_BYTES\(N\) | 32 bytes hash value |
| PatchTransactionsHash | B\_BYTES\(N\) | 32 bytes hash value |
| NormalTransactionsHash | B\_BYTES\(N\) | 32 bytes hash value |
| LogsBloom | B\_BYTES | N\(1~256\) bytes bloom log value |
| Result | B\_BYTES\(N\) | Encoded bytes of the [Result](btp_extension.md#result) |

#### Result

> B\_LIST of followings

| Field | Type | Description |
| :--- | :--- | :--- |
| StateHash | B\_BYTES\(N\) | Hash of world state \(account information\) |
| PatchReceiptHash | B\_BYTES\(N\) | Root hash of [Merkle List](btp_extension.md#merkle-list) of patch receipts |
| NormalReceiptHash | B\_BYTES\(N\) | Root hash of [Merkle List](btp_extension.md#merkle-list) of normal receipts |

### Validators

> B\_LIST of Validators

| Field | Type | Description |
| :--- | :--- | :--- |
| Validator | B\_BYTES | 21 bytes → same as Address Other bytes → public key |

### Votes

> B\_LIST of followings

| Field | Type | Description |
| :--- | :--- | :--- |
| Round | B\_INT | Round for votes. If consensus doesn’t use round, it should be 0\(zero\). |
| BlockPartSetID | [PartSetID](btp_extension.md#partsetid) | PartSetID of the proposed block |
| Items | B\_LIST | List of [VoteItem](btp_extension.md#voteitem) |

#### VoteItem

> B\_LIST of followings

| Field | Type | Description |
| :--- | :--- | :--- |
| Timestamp | B\_INT | Voted time in micro-seconds |
| Signature | B\_BYTES | RSV format signature for [VoteMessage](btp_extension.md#votemessage) by a validator |

Public key of the validator can be recovered with `Signature` and SHA3Sum256\([VoteMessage](btp_extension.md#votemessage)\).

#### VoteMessage

> B\_LIST of followings

| Field | Type | Description |
| :--- | :--- | :--- |
| Height | B\_INT | [BlockHeader](btp_extension.md#blockheader).Height |
| Round | B\_INT | [Votes](btp_extension.md#votes).Round |
| Type | B\_INT | 0 ← PreVote 1 ← PreCommit |
| BlockID | B\_BYTES\(N\) | SHA3Sum256\([BlockHeader](btp_extension.md#blockheader)\) |
| BlockPartSetID | [PartSetID](btp_extension.md#partsetid) | [Votes](btp_extension.md#votes).BlockPartSetID. |
| Timestamp | B\_INT | [VoteItem](btp_extension.md#voteitem).Timestamp |

`Type` field should be `1` for votes of a block.

#### PartSetID

> B\_LIST of followings

| Field | Type | Description |
| :--- | :--- | :--- |
| Count | B\_INT | Number of block parts |
| Hash | B\_BYTES\(N\) | Hash of block parts |

### Receipt

> B\_LIST of followings

| Field | Type | Description |
| :--- | :--- | :--- |
| Status | B\_INT | Result status 0 ← SUCCESS N ← FAILURE \( N is failure code \) |
| To | B\_ADDRESS | The target address of the transaction |
| CumulativeStepUsed | B\_BIGINT | Cumulative step used |
| StepUsed | B\_BIGINT | Step used |
| StepPrice | B\_BIGINT | Step price in LOOP |
| LogsBloom | B\_BIGINT | 2048 bits without padding zeros So, if there is no bit, then it would be a byte with zero. |
| EventLogs | B\_LIST\(N\) of [EventLog](btp_extension.md#eventlog) | A list of event logs \(empty if there is EventLogHash\) |
| SCOREAddress | B\_ADDRESS\(N\) | The address of deployed smart contract |
| EventLogHash | B\_BYTES\(O\) | \(from Revision7\) Root hash of [Merkle List](btp_extension.md#merkle-list) of eventLogs |

#### EventLog

> B\_LIST of followings

| Field | Type | Description |
| :--- | :--- | :--- |
| Addr | B\_ADDRESS | SCORE producing this event log |
| Indexed | B\_LIST of B\_BYTES\(N\) | Indexed data. |
| Data | B\_LIST of B\_BYTES\(N\) | Remaining data. |

### Merkle Patricia Trie

It's similar to [Merkle Patricia Trie](https://github.com/ethereum/wiki/wiki/Patricia-Tree) except that it uses SHA3-256 instead of KECCAK-256.

#### Merkle List

Root hash of the list is SHA3Sum256\([MPT Node](btp_extension.md#mpt-node)\) of the root. The key for the list is B\_INT encoded index of the item.

#### Merkle Proof

It's list of MPT Nodes from the root to the leaf containing the value.

#### MPT Node

MPT Node is one of followings.

* MPT Leaf
* MPT Extension
* MPT Branch

#### MPT Branch

> RLP List of followings

| Field | Type | Description |
| :--- | :--- | :--- |
| Link\(1~16\) | [MPT Link](btp_extension.md#mpt-link) | Link to the Nth child. |
| Value | RLP Bytes | Stored data |

#### MPT Leaf

> RLP List of followings

| Field | Type | Description |
| :--- | :--- | :--- |
| Header | RLP Bytes | [Header](btp_extension.md#mpt-node-header) |
| Value | RLP Bytes | Stored data |

#### MPT Extension

> RLP List of followings

| Field | Type | Description |
| :--- | :--- | :--- |
| Header | RLP Bytes | [Header](btp_extension.md#mpt-node-header) |
| Link | [MPT Link](btp_extension.md#mpt-link) | Link to the child. |

#### MPT Link

> RLP Bytes or MPT Node

If encoded MPT Node is shorter than 32 bytes, then it's embedded in the link. Otherwise, it would be RLP Bytes of SHA3Sum256\([MPT Node](btp_extension.md#mpt-node)\)

#### MPT Node Header

The key for the tree can be spliced into 4 bits nibbles.

| Case | Prefix | Path |
| :--- | :--- | :--- |
| Leaf with odd nibbles | 0x3 | Nibbles |
| Leaf with even nibbles | 0x20 | Nibbles |
| Extension with odd nibbles | 0x1 | Nibbles |
| Extension with even nibbles | 0x00 | Nibbles |

> Examples

| Case | Prefix | Path | Bytes |
| :--- | :--- | :--- | :--- |
| Leaf \[A\] | 0x3 | \[0xA\] | \[ 0x3A \] |
| Extension \[AB\] | 0x00 | \[0xAB\] | \[ 0x00, 0xAB \] |
| Extension \[ABC\] | 0x1 | \[0xA, 0xBC\] | \[ 0x1A, 0xBC \] |

