# Economics Extension

Economic policies on the ICON network are sometimes referred to as _ICON Incentives Scoring System (IISS)_.

## Introduction

This document explains JSON-RPC APIs (version 3) for IISS available to interact with Goloop nodes.

This document was written based on IISS 3.1

## IISS APIs

API path : `<scheme>://<host>/api/v3`

* All IISS APIs follow SCORE API call convention
* Target SCORE Address for IISS APIs: `cx0000000000000000000000000000000000000000`
* Each IISS API `TX` method section explains the content of `data` field in `icx_sendTransaction`
* Each IISS API `QUERY` method section explains the content of `data` field in `icx_call`

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_sendTransaction",
  "params": {
    "version": "0x3",
    "from": "hx8f21e5c54f016b6a5d5fe65486908592151a7c57",
    "to": "cx0000000000000000000000000000000000000000",
    "stepLimit": "0x7e3a85",
    "timestamp": "0x563a6cf330136",
    "nid": "0x3",
    "nonce": "0x0",
    "value": "0x0",
    "signature": "VAia7YZ2Ji6igKWzjR2YsGa2m5...",
    "dataType": "call",
    "data": {
      "method": "setStake",
      "params": {
        "value": "0x1"
      }
    }
  }
}
```

### setStake

Stake some amount of ICX

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_sendTransaction",
  "params": {
    "data": {
      "method": "setStake",
      "params": {
        "value": "0xde0b6b3a7640000"
      }
    },
    ...
  }
}
```

#### Parameters

| Key   | VALUE Type | Required | Description                          |
| ----- | ---------- | -------- | ------------------------------------ |
| value | T\_INT     | true     | Amount of ICX icons in loop to stake |

### getStake

Returns the stake status of a given address

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_call",
  "params": {
    "to": "cx0000000000000000000000000000000000000000",
    "dataType": "call",
    "data": {
      "method": "getStake",
      "params": {
        "address": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb"
      }
    }
  }
}
```

#### Parameters

| Key     | VALUE Type   | Required | Description      |
| ------- | ------------ | -------- | ---------------- |
| address | T\_ADDR\_EOA | true     | Address to query |

> Example responses

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "result": {
    "stake": "0x4e1003b28d9280000",
    "unstakes": [
      {
        "unstake": "0x8ac7230489e80000",
        "unstakeBlockHeight": "0x19",
        "remainingBlocks": "0x11"
      }
    ]
  }
}
```

#### Returns

| Key                         | VALUE Type        | Required | Description                           |
| --------------------------- | ----------------- | -------- | ------------------------------------- |
| stake                       | T\_INT            | true     | ICX amount of stake in loop           |
| unstakes                    | T\_LIST\[T\_DICT] | false    | Unstake info list                     |
| unstakes.unstake            | T\_INT            | false    | ICX amount of unstake in loop         |
| unstakes.unstakeBlockHeight | T\_INT            | false    | BlockHeight when unstake will be done |

### setDelegation

Delegate some ICX amount of stake to P-Reps

* Maximum number of P-Reps to delegate is 100
* The transaction which has duplicated P-Rep addresses will be failed
* This transaction overwrites the previous delegate information

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_sendTransaction",
  "params": {
    "data": {
      "method": "setDelegation",
      "params": {
        "delegations": [
          {
            "address": "hx1d6463e4628ee52a7f751e9d500a79222a7f3935",
            "value": "0x3200000000"
          },
          {
            "address": "hxb6bc0bf95d90cb3cd5b3abafd9682a62f36cc826",
            "value": "0x1000000000"
          }
        ]
      }
    },
    ...
  }
}
```

> Request to revoke all delegations

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_sendTransaction",
  "params": {
    "data": {
      "method": "setDelegation",
      "params": {
        "delegations": []
      }
    },
    ...
  }
}
```

#### Parameters

| Key                 | VALUE Type       | Required | Description                                |
| ------------------- | ---------------- | -------- | ------------------------------------------ |
| delegations         | T\_LIST(T\_DICT) | true     | List of delegation dict (MAX: 100 entries) |
| delegations.address | T\_ADDR\_EOA     | true     | Address of P-Rep to delegate               |
| delegations.value   | T\_INT           | true     | Delegation amount in loop                  |

