# How to run a local network

### Prerequisites

You must install [Docker](https://www.docker.com/get-started/). Install [docker-compose](https://github.com/docker/compose) as well if it is not installed with Docker by default

#### Clone the gochain-local repo

Use the convenience scripts in the [gochain-local](https://github.com/icon-project/gochain-local) repository to run a local ICON network. Here is a brief summary.

```
$ git clone git@github.com:icon-project/gochain-local.git
```

### Using Docker

#### Build the Docker image

```
# Build the image
$ git clone git@github.com:icon-project/goloop.git
$ cd goloop
$ make gochain-icon-image

# Verify the image
$ docker images goloop/gochain-icon
REPOSITORY            TAG       IMAGE ID       CREATED         SIZE
goloop/gochain-icon   latest    74676aec69ef   2 minutes ago   513MB
```

#### Use the convenience script

```
# Go to the gochain-local directory
$ ./run_gochain.sh
Usage: ./run_gochain.sh [start|stop|pause|unpause] (docker-tag)
```

#### Start the container

```
$ ./run_gochain.sh start
>>> START iconee 9082 latest
48e4c66fec68d01e767da91cbbb043c03f595b33cac69c8cdf94f39eaa03b34e

$ docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED         STATUS         PORTS                                        NAMES
48e4c66fec68   goloop/gochain-icon:latest   "/entrypoint /bin/sh…"   9 seconds ago   Up 8 seconds   8080/tcp, 9080/tcp, 0.0.0.0:9082->9082/tcp   gochain-iconee
```

Note that log messages will be generated at `./chain/iconee.log`.

```
$ head ./chain/iconee.log
I|20211008-03:27:35.242715|b6b5|-|main|main.go:433   ____  ___   ____ _   _    _    ___ _   _
I|20211008-03:27:35.243626|b6b5|-|main|main.go:433  / ___|/ _ \ / ___| | | |  / \  |_ _| \ | |
I|20211008-03:27:35.243644|b6b5|-|main|main.go:433 | |  _| | | | |   | |_| | / _ \  | ||  \| |
I|20211008-03:27:35.243659|b6b5|-|main|main.go:433 | |_| | |_| | |___|  _  |/ ___ \ | || |\  |
I|20211008-03:27:35.243678|b6b5|-|main|main.go:433  \____|\___/ \____|_| |_/_/   \_\___|_| \_|
I|20211008-03:27:35.243693|b6b5|-|main|main.go:435 Version : v1.0.0
I|20211008-03:27:35.243713|b6b5|-|main|main.go:436 Build   : linux/amd64 tags(rocksdb)-2021-10-05-08:13:18
I|20211008-03:27:35.243732|b6b5|-|metric|metric.go:150 Initialize rootMetricCtx
T|20211008-03:27:35.244757|b6b5|-|TP|transport.go:383 registerPeerHandler &{0xc0001ca540 0xc0001ca4e0 map[] {{0 0} 0 0 0 0} 0xc0001ca5a0} true
T|20211008-03:27:35.244925|b6b5|-|TP|transport.go:383 registerPeerHandler &{0xc0001ca4b0 :8080} true
```

#### Stop the container

```
$ ./run_gochain.sh stop
>>> STOP gochain-iconee
gochain-iconee
gochain-iconee
```

#### Pause the container

```
$ ./run_gochain.sh pause
```

#### Unpause the container

```
$ ./run_gochain.sh unpause
```

### Using docker-compose

#### Create and Start the container

```
$ docker-compose up -d
[+] Running 2/2
 ⠿ Network gochain-local_default  Created
 ⠿ Container gochain-iconee       Started

$ docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED         STATUS         PORTS                                        NAMES
48e4c66fec68   goloop/gochain-icon:latest   "/entrypoint /bin/sh…"   9 seconds ago   Up 8 seconds   8080/tcp, 9080/tcp, 0.0.0.0:9082->9082/tcp   gochain-iconee
```

#### Check logs

```
$docker-compose logs -f
```

#### Stop the container

```
$docker-compose stop
```

#### Stop and remove the container

```
$docker-compose down
```

#### Pause the container

This will pause the local blockchain. No new blocks will be produced.

```
$docker-compose pause
```

#### Unpause the container

This will resume the blockchain from the same paused height.

```
$docker-compose unpause
```

### Persistence of Data

If you want to persist your data across docker restarts, set `GOCHAIN_CLEAN_DATA` in `./data/dockerenv/iconee` to `false`.

### Further resources

The [IconLoop Docker Hub](https://hub.docker.com/u/iconloop) organization contains useful Docker images related to the ICON project.
