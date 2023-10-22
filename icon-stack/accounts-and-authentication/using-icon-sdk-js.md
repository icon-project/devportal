# Using icon-sdk-js

### Generating private keys, public keys and addresses

&#x20;To create a wallet we can simply call the `.create()` method of the `IconWallet` object in the `icon-sdk-js`.

```javascript
const IconService = require("icon-sdk-js");

const { IconWallet } = IconService.default;

const wallet = IconWallet.create();
console.log(`Private Key: ${wallet.getAddress()}`);
console.log(`Private Key: ${wallet.getPrivateKey()}`);
```

You can call existing EOA by calling `loadKeystore` and `loadPrivateKey` function.

After creation, address and private Key can be looked up.

```javascript
// Load Wallet with private key
const walletLoadedByPrivateKey = IconWallet.loadPrivateKey('38f792b95a5202ab431bfc799f7e1e5c74ec0b9ede5c6142ee7364f2c84d72f6');

console.log(walletLoadedByPrivateKey.getAddress());
// Output: hx902ecb51c109183ace539f247b4ea1347fbf23b5

console.log(walletLoadedByPrivateKey.getPrivateKey());
// Output: 38f792b95a5202ab431bfc799f7e1e5c74ec0b9ede5c6142ee7364f2c84d72f6);

// Get keystore object from wallet
const keystoreFile = walletLoadedByPrivateKey.store('qwer1234!');

// Load wallet with keystore file
const walletLoadedByKeyStore = IconWallet.loadKeystore(keystoreFile, 'qwer1234!');

console.log(walletLoadedByKeyStore.getAddress());
// Output: hx902ecb51c109183ace539f247b4ea1347fbf23b5

console.log(walletLoadedByKeyStore.getPrivateKey());
// Output: 38f792b95a5202ab431bfc799f7e1e5c74ec0b9ede5c6142ee7364f2c84d72f6);
```

After Wallet object creation, Keystore file can be stored by calling store function. After calling `store`, Keystore JSON object can be looked up with the returned value.

```javascript
const privateKey = '38f792b95a5202ab431bfc799f7e1e5c74ec0b9ede5c6142ee7364f2c84d72f6';
const wallet = IconWallet.loadPrivateKey(privateKey);
console.log(wallet.store('qwer1234!'));
// Output:
// {
// "version": 3,
// "id": "e00e113c-1e45-47e4-b732-10f3d1903d75",
// "address": "hx7d3d4c743bb82b927ea8a0551a3b9288e722ac84",
// "crypto": {
//     "ciphertext": "d5df37230528bbfc0015e93c61e60041a31fb63266f61ffec60a31f474d4d7d0",
//     "cipherparams": {
//         "iv": "feaf0cc19678e4b78369904a99ba411e"
//     },
//     "cipher": "aes-128-ctr",
//     "kdf": "scrypt",
//     "kdfparams": {
//         "dklen": 32,
//         "salt": "e2e3666919161044f7b369d6ad4296380d4a13b9b5e844301c64a502ea3da240",
//         "n": 16384,
//         "r": 8,
//         "p": 1
//     },
//     "mac": "43789e78de4744d06c14cf966b9609fadbcd815b5380caf3f778797f9824d9d7"
// }
// }
```

### Generating a signature and sending an RPC JSON request

Using the `icon-sdk-js` we can create a transaction object using the `IcxTransactionBuilder` class and then create the signature using the `SignedTransaction` class.

```javascript
const IconService = require("icon-sdk-js");                                 
                                                                             
const {                                                                     
  IconWallet,                                                               
  IconBuilder,                                                              
  SignedTransaction,                                                        
  IconConverter,                                                            
  IconAmount                                                                
} = IconService.default;                                                    
                                                                             
const { IcxTransactionBuilder } = IconBuilder;                              
                                 
function signTx(wallet, to, amount) {
  try {
// select the correct NID depending on the network
// https://docs.icon.community/icon-stack/icon-networks/main-network
const nid = 2;

// convert amount to transfer into loop units
const amountInLoop = IconAmount.of(amount, IconAmount.Unit.ICX).toLoop();
const txObj = new IcxTransactionBuilder()
   .from(wallet.getAddress())
   .to(to)
   .value(amountInLoop)
   .stepLimit(IconConverter.toBigNumber("2000000"))
   .nid(nid)
   .nonce(IconConverter.toBigNumber("1"))
   .version(IconConverter.toBigNumber("3"))
   .timestamp(new Date().getTime() * 1000)
   .build();
const signedTx = new SignedTransaction(txObj, wallet);
return signedTx;
  } catch (err) {
console.log("Unexpected error signing transaction");
console.log(err);
return null;
  }
}

// private key of wallet sending transaction
const pk = "1111111111111111111111111111111111111111111111111111111111111111";
// create wallet from private key
const wallet = IconWallet.loadPrivateKey(pk);
// address receiving the transfer
const addressTo = "hx1111111111111111111111111111111111111111";

const signedTx = signTx(wallet, addressTo, 1);
console.log(`signed tx: ${JSON.stringify(signedTx.getProperties())}`);
```

The result will be the params of the RPC JSON call like in the following example:

```json
{
  "to": "hx1111111111111111111111111111111111111111",
  "from": "hx396031be52ec56955bd7bf15eacdfa1a1c1fe19e",
  "nid": "0x2",
  "version": "0x3",
  "timestamp": "0x5f55518cac340",
  "stepLimit": "0x1e8480",
  "value": "0xde0b6b3a7640000",
  "nonce": "0x1",
  "signature": "TCaN1RDt8MpVKD2/Fk/PTrHfqHgyaN0ZiiyVkGOmzBYoCJ9iO5H+qyuI1M7nshONy7DHk5w3g+nmsSETOyd9FgE="
}
```

To send the transaction it will be necessary to first create an instance of the `IconService` with the correct `HttpProvider` depending on the network that you want to use and then run an async call to execute the transaction.

```javascript
const IconService = require("icon-sdk-js");                                 
                                                                             
const {                                                                     
  IconWallet,                                                               
  IconBuilder,                                                              
  SignedTransaction,                                                        
  IconConverter,                                                            
  IconAmount,
  HttpProvider // we import the HttpProvider class                                                            
} = IconService.default;

…
…

const httpProvider = new HttpProvider(
  "https://lisbon.net.solidwallet.io/api/v3"
);
const iconService = new IconService.default(httpProvider);

async function runAsync() {
  const txHash = await iconService.sendTransaction(signedTx).execute();
  console.log(`TxHash: ${txHash}`);
}

runAsync()
```