### getDelegation

Returns the delegation status of a given address

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_call",
  "params": {
    "to": "cx0000000000000000000000000000000000000000",
    "dataType": "call",
    "data": {
      "method": "getDelegation",
      "params": {
        "address": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb"
      }
    }
  }
}
```

#### Parameters

| Key     | VALUE Type   | Required | Description      |
| ------- | ------------ | -------- | ---------------- |
| address | T\_ADDR\_EOA | true     | Address to query |

> Example responses

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "result": {
    "totalDelegated": "0xa688906bd8b0000",
    "votingPower": "0x3782dace9d90000",
    "delegations": [
      {
        "address": "hx1d6463e4628ee52a7f751e9d500a79222a7f3935",
        "value": "0x3782dace9d90000"
      },
      {
        "address": "hxb6bc0bf95d90cb3cd5b3abafd9682a62f36cc826",
        "value": "0x6f05b59d3b20000"
      }
    ]
  }
}
```

#### Returns

| Key                 | VALUE Type       | Required | Description                                                         |
| ------------------- | ---------------- | -------- | ------------------------------------------------------------------- |
| totalDelegated      | T\_INT           | true     | The sum of delegation amount                                        |
| votingPower         | T\_INT           | true     | Remaining amount of stake that ICONist can delegate to other P-Reps |
| delegations         | T\_LIST(T\_DICT) | true     | List of delegation dict (MAX: 100 entries)                          |
| delegations.address | T\_ADDR\_EOA     | true     | Address of P-Rep to delegate                                        |
| delegations.value   | T\_INT           | true     | Delegation amount in loop                                           |

### setBond

Bond some ICX amount of stake to P-Reps

* Maximum number of P-Reps to bond is 100
* The transaction which has duplicated P-Rep addresses will be failed
* This transaction overwrites the previous bond information

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_sendTransaction",
  "params": {
    "data": {
      "method": "setBond",
      "params": {
        "bonds": [
          {
            "address": "hx1d6463e4628ee52a7f751e9d500a79222a7f3935",
            "value": "0x3200000000"
          },
          {
            "address": "hxb6bc0bf95d90cb3cd5b3abafd9682a62f36cc826",
            "value": "0x1000000000"
          }
        ]
      }
    },
    ...
  }
}
```

#### Parameters

| Key           | VALUE Type       | Required | Description                          |
| ------------- | ---------------- | -------- | ------------------------------------ |
| bonds         | T\_LIST(T\_DICT) | true     | List of bond dict (MAX: 100 entries) |
| bonds.address | T\_ADDR\_EOA     | true     | Address of P-Rep to bond             |
| bonds.value   | T\_INT           | true     | Bond amount in loop                  |

### getBond

Returns the bond status of a given address

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_call",
  "params": {
    "to": "cx0000000000000000000000000000000000000000",
    "dataType": "call",
    "data": {
      "method": "getBond",
      "params": {
        "address": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb"
      }
    }
  }
}
```

#### Parameters

| Key     | VALUE Type   | Required | Description      |
| ------- | ------------ | -------- | ---------------- |
| address | T\_ADDR\_EOA | true     | Address to query |

