## Smart Contracts Configuration

____

In this example, we would like to show how to config to setup a connection between ICON and Moonriver networks. For other cases, there might be a different configuration

<span style="color:red">**Attention:** This configuration step must be executed after completely deploying smart contracts (BSH, BMC, and BMV) on both connecting networks and generating keystore files of both BTP Message Relays (BMRs)</span>

### 1. ICON-BMC Configuration

____

#### Generate Owner and Register Owner of BMC

```bash
cd $CONFIG_DIR

goloop ks gen --out $CONFIG_DIR/bmc-owner.json --password YOUR_PASSWORD
# Create a secret file 'icon-bmr.secret' and save $YOUR_PASSWORD into that file
echo -n "YOUR_PASSWORD" > $CONFIG_DIR/bmc-owner.secret

# Save icon-bmr address to a file
echo -n "BMC Owner Addresss" > $CONFIG_DIR/bmc-owner.addr

# Register Owner
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/bmc.icon) \
    --key_store godWallet.json --key_password gochain --nid 3 \
    --step_limit 13610920001 \
    --method addOwner \
    --param _addr=$(cat $CONFIG_DIR/bmc-owner.addr) \
    | jq -r . > $CONFIG_DIR/tx.addBMCOwner.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.addBMCOwner.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1


# Add funds to BMC-Owner
AMOUNT=1000000000000000000000000
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx transfer \
--to $(cat $CONFIG_DIR/bmc-owner.addr) --value $AMOUNT \
--key_store $CONFIG_DIR/godWallet.json --key_password gochain \
--step_limit 10000000000 --nid 3 | jq -r . > $CONFIG_DIR/tx.bmcOwner.addFund
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.bmcOwner.addFund)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1  
```

#### Add **BMV**

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/bmc.icon) \
    --key_store bmc-owner.json --key_password $(cat $CONFIG_DIR/bmc-owner.secret) --nid 3 \
    --step_limit 13610920001 \
    --method addVerifier \
    --param _net=$(cat $CONFIG_DIR/net.btp.dst) \
    --param _addr=$(cat $CONFIG_DIR/bmv.icon) \
    | jq -r . > $CONFIG_DIR/tx.verifier.icon

# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.verifier.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'
```

#### Add Connection Link to Moonriver-BMC

```bash
echo $(cat $CONFIG_DIR/bmc_perif.btp.addr) > $CONFIG_DIR/btp.dst

goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/bmc.icon) \
    --key_store bmc-owner.json --key_password $(cat $CONFIG_DIR/bmc-owner.secret) --nid 3 \
    --step_limit 13610920001 \
    --method addLink \
    --param _link=$(cat $CONFIG_DIR/btp.dst) \
    | jq -r . > $CONFIG_DIR/tx.link.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.link.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'    
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
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/bmc.icon) \
    --key_store bmc-owner.json --key_password $(cat $CONFIG_DIR/bmc-owner.secret) --nid 3 \
    --step_limit 13610920001 \
    --method setLinkRotateTerm \
    --param _link=$(cat $CONFIG_DIR/btp.dst) \
    --param _block_interval=0x1770 \
    --param _max_agg=0x08 \
    | jq -r . > $CONFIG_DIR/tx.setLinkRotateTerm.icon

goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/bmc.icon) \
    --key_store bmc-owner.json --key_password $(cat $CONFIG_DIR/bmc-owner.secret) --nid 3 \
    --step_limit 13610920001 \
    --method setLinkDelayLimit \
    --param _link=$(cat $CONFIG_DIR/btp.dst) \
    --param _value=4 \
    | jq -r . > $CONFIG_DIR/tx.setLinkDelayLimit.icon

# Also check whether these transactions are successful 
# For example: goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.setLinkRotateTerm.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'    
```

#### Get Link Status

```bash
# The link status will then be used in BMR settings. Please do not skip this step
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call \
  --to $(cat $CONFIG_DIR/bmc.icon) --method getStatus --param _link=$(cat $CONFIG_DIR/btp.dst) \
  | jq -r . > $CONFIG_DIR/getStatus.bmc.icon

