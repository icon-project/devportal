# BTP Development Instructions

## Setup and Installation

[comment]: <&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![0](https://progress-bar.dev/0/)>

____
This section guides you through a setup of a Blockchain Transmission Protocol (BTP) connecting two networks. ICON and Moonriver networks would be used to demonstrate the scheme in this example. This document is aimed to support various groups of people who might be not developers but requires minimum knowledge of computer skills. As of now, we attempt to make these instructions as simple and detail as possible so everyone can do it manually and can be easy to catch up on. Please follow these instructions and do not skip any steps unless you understand what you are currently doing.

We have a script to make this example get easier. Please follow this instruction [[link](https://github.com/icon-project/btp/blob/icondao/docker-compose/goloop2moonbeam/README.MD)] if you are interested in running via docker. Otherwise, please continue following instructions below

**Supporting Operating Systems:**

- MacOS

- Linux

**Software Requirements:**

- `Docker`: please click on this link [[Docker](https://docs.docker.com/engine/)] and follow the instructions to install Docker on your local machine
- `Docker Compose`: please click on this [[Docker Compose](https://docs.docker.com/compose/install/)] and follow the instructions to install Docker Compose on your local machine
- `Java (JDK 11)`:  please click on this [[JDK11](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)] and download a version that matches the operating system of your local machine

After install `JDK11`, please check your version:

```shell
$ java --version
java 11.0.11 2021-04-20 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.11+9-LTS-194)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.11+9-LTS-194, mixed mode)
```

- `NodeJS` (requires `Node >= 10.x`): You can use this link [[NodeJS](https://nodejs.org/en/download/)] and download a version that matches the operating system of your local machine

Please check your current version:

```shell
$ node -v
v15.12.0
$ npm -v
7.6.3
```

- `Yarn` and `Truffle`: run a command as below to install these util libraries:

```shell
$ npm install --global yarn truffle@5.3.0
```

Please check your current version:

```shell
$ truffle version
Truffle v5.3.0 (core: 5.3.0)
Solidity - 0.7.6 (solc-js)
Node v15.12.0
Web3.js v1.2.9
$ yarn -v
1.22.10
```

- `Gradle`: please click on this [[Gradle](https://gradle.org/install/)] and install the version 6.7.1

```shell
$ gradle -v
------------------------------------------------------------
Gradle 6.7.1
------------------------------------------------------------

Build time:   2020-11-16 17:09:24 UTC
Revision:     2972ff02f3210d2ceed2f1ea880f026acfbab5c0

Kotlin:       1.3.72
Groovy:       2.5.12
Ant:          Apache Ant(TM) version 1.10.8 compiled on May 10 2020
JVM:          11.0.11 (Oracle Corporation 11.0.11+9-LTS-194)
OS:           Mac OS X 10.16 x86_64
```

- `Golang`: requires version GoLang 1.13+. You can use this link [[Go](https://golang.org/doc/install)] and download a version that matches the operating system of your local machine

Please check your current version:

```shell
$ go version
go version go1.16 darwin/amd64
```

- `jq`: requires version jq 1.6. You can use this link [[jq](https://stedolan.github.io/jq/download/)] and download a version that matches the operating system of your local machine

- Download CLI tools:

```bash
# Get goloop-cli
go get github.com/icon-project/goloop/cmd/goloop

# Get ethkey-cli
go get github.com/ethereum/go-ethereum/cmd/ethkey

# **** NOTE ****:
# Add 'path/to/folder/go/bin' to the PATH environment variable.
# For example: 
# export GOPATH=~/go
# export GOBIN=$GOPATH/bin
# export PATH=$PATH:$GOBIN
```

## Deployment Instructions

____
<p align="center">
  <img src="./images/Deployment-Module.png" width="800" height="300" />
</p>

The next following will provide instructions on how to deploy:

- Deploy a node in ICON's network
- Deploy a node in Moonriver's network
- Deploy smart contracts on ICON's network
- Deploy smart contracts on Moonriver's network
- Deploy the BTP Message Relay (BMR)

<span style="color:red">**Attention:** The deployments and settings might have some dependencies, which mean that one setting/deployment must complete prior another ones. Hence, please follow the below order to completely and safely deploy and set up configurations. If you mess up or skip one step, it might affect your deployment and setting which, in fact, lead to failure consequence</span> 

### Create Blockchain Nodes

____

#### 1. Deploy ICON Node

<p align="center">
  <img src="./images/Deploy-ICON-Node.png" width="800" height="300" />
</p>

- Preparation

```bash
mkdir BTPExample && cd BTPExample

PROJECT_DIR=${PWD}

# Clone project
git clone https://github.com/icon-project/btp.git

cd $PROJECT_DIR/btp

git checkout icondao
```

- ICON's configuration files are located in `$PROJECT_DIR/btp/docker-compose/goloop2moonbeam/goloop/config`. You can modify these files for your settings
  - `icon.genesis.json`: ICON's genesis configuration file
  - `goloop.server.json`: ICON's node configuration file
  - `goloop.keystore.json`: node's keystore
  - `goloop.keysecret`: node's password key

- Run below commands to make `btpsimple-image`:

```bash
cd $PROJECT_DIR/btp

make btpsimple-image
```  

- Start ICON's Node

```bash
cd $PROJECT_DIR/btp/docker-compose/goloop2moonbeam

docker-compose up goloop
```

#### 2. Deploy Moonriver Node

<p align="center">
  <img src="./images/Deploy-MOONRIVER-Node.png" width="800" height="300" />
</p>

A node on Moonriver can be easily deployed by running the below command:

```bash
cd $PROJECT_DIR/btp/docker-compose/goloop2moonbeam

docker-compose up moonbeam
```

For the next following parts, we will instruct you to deploy smart contracts on each network. Please click on 'Next' and bear with us

&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;
[<--- Prev](./BTP-Development-Resources.md#prerequisites) &emsp; &emsp; &emsp; &emsp; [Next --->](./Smart-Contracts-ICON.md#smart-contracts-on-icon-deployment)

<!--<p align="center">-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/BTP-Development-Resources.md" class="button"><--- Prev &emsp; &emsp;</a>-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/Smart-Contracts-ICON.md" class="button">&emsp; &emsp; Next </a>-->
<!--</p> -->
