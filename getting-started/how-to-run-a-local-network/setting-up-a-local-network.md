---
description: Steps to setup a local network
---

# Setting up a local network

### Requirements

* Download and install [docker](https://docs.docker.com/get-docker/)
* Pull the `goloop/gochain-icon` image
* Build [goloop/gochain-icon](https://github.com/icon-project/gochain-local) locally or in the server that you would be running the multi-node network.

```bash
$ git clone https://github.com/icon-project/goloop.git
$ cd goloop
$ make gochain-icon-image
# You can also use the prebuild image https://hub.docker.com/r/iconcommunity/gochain
# if you can't get the image to build locally
```

* Verify generated image

```bash
$ docker images goloop/gochain-icon
REPOSITORY        TAG   IMAGE ID   CREATED     SIZE           
goloop/gochain-icon   latest 74676aec69ef   2 minutes ago   513MB
```

After successfully running these steps we now have the correct docker image of `gochain-icon-image` accessible on our local docker.\
\
Move to another folder that you would like to use for setting up the root of your local network (in this tutorial we will be using `~/local-network/`) and clone the `gochain-local` repository.

```bash
$ mkdir ~/local-network # or whatever directory you want
$ cd ~/local-network
$ git clone https://github.com/icon-project/gochain-local.git
$ cd gochain-local
```

Inside the `gochain-local` folder we have the necessary scripts and files that we will be using to run our local networks.

### Usage

The following commands will allow you to `start`, `stop` or `pause` the docker containers either using local scripts or `docker compose`.

### Using local scripts.

#### Start container

```bash
# This script runs the network in single-node mode
$ ./run_gochain.sh start
>>> START with compose-single.yml
Creating network "gochain-local_default" with the default driver
Creating gochain-iconee ... done

$ ./run_gochain.sh ps
  Name               Command           State                       
   Ports
  ----------------------------------------------------------------------------------------------------------------------
  gochain-iconee   /entrypoint /bin/sh -c /go ...   Up  8080/tcp, 9080/tcp, 0.0.0.0:9082->9082/tcp,:::9082->9082/tcp
```

Logs messages will be generated at `./chain/iconee.log`.

```bash
$ head ./chain/iconee.log
I|20211008-03:27:35.242715|b6b5|-|main|main.go:433   ____  ___   ____ _   _ _  
  ___ _   _
I|20211008-03:27:35.243626|b6b5|-|main|main.go:433  / ___|/ _ \ / ___| | | |  / \
 |_ _| \ | |
I|20211008-03:27:35.243644|b6b5|-|main|main.go:433 | |  _| | | | |   | |_| | / _ \
  | ||  \| |
I|20211008-03:27:35.243659|b6b5|-|main|main.go:433 | |_| | |_| | |___|  _  |/ ___
\ | || |\  |
I|20211008-03:27:35.243678|b6b5|-|main|main.go:433  \____|\___/ \____|_| |_/_/   \
_\___|_| \_|
I|20211008-03:27:35.243693|b6b5|-|main|main.go:435 Version : v1.0.0
I|20211008-03:27:35.243713|b6b5|-|main|main.go:436 Build   : linux/amd64 tags(rocksdb)-2021-10-05-08:13:18
I|20211008-03:27:35.243732|b6b5|-|metric|metric.go:150 Initialize rootMetricCtx
T|20211008-03:27:35.244757|b6b5|-|TP|transport.go:383 registerPeerHandler &{0xc0001ca540 0xc0001ca4e0 map[] {{0 0} 0 0 0 0} 0xc0001ca5a0} true
T|20211008-03:27:35.244925|b6b5|-|TP|transport.go:383 registerPeerHandler &{0xc0001ca4b0 :8080} true
```

#### Stop container

```bash
$ ./run_gochain.sh stop
>>> STOP with compose-single.yml
Stopping gochain-iconee ... done
Removing gochain-iconee ... done
Removing network gochain-local_default
```

#### Pause container

```bash
$ ./run_gochain.sh pause
```

#### Unpause container

```bash
$ ./run_gochain.sh unpause
```

### Using docker compose

There are two docker compose files that you can use for single node and multi-node networks.

* `compose-single.yml`: run a single gochain node as the previous script example.
* `compose-multi.yml`: run multiple (four) gochain nodes which validate the same blocks.

#### Create and start container

#### For single gochain node

Execute the following command to run the docker configuration of a single node local network.

```bash
$ docker compose -f compose-single.yml up -d
Creating network "gochain-local_default" with the default driver
Creating gochain-iconee ... done
$ docker-compose -f compose-single.yml ps
  Name               Command           State                       
   Ports
  ----------------------------------------------------------------------------------------------------------------------
  gochain-iconee   /entrypoint /bin/sh -c /go ...   Up  8080/tcp, 9080/tcp,
 0.0.0.0:9082->9082/tcp,:::9082->9082/tcp
```

#### For multiple gochain nodes

Execute the following command to run the docker configuration of a multi node local network.

```bash
$ docker compose -f compose-multi.yml up -d
[+] Running 5/5
 ⠿ Network gochain-local_default Create...                             0.0s
 ⠿ Container gochain-local-node3-1  Star...                               0.9s
 ⠿ Container gochain-local-node1-1  Star...                               0.9s
 ⠿ Container gochain-local-node0-1  Star...                               0.9s
 ⠿ Container gochain-local-node2-1  Star...                               0.6s

```

Execute the following command for docker to show a list of all the containerized nodes in the local network.

```bash
$ docker compose -f compose-multi.yml ps
NAME                COMMAND              SERVICE         STATUS          PORTS
gochain-local-node0-1   "/entrypoint /bin/sh…"   node0           running         0.0.0.0:9080->9080/tcp, :::9080->9080/tcp
gochain-local-node1-1   "/entrypoint /bin/sh…"   node1           running         0.0.0.0:9081->9080/tcp, :::9081->9080/tcp
gochain-local-node2-1   "/entrypoint /bin/sh…"   node2           running         0.0.0.0:9082->9080/tcp, :::9082->9080/tcp
gochain-local-node3-1   "/entrypoint /bin/sh…"   node3           running         0.0.0.0:9083->9080/tcp, :::9083->9080/tcp
```

#### Stop and remove container

```bash
$ docker-compose -f compose-single.yml down
```

or

```bash
$ docker-compose -f compose-multi.yml down
```

### Persistence of data

To persist data across docker restarts you have to set `GOCHAIN_CLEAN_DATA` in `./data/single/iconee.env` to `false` for single node setup or in the case of multi-nodes modify `./data/multi/common.env` instead.



### Next steps

[Decentralizing the local network](decentralizing-a-local-network.md)
