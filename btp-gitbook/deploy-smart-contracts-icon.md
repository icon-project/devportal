# Deploy Smart Contracts \(ICON\)

![](../.gitbook/assets/Deploy-Contracts-ICON%20%281%29%20%281%29%20%281%29.png)

Before deploying these contracts, please make sure you have already started a local node in ICON Network. If you have not yet done this step, please check out this [link](btp-development-instructions.md#1-deploy-icon-node) and accomplish it

## 1. Deploy BMC SCORE Contract

* Run these commands to deploy _**BMC**_ contract on ICON Network

```bash
cd $PROJECT_DIR/btp

export CONFIG_DIR=$PROJECT_DIR/btp/docker-compose/goloop2moonbeam/config

echo "$(cat $CONFIG_DIR/nid.icon).icon" > $CONFIG_DIR/net.btp.icon

# Add '.../go/bin' to the PATH environment variable.
# For example: 
# export GOPATH=~/go
# export GOBIN=$GOPATH/bin
# export PATH=$PATH:$GOBIN

goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx deploy \
    $PROJECT_DIR/btp/build/contracts/javascore/bmc-0.1.0-optimized.jar \
    --key_store $CONFIG_DIR/goloop.keystore.json \
    --key_password $(cat $CONFIG_DIR/goloop.keysecret) \
    --nid $(cat $CONFIG_DIR/nid.icon) \
    --content_type application/java --step_limit 13610920001 \
    --param _net=$(cat $CONFIG_DIR/net.btp.icon) \
    | jq -r . > $CONFIG_DIR/tx.bmc.icon
```

* Extract address of _**BMC**_ contract after deployment as:

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.bmc.icon) \
| jq -r .scoreAddress > $CONFIG_DIR/bmc.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.bmc.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'

goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.bmc.icon) \
| jq -r . > $CONFIG_DIR/tx.bmc.icon.query

# Save 'blockHash' and 'blockHeight', they will be used to deploy BMV contract on Moonriver Network
BLOCK_HASH="$(eval "echo $(jq -r '.blockHash' "$CONFIG_DIR/tx.bmc.icon.query")")"
BLOCK_HEIGHT="$(eval "echo $(jq -r '.blockHeight' "$CONFIG_DIR/tx.bmc.icon.query")")"
echo -n $BLOCK_HASH > $CONFIG_DIR/block.hash.icon
echo -n $BLOCK_HEIGHT > $CONFIG_DIR/block.height.icon
```

* Generate BTP address format of BMC contract as:

```bash
echo "btp://$(cat $CONFIG_DIR/net.btp.icon)/$(cat $CONFIG_DIR/bmc.icon)" > $CONFIG_DIR/btp.icon
```

## 2. Deploy BMV SCORE Contract

### Deploy _**Kusama**_ and _**Moonriver**_ Event Decoder

* Run these commands to deploy _**Kusama**_ and _**Moonriver**_ Event Decoder on ICON Network

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx deploy \
    $PROJECT_DIR/btp/build/contracts/javascore/KusamaEventDecoder-optimized.jar \
    --key_store $CONFIG_DIR/goloop.keystore.json \
    --key_password $(cat $CONFIG_DIR/goloop.keysecret) \
    --nid $(cat $CONFIG_DIR/nid.icon) \
    --content_type application/java --step_limit 13610920001 \
    | jq -r . > $CONFIG_DIR/tx.kusamaDecoder.icon
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx deploy \
    $PROJECT_DIR/btp/build/contracts/javascore/MoonriverEventDecoder-optimized.jar \
    --key_store $CONFIG_DIR/goloop.keystore.json \
    --key_password $(cat $CONFIG_DIR/goloop.keysecret) \
    --nid $(cat $CONFIG_DIR/nid.icon) \
    --content_type application/java --step_limit 13610920001 \
    | jq -r . > $CONFIG_DIR/tx.moonriverDecoder.icon
```

* Extract addresses of _**Kusama**_ and _**Moonriver**_ Event Decoder contract as:

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.kusamaDecoder.icon) \
| jq -r .scoreAddress > $CONFIG_DIR/kusamaDecoder.icon

goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.moonriverDecoder.icon) \
| jq -r .scoreAddress > $CONFIG_DIR/moonriverDecoder.icon

