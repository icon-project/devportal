# Decentralizing a local network

If your goal is to have a local network that closely resembles the public ICON chain and use it for testing, it is necessary to activate the [ICON Incentive Scoring System (IISS).](https://docs.icon.community/v/icon1/introduction/icon-key-concepts/governance-iiss) The IISS activates after the networks achieves decentralization, and in order to do this the following conditions must be met:

1. Number of registered validators (pReps) >= main validator (pRep) count
2. The last main validator's (pRep's) power should be more than `totalSupply * 0.2%` in power descending order.
3. Revision >= 6

The [gochain-local](https://github.com/icon-project/gochain-local) repository has a set of predefined wallets and configurations that you can use for this and will make this process more easier.&#x20;

> _NOTE:_ The purpose of this repository is to quickly create an environment for testing, this repository has 5 wallets with their corresponding keystore and passwords openly available on the internet, DO NOT reuse these wallets in any other place.

The process of decentralizing the local multi-node network will comprise the following steps:

* Start the local network using the `compose-multi.yml` configuration file.
* Making RPC calls to one of the nodes in the chain and transfer balance from the “_god wallet_” (a predefined wallet used to setup the network) to the wallets used on each of the 4 nodes. The keystore file for the “_god wallet_” can be found on `./data/godWallet.json`.
* Making RPC calls with the `registerPrep` method to register each node as a validator.
* Making RPC calls to stake, vote and set up bonds on each validator.

### Start the network and the 4 main validators.

Run the validators by executing the following command:

```bash
$ docker compose -f compose-multi.yml up -d
```

Verify that the nodes are running by executing the following command:

```bash
$ docker ps
CONTAINER ID   IMAGE                    COMMAND              CREATED    STATUS    PORTS                                             NAMES
b9ad9c6dc743   goloop/gochain-icon:latest   "/entrypoint /bin/sh…"   24 hours ago   Up 24 hours   8080/tcp, 0.0.0.0:9081->9080/tcp, :::9081->9080/tcp   gochain-local-node1-1
2fab903adca3   goloop/gochain-icon:latest   "/entrypoint /bin/sh…"   24 hours ago   Up 24 hours   8080/tcp, 0.0.0.0:9080->9080/tcp, :::9080->9080/tcp   gochain-local-node0-1
cb22360a34c2   goloop/gochain-icon:latest   "/entrypoint /bin/sh…"   24 hours ago   Up 24 hours   8080/tcp, 0.0.0.0:9082->9080/tcp, :::9082->9080/tcp   gochain-local-node2-1
b72716387812   goloop/gochain-icon:latest   "/entrypoint /bin/sh…"   24 hours ago   Up 24 hours   8080/tcp, 0.0.0.0:9083->9080/tcp, :::9083->9080/tcp   gochain-local-node3-1
```

### Send balance from god wallet to each validator wallet.

At network genesis the entire initial balance is held in the god wallet used to setup the network. Since we are using the `gochain-local` repo the network also has 4 nodes that are initially unregistered and their wallets hold no balance, so the first thing that we need to do is transfer balance from the god wallet to each of the wallets for the nodes.&#x20;

The keystore file for the god wallet can be found in `data/godWallet.json` and the wallets used for each node can be found in the following paths:

```
data/multi/ks0.json
data/multi/ks1.json
data/multi/ks2.json
data/multi/ks3.json
```

All the keystore files (wallets) are encrypted with the same password which is `gochain`.

To transfer balance to each node wallet we need to sign an RPC call using the `icx_sendTransaction` method. The following `curl` command shows a transfer of _100.000.000_ native coin (ICX) from the god wallet to a node wallet.&#x20;

In this example, we are using a JSON payload with the following data:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "icx_sendTransaction",
  "params": {
    "to": "hxce2e00040745ce07a95082a1c1e2ef023d35d742", // address receiving the transfer
    "from": "hxb6b5791be0b5ef67063b3c10b840fb81514db2fd", // address making the transfer
    "nid": "0x3", // ID for the network as hex value. “0x3” default for local networks
    "version": "0x3", // API version
    "timestamp": "0x5f53fb7b00c10", // timestamp of the transfer
    "stepLimit": "0x7a1200", // maximum fee allowed for the transfer
    "value": "0xde0b6b3a7640000", // amount of ICX to transfer converted to loop units
    "nonce": "0x1", // An arbitrary number used to prevent transaction hash collision
    "signature": "kT4y4NvabHzsn6xSpA/HBxBTueDVFK+JYWQsvvcz6UMvs2yzbl17ooBPLy1sG8nUMoxpYG9SxfsdhvzWEe4J5wA=" // transaction signature
  }
}
```

Execute the following command to make the RPC request using `cURL`:

```bash
$ curl -X POST --data @rpc-data.json http://localhost:9080/api/v3
```

To transfer to any of the other wallets, replace the `“to”` address with the wallet address you want to transfer to.

### Registering the nodes as validators.

The process of registering a node as a validator requires signing a transaction with the [\`registerPRep\` method](https://docs.icon.community/v/icon1/references/reference-manuals/icon-json-rpc-api-v3-specification#registerprep), paying _2000_ ICX and sending a JSON formatted data with the validator information in the following format:

```json
{
  "name": "ABC Node",   // Validator name
  "country": "KOR",  // Validator country ISO 3166-1 alpha-3 formatted
  "city": "Seoul", // validator city
  "email": "abc@example.com", // validator email
  "website": "https://abc.example.com/", // validator website
  "details": "https://abc.example.com/details/", // validator misc information details
  "p2pEndpoint": "abc.example.com:7100", // network info used for connecting among validators
  "nodeAddress": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb", // node public key for consensus
}
```

RPC JSON example:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "icx_sendTransaction",
  "params": {
     "to": "cx0000000000000000000000000000000000000000",
     "from": "hxe9dfcd26dbde32aca8f878c1c4bfd83a0c7f70fc",
     "nid": "0x3",
     "version": "0x3",
     "timestamp": "0x5f4fcf37e1298",
     "stepLimit": "0x7a1200",
     "value": "0x6c6b935b8bbd400000", // amount of ICX to transfer converted to loop units
     "nonce": "0x1",
     "dataType": "call",
     "data": {
        "method": "registerPRep",
        "params": {
           "name": "Node0",
           "country": "USA",
           "city": "Houston",
           "email": "info@test.com",
           "website": "https://test.com",
           "details": "https://test.com/details.json",
           "p2pEndpoint": "127.0.0.1:9000",
           "nodeAddress": "hxe9dfcd26dbde32aca8f878c1c4bfd83a0c7f70fc"
         }
     },
     "signature": "lxhs/MqRC8ccxjxcVrPujD8gOJFAbVAlwnBBbYA98wdZQgygfV1aVRrfFbPgXj9RTXOvrVGKqTa8lMaRgR6R5wE="
  }
}
```

