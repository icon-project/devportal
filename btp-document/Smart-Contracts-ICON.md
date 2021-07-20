## Smart Contracts on ICON Deployment

____

<p align="center">
  <img src="./images/Deploy-Contracts-ICON.png" width="800" height="300" />
</p>

Before deploying these contracts, please make sure you have already started a local node in the ICON Network. If you have not yet done this step, please check out this [link](BTP-Development-Instructions.md#1-deploy-icon-node) and accomplish it

Clone this BTP project and copy this [[Java-Folder](./files/javascore.tar)] to your local folder that has created when you started your local node

```shell
$ cd INode

$ git clone https://github.com/icon-project/btp.git

$ cd btp && git checkout icondao

# Copy javascore.tar to $CONFIG_DIR and extract
$ tar -xzvf javascore.tar
```

### 1. Preparation - Compiling Requiring Java Files

____

- Run below commands to compile Java files and generate requiring `.jar` files:

```shell
$ CONFIG_DIR=/path/to/config/folder/INode

$ cd $CONFIG_DIR/btp/javascore/bmv/eventDecoder

$ gradle buildKusamaDecoder

$ gradle buildMoonriverDecoder

$ cd $CONFIG_DIR/btp/javascore/bmv/parachain

$ gradle optimizedJar
#  The commands above compiles all java files to build BMV contract

#  Next, move on compiling files to build BMC contract
$ cd $CONFIG_DIR/javascore/bmc

$ gradle optimizedJar

#  Next, move on compiling files to build NativeCoinBSH contract
$ cd $CONFIG_DIR/javascore/nativecoin

$ gradle optimizedJar

#  Next, move on compiling files to build IRC31Token contract
$ cd $CONFIG_DIR/javascore/javaee-tokens

$ gradle optimizedJar
```

- Finally, copy all generated `.jar` files to a config folder of goloop container by running these commands:

```shell
$ mkdir -p ${CONFIG_DIR}/javascore
$ cp $CONFIG_DIR/btp/javascore/bmv/parachain/build/libs/parachain-BMV-optimized.jar ${CONFIG_DIR}/javascore/
$ cp $CONFIG_DIR/btp/javascore/bmv/eventDecoder/build/libs/KusamaEventDecoder-optimized.jar ${CONFIG_DIR}/javascore/
$ cp $CONFIG_DIR/btp/javascore/bmv/eventDecoder/build/libs/MoonriverEventDecoder-optimized.jar ${CONFIG_DIR}/javascore/
$ cp $CONFIG_DIR/javascore/bmc/build/libs/bmc-0.1.0-debug.jar ${CONFIG_DIR}/javascore/
$ cp $CONFIG_DIR/javascore/nativecoin/build/libs/nativecoin-0.1.0-debug.jar ${CONFIG_DIR}/javascore/
$ cp $CONFIG_DIR/javascore/javaee-tokens/build/libs/irc31-0.1.0-debug.jar ${CONFIG_DIR}/javascore/
```

### 2. Deploy BMC SCORE Contract

____

- Run these commands to deploy ***BMC*** contract on ICON Network. These ones should be run in a goloop container (running `docker exec -ti --workdir /goloop/config goloop sh`)

```shell
source rpc.sh
rpcch icon
rpcks $GOLOOP_KEY_STORE $GOLOOP_KEY_SECRET
echo "$GOLOOP_RPC_NID.icon" > net.btp.$(rpcch)

goloop rpc sendtx deploy ./javascore/bmc-0.1.0-debug.jar \
    --content_type application/java --step_limit 13610920001 \
    --param _net=$(cat net.btp.$(rpcch)) | jq -r . > tx.bmc.$(rpcch)
```

- Extract address of ***BMC*** contract after deployment as:

```shell
goloop rpc txresult $(cat tx.bmc.$(rpcch)) | jq -r .scoreAddress > bmc.$(rpcch)
```

- Generate BTP address format of BMC contract as:

```shell
echo "btp://$(cat net.btp.$(rpcch))/$(cat bmc.$(rpcch))" > btp.$(rpcch)
```

### 3. Deploy BMV SCORE Contract

____

#### Deploy ***Kusama*** and ***Moonriver*** Event Decoder

- Run these commands to deploy ***Kusama*** and ***Moonriver*** Event Decoder on ICON Network. Again, these commands should be run in a goloop container.

```shell
source rpc.sh
rpcch icon
rpcks $GOLOOP_KEY_STORE $GOLOOP_KEY_SECRET
goloop rpc sendtx deploy ./javascore/KusamaEventDecoder-optimized.jar \
     --content_type application/java --step_limit 13610920001 \
     | jq -r . > tx.kusamaDecoder.$(rpcch)
goloop rpc sendtx deploy ./javascore/MoonriverEventDecoder-optimized.jar \
     --content_type application/java --step_limit 13610920001 \
     | jq -r . > tx.moonriverDecoder.$(rpcch)
```

- Extract addresses of ***Kusama*** and ***Moonriver*** Event Decoder contract as:

```shell
goloop rpc txresult $(cat tx.kusamaDecoder.$(rpcch)) | jq -r .scoreAddress > kusamaDecoder.$(rpcch)
goloop rpc txresult $(cat tx.moonriverDecoder.$(rpcch)) | jq -r .scoreAddress > moonriverDecoder.$(rpcch)
```

#### Deploy ***BMV*** contract on ICON Network

- Preparing setting parameters:

  - `relayMtaOffset`: offset of Merkle Tree Accumulator (MTA) - a block height that BMV starts to sync block on Relaychain
  - `paraMtaOffset`: offset of Merkle Tree Accumulator (MTA) - a block height that BMV starts to sync block on Parachain
  - `bmc`: address of BMC SCORE contract
  - `net`: a network that BMV will handle, e.g. Moonriver
  - `mtaRootSize`: size of MTA roots use for both Parachain and Relaychain
  - `mtaCacheSize`: size of MTA cache use for both Parachain and Relaychain
  - `mtaIsAllowNewerWitness`: allow to verify newer witness. This setting allows BMV to verify in case of MTA block height of a client is higher than MTA block height of BMV contract
  - `relayLastBlockHash`: hash of previous block - `relayMtaOffset`. BMV must check that a previous hash of an incoming block is equal to the `relayLastBlockHash`
  - `paraLastBlockHash`: hash of previous block - `paraMtaOffset`. BMV must check that a previous hash of an incoming block is equal to the `paraLastBlockHash`
  - `encodedValidators`: `Base64(RLP.encode(List<byte[]> validatorPublicKey))`, encoded of validators list of relay chain
  - `relayEventDecoderAddress`: address of Event Decoder for Relaychain - e.g. ***Kusama*** Event Decoder
  - `paraEventDecoderAddress`: address of Event Decoder for Parachain - e.g. ***Moonriver*** Event Decoder
  - `relayCurrentSetId`: set a current counter of updating `encodedValidators`. When a list of Validators is updated, the setID is increased by one
  - `paraChainId`: an ID of Parachain

For a sake of simplicity, we have setup an utility that helps to initialize these parameters. Please follow the instructions below:

- Install Yarn packages:

```shell
$ cd $CONFIG_DIR/btp/javascore/bmv/helper
$ yarn
```

- Specify your configurations:

  - Open a file - `getBMVInitializeParams.ts`

  - Specify Relaychain and Parachain web socket endpoint by replacing `RELAY_ENDPOINT`, `PARA_ENDPOINT` in line 30, and 31 with your configuration. In this example, we use a local testnet, thus the settings are `PARA_ENDPOINT = ws://127.0.0.1:9944`, and `RELAY_ENDPOINT = wss://kusama-rpc.polkadot.io`

  - In order to setting `relayMtaOffset`, and `paraMtaOffset`, you replace `RELAY_OFFSET`, `PARA_OFFSET` in line 33, 34 with the current block height of corresponding chains. You can use [Polkadot Portal](https://polkadot.js.org/apps/?#/explorer) (using Chrome browser) to retrieve these information. Since we are using a local testnet for this example, `paraMtaOffset` is simply set by checking a current block of deployed node and `relayMtaOffset` is the current block of ***Kusama*** that is retrieved via the above link

- Then, run the below command:

```shell
$ yarn getBMVInitializeParams
```

- In success, a file, `BMVInitializeData.json`, will be generated in the `$CONFIG_DIR/javascore/bmv/helper` directory. This JSON file contains initialized parameters that are essential to deploy ***BMV*** contract. Please copy and paste them to the below command:

```shell
source rpc.sh
rpcch icon
rpcks $GOLOOP_KEY_STORE $GOLOOP_KEY_SECRET
goloop rpc sendtx deploy ./javascore/parachain-BMV-optimized.jar \
    --content_type application/java --step_limit 13610920001 \
    --param relayMtaOffset=${Relaychain Offset} \
    --param paraMtaOffset=${Parachain Offset} \
    --param bmc=$(cat bmc.$(rpcch)) \
    --param net=$(cat net.btp.icon) \
    --param mtaRootSize=${MTA root size} \
    --param mtaCacheSize=${MTA caches size} \
    --param mtaIsAllowNewerWitness=${Allow MTA newer witness} \
    --param relayLastBlockHash=${Relaychain last blockhash} \
    --param paraLastBlockHash=${Parachain last blockhash} \
    --param encodedValidators=${Base64 of encodedValidators} \
    --param relayEventDecoderAddress=$(cat kusamaDecoder.icon) \
    --param paraEventDecoderAddress=$(cat moonriverDecoder.icon) \
    --param relayCurrentSetId=${setID} \
    --param paraChainId=${Parachain ID}
     | jq -r . > tx.bmv.$(rpcch)
```

- Extract address of ***BMV*** contract after deployment as:

```shell
goloop rpc txresult $(cat tx.bmv.$(rpcch)) | jq -r .scoreAddress > bmv.$(rpcch)
```

### 4. Deploy IRC31Token Contract

____

- Run these commands to deploy ***IRC31Token*** on ICON Network. Again, these commands should be run in a goloop container.

```shell
source rpc.sh
rpcch icon
rpcks $GOLOOP_KEY_STORE $GOLOOP_KEY_SECRET
goloop rpc sendtx deploy ./javascore/irc31-0.1.0-debug.jar \
    --content_type application/java --step_limit 13610920001 | jq -r . > tx.irc31token.$(rpcch)
```

- Extract address of ***IRC31Token*** contract after deployment as:

```shell
goloop rpc txresult $(cat tx.irc31token.$(rpcch)) | jq -r .scoreAddress > irc31token.$(rpcch)
```

### 5. Deploy NativeCoinBSH Contract

____

- Run these commands to deploy ***NativeCoinBSH*** on ICON Network. Again, these commands should be run in a goloop container.

```shell
source rpc.sh
rpcch icon
rpcks $GOLOOP_KEY_STORE $GOLOOP_KEY_SECRET
goloop rpc sendtx deploy ./javascore/nativecoin-0.1.0-debug.jar \
    --content_type application/java --step_limit 13610920001 \
    --param _bmc=$(cat bmc.$(rpcch)) \
    --param _irc31=$(cat irc31token.$(rpcch)) \
    --param _name=ICX | jq -r . > tx.nativeCoinBsh.$(rpcch)
```

- Extract address of ***NativeCoinBSH*** contract after deployment as:

```shell
goloop rpc txresult $(cat tx.nativeCoinBsh.$(rpcch)) | jq -r .scoreAddress > nativeCoinBsh.$(rpcch)
```

### 6. Deploy FeeAggregation Contract

____

- Run below commands to compile Java files and generate requiring `.jar` files:

```shell
$ cd $CONFIG_DIR/btp/javascore/fee_aggregation

$ ./gradlew build

$ ./gradlew optimizedJar
```

- Finally, deploy ***FeeAggregation*** contract on ICON Network as follow:

```shell
./gradlew deployToLocal -PkeystoreName=<your_wallet_json> -PkeystorePass=<password>
```

***Note that:***

&emsp;   `-PkeystoreName=`: a link to your `keystore.json` that is generated when you start your local node

&emsp;   `-PkeystorePass=`: a password, that is saved in a `keysecret` file, which is generated at the same time with `keystore.json`

For example: 

```shell
./gradlew deployToLocal -PkeystoreName="/Users/myfolder/INode/keystore.json" -PkeystorePass=5a17fdee471e5dd4
```

### 7. ICON-BMC Configuration

____

**Note that: This configuration step must be executed after completely deploying smart contracts (BSH, BMC, and BMV) on both connecting networks. In this example, we would like to show how to config to setup a connection between ICON and Moonriver networks. For other cases, there might be a different configuration**

#### Add **BMV**

**TBA**

```bash

```

#### Add Connection Link to Moonriver-BMC

&emsp; This step requires Moonriver chainID, and an address of Moonriver-BMC contract. The information is generated after deploying Moonbeam-BMC contract ([link](Smart-Contracts-PRA.md#deploy-bmc-contracts-on-moonriver-network))

```shell
echo "btp://${Moonriver_CHAINID}.pra/${MoonriverBMC}" > btp.dst
# For example: Moonriver_CHAINID = 0x501; MoonriverBMC = 0x5CC307268a1393AB9A764A20DACE848AB8275c46

rpcch icon
goloop rpc sendtx call --to $(cat bmc.$(rpcch)) \
    --step_limit 13610920001 \
    --method addLink \
    --param _link=$(cat btp.dst) \
    | jq -r . > tx.link.$(rpcch)
```

In success, a connection link from ICON-BMC to Moonriver-BMC will be set with default setting values:

&emsp; &emsp; &emsp; + `BLOCK_INTERVAL_MSEC`: block interval of Moonriver (default = 1000ms)

&emsp; &emsp; &emsp; + `MAX_AGGREGATION`: max_aggregation value (default = 10)

&emsp; &emsp; &emsp; + `DELAY_LIMIT`: acceptance of delayed submission sending from BMR to BMC (default = 3)

<p align="center">
  <img src="./images/bmc_rotate_round-robin.png" width="800" height="300" />
</p>

<p align="center">
  <img src="./images/bmc_rotate_fail-over.png" width="800" height="300" />
</p>

#### Set Link Configuration

&emsp; This step will help you to change default setting values

```bash
rpcch icon
goloop rpc sendtx call --to $(cat bmc.$(rpcch)) \
    --step_limit 13610920001 \
    --method setLink \
    --param _link=$(cat btp.dst) \
    --param _block_interval=0x1770 \
    --param _max_agg=0x08 \
    --param _delay_limit=4 \
    | jq -r . > tx.setlink.$(rpcch)
```

#### Add BSH Service contract

```bash
rpcch icon
goloop rpc sendtx call --to $(cat bmc.$(rpcch)) \
    --step_limit 13610920001 \
    --method addService \
    --param _addr=$(cat nativeCoinBsh.$(rpcch)) \
    --param _svc=CoinTransfer \
    | jq -r . > tx.addService.$(rpcch)
```

#### Register Relay to ICON-BMC

&emsp; **Note that: To complete this step, keystore of BMR from Moonriver --> ICON ('icon-bmr.keystore.json') must be generated.** Please check out this [link](BMR-deployment.md#create-keystore-files) to complete this requirement

```bash
rpcch icon
goloop rpc sendtx call --to $(cat bmc.$(rpcch)) \
    --step_limit 13610920001 \
    --method addRelay \
    --param _link=$(cat btp.dst) \
    --param _addr=$(jq -r .address icon-bmr.keystore.json)
```

### 8. Config NativeCoin BSH

____

- Register 'PARA' token

```shell
rpcch icon
goloop rpc sendtx call --to $(cat nativeCoinBsh.$(rpcch)) \
    --step_limit 13610920001 \
    --method register \
    --param _name=PARA \
    | jq -r . > tx.registerCoin.$(rpcch)
```

Click on 'Next' to move on deploying smart contracts on Moonriver Network

&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;
[<--- Prev](./BTP-Development-Instructions.md) &emsp; &emsp; &emsp; &emsp; [Next --->](./Smart-Contracts-PRA.md)


<!--<p align="center">-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/BTP-Development-Instructions.md" class="button"><--- Prev &emsp; &emsp;</a>-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/Smart-Contracts-PRA.md" class="button">&emsp; &emsp; Next </a>-->
<!--</p> -->