# Also check whether these transactions are successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.kusamaDecoder.icon)
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.moonriverDecoder.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'
```

### Deploy _**BMV**_ contract on ICON Network

* Preparing setting parameters:
  * `relayMtaOffset`: offset of Merkle Tree Accumulator \(MTA\) - a block height that BMV starts to sync block on Relaychain
  * `paraMtaOffset`: offset of Merkle Tree Accumulator \(MTA\) - a block height that BMV starts to sync block on Parachain
  * `bmc`: address of BMC SCORE contract
  * `net`: a network that BMV will handle, e.g. Moonriver
  * `mtaRootSize`: size of MTA roots use for both Parachain and Relaychain
  * `mtaCacheSize`: size of MTA cache use for both Parachain and Relaychain
  * `mtaIsAllowNewerWitness`: allow to verify newer witness. This setting allows BMV to verify in case of MTA block height of a client is higher than MTA block height of BMV contract
  * `relayLastBlockHash`: hash of previous block - `relayMtaOffset`. BMV must check that a previous hash of an incoming block is equal to the `relayLastBlockHash`
  * `paraLastBlockHash`: hash of previous block - `paraMtaOffset`. BMV must check that a previous hash of an incoming block is equal to the `paraLastBlockHash`
  * `encodedValidators`: `Base64(RLP.encode(List<byte[]> validatorPublicKey))`, encoded of validators list of relay chain
  * `relayEventDecoderAddress`: address of Event Decoder for Relaychain - e.g. _**Kusama**_ Event Decoder
  * `paraEventDecoderAddress`: address of Event Decoder for Parachain - e.g. _**Moonriver**_ Event Decoder
  * `relayCurrentSetId`: set a current counter of updating `encodedValidators`. When a list of Validators is updated, the setID is increased by one
  * `paraChainId`: an ID of Parachain
  * `evmEventIndex`: index of evm log event in para chain
  * `newAuthoritiesEventIndex`: index of new authorities event in relay chain
  * `candidateIncludedEventIndex`: index of candidate included in relay chain

For a sake of simplicity, we have setup an utility that helps to initialize these parameters. Please follow the instructions below:

* Install Yarn packages:

```bash
cd $PROJECT_DIR/btp/build/contracts/javascore/helper
yarn
```

* Specify your configurations:

```bash
# Replace your RELAY_ENDPOINT if needed
export RELAY_ENDPOINT=wss://kusama-rpc.polkadot.io

# Replace your PARA_ENDPOINT if needed
export PARA_ENDPOINT=ws://127.0.0.1:9944

# You can use https://polkadot.js.org/apps/?#/explorer to retrieve this information
export RELAY_CHAIN_OFFSET=8000000

# Checking a current block of deployed Moonriver node
export PARA_CHAIN_OFFSET=27
```

* Then, run the below command:

```text
yarn getBMVInitializeParams
```

* In success, a file, `BMVInitializeData.json`, will be generated in the `$PROJECT_DIR/btp/build/contracts/javascore/helper` directory. This JSON file contains initialized parameters that are essential to deploy _**BMV**_ contract.
  * `MTA root size`: can be set a value `0x8`
  * `MTA caches size`: can be set a value `0x8`
  * `Allow MTA newer witness`: `0x0` \(Not Allow\) or `0x1` \(Allow\)

```bash
HELPER_DIR=$PROJECT_DIR/btp/build/contracts/javascore/helper

VALIDATORS="$(eval "echo $(jq -r '.encodedValidators' "$HELPER_DIR/BMVInitializeData.json")")"

RC_OFFSET="$(eval "echo $(jq -r '.relayMtaOffset' "$HELPER_DIR/BMVInitializeData.json")")"

PC_OFFSET="$(eval "echo $(jq -r '.paraMtaOffset' "$HELPER_DIR/BMVInitializeData.json")")"

RC_LAST_BLOCKHASH="$(eval "echo $(jq -r '.relayLastBlockHash' "$HELPER_DIR/BMVInitializeData.json")")"

PC_LAST_BLOCKHASH="$(eval "echo $(jq -r '.paraLastBlockHash' "$HELPER_DIR/BMVInitializeData.json")")"

SET_ID="$(eval "echo $(jq -r '.relayCurrentSetId' "$HELPER_DIR/BMVInitializeData.json")")"

# Replace your value if needed
MTA_ROOT_SIZE=0x8

# Replace your value if needed
MTA_CATCH_SIZE=0x8

# Replace your value if needed
ALLOW_NEWER_WITNESS=0x1

# Replace your value if needed
PARACHAIN_ID=0x0
```

```bash
cd $PROJECT_DIR/btp

# Replace MOONRIVER_CHAINID=your/chainID/of/Moonriver/Network if needed
MOONRIVER_CHAINID=0x501

echo "${MOONRIVER_CHAINID}.pra" > $CONFIG_DIR/net.btp.dst

goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx deploy \
    $PROJECT_DIR/btp/build/contracts/javascore/parachain-BMV-optimized.jar \
    --key_store $CONFIG_DIR/goloop.keystore.json \
    --key_password $(cat $CONFIG_DIR/goloop.keysecret) \
    --nid $(cat $CONFIG_DIR/nid.icon) \
    --content_type application/java --step_limit 13610920001 \
    --param relayMtaOffset=$RC_OFFSET \
    --param paraMtaOffset=$PC_OFFSET \
    --param bmc=$(cat $CONFIG_DIR/bmc.icon) \
    --param net=$(cat $CONFIG_DIR/net.btp.dst) \
    --param mtaRootSize=$MTA_ROOT_SIZE \
    --param mtaCacheSize=$MTA_CATCH_SIZE \
    --param mtaIsAllowNewerWitness=$ALLOW_NEWER_WITNESS \
    --param relayLastBlockHash=$RC_LAST_BLOCKHASH \
    --param paraLastBlockHash=$PC_LAST_BLOCKHASH \
    --param encodedValidators=$VALIDATORS \
    --param relayEventDecoderAddress=$(cat $CONFIG_DIR/kusamaDecoder.icon) \
    --param paraEventDecoderAddress=$(cat $CONFIG_DIR/moonriverDecoder.icon) \
    --param relayCurrentSetId=$SET_ID \
    --param paraChainId=$PARACHAIN_ID \
    | jq -r . > $CONFIG_DIR/tx.bmv.icon
```

* Extract address of _**BMV**_ contract after deployment as:

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.bmv.icon) \
| jq -r .scoreAddress > $CONFIG_DIR/bmv.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.bmv.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'
```

## 3. Deploy IRC31Token Contract

* Run these commands to deploy _**IRC31Token**_ on ICON Network.

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx deploy \
    $PROJECT_DIR/btp/build/contracts/javascore/irc31-0.1.0-optimized.jar \
    --key_store $CONFIG_DIR/goloop.keystore.json \
    --key_password $(cat $CONFIG_DIR/goloop.keysecret) \
    --nid $(cat $CONFIG_DIR/nid.icon) \
    --content_type application/java --step_limit 13610920001 \
    | jq -r . > $CONFIG_DIR/tx.irc31token.icon
```

* Extract address of _**IRC31Token**_ contract after deployment as:

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.irc31token.icon) \
| jq -r .scoreAddress > $CONFIG_DIR/irc31token.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.irc31token.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'
```

## 4. Deploy NativeCoinBSH Contract

* Run these commands to deploy _**NativeCoinBSH**_ on ICON Network

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx deploy \
    $PROJECT_DIR/btp/build/contracts/javascore/nativecoin-0.1.0-optimized.jar \
    --key_store $CONFIG_DIR/goloop.keystore.json \
    --key_password $(cat $CONFIG_DIR/goloop.keysecret) \
    --nid $(cat $CONFIG_DIR/nid.icon) \
    --content_type application/java --step_limit 13610920001 \
    --param _bmc=$(cat $CONFIG_DIR/bmc.icon) \
    --param _irc31=$(cat $CONFIG_DIR/irc31token.icon) \
    --param _name=ICX | jq -r . > $CONFIG_DIR/tx.nativeCoinBsh.icon
```

* Extract address of _**NativeCoinBSH**_ contract after deployment as:

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.nativeCoinBsh.icon) \
| jq -r .scoreAddress > $CONFIG_DIR/nativeCoinBsh.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.nativeCoinBsh.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'
```

## 5. Deploy FeeAggregation Contract

* Run below command to deploy _**FeeAggregation**_ contract on ICON Network as follow:

```bash
# Replace another Receiver Address if needed
ICON_RECEIVER_FEE_ADDRESS=hxb6b5791be0b5ef67063b3c10b840fb81514db2fd

goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx deploy \
    $PROJECT_DIR/btp/build/contracts/javascore/fee-aggregation-system-1.0-optimized.jar \
    --key_store $CONFIG_DIR/goloop.keystore.json \
    --key_password $(cat $CONFIG_DIR/goloop.keysecret) \
    --nid $(cat $CONFIG_DIR/nid.icon) \
    --content_type application/java --step_limit 13610920001 \
    --param _cps_address=$ICON_RECEIVER_FEE_ADDRESS \
    | jq -r . > $CONFIG_DIR/tx.feeAggregation.icon
```

* Extract address of _**FeeAggregation**_ contract after deployment as:

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon \
    txresult $(cat $CONFIG_DIR/tx.feeAggregation.icon) \
    | jq -r .scoreAddress > $CONFIG_DIR/feeAggregation.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.feeAggregation.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'
```

