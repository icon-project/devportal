# Deploying your SCORE



### Purpose

This document illustrates the steps of SCORE deployment to the local T-Bears server. To learn more about deploying a SCORE to the Mainnet along with the audit process, please read the [Deployment Process](doc:deploy-guideline)

### Prerequisite

We assume that you already have installed T-Bears. If you have not yet installed it, please read [SCORE Quickstart](doc:score-quickstart) or [T-Bears Installation](doc:tbears-installation) first.

* [SCORE Overview](doc:score-overview) 
* [SCORE Quickstart](doc:score-quickstart)
* [Token & Crowdsale](doc:token-crowdsale)

### Deploying SCOREs

#### package.json

Before deploying a SCORE, the required files should be packaged into a zip file. In the zip file, SCORE metadata should be included as well as the smart contract source code itself. The metadata file, `package.json`, contains a version, main module name, and main class.

Here is an example of `package.json` file.

```javascript
{
    "version": "0.0.1",
    "main_module": "hello_world",
    "main_score": "HelloWorld"
}
```

The SCORE execution runtime will find `main_score` class in the `main_module`.py to load and execute it on the ICON network.

If you want to make `main_module` point to a submodule, you can specify the module name as below.

```javascript
{
    "version": "0.0.1",
    "main_module": "sub_module.hello_world",
    "main_score": "HelloWorld"
}
```

#### Deploying SCOREs with T-Bears

Here is an example of deploying a SCORE onto the local T-Bears emulator. If you want to deploy on Mainnet or Testnet, you need to edit `uri`, `nid`, and `keyStore` fields in `tbears_cli_config.json`.

The [SampleToken](token-crowdsale) contract is used in this example.

**Deploy SampleToken on Local T-Bears Emulator**

1. Generate T-Bears configuration files if not yet generated.

   ```bash
    $ tbears genconf
   ```

2. Start T-Bears Server \(node emulator\) on the local environment.

   ```bash
    $ tbears start
   ```

3. Open `tbears_cli_config.json` and edit it as follows. Parameters for `on_install()` method \(`_initialSupply` and `_decimals`\) should be set under `scoreParams` field. Note that every value in the json file must be a **string** type as defined in the [ICON JSON-RPC v3 Specification](doc:icon-json-rpc-v3#section-value-types), and integer value must be represented as a **lowercase** HEX string.

   ```javascript
    {
        "uri": "http://127.0.0.1:9000/api/v3",
        "nid": "0x3",
        "keyStore": null,
        "from": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
        "to": "cx0000000000000000000000000000000000000000",
        "deploy": {
            "stepLimit": "0x10000000",
            "mode": "install",
            "scoreParams": {
                "_initialSupply": "0x3e8",
                "_decimals": "0x12"
            }
        },
        "txresult": {},
        "transfer": {
            "stepLimit": "0xf4240"
        }
    }
   ```

4. Deploy the SampleToken Using `deploy` command in T-Bears, you can deploy the sample\_token project with configuration `tbears_cli_config.json`. T-Bears makes a zip file from the sample\_token directory on the fly and deploys it to the local server.

   ```bash
    $ tbears deploy sample_token -c tbears_cli_config.json
    Send deploy request successfully.
    If you want to check SCORE deployed successfully, execute txresult command
    transaction hash: 0xea834af48150189b4021b9a161d4c0aff3d983ccc47ddc189bac50f55bf580b7
   ```

5. Get transaction result by transaction hash. You can check the result of deployment by querying the transaction hash.

   ```bash
    $ tbears txresult 0xea834af48150189b4021b9a161d4c0aff3d983ccc47ddc189bac50f55bf580b7
   ```

   Here's the result of the transaction. You can find the `scoreAddress`, which is the address of this deployed SCORE, and that address will be used for invoking external methods of SampleToken later.

   ```bash
    Transaction result: {
        "jsonrpc": "2.0",
        "result": {
            "txHash": "0xea834af48150189b4021b9a161d4c0aff3d983ccc47ddc189bac50f55bf580b7",
            "blockHeight": "0xbe",
            "blockHash": "0xdc85bccb32109da6cfe5bb5308121ce68a1494c499d9dd8c724a0bd7397d0729",
            "txIndex": "0x0",
            "to": "cx0000000000000000000000000000000000000000",
            "scoreAddress": "cx0841205d73b93aec1062877d8d4d5ea54c6665bb",
            "stepUsed": "0x2cc8f10",
            "stepPrice": "0x0",
            "cumulativeStepUsed": "0x2cc8f10",
            "eventLogs": [],
            "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "status": "0x1"
        },
        "id": 1
    }
   ```

### Reference

* [T-Bears CLI Reference](doc:t-bears-cli-reference)

