# Node operation and configuration

### **Start Node**

**Run docker-compose up.**

```text
$ docker-compose up -d
```

The `docker ps` command shows the list of running docker containers.

```text
$ docker ps
CONTAINER ID   IMAGE                                                          COMMAND                CREATED              STATUS                          PORTS                                                                 NAMES
0de99e33cdc9     iconloop/prep-node:1910261021xc97f33    "/src/entrypoint.sh"      2 minutes ago        Up 2 minutes(healthy)    0.0.0.0:7100->7100/tcp, 0.0.0.0:9000->9000/tcp prep_prep_1
```

Corresponding columns,

| Column | Description |
| :--- | :--- |
| CONTAINER ID | Container  ID |
| IMAGE | P-Rep Node's  image name |
| COMMAND | The script will be executed whenever a P-Rep Node container is run |
| STATUS | Healthcheck status. One of "starting" , "healthy", "unhealthy" or "none" |
| PORTS | Exposed ports on the running container |
| NAMES | Container name |

You can read the container booting log from the log folder.

```text
$ tail -f data/PREP-MainNet/log/booting_20190419.log
[2019-10-28 10:43:05.204] Your IP: xx.xx.xx.xx
[2019-10-28 10:43:05.209] RPC_PORT: 9000 / RPC_WORKER: 3
[2019-10-28 10:43:05.214] DEFAULT_PATH=/data/mainnet in Docker Container
[2019-10-28 10:43:05.219] DEFAULT_LOG_PATH=/data/mainnet/log
[2019-10-28 10:43:05.224] DEFAULT_STORAGE_PATH=/data/mainnet/.storage
[2019-10-28 10:43:05.229] scoreRootPath=/data/mainnet/.score_data/score
[2019-10-28 10:43:05.234] stateDbRootPath=/data/mainnet/.score_data/db
[2019-10-28 10:43:05.239] Time synchronization with NTP / NTP SERVER: time.google.com
[2019-10-28 10:43:12.022] P-REP package version info - _1910261021xc97f33
[2019-10-28 10:43:12.853] iconcommons             1.1.2
iconrpcserver           1.4.5
iconsdk                 1.2.0
iconservice             1.5.15
loopchain               2.4.16
```

### **Entrypoint.sh Diagram**

