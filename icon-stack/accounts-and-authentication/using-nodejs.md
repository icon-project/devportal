# Using nodejs

### Generating private keys, public keys and addresses

To generate a private key we are going to use the [`randomBytes`](https://nodejs.org/api/crypto.html#cryptorandombytessize-callback) method of the [`crypto`](https://nodejs.org/api/crypto.html) package in `nodejs` to have a reasonable source of randomness and then use the [`secp256k1`](https://github.com/cryptocoinjs/secp256k1-node) package to verify the validity of the private key.

Open the `main.js` file and add the following.

```javascript
const { randomBytes } = require("crypto");
const secp256k1 = require("secp256k1");

function makePrivateKey() {
  let pk;

  do {
      pk = randomBytes(32);
  } while (!secp256k1.privateKeyVerify(pk));

  return pk;
}

const privateKey = makePrivateKey();
console.log(`Private Key: ${privateKey.toString("hex")}`);
```

To generate the public key we will use the `publicKeyCreate` method of the `secp256k1` package, this will return a [`buffer`](https://nodejs.org/api/buffer.html) object that we are later converting into a hexadecimal string.

```javascript
const secp256k1 = require("secp256k1");

// sample private key
const pk = "1111111111111111111111111111111111111111111111111111111111111111";

function makePublicKey(pk) {
  // slice(1) removes the 'type byte' which is hardcoded as '04'
  const publicKey = secp256k1.publicKeyCreate(pk, false).slice(1);
  return publicKey;
}

function getPublicKey(pubK) {
  let pubKHex = null;

  try {
pubKHex = Buffer.from(pubK).toString("hex");
  } catch (err) {
console.log("Unexpected error parsing public key into hex");
console.log(err);
  }

  return pubKHex;
}

function privateKeyAsBuffer(pk) {
  let pkBuffer = null;

  try {
pkBuffer = Buffer.from(pk, "hex");
  } catch (err) {
console.log("Unexpected error parsing private key into buffer");
  }

  return pkBuffer;
}

const pkAsBuffer = privateKeyAsBuffer(pk);
const pubKBuffer = makePublicKey(pkAsBuffer);
const pubKHex = getPublicKey(pubKBuffer);
console.log(`Public Key: ${pubKHex}`);
```

To generate the wallet address we convert the hexadecimal string of the public key into a Buffer object and then apply the SHA3-256 hashing algorithm to it, we then take the last 40 characters of the resulting hexadecimal string and add “hx” to the beginning.

```javascript
const sha3_256 = require("js-sha3").sha3_256;

const pubK =
  "4f355bdcb7cc0af728ef3cceb9615d90684bb5b2ca5f859ab0f0b704075871aa385b6b1b8ead809ca67454d9683fcf2ba03456d6fe2c4abe2b07f0fbdbb2f1c1";

function getAddress(pubK) {
  let address = null;

  try {
address = "hx" + sha3_256(pubK).slice(-40);
  } catch (err) {
console.log("Unexpected error parsing address");
console.log(err);
  }

  return address;
}

function hexKeyToBuffer(key) {
  let keyAsBuffer = null;

  try {
keyAsBuffer = Buffer.from(key, "hex");
  } catch (err) {
console.log("Unexpected error parsing key into buffer");
console.log(err);
  }

  return keyAsBuffer;
}
const pubKAsBuffer = hexKeyToBuffer(pubK);
const address = getAddress(pubKAsBuffer);
console.log(`Address: ${address}`);
```

### Generating a signature and sending an RPC JSON request

To generate a signature is necessary to execute the following steps:

* Serialized transaction data
* Create a transaction signature
  * Hash the serialized transaction data
  * Create a recoverable ECDSA signature
  * Encode the generated recoverable ECDSA signature as a Base64-encoded string
* Add the signature to the RPC JSON request as a param.

Transaction data is serialized by concatenating key, value pairs in `params` with `.` as a delimiter. Adding the method name, `icx_sendTransaction`, to the serialized string as a prefix completes the serialization process.

For a more detailed explanation of how to serialize transaction data you can [visit the following tutorial](https://docs.icon.community/getting-started/how-to-use-the-json-rpc-api#how-to-serialize-transaction-data).

An example of a serialized transaction would be the following:

```javascript
“icx_sendTransaction.” + 
“from.hx396031be52ec56955bd7bf15eacdfa1a1c1fe19e.” + 
“nid.0x2.” +
 “nonce.0x1.” + 
“stepLimit.0x1e8480.” + 
“timestamp.0x5f569aaf7c100.” + 
“to.hx81765cee88107ca5d3de480d25778ec71d8ae65f.” + 
“value.0xde0b6b3a7640000.” +
 “version.0x3”
```

To generate the signature we hash the serialized data with the `sha3-256` algorithm, convert that data into a Buffer and apply the `ecdaSign` method of the `secp256k1` library which will return a value of type Buffer. This data is then converted into a `base64` string to have our signature.

```javascript
const secp256k1 = require("secp256k1");                                     
const sha3_256 = require("js-sha3").sha3_256;                               
                                                                             
function sign(serializedData, pk) {                                         
  const serializedHash = sha3_256(serializedData);                          
  const msgAsBuffer = Buffer.from(serializedHash, "hex");                   
  const signing = secp256k1.ecdsaSign(msgAsBuffer, pk);                     
  const recovery = new Uint8Array(1);                                       
  recovery[0] = signing.recid;                                              
  return concatTypedArrays(signing.signature, recovery);                    
}                                                                           
                                                                             
function concatTypedArrays(a, b) {                                          
  const c = new a.constructor(a.length + b.length);                                                                                             
  c.set(a, 0);
  c.set(b, a.length);

  return c;
}

function hexKeyToBuffer(key) {
  let keyAsBuffer = null;

  try {
keyAsBuffer = Buffer.from(key, "hex");
  } catch (err) {
console.log("Unexpected error parsing key into buffer");
console.log(err);
  }

  return keyAsBuffer;
}

const pk = "1111111111111111111111111111111111111111111111111111111111111111";

const serialized = "icx_sendTransaction.from.hx396031be52ec56955bd7bf15eacdfa1a1c1fe19e.nid.0x2.nonce.0x1.stepLimit.0x1e8480.timestamp.0x5f569aaf7c100.to.hx81765cee88107ca5d3de480d25778ec71d8ae65f.value.0xde0b6b3a7640000.version.0x3";

const PK1AsBuffer = hexKeyToBuffer(pk);

const signaturePk1 = sign(serialized, PK1AsBuffer);
const signatureAsBase64 = Buffer.from(signaturePk1).toString("base64");

console.log(signatureAsBase64);
```

### Next steps

[Accounts and authentication using icon-sdk-js](using-icon-sdk-js.md)
