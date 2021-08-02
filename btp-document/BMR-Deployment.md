## BTP Message Relay (BMR) Deployment

____

<p align="center">
  <img src="./images/Deploy-BMR.png" width="800" height="300" />
</p>

BTP Message Relay (BMR) is a component, that consists of several unidirectional Relays, which takes responsibility for monitoring and gathering proofs for BTP events in the network. It means that ***BMR*** always has at least two Relays working concurrently:

- Relay 1: listen/collect event's data from Network A --> build `Relay Message` --> push it to ***BMC*** of Network B
- Relay 2: listen/collect event's data from Network B --> build `Relay Message` --> push it to ***BMC*** of Network A

Before moving on guidance to deploy BMRs, we would like to make short explanation so that the following instructions would be easier to follow up. As mentioned earlier, this example demonstates to connect ICON Network to Moonriver Network

- Confusion point 1: BMR handles a connection from A to B. BMR listens on A, but it must be registered on B

BMR1: listen blocks generating from ICON Network --> build `Relay Message` --> push this message to Moonriver-BMC contract.

==> In order to push `Relay Message` to Moonriver-BMC contract, BMR1 must be registered into Moonriver-BMC contract. Thus, it requires BMR1's address being generated in Moonriver Network. Then, `Operator` of Moonriver-BMC contract can manually add this address to handle a connection link (ICON --> Moonriver)

BMR2: listen blocks generating from Moonriver Network --> build `Relay Message` --> push this message to ICON-BMC contract.

==> In order to push `Relay Message` to ICON-BMC contract, BMR2 must be registered into ICON-BMC contract. Thus, it requires BMR2's address being generated in ICON Network. Then, `Operator` of Moonriver-BMC contract can manually add this address to handle a connection link (Moonriver --> ICON)

- Confusion point 2: BMR handles a connection from A to B. Similar to the above confusion point, BMR listens block generating on A, but it must query this offet through calling `getBMCLinkStatus` on B

==> To avoid chicken-and-egg problem, `Operator` will choose an `offset` and set it into BMV contract when this contract is deployed. From that point, blocks can be verified and synced. Upon successful verification, BMV updates this `offset` to the latest block height. In addition, current block height on A might be higher than current block height was verified by BMV contract on B. Thus, BMR is required to retrieve the latest `offset` from BMV contract through calling `getBMCLinkStatus` on B

### 1. Build executable files

____

```bash
# INode is a folder that was created when deploying a local node in ICON Network
CONFIG_DIR=path/to/config/folder/INode

cd $CONFIG_DIR

mkdir BMR && cd BMR

#  Clone BTP Project
git clone https://github.com/icon-project/btp.git

cd btp && git checkout refactor-layout

# Output binaries are placed under bin/ directory.
make btpsimple
# Copy binary file 'btpsimple' to local user's local binary folder
cp $CONFIG_DIR/BMR/btp/bin/btpsimple /usr/local/bin
```

### 2. Generate keystore and configuration files

____

#### Create keystore files

- Generate keystore of BMR from Moonriver --> ICON

```bash
goloop ks gen --out $CONFIG_DIR/icon-bmr.keystore.json --password YOUR_PASSWORD

# Create a secret file 'icon-bmr.secret' and save $YOUR_PASSWORD into that file
echo -n "YOUR_PASSWORD" > $CONFIG_DIR/icon-bmr.secret

# Save icon-bmr address to a file
echo -n "ICON-BMR Addresss" > $CONFIG_DIR/icon-bmr.addr
```

- Generate keystore of BMR from ICON --> Moonriver

```bash
ethkey generate $CONFIG_DIR/moon-bmr.keystore.json
# Enter your password
# Repeat your password

cat <<< $(jq '. += {"coinType":"evm"}' $CONFIG_DIR/moon-bmr.keystore.json) > $CONFIG_DIR/moon-bmr.keystore.json

# Create a secret file 'moon-bmr.secret' and save $YOUR_PASSWORD into that file
echo -n "YOUR_PASSWORD" > $CONFIG_DIR/moon-bmr.secret

# Save moon-bmr address to a file
echo -n "MOON-BMR Addresss" > $CONFIG_DIR/moon-bmr.addr
```

#### Query `Offset` and a list of `Relays` from BMC

&emsp; This step checks whether **BMRs** have been registered and added into **BMC**. As designed, only registered **BMRs**, that approved and added by `Operator`, are allowed to build `Relay Message` and push it to **BMC** contract

