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

```shell
# INode is a folder that was created when deploying a local node in ICON Network
$ CONFIG_DIR=path/to/config/folder/INode

$ cd $CONFIG_DIR

#  Clone BTP Project
$ git clone https://github.com/icon-project/btp.git

$ cd btp && git checkout refactor-layout

# Output binaries are placed under bin/ directory.
$ make btpsimple
```

### 2. Generate keystore and configuration files

____

#### Download CLI tools

```shell
# Get goloop-cli
$ go get github.com/icon-project/goloop/cmd/goloop

# Get ethkey-cli
$ go get github.com/ethereum/go-ethereum/cmd/ethkey
```

#### Create keystore files

- Generate keystore of BMR from Moonriver --> ICON

```shell
$ goloop ks gen --out $CONFIG_DIR/icon-bmr.keystore.json --password $YOUR_PASSWORD

# Create a secret file 'icon-bmr.secret' and save $YOUR_PASSWORD into that file
$ cat > icon-bmr.secret
# Enter your password
```

- Generate keystore of BMR from ICON --> Moonriver

```shell
$ ethkey generate moon-bmr.keystore.json
# Enter your password
# Repeat your password

# Create a secret file 'moon-bmr.secret' and save $YOUR_PASSWORD into that file
$ cat > moon-bmr.secret
# Enter your password
```

#### Query `Offset` and a list of `Relays` from BMC

&emsp; This step checks whether **BMRs** have been registered and added into **BMC**. As designed, only registered **BMRs**, that approved and added by `Operator`, are allowed to build `Relay Message` and push it to **BMC** contract

- Get `BMCLinkStatus` (Moonriver --> ICON): please run below commands

```bash
$ goloop rpc --uri http://goloop:9080/api/v3/icon \
    call --to $JAVA_SCORE_BMC_ADDRESS \
    --method getStatus \
    --param _link= $BTPSIMPLE_SRC_ADDRESS

# For example:
# goloop rpc --uri http://goloop:9080/api/v3/icon \
#     call --to cx8eb24849a7ceb16b8fa537f5a8b378c6af4a0247 \
#     --method getStatus \
#     --param _link=btp://0x501.pra/0x5b5B619E6A040EBCB620155E0aAAe89AfA45D090
```

In success, you receive a response similar as:

```json
{
    "block_interval_dst": "0x3e8",
    "block_interval_src": "0x7d0",
    "cur_height": "0x14d54",
    "delay_limit": "0x3",
    "max_agg": "0x10",
    "relay_idx": "0x0",
    "relays": [
        {
        "address": "hx2dbd4f999f0e6b3c017c029d569dd86950c23104",
        "block_count": "0xd00",
        "msg_count": "0x0"
        }
    ],
    "rotate_height": "0x137ea",
    "rotate_term": "0x8",
    "rx_height": "0xca",
    "rx_height_src": "0x16770",
    "rx_seq": "0x0",
    "tx_seq": "0x2",
    "verifier": {
        "height": "0x235fe",
        "last_height": "0x234a0",
        "offset": "0x234a0"     //  0x234a0 = 144544
    }
}
```

- Get `BMCLinkStatus` (ICON --> Moonriver): please check out this [link](Smart-Contracts-ICON.md) to complete this requirement

#### Create configuration files

- Generate configuration file of BMR from ICON --> Moonriver

```bash
$ BTPSIMPLE_CONFIG=$CONFIG_DIR/moon.config.json

$ BTPSIMPLE_SRC_ADDRESS=btp://0x3.icon/address-of-BMC-ICON
# For example: BTPSIMPLE_SRC_ADDRESS=btp://0x3.icon/cx8eb24849a7ceb16b8fa537f5a8b378c6af4a0247
  
$ BTPSIMPLE_SRC_ENDPOINT=http://goloop:9080/api/v3/icon
  
$ BTPSIMPLE_DST_ADDRESS=btp://0x501.pra/address-of-BMC-Moonriver
# For example: BTPSIMPLE_DST_ADDRESS=btp://0x501.pra/0x5b5B619E6A040EBCB620155E0aAAe89AfA45D090

$ BTPSIMPLE_DST_ENDPOINT=ws://localhost:9944
  
$ BTPSIMPLE_OFFSET=above-query-offset-ICON-->Moonriver
# For example: BTPSIMPLE_OFFSET=3562
  
$ BTPSIMPLE_KEY_STORE=$CONFIG_DIR/moon-bmr.keystore.json
  
$ BTPSIMPLE_KEY_SECRET=$CONFIG_DIR/moon-bmr.secret

$ $CONFIG_DIR/btp/bin/btpsimple save $BTPSIMPLE_CONFIG
```

- Generate configuration file of BMR from Moonriver --> ICON

```bash
$ BTPSIMPLE_CONFIG=$CONFIG_DIR/icon.config.json

$ BTPSIMPLE_SRC_ADDRESS=btp://0x501.pra/address-of-BMC-Moonriver
# For example: BTPSIMPLE_SRC_ADDRESS=btp://0x501.pra/0x5b5B619E6A040EBCB620155E0aAAe89AfA45D090

$ BTPSIMPLE_SRC_ENDPOINT=ws://localhost:9944
  
$ BTPSIMPLE_DST_ADDRESS=btp://0x3.icon/address-of-BMC-ICON
# For example: BTPSIMPLE_DST_ADDRESS=btp://0x3.icon/cx8eb24849a7ceb16b8fa537f5a8b378c6af4a0247

$ BTPSIMPLE_DST_ENDPOINT=http://goloop:9080/api/v3/icon
  
$ BTPSIMPLE_OFFSET=above-query-offset-Moonriver-->ICON
# For example: BTPSIMPLE_OFFSET=144544
  
$ BTPSIMPLE_KEY_STORE=$CONFIG_DIR/icon-bmr.keystore.json
  
$ BTPSIMPLE_KEY_SECRET=$CONFIG_DIR/icon-bmr.secret

$ $CONFIG_DIR/btp/bin/btpsimple save $BTPSIMPLE_CONFIG
```

### 3. Start BMRs

____

- Start BMR from ICON --> Moonriver

```shell
$ $CONFIG_DIR/btp/bin/btpsimple start --config $CONFIG_DIR/moon.config.json
```

- Start BMR from Moonriver --> ICON

```shell
$ $CONFIG_DIR/btp/bin/btpsimple start --config $CONFIG_DIR/icon.config.json
```

&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;
[<--- Prev](./Smart-Contracts-PRA.md)

<!--<p align="center">-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/Smart-Contracts-PRA.md"><--- Prev</a>-->
<!--</p> -->