> Example responses

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "result": {
    "bonds": [
      {
        "address": "hx1d6463e4628ee52a7f751e9d500a79222a7f3935",
        "value": "0x3782dace9d90000"
      },
      {
        "address": "hxb6bc0bf95d90cb3cd5b3abafd9682a62f36cc826",
        "value": "0x6f05b59d3b20000"
      }
    ],
    "unbonds": [
      {
        "address": "hx1d6463e4628ee52a7f751e9d500a79222a7f3935",
        "value": "0x3782dace9d90000",
        "expireBlockHeight": "0xa"
      },
      {
        "address": "hxb6bc0bf95d90cb3cd5b3abafd9682a62f36cc826",
        "value": "0x6f05b59d3b20000",
        "expireBlockHeight": "0xa"
      }
    ]
  }
}
```

#### Returns

| Key                       | VALUE Type                  | Required | Description                             |
| ------------------------- | --------------------------- | -------- | --------------------------------------- |
| bonds                     | T\_LIST(T\_DICT)            | true     | List of bond dict                       |
| bonds.address             | T\_ADDR\_EOA,T\_ADDR\_SCORE | true     | Address of P-Rep to delegate            |
| bonds.value               | T\_INT                      | true     | Bond amount in loop                     |
| unbonds                   | T\_LIST(T\_DICT)            | true     | List of unbond dict                     |
| unbonds.address           | T\_ADDR\_EOA,T\_ADDR\_SCORE | true     | Address of P-Rep to delegate            |
| unbonds.value             | T\_INT                      | true     | Unbonding amount in loop                |
| unbonds.expireBlockHeight | T\_INT                      | true     | BlockHeight when unBonding will be done |

### claimIScore

Claim the total reward that a ICONist has received

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_sendTransaction",
  "params": {
    "data": {
      "method": "claimIScore"
    },
    ...
  }
}
```

#### Parameters

N/A

#### EventLog

| Name                             | Data Type | Indexed | Description             |
| -------------------------------- | --------- | ------- | ----------------------- |
| IScoreClaimedV2(Address,int,int) | T\_STRING | true    | Signature               |
| IScore                           | T\_INT    | false   | Reward amount in IScore |
| ICX                              | T\_INT    | false   | Reward amount in loop   |

### queryIScore

Returns the amount of I-Score that a ICONist has received as a reward

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_call",
  "params": {
    "to": "cx0000000000000000000000000000000000000000",
    "dataType": "call",
    "data": {
      "method": "queryIScore",
      "params": {
        "address": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb"
      }
    }
  }
}
```

#### Parameters

| Key     | VALUE Type   | Required | Description      |
| ------- | ------------ | -------- | ---------------- |
| address | T\_ADDR\_EOA | true     | Address to query |

> Example responses

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "result": {
    "blockHeight": "0xe3d2",
    "iscore": "0x3e8",
    "estimatedICX": "0x1"
  }
}
```

#### Returns

| Key          | VALUE Type | Required | Description                                               |
| ------------ | ---------- | -------- | --------------------------------------------------------- |
| blockHeight  | T\_INT     | true     | Block height when I-Score is estimated                    |
| iscore       | T\_INT     | true     | Amount of I-Score                                         |
| estimatedICX | T\_INT     | true     | <p>Estimated amount in loop<br>1000 I-Score == 1 loop</p> |

### registerPRep

Register an address as a P-Rep to Blockchain

* 2000 ICX are required as a registration fee

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_sendTransaction",
  "params": {
    "value": "0x6c6b935b8bbd400000",
    "data": {
      "method": "registerPRep",
      "params": {
        "name": "ABC Node",
        "country": "KOR",
        "city": "Seoul",
        "email": "abc@example.com",
        "website": "https://abc.example.com/",
        "details": "https://abc.example.com/details/",
        "p2pEndpoint": "abc.example.com:7100",
        "nodeAddress": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb"
      }
    },
    ...
  }
}
```

#### Parameters

| Key         | VALUE Type | Required | Description                                                                                                                        |
| ----------- | ---------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| name        | T\_STRING  | true     | <p>P-Rep name<br>"ABC Node"</p>                                                                                                    |
| email       | T\_STRING  | true     | <p>P-Rep email<br>"abc@example.com"</p>                                                                                            |
| country     | T\_STRING  | true     | <p><a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3">ISO 3166-1 ALPHA-3</a><br>"KOR", "USA", "CHN"</p>                    |
| city        | T\_STRING  | true     | "Seoul", "New York", "Paris"                                                                                                       |
| website     | T\_STRING  | true     | <p>P-Rep homepage url<br>"<a href="https://abc.example.com">https://abc.example.com</a>"</p>                                       |
| detailes    | T\_STRING  | true     | <p>Url including P-Rep detail information<br>"<a href="https://abc.example.com/details/">https://abc.example.com/details/</a>"</p> |
| p2pEndpoint | T\_STRING  | true     | <p>Network info used for connecting among P-Rep nodes<br>"123.45.67.89:7100", "node.example.com:7100"</p>                          |
| nodeAddress | T\_STRING  | False    | <p>Node Key for only consensus<br>"hxe7af5fcfd8dfc67530a01a0e403882687528dfcb"</p>                                                 |

\*details : See [JSON Standard for P-Rep Detailed Information](broken-reference)

#### EventLog

| Name                    | Data Type    | Indexed | Description   |
| ----------------------- | ------------ | ------- | ------------- |
| PRepRegistered(Address) | T\_STRING    | true    | Signature     |
| Address                 | T\_ADDR\_EOA | false   | P-Rep address |

### unregisterPRep

Unregister a P-Rep

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_sendTransaction",
  "params": {
    "data": {
      "method": "unregisterPRep"
    },
    ...
  }
}
```

