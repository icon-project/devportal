---
description: 'The source code is found on GitHub at https://github.com/icon-project/t-bears'
---

# Installation

This guide will walk you through the steps of T-Bears installation. There are three ways of installing T-Bears - Docker, PyPi, and building from source.

### T-Bears Docker \(Quick and Easy\)

[https://hub.docker.com/r/iconloop/tbears](https://hub.docker.com/r/iconloop/tbears)

You can run T-Bears on your machine using Docker. The below command will download T-Bears Docker image and run [T-Bears Docker container](installation.md#t-bears-docker-quick-and-easy).

```bash
docker run -it --name tbears-container -p 9000:9000 iconloop/tbears:mainnet
```

Please check the following links for more information. [T-Bears Docker](https://github.com/icon-project/t-bears/tree/master/Docker)

### Install T-Bears with PIP

[https://pypi.org/project/tbears](https://pypi.org/project/tbears)

This chapter will explain how to install T-Bears on your system with PIP.

#### Requirements

ICON SCORE development and execution requires the following environments :

* OS: MacOS, Linux
  * Windows are not supported yet.
* Python
  * Make Virtual Env for Python 3.6.5+ \(recommended version, 3.7 is not supported\)
  * check your python version

    ```bash
    $ python3 -V
    ```

  * IDE: Pycharm is recommended.

**Softwares**

* RabbitMQ: 3.7 and above. [homepage](https://www.rabbitmq.com/)

**Libraries**

| name | description | github |
| :--- | :--- | :--- |
| LevelDB | ICON SCORE uses levelDB to store its states. | [LevelDB GitHub](https://github.com/google/leveldb) |
| libsecp256k1 | ICON SCORE uses secp256k1 to sign and validate a digital signature. | [secp256k1 GitHub](https://github.com/bitcoin-core/secp256k1) |

#### Setup on MacOS

```bash
# install develop tools
$ brew install leveldb
$ brew install autoconf automake libtool pkg-config

# install RabbitMQ and start service
$ brew install rabbitmq
$ brew services start rabbitmq

# Create a working directory
$ mkdir work
$ cd work

# setup the python virtualenv development environment
$ pip3 install virtualenv
$ virtualenv -p python3 .
$ source bin/activate

# Install the ICON SCORE dev tools
(work) $ pip install tbears
```

#### Setup on Linux

```bash
# install develop tools
$ sudo apt-get install autoconf automake libtool pkg-config
# Install levelDB
$ sudo apt-get install libleveldb-dev
# Install libSecp256k
$ sudo apt-get install libsecp256k1-dev

# install RabbitMQ and start service
$ sudo apt-get install rabbitmq-server
$ sudo service rabbitmq-server start

# Create a working directory
$ mkdir work
$ cd work

# Setup the python virtualenv development environment
$ virtualenv -p python3 .
$ source bin/activate

# Install the ICON SCORE dev tools
(work) $ pip install tbears
```

### Build from source

First, clone this project \([https://github.com/icon-project/t-bears](https://github.com/icon-project/t-bears)\). Then go to the project directory, create a virtualenv environment, and run the build script. You can then install T-Bears with the .whl file.

```bash
$ virtualenv -p python3 venv  # Create a virtual environment.
$ source venv/bin/activate    # Enter the virtual environment.
(venv)$ ./build.sh            # run build script
(venv)$ ls dist/              # check result wheel file
tbears-x.y.z-py3-none-any.whl
```

