# Relays Setup

![](../.gitbook/assets/Deploy-BMR.png)

BTP Message Relay (BMR) is a component, that consists of several unidirectional Relays, which takes responsibility for monitoring and gathering proofs for BTP events in the network. It means that _**BMR**_ always has at least two Relays working concurrently:

* Relay 1: listen/collect event's data from Network A --> build `Relay Message` --> push it to _**BMC**_ of Network B
* Relay 2: listen/collect event's data from Network B --> build `Relay Message` --> push it to _**BMC**_ of Network A

Before moving on guidance to deploy BMRs, we would like to make short explanation so that the following instructions would be easier to follow up. As mentioned earlier, this example demonstates to connect ICON Network to Moonriver Network

* Confusion point 1: BMR handles a connection from A to B. BMR listens on A, but it must be registered on B

BMR1: listen blocks generating from ICON Network --> build `Relay Message` --> push this message to Moonriver-BMC contract.

\==> In order to push `Relay Message` to Moonriver-BMC contract, BMR1 must be registered into Moonriver-BMC contract. Thus, it requires BMR1's address being generated in Moonriver Network. Then, `Operator` of Moonriver-BMC contract can manually add this address to handle a connection link (ICON --> Moonriver)

BMR2: listen blocks generating from Moonriver Network --> build `Relay Message` --> push this message to ICON-BMC contract.

\==> In order to push `Relay Message` to ICON-BMC contract, BMR2 must be registered into ICON-BMC contract. Thus, it requires BMR2's address being generated in ICON Network. Then, `Operator` of Moonriver-BMC contract can manually add this address to handle a connection link (Moonriver --> ICON)

* Confusion point 2: BMR handles a connection from A to B. Similar to the above confusion point, BMR listens block generating on A, but it must query this offet through calling `getBMCLinkStatus` on B

\==> To avoid chicken-and-egg problem, `Operator` will choose an `offset` and set it into BMV contract when this contract is deployed. From that point, blocks can be verified and synced. Upon successful verification, BMV updates this `offset` to the latest block height. In addition, current block height on A might be higher than current block height was verified by BMV contract on B. Thus, BMR is required to retrieve the latest `offset` from BMV contract through calling `getBMCLinkStatus` on B

## Generate keystores

* Generate keystore of BMR from Moonriver --> ICON

```bash
cd $PROJECT_DIR/btp

# Replace YOUR_PASSWORD if needed
YOUR_PASSWORD=1234

goloop ks gen --out $CONFIG_DIR/icon-bmr.keystore.json --password $YOUR_PASSWORD

# Save icon-bmr address to a file
echo $(jq -r '.address' "$CONFIG_DIR/icon-bmr.keystore.json") > $CONFIG_DIR/icon-bmr.addr

# Create a secret file 'icon-bmr.secret' and save $YOUR_PASSWORD into that file
echo -n $YOUR_PASSWORD > $CONFIG_DIR/icon-bmr.secret
```

* Generate keystore of BMR from ICON --> Moonriver

```bash
# Please run `ethkey` to generate keystore first
ethkey generate $CONFIG_DIR/moon-bmr.keystore.json
# Enter your password
# Repeat your password

# Replace YOUR_ENTERED_PASSWORD if needed
YOUR_ENTERED_PASSWORD=1234

cat <<< $(jq '. += {"coinType":"evm"}' $CONFIG_DIR/moon-bmr.keystore.json) > $CONFIG_DIR/moon-bmr.keystore.json

# Save moon-bmr address to a file
echo -n "0x$(jq -r '.address' "$CONFIG_DIR/moon-bmr.keystore.json")" > $CONFIG_DIR/moon-bmr.addr

# Create a secret file 'moon-bmr.secret' and save YOUR_ENTERED_PASSWORD into that file
echo -n $YOUR_ENTERED_PASSWORD > $CONFIG_DIR/moon-bmr.secret
```