#### Parameters

N/A

#### EventLog

| Name                      | Data Type    | Indexed | Description   |
| ------------------------- | ------------ | ------- | ------------- |
| PRepUnregistered(Address) | T\_STRING    | true    | Signature     |
| Address                   | T\_ADDR\_EOA | false   | P-Rep address |

### setPRep

Update P-Rep register information

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_sendTransaction",
  "params": {
    "data": {
      "method": "setPRep",
      "params": {
        "name": "Banana Node",
        "email": "banana@email.com",
        "nodeAddress": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb"
      }
    },
    ...
  }
}
```

#### Parameters

| Key         | VALUE Type | Required | Description                                                                                                                        |
| ----------- | ---------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| name        | T\_STRING  | false    | <p>P-Rep name<br>"ABC Node"</p>                                                                                                    |
| email       | T\_STRING  | false    | <p>P-Rep email<br>"abc@example.com"</p>                                                                                            |
| country     | T\_STRING  | false    | <p><a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3">ISO 3166-1 ALPHA-3</a><br>"KOR", "USA", "CHN"</p>                    |
| city        | T\_STRING  | false    | "Seoul", "New York", "Paris"                                                                                                       |
| website     | T\_STRING  | false    | <p>P-Rep homepage url<br>"<a href="https://abc.example.com">https://abc.example.com</a>"</p>                                       |
| detailes    | T\_STRING  | false    | <p>Url including P-Rep detail information<br>"<a href="https://abc.example.com/details/">https://abc.example.com/details/</a>"</p> |
| p2pEndpoint | T\_STRING  | false    | <p>Network info used for connecting among P-Rep nodes<br>"123.45.67.89:7100", "node.example.com:7100"</p>                          |
| nodeAddress | T\_STRING  | false    | <p>Node Key for only consensus<br>"hxe7af5fcfd8dfc67530a01a0e403882687528dfcb"</p>                                                 |

\*details : See [JSON Standard for P-Rep Detailed Information](broken-reference)

#### EventLog

| Name             | Data Type    | Indexed | Description   |
| ---------------- | ------------ | ------- | ------------- |
| PRepSet(Address) | T\_STRING    | true    | Signature     |
| Address          | T\_ADDR\_EOA | false   | P-Rep address |

### getPRep

Returns P-Rep register information

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_call",
  "params": {
    "to": "cx0000000000000000000000000000000000000000",
    "dataType": "call",
    "data": {
      "method": "getPRep",
      "params": {
        "address": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb"
      }
    }
  }
}
```

#### Parameters

| Key     | VALUE Type   | Required | Description      |
| ------- | ------------ | -------- | ---------------- |
| address | T\_ADDR\_EOA | true     | Address to query |

