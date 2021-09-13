# Quickstart

## **Becoming a Public Representative \(P-Rep\)**

P-Reps are the consensus nodes that produce, verify blocks and participate in network policy decisions on the ICON Network. To register and become a P-Rep will consist of the following three steps,

### 1. Install P-Rep tools \(CLI for on-chain P-Rep operations\)

### 2. Register as a P-Rep via P-Rep tools

### 3. Build MainNet P-Rep node

## **1. Install P-Rep Tools**

### **Quick Install**

```bash
$ virtualenv venv -p python3    # Create a python 3.6.5+ virtual environment.
$ source venv/bin/activate        # Enter the virtual environment.
(venv) $ pip install preptools
```

### **Manual Install**

For manual installation and P-Rep tool commands, see [full documentation](p-rep-tools.md)

### **2. Register via P-Rep Tools**

**\(Registration fee is 2,000 ICX\)**

There are three ways supply P-Rep registration info to P-Rep Tools; using a json configuration file, using optional arguments, or using interactive command line.

```bash
(venv) $ cat registerPRep.json
{
    "name": "banana node",
    "country": "USA",
    "city": "New York",
    "email": "banana@example.com",
    "website": "https://icon.banana.com",
    "details": "https://icon.banana.com/json",
    "p2pEndpoint": "node.example.com:7100"
}

# Using json file
(venv) $ preptools registerPRep --prep-json registerPRep.json -k keystore

# Using optional argument and interactive command line
(venv) $ preptools registerPRep -k keystore --name "kokoa node" --email "kokoa@example.com"
> Password: 
 > country : USA
 > city : New York
 > website : https://icon.kokoa.com
 > details : https://icon.kokoa.com/json/details.json
 > p2pEndpoint : node.example.com:7100

[Request] ======================================================================
{
    "from_": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6",
    "to": "cx0000000000000000000000000000000000000000",
    "value": 2000000000000000000000,
    "step_limit": 268435456,
    "nid": 3,
    "nonce": null,
    "version": 3,
    "timestamp": null,
    "method": "registerPRep",
    "data_type": "call",
    "params": {
        "name": "kokoa node",
        "email": "kokoa@example.com",
        "country": "USA",
        "city": "New York",
        "website": "https://icon.kokoa.com",
        "details": "https://icon.kokoa.com/json/details.json",
        "p2pEndpoint": "node.example.com:7100"
    }
}
```

See [registerPRep command details](p-rep-tools.md#command-line-interfaces-clis)

The "details" parameter is an external json file that holds additional information about the P-Rep. We strongly recommend that you register this information. Example,

```javascript
{
    "representative": {
        "logo": {
            "logo_256": "https://icon.foundation/img/img-256.png",
            "logo_1024": "https://icon.foundation/img/img-1024.png",
            "logo_svg": "https://icon.foundation/img/img-logo.svg"
        },
        "media": {
            "steemit": "",
            "twitter": "",
            "youtube": "",
            "facebook": "",
            "github": "",
            "reddit": "",
            "keybase": "",
              "telegram": "",
            "wechat": ""
        }
  },  "server": {
        "location": {
            "country": "",
            "city": ""
        },
        "server_type": "",
        "api_endpoint": ""
    }
}
```

## **3. Build MainNet P-Rep Node**

{% page-ref page="../icon-2.0/goloop/get-started/build.md" %}

### **Pre-requisites**

The specifications below are recommended for main P-Reps \(top 22\). Sub P-Reps \(outside of top 22\) can run lower-end specs as they do not produce/validate blocks but only serve as citizen nodes. Current blockchain size is 1.1T as of this update \(2021, September 13th\).

#### **Hardware considering the node is running on AWS**

* c5.4xlarge \(2TB SSD, gp3, 3000 IOPS, 125MBps\)
  * [https://aws.amazon.com/ec2/instance-types/c5/](https://aws.amazon.com/ec2/instance-types/c5/)

#### **Software**

**OS Requirement**

* Linux \(CentOS 7 or Ubuntu 16.04+\)

**Open Ports**

* TCP 7100: gRPC port used for peer-to-peer connection between peer nodes.
* TCP 9000: JSON-RPC or RESTful API port serving application requests.

### **3.1 Install Docker**

**Docker 18.x+**

```text
## Update the apt package index:
$ sudo apt-get update

## Install necessary packages:
$ sudo apt-get install  -y systemd apt-transport-https ca-certificates curl gnupg-agent software-properties-common 

## Add Docker's official GPG key:
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

## Add the apt repository
$ add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

## Update the apt package index:
$ sudo apt-get update

## Install docker-ce:
$ sudo apt-get -y install docker-ce docker-ce-cli containerd.io

## Add your user to the docker group with the following command.
$ sudo usermod -aG docker $(whoami)

## Set Docker to start automatically at boot time:
$ sudo systemctl enable docker.service

## Finally, start the Docker service:
$ sudo systemctl start docker.service

## Then we'll verify docker is installed successfully by checking the version:
$ docker version
```

### **3.2 Install Docker Compose**

```text
## Install python-pip
$ sudo apt-get install -y python-pip

## Then install Docker Compose:
$ sudo pip install docker-compose

## To verify the successful Docker Compose installation, run:
$ docker-compose version
```

### **3.3 Running a P-Rep Node Container using docker-compose**

#### Pull ICON2 image <a id="docs-internal-guid-521fbdaf-7fff-9b14-9d7d-7f7424107303"></a>

```text
# docker pull <goloop package image>
```

#### Keystore

If you have your own KEYSTORE, store it and the password file in the configuration directory.

```text
# cp keystore.json ./config/keystore.json
# echo -n "password" > ./config/keysecret
```

If you have your own KEYSTORE, store it and the password file in the configuration directory.

#### Create docker-compose config

```text
version: "3.7"
services:
    node:
        image: iconloop/goloop-icon:latest
        env_file:
            - ./node.env
        volumes:
            - ./data:/goloop/data
            - ./mainnet:/mainnet
            - ./config:/goloop/config
        ports:
            - "9080:9080"
            - "8080:8080"
```

Start containers

```text
# docker-compose up -d
```

Check the status

```text
# docker-compose exec node goloop system info
{
  "buildVersion": "v0.9.9-338-g19c58f03-dirty",
  "buildTags": "darwin/amd64 tags()-2021-09-03-14:35:59",
  "setting": {
    "address": "hx14637aec2d2941f56d4878557b298bea5460fdaa",
    "p2p": "node0:8080",
    "p2pListen": "0.0.0.0:8080",
    "rpcAddr": ":9080",
    "rpcDump": false
  },
  "config": {
    "eeInstances": 1,
    "rpcDefaultChannel": "",
    "rpcIncludeDebug": false
  }
}
```

### **Citizen Node**

Citizen nodes are non-producing nodes on the ICON network. A non-producing node is a node that is not configured to produce blocks, instead it is constantly synchronizing the latest blocks from the network and can be used for on-chain queries. Citizen nodes **DO NOT** require registration.

ICON node software is identical for P-Rep nodes and Citizen nodes, one can follow the same procedures to set up a P-Rep node, without registration, to operate a Citizen node.

### \*\*\*\*[**Experiment on TestNet**](../introduction/the-icon-network/testnet.md#testnet)\*\*\*\*

## **Having Trouble?**

For troubleshoots and other questions, please join [ICON P-Rep MainNet](https://t.me/joinchat/H33WtRIOelpmVW2JExULOQ) or [ICON P-Rep TestNet](https://t.me/joinchat/H33WtUfvyjOtm6Uh1D9TnA) Telegram Channels.

