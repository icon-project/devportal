---
description: WIP
---

# ICON2 Operation and configuration for Exchanges and Citizen

## Quick start to running ICON2 node <a href="quick-start-to-running-icon2-node" id="quick-start-to-running-icon2-node"></a>

### Prerequisites

We recommend running nodes using Docker.

* Install Docker Engine: [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)
* Install Docker Compose: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)
* Official Docker Image: `iconloop/icon2-node`

{% hint style="info" %}
In **STAGE3-1** step, **deposits and withdrawals must be stopped**. In **STAGE3-9** step, the **new ICON2 node is launched**. Sejongnet can be tested immediately, and the MainNet upgrade time will be announced by the Foundation.



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

      cap_add:
         - SYS_TIME

      volumes:         
         - ./data:/data # mount a data volumes
         - ./config:/config # mount a data volumes , key file

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
      ###################################################
      ROLE: 0                   # citizen = 0, preps = 3 
      ###################################################
    cap_add:
      - SYS_TIME

    volumes:
      - ./config:/goloop/config
      - ./data:/goloop/data
      - ./logs:/goloop/logs

    ports:
      - 7100:7100
      - 9000:9000
```