> Example responses

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "result": {
    "status": "0x0",
    "grade": "0x2",
    "address": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
    "name": "banana",
    "country": "KOR",
    "city": "Seoul",
    "email": "banana@email.com",
    "website": "https://icon.banana.com",
    "details": "https://icon.banana.com/json",
    "p2pEndpoint": "123.45.67.89:7100",
    "nodeAddress": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
    "irep": "0xa968163f0a57b400000",
    "irepUpdateBlockHeight": "0x847ea",
    "stake": "0x38372",
    "delegated": "0x74287392847",
    "totalBlocks": "0x83261e7",
    "validatedBlocks": "0x83258a9"
  }
}
```

#### Returns

| Key                     | VALUE Type   | Required | Description                                                                                                                        |
| ----------------------- | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| status                  | T\_INT       | true     | <p>0: active<br>1: unregistered</p>                                                                                                |
| grade                   | T\_INT       | true     | <p>0: Main P-Rep<br>1: Sub P-Rep<br>2: P-Rep candidate</p>                                                                         |
| address                 | T\_ADDR\_EOA | true     | P-Rep address                                                                                                                      |
| name                    | T\_STRING    | true     | <p>P-Rep name<br>"ABC Node"</p>                                                                                                    |
| email                   | T\_STRING    | true     | <p>P-Rep email<br>"abc@example.com"</p>                                                                                            |
| country                 | T\_STRING    | true     | <p><a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3">ISO 3166-1 ALPHA-3</a><br>"KOR", "USA", "CHN"</p>                    |
| city                    | T\_STRING    | true     | "Seoul", "New York", "Paris"                                                                                                       |
| website                 | T\_STRING    | true     | <p>P-Rep homepage url<br>"<a href="https://abc.example.com">https://abc.example.com</a>"</p>                                       |
| detailes                | T\_STRING    | true     | <p>Url including P-Rep detail information<br>"<a href="https://abc.example.com/details/">https://abc.example.com/details/</a>"</p> |
| p2pEndpoint             | T\_STRING    | true     | <p>Network info used for connecting among P-Rep nodes<br>"123.45.67.89:7100", "node.example.com:7100"</p>                          |
| nodeAddress             | T\_STRING    | true     | <p>Node Key for only consensus<br>"hxe7af5fcfd8dfc67530a01a0e403882687528dfcb"</p>                                                 |
| irep                    | T\_INT       | true     | <p>Incentive rep used to calculate the reward for P-Rep<br>Limit: +- 20% of the previous value<br>Unit: loop</p>                   |
| irepUpdatedBlockHeight  | T\_INT       | true     | Block height when a P-Rep changed I-Rep value                                                                                      |
| lastGenerateBlockHeight | T\_INT       | true     | Height of the last block which a P-Rep generated                                                                                   |
| stake                   | T\_INT       | true     | Amount of stake that a P-Rep has                                                                                                   |
| delegated               | T\_INT       | true     | Delegation amount that a P-Rep receives from ICONists                                                                              |
| totalBlocks             | T\_INT       | true     | The number of blocks that a P-Rep received when running as a Main P-Rep                                                            |
| validatedBlocks         | T\_INT       | true     | The number of blocks that a P-Rep validated when running as a Main P-Rep                                                           |

### getPReps

Returns the status of all registered P-Rep candidates in descending order by delegated ICX amount

* Unregistered or disqualified P-Reps are not included

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_call",
  "params": {
    "to": "cx0000000000000000000000000000000000000000",
    "dataType": "call",
    "data": {
      "method": "getPReps",
      "params": {
        "startRanking": "0x1",
        "endRanking": "0xa"
      }
    }
  }
}
```

#### Parameters

| Key          | VALUE Type | Required | Description                                                     |
| ------------ | ---------- | -------- | --------------------------------------------------------------- |
| startRanking | T\_INT     | false    | <p>Default: 1<br>P-Rep list which starts from start ranking</p> |
| endRanking   | T\_INT     | false    | Default: the last ranking                                       |