ICON_OFFSET="$(eval "echo $(jq -r '.verifier.offset' "$CONFIG_DIR/getStatus.bmc.icon")")"  

echo -n $ICON_OFFSET > $CONFIG_DIR/icon.offset
```

#### Add BSH Service contract

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/bmc.icon) \
    --key_store bmc-owner.json --key_password $(cat $CONFIG_DIR/bmc-owner.secret) --nid 3 \
    --step_limit 13610920001 \
    --method addService \
    --param _addr=$(cat $CONFIG_DIR/nativeCoinBsh.icon) \
    --param _svc=nativecoin \
    | jq -r . > $CONFIG_DIR/tx.addService.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.addService.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'    
```

#### Register Relay to ICON-BMC

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/bmc.icon) \
    --key_store bmc-owner.json --key_password $(cat $CONFIG_DIR/bmc-owner.secret) --nid 3 \
    --step_limit 13610920001 \
    --method addRelay \
    --param _link=$(cat $CONFIG_DIR/btp.dst) \
    --param _addr=$(jq -r .address $CONFIG_DIR/icon-bmr.keystore.json) \
    | jq -r . > $CONFIG_DIR/tx.registerRelay.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.registerRelay.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'    
```

#### Set FeeAggregation address to ICON-BMC

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/bmc.icon) \
    --key_store bmc-owner.json --key_password $(cat $CONFIG_DIR/bmc-owner.secret) --nid 3 \
    --step_limit 13610920001 \
    --method setFeeAggregator \
    --param _addr=$(cat $CONFIG_DIR/feeAggregation.icon) \
    | jq -r . > $CONFIG_DIR/tx.addFeeAggregation.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.addFeeAggregation.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'    
```

### 2. Config NativeCoin BSH

____

- Generate Owner and Register Owner of NativeCoinBSH

```bash
goloop ks gen --out $CONFIG_DIR/nativecoinBSH-owner.json --password YOUR_PASSWORD
# Create a secret file 'icon-bmr.secret' and save $YOUR_PASSWORD into that file
echo -n "YOUR_PASSWORD" > $CONFIG_DIR/nativecoinBSH-owner.secret

# Save icon-bmr address to a file
echo -n "NativeCoinBSH Owner Addresss" > $CONFIG_DIR/nativecoinBSH-owner.addr

# Register Owner
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/nativeCoinBsh.icon) \
    --key_store godWallet.json --key_password gochain --nid 3 \
    --step_limit 13610920001 \
    --method addOwner \
    --param _addr=$(cat $CONFIG_DIR/nativecoinBSH-owner.addr) \
    | jq -r . > $CONFIG_DIR/tx.addNativeCoinBSHOwner.icon

# Add funds to BSH-Owner
AMOUNT=1000000000000000000000000
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx transfer \
--to $(cat $CONFIG_DIR/nativecoinBSH-owner.addr) --value $AMOUNT \
--key_store $CONFIG_DIR/godWallet.json --key_password gochain \
--step_limit 10000000000 --nid 3 | jq -r . > $CONFIG_DIR/tx.bshOwner.addFund
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.bshOwner.addFund)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1      
```

- Register 'DEV' token

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/nativeCoinBsh.icon) \
    --key_store nativecoinBSH-owner.json --key_password $(cat $CONFIG_DIR/nativecoinBSH-owner.secret) \
    --nid 3 --step_limit 13610920001 \
    --method register \
    --param _name=DEV \
    | jq -r . > $CONFIG_DIR/tx.registerCoin.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.registerCoin.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'    
```

- Set Fee Ratio

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/nativeCoinBsh.icon) \
    --key_store nativecoinBSH-owner.json --key_password $(cat $CONFIG_DIR/nativecoinBSH-owner.secret) \
    --nid 3 --step_limit 13610920001 \
    --method setFeeRatio \
    --param _feeNumerator=100 \
    | jq -r . > $CONFIG_DIR/tx.setFeeRatio.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.setFeeRatio.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'    
```

- Set `NativeCoinBSH` as an Owner of `IRC31Token`

