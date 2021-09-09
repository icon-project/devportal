# Appendix

In this section, we would like to provides commands to request/to query information from smart contracts \(BSH, BMV, BMC\) on both networks and also some necessary error response codes that might occurs regarding of a failure deployment/request

## ICON-BSH

```bash
# Query all supporting coins
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "NativeCoinBSH address" --method coinNames

# Query coinID of a given '_coinName'
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "NativeCoinBSH address" --method coinId --param _coinName="Coin Name"

# Query current setting of feeRatio
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "NativeCoinBSH address" --method feeRatio

# Query a list of current Owners
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "NativeCoinBSH address" --method getOwners

# Check whether one address has an Ownership role
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "NativeCoinBSH address" --method isOwner --param _addr="Address of account being verified"

# Get total supply of one wrapped coin given by an '_id'
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "IRC31Token address" --method totalSupply --param _id="Coin ID"

# Query a list of current Owners
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "IRC31Token address" --method getOwners

# Check whether one address has an Ownership role
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "IRC31Token address" --method isOwner --param _addr="Address of account being verified"

# Query balance of an account given by '_coinID'
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call \
--to "IRC31Token address" --method balanceOf \
--param _owner="Account Address" --param _id="Coin ID"

# Query balance of accounts given by a list of '_coinIDs'
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call \
--to "IRC31Token address" --method balanceOf \
--param _addr=Array of Accounts --param _ids=Array of Coin IDs
```

## ICON-BMC

```bash
# Get BTP Address of BMC contract
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getBtpAddress

# Get registered BMVs
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getVerifiers 

# Get registered BSH Services
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getServices 

# Get registered Links
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getLinks 

# Get registered Routes
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getRoutes

# Get registered ServiceCandidates
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getServiceCandidates

# Get status of a given link
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getStatus --param _link="Link"

# Get registered Relays of a given link
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getRelays --param _link="Link"

# Get Fee Gathering Term
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getFeeGatheringTerm

# Get an address of Fee Aggregator
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getFeeAggregator

# Get a map of Relayers
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getRelayers

# Get Min Bond of Relayer
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getRelayerMinBond

# Get Relayer Term
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getRelayerTerm

# Get Min Bond of Relayer
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getRelayerRewardRank

# Get Min Bond of Relayer
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMC Address" --method getRelayerManagerProperties
```

## ICON-BMV

```bash
# Get connected BMC
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMV Address" --method bmc

# Get Net Address
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMV Address" --method netAddress

# Get setID
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMV Address" --method setID

# Get BMV Status
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMV Address" --method getStatus

# Get address of PARACHAIN Event Decoder
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMV Address" --method paraEventDecoder

# Get address of RELAYCHAIN Event Decoder
goloop rpc --uri http://127.0.0.1:9080/api/v3/icon call --to "BMV Address" --method relayEventDecoder
```

## MOON-BSH

```bash
# Attention: These commands should be executed within truffle console and in the directory (../solidity/bsh)
truffle console --network moonbeamlocal

# Below commands working with BSHCore contract
# First, get an instance of BSHCore contract
truffle(moonbeamlocal)> let bshCore = await BSHCore.deployed()

# Add additional Ownership role - Caller: Owner
truffle(moonbeamlocal)> await bshCore.addOwner("Address of an additional Owner")

# Remove current Ownership role - Caller: Owner
truffle(moonbeamlocal)> await bshCore.removeOwner("Address of Owner being removed")

# Query a list of current Owners - Caller: Any
truffle(moonbeamlocal)> await bshCore.getOwners()

# Update a new address of BSHPerif contract - Caller: Owner
truffle(moonbeamlocal)> await bshCore.updateBSHPeriphery("Address of new BSHPerif contract")

# Update a new URI - Caller: Owner
truffle(moonbeamlocal)> await bshCore.updateUri("New URI")

# Set a new fee ratio - Caller: Owner
truffle(moonbeamlocal)> await bshCore.setFeeRatio(New Fee Ratio)

# Register a new coin/wrapped coin - Caller: Owner
truffle(moonbeamlocal)> await bshCore.register("A coin name")

# Query current supporting coin/wrapped coins - Caller: Any
truffle(moonbeamlocal)> await bshCore.coinNames()

# Query a coinID of given 'coinName' - Caller: Any
truffle(moonbeamlocal)> await bshCore.coinId("A coin name")

# Query a balance of one account given by a 'coinName' - Caller: Any
# Returning balance consist of _usableBalance, _lockedBalance, and _refundableBalance
truffle(moonbeamlocal)> await bshCore.getBalanceOf("Account Address", "Coin name")

# Query a balance of one account given by a list of 'coinNames' - Caller: Any
# Returning balance consist of _usableBalances[], _lockedBalances[], and _refundableBalances[]
# respectively to a given list of 'coinNames'
truffle(moonbeamlocal)> await bshCore.getBalanceOfBatch("Account Address", An array of Coin Names)

# Query Aggregation Fees - Caller: Any
# Return ['Coin Name', Amount]
truffle(moonbeamlocal)> await bshCore.getAccumulatedFees()

# Transfer Native Coin of Moonriver to another network - Caller: Any
truffle(moonbeamlocal)> await bshCore.transferNativeCoin("BTP address of receiver", {from: "Account Address", value: _amount})

# Transfer Wrapped Coin from Moonriver to another network - Caller: Any
truffle(moonbeamlocal)> await bshCore.transfer("Coin Name", _amount, "BTP address of receiver")

# Transfer Wrapped Coins from Moonriver to another network - Caller: Any
truffle(moonbeamlocal)> await bshCore.transferBatch(Array of Coin Names, Array of Amount, "BTP address of receiver")

# Transfer Coin + Wrapped Coins from Moonriver to another network - Caller: Any
# Warning: please do not attach a native coin name into 'Array of Coin Names'. If so, it's failed likely
truffle(moonbeamlocal)> await bshCore.transferBatch(Array of Coin Names, Array of Amount, "BTP address of receiver", {from: "Account Address", value: _amount})

# Reclaim a refundable balance - Caller: Any
truffle(moonbeamlocal)> await bshCore.reclaim("Coin Name", _amount)


# Below command working with BSHPeriphery contract
# First, get an instance of BSHPeriphery contract
truffle(moonbeamlocal)> let bshPeriphery = await BSHPeriphery.deployed()

# Check whether BSHPeriphery contract has any pending request
# Return True/False
truffle(moonbeamlocal)> await bshPeriphery.hasPendingRequest()
```