> Example responses

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "result": {
    "blockHeight": "0x1234",
    "startRanking": "0x1",
    "totalDelegated": "0x2863c1f5cdae42f9540000000",
    "totalStake": "0x193e5939a08ce9dbd480000000",
    "preps": [
      {
        "status": "0x0",
        "grade": "0x2",
        "address": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
        "name": "banana",
        "country": "KOR",
        "city": "Seoul",
        "email": "banana@email.com",
        "website": "https://icon.banana.com",
        "details": "https://icon.banana.com/json",
        "p2pEndpoint": "123.45.67.89:7100",
        "nodeAddress": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
        "irep": "0xa968163f0a57b400000",
        "irepUpdateBlockHeight": "0x847ea",
        "stake": "0x38372",
        "delegated": "0x74287392847",
        "totalBlocks": "0x83261e7",
        "validatedBlocks": "0x83258a9"
      },
      {
        "status": "0x0",
        "grade": "0x2",
        "name": "ABC Node",
        "address": "hx1d6463e4628ee52a7f751e9d500a79222a7f3935",
        "country": "USA",
        "city": "New York",
        "email": "banana@email.com",
        "website": "https://icon.banana.com",
        "details": "https://icon.banana.com/json",
        "p2pEndpoint": "123.45.67.89:7100",
        "nodeAddress": "hx1d6463e4628ee52a7f751e9d500a79222a7f3935",
        "irep": "0xc350",
        "irepUpdateBlockHeight": "0x1100",
        "lastGenerateBlockHeight": "0x1200",
        "stake": "0x28a857425466f800000",
        "delegated": "0x9ed194db19b238c000000",
        "totalBlocks": "0x2720",
        "validatedBlocks": "0x2348"
      },
      ...
    ]
  }
}
```

#### Returns

| Key            | VALUE Type       | Required | Description                                                              |
| -------------- | ---------------- | -------- | ------------------------------------------------------------------------ |
| blockHeight    | T\_INT           | true     | The latest block height when this request was processed                  |
| startRanking   | T\_INT           | true     | Start ranking of P-Rep list                                              |
| totalDelegated | T\_INT           | true     | Total delegation amount that all P-Reps receive                          |
| totalStake     | T\_INT           | true     | The sum of ICX that all ICONists stake                                   |
| preps          | T\_LIST(T\_DICT) | true     | P-Rep list. Details : refer to [getPRep](economics-extension.md#getPRep) |

### setBonderList

Set allowed bonder list to P-Rep

* Maximum number of allowed ICONists to bond is 10
* This transaction overwrites the previous bonder list information

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_sendTransaction",
  "params": {
    "data": {
      "method": "setBonderList",
      "params": {
        "bonderList": [
          "hx1d6463e4628ee52a7f751e9d500a79222a7f3935",
          "cxb6bc0bf95d90cb3cd5b3abafd9682a62f36cc826"
        ]
      }
    },
    ...
  }
}
```

#### Parameters

| Key        | VALUE Type                           | Required | Description                        |
| ---------- | ------------------------------------ | -------- | ---------------------------------- |
| bonderList | T\_LIST(T\_ADDR\_EOA,T\_ADDR\_SCORE) | true     | List of address (MAX: 100 entries) |

### getBonderList

Returns the allowed bonder list

> Request

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "method": "icx_call",
  "params": {
    "to": "cx0000000000000000000000000000000000000000",
    "dataType": "call",
    "data": {
      "method": "getBonderList",
      "params": {
        "address": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb"
      }
    }
  }
}
```

#### Parameters

| Key     | VALUE Type   | Required | Description      |
| ------- | ------------ | -------- | ---------------- |
| address | T\_ADDR\_EOA | true     | Address to query |

> Example responses

```javascript
{
  "jsonrpc": "2.0",
  "id": 1234,
  "result": {
    "bonderList": [
      "hx1d6463e4628ee52a7f751e9d500a79222a7f3935",
      "cxb6bc0bf95d90cb3cd5b3abafd9682a62f36cc826"
    ]
  }
}
```

#### Returns

| Key        | VALUE Type                           | Required | Description                        |
| ---------- | ------------------------------------ | -------- | ---------------------------------- |
| bonderList | T\_LIST(T\_ADDR\_EOA,T\_ADDR\_SCORE) | true     | List of address (MAX: 100 entries) |

## References

* [Goloop JSON-RPC API v3](broken-reference)
