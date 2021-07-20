## Smart Contracts on Moonriver Deployment

____

<p align="center">
  <img src="./images/Deploy-Contracts-Moon.png" width="800" height="300" />
</p>

Before deploying these contracts, please make sure you have deployed a local node in the Moonriver Network. If you have not done this step, please check out this [link](BTP-Development-Instructions.md#2-deploy-moonriver-node) and accomplish it

Create a folder and clone this project to your local machine as follow:

```bash
$ mkdir PRA-Contracts && cd PRA-Contracts

$ git clone https://github.com/icon-project/btp.git

$ cd btp && git checkout icondao

# Set environment variable
$ BTP_PROJECT_DIR=/path/to/btp
```

### 1. Deploy BMC Contracts on Moonriver Network

____

#### Deploy BMC contracts

- Run following commands:

```bash
$ cd ${BTP_PROJ_DIR}/solidity/bmc

$ yarn

$ BMC_PRA_NET=0x501.pra truffle migrate --network Moonriverlocal
```

- After running above commands, you have succeed to deploy required BMC contracts onto the Moonriver Network. In success, you will have a result similar as follows:

```bash
$ yarn
yarn install v1.22.10
[1/4] 🔍  Resolving packages...
success Already up-to-date.
✨  Done in 0.51s.
$ BMC_PRA_NET=0x501.pra truffle migrate --network Moonriverlocal

Compiling your contracts...
===========================
> Everything is up to date, there is nothing to compile.

Starting migrations...
======================
> Network name:    'Moonriverlocal'
> Network id:      1281
> Block gas limit: 15000000 (0xe4e1c0)

1_deploy_bmc.js
===============

   Deploying 'BMCManagement'
   -------------------------
   > transaction hash:    0x1397f88c212655fb3bfc2cc1ca532c03266c9c7182276e4cb519ba4806827285
   > Blocks: 0            Seconds: 0
   > contract address:    0xc01Ee7f10EA4aF4673cFff62710E1D7792aBa8f3
   > block number:        1
   > block timestamp:     1626149168
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.814645268174706176
   > gas used:            4969361 (0x4bd391)
   > gas price:           1 gwei
   > value sent:          0 ETH
   > total cost:          0.004969361 ETH


   Deploying 'ProxyAdmin'
   ----------------------
   > transaction hash:    0xdeb734cb515dfbf5108212753d9ed4afbe1cd951239c8968643590b08d487e33
   > Blocks: 0            Seconds: 0
   > contract address:    0x970951a12F975E6762482ACA81E57D5A2A4e73F4
   > block number:        2
   > block timestamp:     1626149170
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.814162548174706176
   > gas used:            482720 (0x75da0)
   > gas price:           1 gwei
   > value sent:          0 ETH
   > total cost:          0.00048272 ETH


   Deploying 'TransparentUpgradeableProxy'
   ---------------------------------------
   > transaction hash:    0x50c6c4f965c0f4edfe1628189d95d7df39f0a12b199608d608e08989d7d3d3de
   > Blocks: 0            Seconds: 0
   > contract address:    0x3ed62137c5DB927cb137c26455969116BF0c23Cb
   > block number:        3
   > block timestamp:     1626149170
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.813502809174706176
   > gas used:            659739 (0xa111b)
   > gas price:           1 gwei
   > value sent:          0 ETH
   > total cost:          0.000659739 ETH


   Deploying 'BMCPeriphery'
   ------------------------
   > transaction hash:    0xe3cd1177564bf19985b9f0fcdfded6fa15105c533d0d7b97d55edae5715159b7
   > Blocks: 0            Seconds: 0
   > contract address:    0x962c0940d72E7Db6c9a5F81f1cA87D8DB2B82A23
   > block number:        4
   > block timestamp:     1626149171
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.808977907174706176
   > gas used:            4524902 (0x450b66)
   > gas price:           1 gwei
   > value sent:          0 ETH
   > total cost:          0.004524902 ETH


   Deploying 'TransparentUpgradeableProxy'
   ---------------------------------------
   > transaction hash:    0xb58d34400063b7f29af1ef38cf4967b995b6093771d7ff123f409455945620d7
   > Blocks: 0            Seconds: 0
   > contract address:    0x5CC307268a1393AB9A764A20DACE848AB8275c46
   > block number:        5
   > block timestamp:     1626149172
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.808236509174706176
   > gas used:            741398 (0xb5016)
   > gas price:           1 gwei
   > value sent:          0 ETH
   > total cost:          0.000741398 ETH

   > Saving artifacts
   -------------------------------------
   > Total cost:          0.01137812 ETH


Summary
=======
> Total deployments:   5
> Final cost:          0.01137812 ETH
```

#### Get a contract address and full BTP address format of deployed BMCPeriphery contract

- Run following commands to query:

```bash
$ truffle console --network Moonriverlocal

$ truffle(Moonriverlocal)> let bmcPeriphery = await BMCPeriphery.deployed()

$ truffle(Moonriverlocal)> bmcPeriphery.address

$ truffle(Moonriverlocal)> await bmcPeriphery.getBmcBtpAddress()
```

You will receive a query information similar as:

```bash
$ truffle(Moonriverlocal)> bmcPeriphery.address
'0x5CC307268a1393AB9A764A20DACE848AB8275c46'

$ truffle(Moonriverlocal)> await bmcPeriphery.getBmcBtpAddress()
'btp://0x501.pra/0x5CC307268a1393AB9A764A20DACE848AB8275c46'
```

**Note that:** This information will later be used on another deployments. Please do not skip these steps.

### 2. Deploy BSH Contracts on Moonriver Network

____

Open a new terminal window to deploy BSH contracts on the Moonriver Network

- Run following commands and make sure use a correct contract address of `BMCPeriphery`:

```bash
$ cd ${BTP_PROJ_DIR}/solidity/bsh

$ yarn

$ BSH_COIN_URL=https://Moonriver.network/ \
  BSH_COIN_NAME=DEV \
  BSH_COIN_FEE=100 \
  BMC_PERIPHERY_ADDRESS=address/of/BMCPeriphery-Contract \
  BSH_SERVICE=CoinTransfer \
  truffle migrate --network Moonriverlocal
```

- In success, you will have a result similar as:

```bash
Compiling your contracts...
===========================
> Everything is up to date, there is nothing to compile.



Starting migrations...
======================
> Network name:    'Moonriverlocal'
> Network id:      1281
> Block gas limit: 15000000 (0xe4e1c0)


1_deploy_bsh.js
===============

   Replacing 'BSHCore'
   -------------------
   > transaction hash:    0xf1da60278662660dff74a4c9701f8dbf87724924780651781b442063e9c35a22
   > Blocks: 0            Seconds: 0
   > contract address:    0x746DFE0F96789e62CECeeA3CA2a9b5556b3AaD6c
   > block number:        17
   > block timestamp:     1626151632
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.601491923174706176
   > gas used:            4535311 (0x45340f)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.09070622 ETH


   Deploying 'ProxyAdmin'
   ----------------------
   > transaction hash:    0xe515f6aff766cec9a3af1ffd0cafd8c6af9763d5d1633d630a9d7df0ad7555d4
   > Blocks: 0            Seconds: 0
   > contract address:    0x294c664f6D63bd1521231a2EeFC26d805ce00a08
   > block number:        18
   > block timestamp:     1626151633
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.591837523174706176
   > gas used:            482720 (0x75da0)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.0096544 ETH


   Deploying 'TransparentUpgradeableProxy'
   ---------------------------------------
   > transaction hash:    0x9d4e422af278e00133dfe05b04626035fe1eb5e24972c026fa78ff42ddbcf38b
   > Blocks: 0            Seconds: 0
   > contract address:    0x82745827D0B8972eC0583B3100eCb30b81Db0072
   > block number:        19
   > block timestamp:     1626151634
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.574454443174706176
   > gas used:            869154 (0xd4322)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.01738308 ETH


   Replacing 'BSHPeriphery'
   ------------------------
   > transaction hash:    0x50b4942f25173db2a9d7a17a749f4266e18c254c52a84d4e82f98b2bef2b0c3d
   > Blocks: 0            Seconds: 0
   > contract address:    0xEC69d4f48f4f1740976968FAb9828d645Ad1d77f
   > block number:        20
   > block timestamp:     1626151636
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.512091723174706176
   > gas used:            3118136 (0x2f9438)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.06236272 ETH


   Deploying 'TransparentUpgradeableProxy'
   ---------------------------------------
   > transaction hash:    0x98ee35989b5b21ca4fc222e7bc639b40cc25639f6029ff7eb2f41354985b9e47
   > Blocks: 0            Seconds: 0
   > contract address:    0x493275370aF3f63d9ccd10a6539435121cF4fbb9
   > block number:        21
   > block timestamp:     1626151636
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.497248583174706176
   > gas used:            742157 (0xb530d)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.01484314 ETH

   > Saving artifacts
   -------------------------------------
   > Total cost:          0.19494956 ETH


Summary
=======
> Total deployments:   5
> Final cost:          0.19494956 ETH
```

### 3. Deploy BMV Contracts on Moonriver Network

____

Open a new terminal window to deploy BMV contracts on the Moonriver Network

#### Deploy BMV contracts

- Run following commands and make sure use a correct contract address of `BMCPeriphery`

```bash
$ cd ${BTP_PROJ_DIR}/solidity/bmv

$ yarn

$ BMC_CONTRACT_ADDRESS=address/of/BMCPeriphery-Contract \
  BMV_ICON_NET=0x3.icon \
  BMV_ICON_ENCODED_VALIDATORS=0xd69500b6b5791be0b5ef67063b3c10b840fb81514db2fd \
  BMV_ICON_INIT_OFFSET=1 \
  BMV_ICON_INIT_ROOTSSIZE=8 \
  BMV_ICON_INIT_CACHESIZE=8 \
  BMV_ICON_LASTBLOCK_HASH=0x09e4564af11e93a5ed0b6f78f27c742ccd5a7f6a2df8f5b72a5d293ab7ca9e6e \
  truffle migrate --network Moonriverlocal
```

- In success, you will have a result similar as:

```bash
Starting migrations...
======================
> Network name:    'Moonriverlocal'
> Network id:      1281
> Block gas limit: 15000000 (0xe4e1c0)


1_deploy_bmv.js
===============

   Deploying 'DataValidator'
   -------------------------
   > transaction hash:    0xa91d6e664d590cbbbb21d7ec8f79525129dcb4a6749493fb8ef78ce79ecb8b8f
   > Blocks: 0            Seconds: 0
   > contract address:    0x38762083399e60af42e6fD694e7d430a170c9Caf
   > block number:        23
   > block timestamp:     1626152078
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.453746063174706176
   > gas used:            2127765 (0x207795)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.0425553 ETH


   Deploying 'ProxyAdmin'
   ----------------------
   > transaction hash:    0x9b02f32f374892e1babba0f922975d591c82ec0dbbf4a355577f7f0775463cdc
   > Blocks: 0            Seconds: 0
   > contract address:    0x1377Ce7BadadB01a2D06Dc41f31e8B57d9882888
   > block number:        24
   > block timestamp:     1626152079
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.444091663174706176
   > gas used:            482720 (0x75da0)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.0096544 ETH


   Deploying 'TransparentUpgradeableProxy'
   ---------------------------------------
   > transaction hash:    0x492efc4dc89f4fabb2db532923a135d5a719abddb08204e545bc52feda963443
   > Blocks: 0            Seconds: 0
   > contract address:    0xab7785d56697E65c2683c8121Aac93D3A028Ba95
   > block number:        25
   > block timestamp:     1626152079
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.431740503174706176
   > gas used:            617558 (0x96c56)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.01235116 ETH


   Deploying 'BMV'
   ---------------
   > transaction hash:    0xde6dd1633bc9e91c9f859679bf7feb34a973ba804cff6f720a3574ae3fa1de97
   > Blocks: 0            Seconds: 0
   > contract address:    0xb5F73112516ebeB89c7EF67a507513b441Ac28fA
   > block number:        26
   > block timestamp:     1626152079
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.342546463174706176
   > gas used:            4459702 (0x440cb6)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.08919404 ETH


   Deploying 'TransparentUpgradeableProxy'
   ---------------------------------------
   > transaction hash:    0x2dcb6a4570889e133e6c5ca919086a42e62e1ff517640c061bae208ee00372d5
   > Blocks: 0            Seconds: 0
   > contract address:    0x7acc1aC65892CF3547b1b0590066FB93199b430D
   > block number:        27
   > block timestamp:     1626152080
   > account:             0xf24FF3a9CF04c71Dbc94D0b566f7A27B94566cac
   > balance:             1207825.323977043174706176
   > gas used:            928471 (0xe2ad7)
   > gas price:           20 gwei
   > value sent:          0 ETH
   > total cost:          0.01856942 ETH

   > Saving artifacts
   -------------------------------------
   > Total cost:          0.17232432 ETH


Summary
=======
> Total deployments:   5
> Final cost:          0.17232432 ETH
```

#### Get address and check status of BMV contract

```bash
$ truffle console --network Moonriverlocal

$ truffle(Moonriverlocal)> let bmv = await BMV.deployed()

$ truffle(Moonriverlocal)> bmv.address
```

You will receive information similar as:

```bash
$ truffle(Moonriverlocal)> bmv.address
'0x7acc1aC65892CF3547b1b0590066FB93199b430D'
```

**Note that:** Please do not skip these steps. This information will later be used to register ***Veriffier*** (`addVerifier()`) in ***BMCManagement*** contract

### 4. Config Moonriver BMC

____

**Note that: This configuration step must be executed after completely deploying smart contracts (BSH, BMC, and BMV) on both connecting networks. In this example, we would like to show how to config to setup a connection between ICON and Moonriver networks. For other cases, there might be a different configuration**

- Launch Truffle console

```bash
$ cd ${BTP_PROJ_DIR}/solidity/bmc

$ truffle console --network Moonriverlocal
```

- Add **BMV**

```bash
$ truffle(Moonriverlocal)> let bmcManagement = await BMCManagement.deployed()

$ truffle(Moonriverlocal)> let bmcPeriphery = await BMCPeriphery.deployed()

$ truffle(Moonriverlocal)> await bmcManagement.addVerifier("0x3.icon", bmv-contract-address)
# For example: bmv-contract-address = "0x7acc1aC65892CF3547b1b0590066FB93199b430D"
```

- Add a connection link to ICON's BMC

```bash
$ truffle(Moonriverlocal)> await bmcManagement.addLink("btp://0x3.icon/icon-bmc-contract-address")
# For example: icon-bmc-contract-address = cx8eb24849a7ceb16b8fa537f5a8b378c6af4a0247
```

The above command helps to set a connection link from Moonriver-BMC to ICON-BMC with default setting values:

&emsp; &emsp; &emsp; + `BLOCK_INTERVAL_MSEC`: block interval of ICON (default = 1000ms)

&emsp; &emsp; &emsp; + `MAX_AGGREGATION`: max_aggregation value (default = 10)

&emsp; &emsp; &emsp; + `DELAY_LIMIT`: acceptance of delayed submission sending from BMR to BMC (default = 3)

- Set Link Configuration: this step will help you to change default setting values from the command above

```bash
$ truffle(Moonriverlocal)> await bmcManagement.setLink("btp://0x3.icon/icon-bmc-contract-address", 3000, 5, 3)
# For example: icon-bmc-contract-address = cx8eb24849a7ceb16b8fa537f5a8b378c6af4a0247
```

- Add BSH Service contract

```bash
$ truffle(Moonriverlocal)> await bmcManagement.addService("CoinTransfer", bsh-contract-address)
# For example: bsh-contract-address = "0xD5Cd1ea4E9874FfB7d553c517Cec812B3069e322"
```

- Get Link Status: please **DO NOT** skip this step. This output will then be used to set parameters to deploy BMR

```bash
$ truffle(Moonriverlocal)> await bmcPeriphery.getStatus("btp://0x3.icon/icon-bmc-contract-address")
# For example: icon-bmc-contract-address = cx8eb24849a7ceb16b8fa537f5a8b378c6af4a0247
```

### 5. Config Moonriver BSH

____

- Launch Truffle console

```bash
$ cd ${BTP_PROJ_DIR}/solidity/bmc

$ truffle console --network Moonriverlocal
```

- Register 'ICX' token

```bash
$ truffle(Moonriverlocal)> let bshPeriphery = await BSHPeriphery.deployed()

$ truffle(Moonriverlocal)> let bshCore = await BSHCore.deployed()

$ truffle(Moonriverlocal)> await bshCore.register("ICX")
```

&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;
[<--- Prev](./Smart-Contracts-ICON.md) &emsp; &emsp; &emsp; &emsp; [Next --->](./BMR-Deployment.md)

<!--<p align="center">-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/Smart-Contracts-ICON.md" class="button"><--- Prev &emsp; &emsp;</a>-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/BMR-Deployment.md" class="button">&emsp; &emsp; Next </a>-->
<!--</p> -->