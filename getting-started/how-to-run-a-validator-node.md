# How to run a Validator node

### Prerequisites

Install [Docker](https://www.docker.com/get-started/)

Install [Docker Engine](https://docs.docker.com/engine/install/)

Install [docker-compose](https://github.com/docker/compose) as well if it is not installed with Docker by default

Get the official Docker Image: `iconloop/icon2-node`

```
System Requirements 

CPU:  minimum 4core, recommend 8core +
RAM:  minimum 16GB, recommend 32GB + 
DISK : minimum SSD 1.5TB, recommend SSD 2TB + 
Network: minimum 1Gbps, recommend 2Gbps +

External Communications 
TCP 7100: TCP port used for peer-to-peer connection between peer nodes.
TCP 9000: JSON-RPC or RESTful API port serving application requests.P-Rep must allow TCP connections to port 7100 and 9000 of an external host. ( Use 0.0.0.0/0 for a source from any network )
```

### Getting started

Open docker-compose.yml in a text editor and add the following content:

```
version: "3"
services:
   prep:
      image: iconloop/icon2-node
      container_name: "icon2-node"
      network_mode: host
      restart: "on-failure"
      environment:
         SERVICE: "MainNet"  # MainNet, SejongNet  ## network type 
         GOLOOP_LOG_LEVEL: "debug" # trace, debug, info, warn, error, fatal, panic          
         KEY_STORE_FILENAME: "INPUT_YOUR_KEY_STORE_FILENAME" # e.g. keystore.json read a config/keystore.json
         # e.g. "/goloop/config/keystore.json" read a "config/keystore.json" of host machine
         KEY_PASSWORD: "INPUT_YOUR_KEY_PASSWORD"
         FASTEST_START: "true"    # It can be restored from latest Snapshot DB.
         # You must enter your ICON1 node address. Recent blocks that are not in the backup DB are synchronized from your ICON1 node.
         MIG_ENDPOINT: "http://YOUR_ICON1_SERVER_IPADDR:9000"
     
         ROLE: 3 # Validator = 3, API Endpoint = 0

      cap_add:
         - SYS_TIME

      volumes:         
         - ./data:/goloop/data # mount a data volumes
         - ./config:/goloop/config # mount a config volumes ,Put your used keystore file here.     
         - ./logs:/goloop/logs
```

Start up

```
$ docker-compose pull && docker-compose up -d
```

To see the logs of the ICON2 node you can execute

```
$ tail -f logs/booting.log


$ tail -f logs/goloop.log
```

The directories(data, config, icon, logs …) are created by docker engine, but config directory needs to importing your keystore file.

```
.
├── docker-compose.yml 
├── config               # configuration files                         
│   └── keystore.json   # Import the your keystore file

├── data                # block data
│   ├── 1
│   ├── auth.json
│   ├── cli.sock
│   ├── ee.sock
│   └── rconfig.json

├── icon   # icon1 data for migrate. If a migration is completed, it will be auto-remove
│   └── migrator_bm
└── logs   # log files
    ├── booting.log   
    ├── health.log    # health state log
    ├── chain.log     # goloop chain action logs
    ├── download.log
    ├── download_error.log # download  
    └── goloop.log   # goloop's log file
```

### Docker environments settings

| Name                 | default                 | type | required | description                                                                      |
| -------------------- | ----------------------- | ---- | -------- | -------------------------------------------------------------------------------- |
| SERVICE              | MainNet                 | str  | false    | Service Name - (MainNet, SejongNet)                                              |
| ROLE                 | 3                       | int  | true     | Role of running node. 0: Citizen, 3: P-Rep                                       |
| CONFIG\_URL          |                         | str  | false    |                                                                                  |
| CONFIG\_URL\_FILE    | default\_configure.json | str  | false    |                                                                                  |
| CONFIG\_LOCAL\_FILE  | configure.json          | str  | false    |                                                                                  |
| IS\_AUTOGEN\_CERT    | false                   | bool | false    | Automatically generate certificates                                              |
| FASTEST\_START       | false                   | bool | false    | Download snapshot DB                                                             |
| KEY\_STORE\_FILENAME | keystore.json           | str  | true     | keystore.json file name                                                          |
| KEY\_PASSWORD        |                         | str  | true     | password of keystore.json file                                                   |
| USE\_NTP\_SYNC       | True                    | bool | false    | Whether to use NTP in container                                                  |
| NTP\_SERVER          |                         | str  | false    | NTP Server                                                                       |
| NTP\_REFRESH\_TIME   |                         | int  | false    | ntp refresh time                                                                 |
| SLACK\_WH\_URL       |                         | str  | false    | slack web hook url - If a problem occurs, you can receive an alarm with a slack. |
| USE\_HEALTH\_CHECK   | True                    | bool | false    | Whether to use health check                                                      |
| CHECK\_TIMEOUT       | 10                      | int  | false    | sec - TIMEOUT when calling REST API for monitoring                               |
| CHECK\_PEER\_STACK   | 6                       | int  | false    | sec - Stack value to check the peer for monitoring.                              |
| CHECK\_BLOCK\_STACK  | 10                      | int  | false    | sec - Stack value to check the block for monitoring.                             |
| CHECK\_INTERVAL      | 10                      | int  | false    | sec - check interval for monitoring                                              |
| CHECK\_STACK\_LIMIT  | 360                     | int  | false    | count - count- Restart container when stack value is reached                     |
| GOLOOP\_LOG\_LEVEL   | debug                   | str  | false    | Log Level - (trace,debug,info,warn,error,fatal,panic                             |
| LOG\_OUTPUT\_TYPE    | file                    | str  | false    | sec - check interval for monitoring                                              |

### Further Resources

[Delegate tools](https://github.com/icon-project/preptools/)