```bash
$ curl -X POST --data @rpc-data.json http://localhost:9080/api/v3
```

Repeat this process to register all 4 nodes.

### Staking native coin (ICX) on each registered node wallet.

After successfully sending balance from god wallet to each of the node wallets and registering the wallets as validators we need to allocate votes to each validator and place the required bond, in order to do this we need to first stake.\
\
The following curl command stakes _10.000.000_ native coin (ICX) on one of the wallets using the `setStake` method of the governance contract (the process needs to be done on all 4 validator wallets).

RPC JSON example:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "icx_sendTransaction",
  "params": {
     "to": "cx0000000000000000000000000000000000000000",
     "from": "hxe9dfcd26dbde32aca8f878c1c4bfd83a0c7f70fc",
     "nid": "0x3",
     "version": "0x3",
     "timestamp": "0x5f500e34e39a8",
     "stepLimit": "0x7a1200",
     "nonce": "0x1",
     "dataType": "call",
     "data": {
        "method": "setStake",
        "params": { "value": "0x84595161401484a000000" }
      },
     "signature": "o8uLrAQqNTTArLUAIQ84usVedT5r67aUocwWF3H0a1ot9YD2TFpbWbNRaOgrNLMLRRc9tiL6eSRYj6JjxfGUHAA="
  }
}
```

```bash
$ curl -X POST --data @rpc-data.json http://localhost:9080/api/v3
```

### Self voting on each validator wallet.

For the process of adding votes we are going to self vote an amount of _8.000.000_ native coin (ICX) with each wallet by calling the `setDelegation` of the governance contract using the following curl command.

RPC JSON example:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "icx_sendTransaction",
  "params": {
     "to": "cx0000000000000000000000000000000000000000",
     "from": "hxe9dfcd26dbde32aca8f878c1c4bfd83a0c7f70fc",
     "nid": "0x3",
     "version": "0x3",
     "timestamp": "0x5f5015e52b3e0",
     "stepLimit": "0x7a1200",
     "nonce": "0x1",
     "dataType": "call",
     "data": {
        "method": "setDelegation",
        "params": {
           "delegations": [
              {
                "address": "hxe9dfcd26dbde32aca8f878c1c4bfd83a0c7f70fc",
                "value": "0x69e10de76676d08000000"
              }
           ]
         }
     },
    "signature": "hP1GlVec6WFrrVgLFYPNwupHvhOHHVVDVvl0aPVI9xRHnLSIdZstwD6Rk+7GBujnZB8nEh/fWaDsSSxy+hAZPAA="
  }
}
```

