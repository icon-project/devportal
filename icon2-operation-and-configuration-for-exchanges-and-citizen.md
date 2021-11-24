# ICON2 Operation and configuration for Exchanges and Citizen

## Quick start to running ICON2 node <a href="quick-start-to-running-icon2-node" id="quick-start-to-running-icon2-node"></a>

### Prerequisites

We recommend running nodes using Docker.

* Install Docker Engine: [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)
* Install Docker Compose: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)
* Official Docker Image: `iconloop/icon2-node`

{% hint style="info" %}
**System Requirements**

* CPU: minimum 2core, recommend 4core +
* RAM: minimum 8GB, recommend 16GB +
* DISK : minimum 1.5TB, recommend SSD 2TB +
* Network: minimum 100Mbps, recommend 1 Gbps+

**External Communications**

* TCP 7100: TCP port used for peer-to-peer connection between peer nodes.
* TCP 9000: JSON-RPC or RESTful API port serving application requests.

P-Rep must allow TCP connections to port 7100 and 9000 of an external host. ( Use 0.0.0.0/0 for a source from any network )
{% endhint %}

{% hint style="info" %}
All RESTful APIs are the same as ICON1. However, only the "debug endpoint" of "estimate Step" used for fee inquiry was changed. Only the debug endpoint used for the fee inquiry( estimateStep ) has been changed.

The debug endpoint is different.

* ICON1: /api/debug/v3.
* **ICON2: /api/v3d**
{% endhint %}

### How to install ICON2 node for MainNet <a href="how-to-install-icon2-node-for-mainnet" id="how-to-install-icon2-node-for-mainnet"></a>

#### **Default Directory Structure**

We recommend that you use the ICON2 official image(iconloop/icon2-node).** **If you want to create your new image, see the next section.

If you have imported or created an image, proceed to the next step.

1. Create a `base` directory.
2. Create a `config` directory and import a Keystore file from `ICON1` server.

```
$ mkdir /app/icon2-node # make a base directory
$ cd /app/icon2-node
$ mkdir -p /app/icon2-node/config  # make a config directory, 

# Import a keystore file to /app/icon2-node/config/ from ICON1 server
```

Open docker-compose.yml in a text editor and add the following content:

```
version: "3"
services:
   prep:
      image: iconloop/icon2-node
      container_name: "icon2-mainnet"
      network_mode: host
      restart: "on-failure"
      environment:
         SERVICE: "MainNet"  # MainNet, sejong
         GOLOOP_LOG_LEVEL: "debug" # trace, debug, info, warn, error, fatal, panic          
         IS_AUTOGEN_CERT: "true"   # true, false
         FASTEST_START: "true"     # true, false  # It will be download the Snapshot DB
         ROLE: 0 # citizen = 0, preps = 3 
         SEEDS: "seed-ctz.solidwallet.io:7100" 

      cap_add:
         - SYS_TIME

      volumes:         
         - ./config:/goloop/config # mount a data volumes , key file
         - ./data:/goloop/data # mount a data volumes
         - ./logs:/goloop/logs # mount a log volumes
```

To start an ICON2 node, enter the following commands: (These commands includes docker pull )

```
$ docker-compose up -d
```

### How to install ICON2 node for SejongNet(TestNet)

1. Create a `base` directory.
2. Create a config directory and import a Keystore file from ICON1 server.

```
$ mkdir /app/icon2-testnet # make a base directory
$ cd /app/icon2-testnet
$ mkdir -p /app/icon2-testnet/config  # make a config directory, 

# Import a keystore file to /app/icon2-testnet/config/ from ICON1 server
```

Using docker-compose command(Recommended)

```
version: '3'
services:
  icon2-node:
    image: 'looploy/icon2-node'
    restart: "on-failure"
    container_name: "icon2-testnet"
    network_mode: "host"
    environment:
      SERVICE: "SejongNet"  # MainNet, SejongNet(TestNet) 
      IS_AUTOGEN_CERT: "true"   # true, false
      FASTEST_START: "true"     # true, false  # It will be download the Snapshot DB
      ROLE: 0                   # citizen = 0, preps = 3
      SEEDS: "seed-sejong.solidwallet.io:7100" 
    cap_add:
      - SYS_TIME

    volumes:
      - ./config:/goloop/config
      - ./data:/goloop/data
      - ./logs:/goloop/logs
```

## How to check the node state <a href="how-to-check-the-node-state" id="how-to-check-the-node-state"></a>

* get state

```
# docker exec -it icon2-node goloop chain ls 
[
  {
    "cid": "0x1",
    "nid": "0x1",
    "channel": "icon_dex",
    "state": "import_icon 14462097 running",
    "height": 57940,
    "lastError": ""
  }
]
```

* get detail state