- Get `BMCLinkStatus` (Moonriver --> ICON):  please check out this [link](Smart-Contracts-ICON.md#get-link-status) to complete this requirement

- Get `BMCLinkStatus` (ICON --> Moonriver): please check out this [link](Smart-Contracts-PRA.md#4-config-moonriver-bmc) to complete this requirement

#### Create configuration files

- Generate configuration file of BMR from ICON --> Moonriver

```bash
cd $CONFIG_DIR/BMR/btp

# Make sure 'BTP_PROJ_DIR' has already defined
# If not, please define as BTP_PROJ_DIR=/path/to/PRA-Contracts/btp

chmod +x ./entrypoint.sh

BTPSIMPLE_CONFIG=$CONFIG_DIR/moon.config.json \
BTPSIMPLE_SRC_ADDRESS=$(cat $CONFIG_DIR/btp.icon) \
BTPSIMPLE_SRC_ENDPOINT=http://127.0.0.1:9080/api/v3/icon \
BTPSIMPLE_DST_ADDRESS=$(cat $BTP_PROJ_DIR/bmc_perif.btp.addr) \
BTPSIMPLE_DST_ENDPOINT=ws://localhost:9944 \
BTPSIMPLE_OFFSET=$(cat $BTP_PROJ_DIR/moon.offset) \
BTPSIMPLE_KEY_STORE=$CONFIG_DIR/moon-bmr.keystore.json \
BTPSIMPLE_KEY_SECRET=$CONFIG_DIR/moon-bmr.secret \
BTPSIMPLE_LOG_WRITER_FILENAME=$CONFIG_DIR/moon-bmr.log \
./entrypoint.sh
```

- Generate configuration file of BMR from Moonriver --> ICON

```bash
BTPSIMPLE_CONFIG=$CONFIG_DIR/icon.config.json \
BTPSIMPLE_SRC_ADDRESS=$(cat $BTP_PROJ_DIR/bmc_perif.btp.addr) \
BTPSIMPLE_SRC_ENDPOINT=ws://localhost:9944 \
BTPSIMPLE_DST_ADDRESS=$(cat $CONFIG_DIR/btp.icon) \
BTPSIMPLE_DST_ENDPOINT=http://127.0.0.1:9080/api/v3/icon \
BTPSIMPLE_OFFSET=$(cat $CONFIG_DIR/icon.offset) \
BTPSIMPLE_KEY_STORE=$CONFIG_DIR/icon-bmr.keystore.json \
BTPSIMPLE_KEY_SECRET=$CONFIG_DIR/icon-bmr.secret \
BTPSIMPLE_LOG_WRITER_FILENAME=$CONFIG_DIR/icon-bmr.log \
./entrypoint.sh
```

Add `"options": {"stepLimit": 50000000000000}` into `$CONFIG_DIR/icon.config.json`. For Example:

```json
"dst": {
    "address": "btp://0x3.icon/cxbcad01c6b50459f0e2110fb90507f30d59f95579",
    "endpoint": "http://127.0.0.1:9080/api/v3/icon",
    "options": {
      "stepLimit": 50000000000000
    }
},
```

### 3. Start BMRs

____

Before starting the BMRs, we have to add some "fuels"

- Adding funds to Moonbeam-BMR

```bash
export MOON_BMR=$(cat $CONFIG_DIR/moon-bmr.addr)
# change your current directory to a directory that contains BMC, BSH or BMV
# For example: mine is $BTP_PROJ_DIR (/Users/myfolder/PRA-Contracts/btp)
cd $BTP_PROJ_DIR/solidity/bmc

truffle console --network moonbeamlocal

# Moonbeam provides some prefund accounts
# Please make sure that these ones have been added into a truffle-config.js
# so then below command can return them
truffle(moonbeamlocal)> let accounts = await web3.eth.getAccounts()

# Check a balance of one of these accounts
# It returns a balance of accounts[0] in Wei
truffle(moonbeamlocal)> await web3.eth.getBalance(accounts[0])
'1207825499178649173706176'

# Now, transfer 100 ETH to Moonbeam-BMR
truffle(moonbeamlocal)> await web3.eth.sendTransaction({from: accounts[0], to: process.env.MOON_BMR, value: web3.utils.toBN("10000000000000000000000")})
{
  blockHash: '0x7735fde972b10ffe4456d3fe2af7f8f9070526186d5444ee9d3d724230be52b2',
  blockNumber: 9,
  contractAddress: null,
  cumulativeGasUsed: 21000,
  from: '0xf24ff3a9cf04c71dbc94d0b566f7a27b94566cac',
  gasUsed: 21000,
  logs: [],
  logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
  status: true,
  to: '0xa29ce7c50d01a9b20fe92dba2c149aaee0acec9c',
  transactionHash: '0xeee0579439622198a68b0c9cf3c07202cf2f9e15628b03c4acd86e1a5334cf34',
  transactionIndex: 0
}

# Check the balance of Moonbeam-BMR
truffle(moonbeamlocal)> await web3.eth.getBalance(process.env.MOON_BMR)
'10000000000000000000000'
```

- Add funds to ICON-BMR

```bash
# change your current directory to one that is being used to run 'goloop' command 
# For example: mine is $CONFIG_DIR (/Users/myfolder/INode)
# Note that: all generated files, including keystores, were saved in this directory as well
AMOUNT=1000000000000000000000000
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx transfer \
--to $(cat $CONFIG_DIR/icon-bmr.addr) --value $AMOUNT \
--key_store $CONFIG_DIR/godWallet.json --key_password gochain \
--step_limit 10000000000 --nid 3

# Check the balance of ICON-BMR
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon balance $(cat $CONFIG_DIR/icon-bmr.addr)
```

Now, let start the BMRs

- Start BMR from ICON --> Moonriver

```bash
./bin/btpsimple start --config $CONFIG_DIR/moon.config.json
```

- Start BMR from Moonriver --> ICON

```bash
./bin/btpsimple start --config $CONFIG_DIR/icon.config.json
```

In the next section, we are going to guide you how to transfer the native coins between ICON and Moonriver networks manually. Click on 'Next' if you're interested

&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;
[<--- Prev](./Smart-Contracts-PRA.md) &emsp; &emsp; &emsp; &emsp; [Next --->](./NativeCoin-Transfer-Example.md)
<!--<p align="center">-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/Smart-Contracts-PRA.md"><--- Prev</a>-->
<!--</p> -->