```bash
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call --to $(cat $CONFIG_DIR/irc31token.icon) \
    --key_store godWallet.json --key_password gochain --nid 3 \
    --step_limit 13610920001 \
    --method addOwner \
    --param _addr=$(cat $CONFIG_DIR/nativeCoinBsh.icon) \
    | jq -r . > $CONFIG_DIR/tx.addOwnerIrc31.icon
# Also check whether this transaction is successful 
# goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat tx.addOwnerIrc31.icon)
# If fail, it shows error message and status '0x0'
# Otherwise, status '0x1'    
```

### 3. Config Moonriver BMC

____

- Preparation
  - Generate Relay's address ---> follow this instruction [[link](BMR-Deployment.md#create-keystore-files)] if you have not completed this step
  - Prepare addresses of BMC, BSH, BMV and Relay

```bash
export ICON_BMC=$(cat $CONFIG_DIR/btp.icon)
export MOON_BSH=$(cat $CONFIG_DIR/bsh_perif.addr)
export MOON_BMV=$(cat $CONFIG_DIR/bmv.addr)
export RELAY=$(cat $CONFIG_DIR/moon-bmr.addr)
export ICON_NET=$(cat $CONFIG_DIR/net.btp.icon)
```

- Launch Truffle console

```bash
cd $CONFIG_DIR/Moonriver/btp/solidity/bmc

truffle console --network moonbeamlocal
```

- Add **BMV**

```bash
truffle(moonbeamlocal)> let bmcManagement = await BMCManagement.deployed()

truffle(moonbeamlocal)> let bmcPeriphery = await BMCPeriphery.deployed()

truffle(moonbeamlocal)> await bmcManagement.addVerifier(process.env.ICON_NET, process.env.MOON_BMV)
```

- Add a connection link to ICON's BMC

```bash
truffle(moonbeamlocal)> await bmcManagement.addLink(process.env.ICON_BMC)
```

The above command helps to set a connection link from Moonriver-BMC to ICON-BMC with default setting values:

&emsp; &emsp; &emsp; + `BLOCK_INTERVAL_MSEC`: block interval of ICON (default = 1000ms)

&emsp; &emsp; &emsp; + `MAX_AGGREGATION`: max_aggregation value (default = 10)

&emsp; &emsp; &emsp; + `DELAY_LIMIT`: acceptance of delayed submission sending from BMR to BMC (default = 3)

- Set Link Configuration: this step will help you to change default setting values from the command above

```bash
truffle(moonbeamlocal)> await bmcManagement.setLink(process.env.ICON_BMC, 3000, 5, 3)
```

- Add BSH Service contract

```bash
truffle(moonbeamlocal)> await bmcManagement.addService("nativecoin", process.env.MOON_BSH)
```

- Add Relays and query Relays of one link

```bash
truffle(moonbeamlocal)> await bmcManagement.addRelay(process.env.ICON_BMC, [process.env.RELAY])

truffle(moonbeamlocal)> await bmcManagement.getRelays(process.env.ICON_BMC)
```

- Get Link Status: please **DO NOT** skip this step. This output will then be used to set parameters to deploy BMR

```bash
truffle(moonbeamlocal)> await bmcPeriphery.getStatus(process.env.ICON_BMC)

# Exit truffle console via ".exit", then save 'offsetMTA' to a file
# Replace "OFFSET MTA" by a corresponding value from query result
echo -n "OFFSET MTA" > $CONFIG_DIR/moon.offset
```

### 4. Config Moonriver BSH

____

- Launch Truffle console

```bash
cd $CONFIG_DIR/Moonriver/btp/solidity/bsh

truffle console --network moonbeamlocal
```

- Register 'ICX' token

```bash
truffle(Moonriverlocal)> let bshPeriphery = await BSHPeriphery.deployed()

truffle(Moonriverlocal)> let bshCore = await BSHCore.deployed()

truffle(Moonriverlocal)> await bshCore.register("ICX")

# Query current supporting coins
truffle(moonbeamlocal)> await bshCore.coinNames()
```

&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;
[<--- Prev](./BMR-Setup.md#btp-message-relay-(bmr)-setup) &emsp; &emsp; &emsp; &emsp; [Next --->](./BMR-Deployment.md#btp-message-relay-(bmr)-deployment)