```
# docker exec -it icon2-node goloop chain inspect 0x1
{
  "cid": "0x1",
  "nid": "0x1",
  "channel": "icon_dex",
  "state": "import_icon 14464527 running",
  "height": 57944,
  "lastError": "",
  "genesisTx": {
    "accounts": [
      {
        "address": "hx54f7853dc6481b670caf69c5a27c7c8fe5be8269",
        "balance": "0x2961fff8ca4a62327800000",
        "name": "god"
      },
      {
        "address": "hx1000000000000000000000000000000000000000",
        "balance": "0x0",
        "name": "treasury"
      }
    ],
    "message": "A rhizome has no beginning or end; it is always in the middle, between things, interbeing, intermezzo. The tree is filiation, but the rhizome is alliance, uniquely alliance. The tree imposes the verb \"to be\" but the fabric of the rhizome is the conjunction, \"and ... and ...and...\"This conjunction carries enough force to shake and uproot the verb \"to be.\" Where are you going? Where are you coming from? What are you heading for? These are totally useless questions.\n\n - Mille Plateaux, Gilles Deleuze \u0026 Felix Guattari\n\n\"Hyperconnect the world\""
  },
  "config": {
    "dbType": "rocksdb",
    "platform": "icon",
    "seedAddress": "20.20.6.87:7100",
    "role": 3,
    "concurrencyLevel": 1,
    "normalTxPool": 10000,
    "nodeCache": "small",
    "channel": "icon_dex",
    "secureSuites": "none,tls,ecdhe",
    "secureAeads": "chacha,aes128,aes256",
    "defaultWaitTimeout": 0,
    "maxWaitTimeout": 0,
    "txTimeout": 60000,
    "autoStart": false
  },
  "module": {
    "metrics": {
      "consensus_height": 57945,
      "consensus_height_duration": 63234,
      "consensus_round": 0,
      "consensus_round_duration": 63234,
      "network_recv_cnt": 7613146,
      "network_recv_sum": 5932966367,
      "network_send_cnt": 7663766,
      "network_send_sum": 7377659111,
      "txlatency_commit": null,
      "txlatency_finalize": null,
      "txpool_add_cnt": null,
      "txpool_add_sum": null,
      "txpool_drop_cnt": null,
      "txpool_drop_sum": null,
      "txpool_remove_cnt": null,
      "txpool_remove_sum": null,
      "txpool_user_add_cnt": null,
      "txpool_user_add_sum": null,
      "txpool_user_drop_cnt": null,
      "txpool_user_drop_sum": null,
      "txpool_user_remove_cnt": null,
      "txpool_user_remove_sum": null
    },
    "network": {
      "p2p": {
        "children": [],
        "friends": [
          {
            "addr": "20.20.6.84:7100",
            "id": "hx59fbe7660aac5e1487dfb21024940441fd7d21fc",
            "in": true,
            "role": 3
          },
          {
            "addr": "20.20.6.85:7100",
            "id": "hx51a27fe90ced46309d8d9a0147889c02469c988b",
            "in": true,
            "role": 3
          },
          {
            "addr": "20.20.6.87:7100",
            "id": "hx8c0d92d64087cafb00353209e0736ff382c05e0c",
            "in": true,
            "role": 3
          }
        ],
        "nephews": [],
        "orphanages": [
          {
            "addr": "20.20.6.86:7100",
            "id": "hxa1d93a0cf251af0d4f17863b0b912b6b3a75b606",
            "in": true,
            "role": 1
          }
        ],
        "others": [],
        "parent": {},
        "roots": {
          "20.20.6.83:7100": "hx13dae65d3955c87eb671be0659276a0be0fc44a6",
          "20.20.6.84:7100": "hx59fbe7660aac5e1487dfb21024940441fd7d21fc",
          "20.20.6.85:7100": "hx51a27fe90ced46309d8d9a0147889c02469c988b",
          "20.20.6.87:7100": "hx8c0d92d64087cafb00353209e0736ff382c05e0c"
        },
        "seeds": {
          "20.20.6.83:7100": "hx13dae65d3955c87eb671be0659276a0be0fc44a6",
          "20.20.6.84:7100": "hx59fbe7660aac5e1487dfb21024940441fd7d21fc",
          "20.20.6.85:7100": "hx51a27fe90ced46309d8d9a0147889c02469c988b",
          "20.20.6.86:7100": "hxa1d93a0cf251af0d4f17863b0b912b6b3a75b606",
          "20.20.6.87:7100": "hx8c0d92d64087cafb00353209e0736ff382c05e0c"
        },
        "self": {
          "addr": "20.20.6.83:7100",
          "id": "hx13dae65d3955c87eb671be0659276a0be0fc44a6",
          "in": false,
          "role": 3
        },
        "trustSeeds": {
          "20.20.6.87:7100": ""
        },
        "uncles": []
      }
    }
  }
}
```

## Network separation using multiple citizens <a href="network-separation-using-multiple-citizens" id="network-separation-using-multiple-citizens"></a>

The port used for network separation has been changed.

In block synchronization, TCP 7100 is synchronized instead of a WebSocket using TCP 9000.

* Before change
  * External citizen node ← TCP 9000(WebSocket) → Internal citizen node
* After change
  * External citizen node ← TCP 7100 → Internal citizen node

For example:

\[ External Citizen Node ]

IPaddr : 20.20.20.199

```
version: "3"
services:
   prep:
      image: iconloop/icon2-node
      container_name: "icon2-mainnet"
      network_mode: host
      restart: "on-failure"
      environment:
         SERVICE: "MainNet"  # MainNet, sejong
         GOLOOP_LOG_LEVEL: "debug" # trace, debug, info, warn, error, fatal, panic          
         IS_AUTOGEN_CERT: "true"   # true, false
         FASTEST_START: "true"     # true, false  # It will be download the Snapshot DB
         ROLE: 0 # citizen = 0, preps = 3 

      cap_add:
         - SYS_TIME

      volumes:         
         - ./data:/data # mount a data volumes
         - ./config:/config # mount a data volumes , key file
```
