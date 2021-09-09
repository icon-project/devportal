# Transfer Example

In this section, we would like to instruct you how to transfer:

* A native coin of Moonriver network, e.g. `DEV`, to ICON network
* A native coin of ICON network, `ICX`, to Moonriver network

## Transfer 'DEV' to ICON Network

* Query balance of a receiving account before receiving 'DEV' from an account on Moonriver network

```bash
# In this example, we are going to use an abitrary account to demonstrate this transfer
# Please specify your own account if needed
cd $PROJECT_DIR/btp
echo -n "hx8062076aa5e68f021121d1c3b4b3979d21a6dcae" > $CONFIG_DIR/carol.addr
echo "btp://$(cat $CONFIG_DIR/net.btp.icon)/$(cat $CONFIG_DIR/carol.addr)" > $CONFIG_DIR/carol.btp.addr

# Check the balance of Carol
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon balance $(cat $CONFIG_DIR/carol.addr)
```

```bash
# Change your current directory to '.../solidity/bsh'
cd $PROJECT_DIR/btp/build/contracts/solidity/bsh

export CAROL_ADDR=$(cat $CONFIG_DIR/carol.addr)
export CAROL_BTP_ADDR=$(cat $CONFIG_DIR/carol.btp.addr)
export AMOUNT=74706176

# Start truffle console
truffle console --network moonbeamlocal

# Get BSHCore contract instance
truffle(moonbeamlocal)> let bshCore = await BSHCore.deployed()

# Get pre-funds accounts
truffle(moonbeamlocal)> let accounts = await web3.eth.getAccounts()

# Check current balance of accounts[2] and make sure it has sufficient amount of coins
truffle(moonbeamlocal)> web3.eth.getBalance(accounts[2])

# Then, make a transfer request
# You can replace 'hx8062076aa5e68f021121d1c3b4b3979d21a6dcae' by your address on ICON network
# You can specify your own value to transfer
truffle(moonbeamlocal)> await bshCore.transferNativeCoin(process.env.CAROL_BTP_ADDR, {from: accounts[2], value: process.env.AMOUNT})

# After a few seconds, please check the account's balance again
# If success, the balance will be deducted. Otherwise, an account will be refunded
truffle(moonbeamlocal)> web3.eth.getBalance(accounts[2])
```

* Query balance of a receiving account after receiving 'DEV' from an account on Moonriver network

```bash
cd $PROJECT_DIR/btp
# call nativecoinBSH to query coinID of 'DEV'
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call \
--to $(cat $CONFIG_DIR/nativecoinBsh.icon) --method coinId \
--param _coinName=DEV | jq -r . > $CONFIG_DIR/tx.coinID

# call irc31token to query balance of Carol
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to $(cat $CONFIG_DIR/irc31token.icon) \
--method balanceOf --param _owner=$(cat $CONFIG_DIR/carol.addr) \
--param _id=$(cat $CONFIG_DIR/tx.coinID)
```

## Transfer 'ICX' to Moonriver Network

* Generate Alice's keystore and address on ICON network

```bash
cd $PROJECT_DIR/btp

# Replace YOUR_PASSWORD if needed
YOUR_PASSWORD=1234

goloop ks gen --out $CONFIG_DIR/alice.keystore.json --password $YOUR_PASSWORD

# Create a secret file 'alice.secret' and save $YOUR_PASSWORD into that file
echo -n $YOUR_PASSWORD > $CONFIG_DIR/alice.secret

# Save Alice address to a file
echo $(jq -r '.address' "$CONFIG_DIR/alice.keystore.json") > $CONFIG_DIR/alice.addr
```

* Add "fuels" to Alice's account

```bash
AMOUNT=1000000000000000000000000

# transfer 'amount' of ICX coins to Alice's account
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx transfer \
    --to $(cat $CONFIG_DIR/alice.addr) --value $AMOUNT \
    --key_store $CONFIG_DIR/goloop.keystore.json \
    --key_password $(cat $CONFIG_DIR/goloop.keysecret) \
    --nid $(cat $CONFIG_DIR/nid.icon) \
    --step_limit 10000000000

# Check the balance of Alice
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon balance $(cat $CONFIG_DIR/alice.addr)
```

* In this example, we would like to use pre-funds accounts, which was provided in the `truffle.config.js`, as receiver on Moonriver network. You can specify your account for an experiment

```bash
cd $PROJECT_DIR/btp/build/contracts/solidity/bsh

# Start truffle console
truffle console --network moonbeamlocal

# Get pre-funds accounts
truffle(moonbeamlocal)> let accounts = await web3.eth.getAccounts()

# Specify one account as destination to receive 'ICX'
truffle(moonbeamlocal)> accounts[1]
'0x3Cd0A705a2DC65e5b1E1205896BaA2be8A07c6e0'

# Get BSHCore contract instance
truffle(moonbeamlocal)> let bshCore = await BSHCore.deployed()

# Query balance of accounts[1] before receiving 'ICX' from Alice
truffle(moonbeamlocal)> let balance = await bshCore.getBalanceOf(accounts[1], 'ICX')

# Convert BigNumber to easy-reading number
truffle(moonbeamlocal)> web3.utils.BN(balance._usableBalance).toNumber()

# Exit truffle console (".exit") and save a responded address as destination
# Replace BOB_ADDR=returned/account/above if needed
BOB_ADDR=0x3Cd0A705a2DC65e5b1E1205896BaA2be8A07c6e0
echo "btp://$(cat $CONFIG_DIR/net.btp.dst)/$BOB_ADDR" > $CONFIG_DIR/bob.btp.addr
```

* Transfer 'ICX' from Alice to Bob

```bash
cd $PROJECT_DIR/btp

# Change your amount if needed
AMOUNT=1000000
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon sendtx call \
    --to $(cat $CONFIG_DIR/nativeCoinBsh.icon) --method transferNativeCoin \
    --param _to=$(cat $CONFIG_DIR/bob.btp.addr) --value $AMOUNT \
    --key_store $CONFIG_DIR/alice.keystore.json --key_password $(cat $CONFIG_DIR/alice.secret) \
    --nid $(cat $CONFIG_DIR/nid.icon) \
    --step_limit 10000000000 | jq -r . > $CONFIG_DIR/tx.AliceToBob.transfer

# Check whether a transfer is successful
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon txresult $(cat $CONFIG_DIR/tx.AliceToBob.transfer)
```

* Check balance of Bob's account after receiving 'ICX' from Alice

```bash
cd $PROJECT_DIR/btp/build/contracts/solidity/bsh

truffle console --network moonbeamlocal

truffle(moonbeamlocal)> let bshCore = await BSHCore.deployed()

# Query balance of accounts[1] after receiving 'ICX' from Alice
truffle(moonbeamlocal)> let balance = await bshCore.getBalanceOf(accounts[1], 'ICX')

# Convert BigNumber to easy-reading number
# It might take time for BMRs and BMV to sync blocks. 
# Thus, if the balance is still '0', please try it later
# exit truffle console, wait a bit, then re-run checking balance again
truffle(moonbeamlocal)> web3.utils.BN(balance._usableBalance).toNumber()
```

