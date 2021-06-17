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

For manual installation and P-Rep tool commands, see [full documentation](https://www.icondev.io/docs/p-rep-tools-preptools-tutorial)

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

See [registerPRep command details](https://www.icondev.io/docs/p-rep-tools-preptools-tutorial#section-register-p-rep)

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

**Examples using Ubuntu 16.04+**

### **Pre-requisites**

The specifications below are recommended for main P-Reps \(top 22\). Sub P-Reps \(outside of top 22\) can run lower-end specs as they do not produce/validate blocks but only serve as citizen nodes. Current blockchain size is 153G as of this update \(2020/3/17\).

#### **Hardware**

| Description | Minimum Specifications | Recommended Specifications |
| :--- | :--- | :--- |
| CPU model | Intel\(R\) Xeon\(R\) CPU @ 3.00GHz | Intel\(R\) Xeon\(R\) CPU @ 3.00GHz |
| vCPU \(core\) | 16 | 36 |
| RAM | 32G | 72G |
| Disk | 200G | 500G |
| Network | 1Gbps | 1Gbps |

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

Create a file named `docker-compose.yml` in a text editor and add the following content:

```yaml
version: "3"
services:
  prep-node:
     image: "iconloop/prep-node"
     container_name: "prep-mainnet"
     network_mode: host     
     restart: "always"
     environment:
        NETWORK_ENV: "mainnet"
        LOG_OUTPUT_TYPE: "file"
        SWITCH_BH_VERSION3: "10324749"
        CERT_PATH: "/cert"
        LOOPCHAIN_LOG_LEVEL: "DEBUG"
        ICON_LOG_LEVEL: "DEBUG"        
        FASTEST_START: "yes" # Restore from lastest snapshot DB
        PRIVATE_KEY_FILENAME: "YOUR_KEYSTORE or YOUR_CERTKEY FILENAME" # only file name
        PRIVATE_PASSWORD: "YOUR_KEY_PASSWORD"
     cap_add:
        - SYS_TIME      
     volumes:
        - ./data:/data # mount a data volumes
        - ./cert:/cert # Automatically generate cert key files here
     ports:
        - 9000:9000
        - 7100:7100
```

**Run docker-compose**

```text
$ docker-compose up -d
```

The parameter provided **FASTEST\_START: "yes"** will download the latest full-hour blockchain snapshot, and the remaining blocks will be synchronized automatically. You can check server status and current block height via this API: [http://{YOUR\_NODE\_IP}:9000/api/v1/status/peer](http://{YOUR_NODE_IP}:9000/api/v1/status/peer).

### **Citizen Node**

Citizen nodes are non-producing nodes on the ICON network. A non-producing node is a node that is not configured to produce blocks, instead it is constantly synchronizing the latest blocks from the network and can be used for on-chain queries. Citizen nodes **DO NOT** require registration.

ICON node software is identical for P-Rep nodes and Citizen nodes, one can follow the same procedures to set up a P-Rep node, without registration, to operate a Citizen node.

### **Experiment on TestNet**

TestNet Pagoda is available for node operation testing, you can experiment with this  [docker-compose template](http://download.solidwallet.io/zicon/docker-compose.yml).

#### **Pagoda Network Information**

|  |  |
| :--- | :--- |
| Name | Pagoda |
| Node | [https://zicon.net.solidwallet.io](https://zicon.net.solidwallet.io) |
| API endpoint | [https://zicon.net.solidwallet.io/api/v3](https://zicon.net.solidwallet.io/api/v3) |
| Network ID \(nid\) | 80 |
| Tracker | [https://zicon.tracker.solidwallet.io/](https://zicon.tracker.solidwallet.io/) |
| Transaction fee | on |
| SCORE audit | on |

To receive test ICX, please send email to `testicx@icon.foundation` with the following information.

* Testnet node URL
* Address to receive the testnet ICX. Address should start with `hx` followed by a 20-byte string.

## **Having Trouble?**

For troubleshoots and other questions, please join [ICON P-Rep MainNet](https://t.me/joinchat/H33WtRIOelpmVW2JExULOQ) or [ICON P-Rep TestNet](https://t.me/joinchat/H33WtUfvyjOtm6Uh1D9TnA) Telegram Channels.

