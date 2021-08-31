## BTP Message Relay (BMR) Deployment

____

### 1. Create configuration files

- Generate configuration file of BMR from ICON --> Moonriver

```bash
cd $PROJECT_DIR/btp

make btpsimple
# Output binaries of `btpsimple` is placed under bin/ directory.
# Add /bin directory to PATH environment variable
export BTPSIMPLE=$PROJECT_DIR/btp/bin
export PATH=$PATH:$BTPSIMPLE

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
cd $PROJECT_DIR/btp/build/contracts/solidity/bmc

truffle exec $SCRIPT_DIR/mb_fund_bmr.js --network moonbeamlocal
```

- Add funds to ICON-BMR

```bash
cd $PROJECT_DIR/btp

AMOUNT=1000000000000000000000000
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx transfer \
  --to $(cat $CONFIG_DIR/icon-bmr.addr) --value $AMOUNT \
  --key_store $CONFIG_DIR/goloop.keystore.json \
  --key_password $(cat $CONFIG_DIR/goloop.keysecret) \
  --nid $(cat $CONFIG_DIR/nid.icon) \
  --step_limit 10000000000

# Check the balance of ICON-BMR
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon balance $(cat $CONFIG_DIR/icon-bmr.addr)
```

Now, let start the BMRs

- Start BMR from ICON --> Moonriver

```bash
$PROJECT_DIR/btp/bin/btpsimple start --config $CONFIG_DIR/moon.config.json
```

- Start BMR from Moonriver --> ICON

```bash
$PROJECT_DIR/btp/bin/btpsimple start --config $CONFIG_DIR/icon.config.json
```

In the next section, we are going to guide you how to transfer the native coins between ICON and Moonriver networks manually. Click on 'Next' if you're interested

&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;
[<--- Prev](./Smart-Contracts-Configuration.md#smart-contracts-configuration) &emsp; &emsp; &emsp; &emsp; [Next --->](./NativeCoin-Transfer-Example.md)
<!--<p align="center">-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/Smart-Contracts-PRA.md"><--- Prev</a>-->
<!--</p> -->