## MOON-BMC

```bash
# Attention: These commands should be executed within truffle console and in the directory (../solidity/bmc)
truffle console --network moonbeamlocal

# Below commands working with BMCPeriphery contract
# First, get an instance of BMCPeriphery contract
let bmcPeriphery = await BMCPeriphery.deployed()

# Query BTP address of BMCPeriphery - Caller: Any
truffle(moonbeamlocal)> await bmcPeriphery.getBmcBtpAddress()

# Get status of a given link 'Link Name' - Caller: Any
truffle(moonbeamlocal)> await bmcPeriphery.getStatus("Link Name")

# Below commands working with BMCManagement contract
# First, get an instance of BMCManagement contract
let bmcManagement = await BMCManagement.deployed()

# Set a new address of BMCPeriphery contract - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.setBMCPeriphery("Address of BMCPerif")

# Add additional Ownership role - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.addOwner("Address of an additional Owner")

# Remove current Ownership role - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.removeOwner("Address of Owner being removed")

# Check whether one address has an Ownership role - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.isOwner("Address of account being verified")

# Register BSH Service - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.addService("Service Name", "Address of BSH Service")

# Remove current BSH Service - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.removeService("Service Name")

# Get all services - Caller: Any
# Return ['Service Name', 'Address of BSH']
truffle(moonbeamlocal)> await bmcManagement.getServices()

# Register BMV - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.addVerifier("Netword ID", "Address of BMV")

# Remove current BMV - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.removeVerifier("Netword ID")

# Get all BMVs - Caller: Any
# Return ['Netword ID', 'Address of BMV']
truffle(moonbeamlocal)> await bmcManagement.getVerifiers()

# Add connecting link - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.addLink("BTP address of connecting BMC")

# Remove connected link - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.removeLink("BTP address of connected BMC")

# Get current connected links - Caller: Any
truffle(moonbeamlocal)> await bmcManagement.getLinks()

# Set new configuration of connected link - Caller: Any
truffle(moonbeamlocal)> await bmcManagement.setLink("BTP address of connected BMC", _blockInterval, _maxAggregation, _delayLimit)

# Add Route - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.addRoute("BTP address of destination BMC", "BTP address of BMC before destination")

# Remove Route - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.removeRoute("BTP address of destination BMC")

# Get Route - Caller: Any
truffle(moonbeamlocal)> await bmcManagement.getRoutes("BTP address of destination BMC")

# Add Relays - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.addRelay("BTP address of connected BMC", Array address of Relays)

# Remove Relay - Caller: Owner
truffle(moonbeamlocal)> await bmcManagement.removeRelay("BTP address of connected BMC", "Address of Relay being removed")

# Get Relays - Caller: Any
truffle(moonbeamlocal)> await bmcManagement.getRelays("BTP address of connected BMC")
```

## MOON-BMV

```bash
# Attention: These commands should be executed within truffle console and in the directory (../solidity/bmv)
truffle console --network moonbeamlocal

# First, get an instance of BMV contract
truffle(moonbeamlocal)> let bmv = await BMV.deployed()

# Get connected BMC - Caller: Any
truffle(moonbeamlocal)> await bmv.getConnectedBMC()

# Get Network ID - Caller: Any
truffle(moonbeamlocal)> await bmv.getNetAddress()
```

## ICON-Error Response Codes

BTP Exception Response Code has a range as:

* BTPException.BTP =&gt; 0 ~ 9
* BTPException.BMC =&gt; 10 ~ 24
* BTPException.BMV =&gt; 25 ~ 39
* BTPException.BSH =&gt; 40 ~ 54
* BTPException.RESERVED =&gt; 55 ~ 68

BMC - BTP Exception of common errors:

* Unknown\(10\)
* Unauthorized\(11\)
* InvalidSn\(12\)
* AlreadyExistsBMV\(13\)
* NotExistsBMV\(14\)
* AlreadyExistsBSH\(15\)
* NotExistsBSH\(16\)
* AlreadyExistsLink\(17\)
* NotExistsLink\(18\)
* AlreadyExistsBMR\(19\)
* NotExistsBMR\(20\)
* Unreachable\(21\)

BSH - BTP Exception of common errors:

* Unknown\(40\)
* Unauthorized\(41\)
* IRC31Failure\(42\)
* IRC31Reverted\(43\)