```bash
$ curl -X POST --data @rpc-data.json http://localhost:9080/api/v3
```

### Placing bond for each validator.

The process of setting up bond on each validator requires the following two steps:

* Sign a transaction with each validator wallet calling the [`setBonderList`](https://github.com/icon-project/devportal/blob/1f63be996ea77f953a8dbfb72dda0becdd3da4ad/icon-2.0/goloop/json-rpc/iiss\_extension.md#setbonderlist) method of the governance contract, to setup which wallets can place bonds for the validator.
* Place the bond with the wallet that was previously whitelisted in the previous step by signing a transaction calling the [`setBond`](https://github.com/icon-project/devportal/blob/e791e8a762eee66e77b20946efa871566a5093d8/icon-stack/client-apis/json-rpc-api/economics-extension.md#setbond) method of the governance wallet.

#### Signing transactions for the `setBonderList` method.

The following is the curl command to sign a transaction calling the `setBonderList` method of the governance contract, this process is done with each validator wallet and in this example we are whitelisting the validator public address of each validator to then self bond.

RPC JSON Example:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "icx_sendTransaction",
  "params": {
    "to": "cx0000000000000000000000000000000000000000",
    "from": "hxe9dfcd26dbde32aca8f878c1c4bfd83a0c7f70fc",
    "nid": "0x3",
    "version": "0x3",
    "timestamp": "0x5f58c1531b900",
    "stepLimit": "0x7a1200",
    "nonce": "0x1",
    "dataType": "call",
    "data": {
      "method": "setBonderList",
      "params": { "bonderList": ["hxe9dfcd26dbde32aca8f878c1c4bfd83a0c7f70fc"] }
    },
    "signature": "nq8amjA3fG7o1/ix3dsxVgLJ3tBWJZ5E88B4/Kspl01lagZye66V/8a1/nA5UR1oraxqgc0QoLqWa02TAkYfZQA="
  }
}
```

```bash
curl -X POST --data @rpc-data.json http://localhost:9080/api/v3
```

#### Signing transactions for the `setBond` method.

The following is the curl command to sign a transaction calling the `setBond` method of the governance contract, this process is done with each validator wallet and in this example we are bonding an amount of _1.000.000_ native coin (ICX).

RPC JSON example:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "icx_sendTransaction",
  "params": {
    "to": "cx0000000000000000000000000000000000000000",
    "from": "hxe9dfcd26dbde32aca8f878c1c4bfd83a0c7f70fc",
    "nid": "0x3",
    "version": "0x3",
    "timestamp": "0x5f58c26679370",
    "stepLimit": "0x7a1200",
    "nonce": "0x1",
    "dataType": "call",
    "data": {
      "method": "setBond",
      "params": {
        "bonds": [
          {
            "address": "hxe9dfcd26dbde32aca8f878c1c4bfd83a0c7f70fc",
            "value": "0xd3c21bcecceda1000000"
          }
        ]
      }
    },
    "signature": "W6OhFPH8sEg4t3RHUuAqrHucJ+HOltxjujbHwZKkCnc7CopyAKJN+uFWigPlpekMI5MJ8l6druKR6g5jq8G0eAE="
  }
}
```

```bash
curl -X POST --data @rpc-data.json http://localhost:9080/api/v3as
```

### Verify decentralization of the network

After finishing the process of funding the node wallets, registering the wallets as validators, staking, voting and setting up bond you can verify that the network is decentralized by calling the `getPReps` as following:

```json
{
  "jsonrpc": "2.0",
  "method": "icx_call",
  "id": 365,
  "params": {
    "to": "cx0000000000000000000000000000000000000000",
    "dataType": "call",
    "data": {
      "method": "getPReps"
    }
  }
}
```

```bash
curl -X POST --data @rpc-data.json http://localhost:9080/api/v3
```

### Resources

* [https://github.com/icon-project/gochain-local](https://github.com/icon-project/gochain-local)
* [https://github.com/icon-project/btp/blob/iconloop-v2/e2edemo/scripts/setup/setup\_node.ts](https://github.com/icon-project/btp/blob/iconloop-v2/e2edemo/scripts/setup/setup\_node.ts)
* [https://docs.icon.community/v/icon1/icon-sdks/javascript](https://docs.icon.community/v/icon1/icon-sdks/javascript)&#x20;
* [https://docs.icon.community/v/icon1/references/reference-manuals/icon-json-rpc-api-v3-specification](https://docs.icon.community/v/icon1/references/reference-manuals/icon-json-rpc-api-v3-specification)&#x20;
