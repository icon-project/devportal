# Using goloop CLI

Goloop CLI is a terminal utility that you can use to generate accounts and make RPC JSON requests easily from the command line.

For a list of all the commands that you can use please visit the [following link](https://github.com/icon-project/goloop/blob/master/doc/goloop\_cli.md#goloop).

### Generating private keys, public keys and addresses

Using the `goloop` CLI you can create [keystores files](https://ethereum.org/en/developers/docs/data-structures-and-encoding/web3-secret-storage/) (accounts) easily by using the following command:

```bash
$ goloop ks gen -p 1234
```

The `-p` flag can be used to setup the password for the keystore file.

### Generating a signature and sending an RPC JSON request

Authentication and generating the signature for a transaction is handled internally by the `goloop` CLI, you just need to specify the keystore file and the password to send the RPC JSON request for a transaction.&#x20;

An example of a transaction for sending ICX to another account is described in the following code block:

```bash
$ goloop rpc sendtx transfer --uri "http://localhost:9000/api/v3" --nid "3" --step_limit "2000000" --to "hxb6b5791be0b5ef67063b3c10b840fb81514db2fd" --value "1000" --key_password "1234" --key_store ./path/to/wallet.json
```

For a list of all possible commands related to RPC JSON Requests please visit the [following link](https://github.com/icon-project/goloop/blob/master/doc/goloop\_cli.md#goloop-rpc).

### Next steps

[Accounts and authentication using nodejs](using-nodejs.md)
