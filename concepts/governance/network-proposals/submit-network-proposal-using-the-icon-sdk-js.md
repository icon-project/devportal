# Submit Network Proposal using the icon-sdk-js

The process of submitting a network proposal requires signing a transaction calling the `registerProposal` method of the governance contract using your node validator wallet. The following section will describe how to use the `icon-sdk-js` to submit a network proposal.&#x20;

The first step would be to initiate a nodejs project in a folder of your choice and install the `icon-sdk-js` by running the following command:

```bash
npm init -y && npm install icon-sdk-js
Wrote to /home/fidel/code/js/projects/icon-projects/icon-nodejs-tools/network-proposal-test/package.json:

{
  "name": "network-proposal-test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}



added 66 packages, and audited 67 packages in 10s

6 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

Add the keystore file to the root folder and edit the `index.js` file with the following code.

```javascript
const IconService = require("icon-sdk-js");
const fs = require("fs");

const {
  IconWallet,
  IconBuilder,
  SignedTransaction,
  IconConverter,
  HttpProvider
} = IconService.default;

const { CallTransactionBuilder } = IconBuilder;

const rpcNodeUrl = "https://ctz.solidwallet.io/api/v3";
const keystorePath = "./keystore.json";
const keystorePassword = "password";

const httpProvider = new HttpProvider(rpcNodeUrl);
const iconService = new IconService(httpProvider);

/*
 * Gets keystore object from keystore file
 * @param {string} path - path to keystore file
 * @return {object} keystore object
 */
function getKeystore(path) {
  const keystore = fs.readFileSync(path, "utf8");
  return keystore;
}

/*
 * Submits a network proposal
 * @param {object} params - proposal parameters
 * @param {string} amount - amount of ICX to send with transaction
 * @return {string} transaction hash
 * @throws {Error} if transaction fails
 */
async function submitNetworkProposal(params, amount) {
  try {
    const wallet = IconWallet.loadKeystore(
      getKeystore(keystorePath),
      keystorePassword
    );

    const txObj = new CallTransactionBuilder()
      .from(wallet.getAddress())
      .to("cx0000000000000000000000000000000000000001")
      .stepLimit(IconConverter.toBigNumber("2000000"))
      .nid(IconConverter.toBigNumber(1))
      .nonce(IconConverter.toBigNumber(1))
      .version(IconConverter.toBigNumber(3))
      .timestamp(new Date().getTime() * 1000)
      .method("registerProposal")
      .params(params)
      .amount(amount)
      .build();

    const signedTransaction = new SignedTransaction(txObj, wallet);
    return await iconService.sendTransaction(signedTransaction).execute();
  } catch (err) {
    console.log(err);
  }
}

async function main() {
  const value = {
    name: "Proposal Name",
    value: {
      text: "Proposal Description"
    }
  };
  const parsedValue = IconConverter.fromUtf8(JSON.stringify(value));

  const params = {
    title: "Proposal Title",
    description: "Proposal Description",
    value: parsedValue
  };

  const networkProposalFee = "0x56bc75e2d63100000";
  const txHash = await submitNetworkProposal(params, networkProposalFee);
  console.log(txHash);
}

main();
```
