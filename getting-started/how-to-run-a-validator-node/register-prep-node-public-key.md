# Register PRep Node Public Key

The process of registering a PRep Node Public Key requires you to first generate a public key from the node wallet and then invoke the [`registerPRepNodePublicKey`](https://github.com/icon-project/goloop/blob/master/doc/icon_chainscore_api.md#registerprepnodepublickey) method of the chain contract (`cx000..00`).

The following is an example of the RPC JSON Call required to call `registerPRepNodePublicKey`.

```json
{
  "jsonrpc": "2.0",
  "method": "icx_call",
  "id": 1234,
  "params": {
    "to": "cx0000000000000000000000000000000000000000",
    "from": "hxb6b5791be0b5ef67063b3c10b840fb81514db2fd",
    "nid": "0x1",
    "version": "0x3",
    "timestamp": "0x603670a75b140",
    "stepLimit": "0x1312d00",
    "nonce": "0x1",
    "dataType": "call",
    "data": {
      "method": "registerPRepNodePublicKey",
      "params": {
        "pubKey": "038b...34",
        "address": "hxb6b5791be0b5ef67063b3c10b840fb81514db2fd"
      }
    },
    "signature": "Wk/...30gE="
  }
}
```

### Checking if a Public Key has already been registered.

You can verify if your PRep node has already registered a Public Key by calling the [`getPRep`](https://docs.icon.community/icon-stack/client-apis/json-rpc-api/v3#getprep) method of the chain contract.

The response object will have a param called [`hasPublicKey`](https://docs.icon.community/icon-stack/client-apis/javascript-sdk#getpublickey) showing if a Public Key has already been registered or not.

The following curl command showcase how to call the `getPRep` readonly method of the chain contract.

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"icx_call","id":484,"params":{"to":"cx0000000000000000000000000000000000000000","dataType":"call","data":{"method":"getPRep", "params": {"address": "hxb6b5791be0b5ef67063b3c10b840fb81514db2fd"}}}}' https://RPC_URL:PORT/api/v3
```

The following example shows the response of the call.

```json
{
  "jsonrpc":"2.0",
  "result": {
    "address": "hxb6b5791be0b5ef67063b3c10b840fb81514db2fd",
    "bonded":"0x152d02c7e14af6800000",
    "city":"Seoul",
    "country":"KOR",
    "delegated":"0x153020c0eaa50f7600000",
    "details":"https://test.example.com/details",
    "email":"test@example.com",
    "grade":"0x0",
    "hasPublicKey":"0x1",
    "irep":"0xa968163f0a57b400000",
    "irepUpdateBlockHeight":"0x0",
    "lastHeight":"0x66",
    "name":"node_hxb6b5791be0b5ef67063b3c10b840fb81514db2fd",
    "nodeAddress":"hxb6b5791be0b5ef67063b3c10b840fb81514db2fd",
    "p2pEndpoint":"test.example.com:7100",
    "penalty":"0x0",
    "power":"0x1682f0ed68b9bede00000",
    "status":"0x0",
    "totalBlocks":"0xd2f54",
    "validatedBlocks":"0xd2f54",
    "website":"https://test.example.com"
    },
  "id":484
}
```

### Generating a Public Key with the goloop CLI

To generate the Public Key you can use the [goloop CLI](https://docs.icon.community/concepts/computational-utilities/goloop). If you dont have the goloop CLI installed in your computer please follow [these instructions](https://docs.icon.community/concepts/computational-utilities/goloop/setup).

Once you have the goloop CLI installed you can use `goloop ks pubkey` [command](https://github.com/icon-project/goloop/blob/master/doc/goloop_cli.md#goloop-ks-pubkey) command to generate the public key of your wallet

```bash
goloop ks pubkey -k KEYSTORE_FILE.json -p KEYSTORE_PASSWORD
```

### Generating a Public Key with the icon-sdk-js

You can also use the [`icon-sdk-js`](https://docs.icon.community/icon-stack/client-apis/javascript-sdk) and call the `.getPublicKey(bool)` method of the [`IconWallet` object](https://docs.icon.community/icon-stack/client-apis/javascript-sdk#iconservice-iconwallet-wallet) to generate a Public key.

```

/**
* Get public key of wallet instance.
* param {bool} compressed - Select compressed or uncompressed key
* @return {string} The public key.
* returns compressed/uncompressed publicKey according to passed `compressed` argument
*/
```

Instantiate a wallet object from the [IconWallet object class](https://docs.icon.community/icon-stack/client-apis/javascript-sdk#iconservice-iconwallet-wallet) using your private key and call the `.getPublicKey(true)` method to get your wallet private key.

```js
// Load wallet object
const wallet = IconWallet.loadPrivateKey('2ab···e4c');
// Get public key of `Wallet` instance.
const publicKey = wallet.getPublicKey(true)
```
