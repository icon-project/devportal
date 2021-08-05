## BTP Message Relay (BMR) Deployment

____

### 1. Create configuration files

- Generate configuration file of BMR from ICON --> Moonriver

```bash
cd $CONFIG_DIR/BMR/btp
chmod +x ./entrypoint.sh

BTPSIMPLE_CONFIG=$CONFIG_DIR/moon.config.json \
BTPSIMPLE_SRC_ADDRESS=$(cat $CONFIG_DIR/btp.icon) \
BTPSIMPLE_SRC_ENDPOINT=http://127.0.0.1:9080/api/v3/icon \
BTPSIMPLE_DST_ADDRESS=$(cat $CONFIG_DIR/bmc_perif.btp.addr) \
BTPSIMPLE_DST_ENDPOINT=ws://localhost:9944 \
BTPSIMPLE_OFFSET=$(cat $CONFIG_DIR/moon.offset) \
BTPSIMPLE_KEY_STORE=$CONFIG_DIR/moon-bmr.keystore.json \
BTPSIMPLE_KEY_SECRET=$CONFIG_DIR/moon-bmr.secret \
BTPSIMPLE_LOG_WRITER_FILENAME=$CONFIG_DIR/moon-bmr.log \
./entrypoint.sh
```

- Generate configuration file of BMR from Moonriver --> ICON

```bash
BTPSIMPLE_CONFIG=$CONFIG_DIR/icon.config.json \
BTPSIMPLE_SRC_ADDRESS=$(cat $CONFIG_DIR/bmc_perif.btp.addr) \
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

### 2. Start BMRs

____

Before starting the BMRs, we have to add some "fuels"

- Adding funds to Moonbeam-BMR

```bash
export MOON_BMR=$(cat $CONFIG_DIR/moon-bmr.addr)
cd $CONFIG_DIR/Moonriver/btp/solidity/bmc

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
cd $CONFIG_DIR
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
$CONFIG_DIR/BMR/btp/bin/btpsimple start --config $CONFIG_DIR/moon.config.json
```

- Start BMR from Moonriver --> ICON

```bash
$CONFIG_DIR/BMR/btp/bin/btpsimple start --config $CONFIG_DIR/icon.config.json
```

In the next section, we are going to guide you how to transfer the native coins between ICON and Moonriver networks manually. Click on 'Next' if you're interested

&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;
[<--- Prev](./Smart-Contracts-Configuration.md#smart-contracts-configuration) &emsp; &emsp; &emsp; &emsp; [Next --->](./NativeCoin-Transfer-Example.md)
<!--<p align="center">-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/Smart-Contracts-PRA.md"><--- Prev</a>-->
<!--</p> -->