# Configuration

`tbears genconf` command will generate the default configuration files and a test keystore file.  
If the configurations files do not exist, T-Bears follows the default config values which are presented below. If the default config files \(`tbears_server_config.json` or `tbears_cli_config.json`\) exist in the same folder where `tbears` is executed, `tbears` loads the values from the file. You can override the default behavior by updating the values in the config file or specifying a custom config file path with `-c` option. For the details of T-Bears commands, please read [CLI Commands](doc:how-to-use-t-bears).

### tbears\_server\_config.json

When starting T-Bears \(`tbears start`\), "tbears\_server\_config.json" is used to configure the parameters and initial settings of the T-Bears Server. In this configuration file, for example, you can configure log file path, turn on/off transaction fees, and add another test account with initial ICX balance.

```javascript
// tbears_server_config.json
{
    "hostAddress": "127.0.0.1",
    "port": 9000,
    "scoreRootPath": "./.score",
    "stateDbRootPath": "./.statedb",
    "log": {
        "logger": "tbears",
        "level": "info",
        "filePath": "./tbears.log",
        "colorLog": true,
        "outputType": "file",
        "rotate": {
            "type": "bytes",
            "maxBytes": 10485760,
            "backupCount": 10
        }
    },
    "service": {
        "fee": false,
        "audit": false,
        "deployerWhiteList": false
    },
    "genesis": {
        "nid": "0x3",
        "accounts": [
            {
                "name": "genesis",
                "address": "hx0000000000000000000000000000000000000000",
                "balance": "0x2961fff8ca4a62327800000"
            },
            {
                "name": "fee_treasury",
                "address": "hx1000000000000000000000000000000000000000",
                "balance": "0x0"
            },
            {
                "name": "test1",
                "address": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
                "balance": "0x2961fff8ca4a62327800000"
            }
        ]
    },
    "blockConfirmInterval": 10,
    "blockConfirmEmpty": true
}
```

| Field | Data type | Description |
| :--- | :--- | :--- |
| hostAddress | string | IP address that T-Bears service will listen on. |
| port | int | Port number that T-Bears service will listen on. |
| scoreRootPath | string | Root directory where SCORE will be installed. |
| stateDbRootPath | string | Root directory where state DB file will be created. |
| log | dict | T-Bears log setting |
| log.logger | string | Main logger in process |
| log.level | string | log level.  "debug", "info", "warning", "error" |
| log.filePath | string | Log file path. |
| log.colorLog | boolean | Log display option \(color or black\) |
| log.outputType | string | “console”: log outputs to the console that T-Bears is running. “file”: log outputs to the file path. “console\|file”: log outputs to both console and file. |
| log.rotate | dict | Log rotate setting |
| log.rotate.type | string | "period": rotate by period. "bytes": rotate by maxBytes. "period\|bytes": log rotate to both period and bytes. |
| log.rotate.period | string | use logging.TimedRotatingFileHandler 'when'  ex\) daily, weekly, hourly or minutely |
| log.rotate.interval | string | use logging.TimedRotatingFileHandler 'interval'  ex\) \(period: hourly, interval: 24\) == \(period: daily, interval: 1\) |
| log.rotate.maxBytes | integer | use logging.RotatingFileHandler 'maxBytes'  ex\) 10mb == 10  _1024_  1024 |
| log.rotate.backupCount | integer | limit log file count |
| service | dict | T-Bears service setting |
| service.fee | boolean | true \| false. Charge a fee per transaction when enabled |
| service.audit | boolean | true \| false. Audit deploy transactions when enabled |
| service.deployerWhiteList | boolean | true \| false. Limit SCORE deploy permission when enabled |
| genesis | dict | Genesis information of T-Bears node. |
| genesis.nid | string | Network ID. |
| genesis.accounts | list | List of accounts that holds initial coins.  \(index 0\) genesis: account that holds initial coins. \(index 1\) fee\_treasury: account that collects transaction fees. \(index 2~\): test accounts that you can add. |
| channel | string | channel name interact with iconrpcserver and iconservice |
| amqpKey | string | amqp key name interact with iconrpcserver and iconservice |
| amqpTarget | string | amqp target name interact with iconrpcserver and iconservice |
| blockConfirmInterval | integer | Confirm block every N seconds |
| blockConfirmEmpty | boolean | true \| false. Confirm empty block when enabled |

### tbears\_cli\_config.json

For every T-Bears SCORE & Other CLI Commands except `init` and `test`, this file is used to configure the default parameters and initial settings.

In this configuration file, you can define default option values for some CLI commands. For example, SCORE's `on_install()` or `on_update()` method is called on deployment. In this config file, you can set the deploy "mode" and the parameters \("scoreParams"\) of `on_install()` or `on_update()` as shown in the following example.

```javascript
// tbears_cli_config.json
{
    "uri": "http://127.0.0.1:9000/api/v3",
    "nid": "0x3",
    "keyStore": null,
    "from": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
    "to": "cx0000000000000000000000000000000000000000",
    "deploy": {
        "mode": "install",
        "scoreParams": {}
    },
    "txresult": {},
    "transfer": {}
}
```

| Field | Data  type | Description |
| :--- | :--- | :--- |
| uri | string | URI to send the request. |
| nid | string | Network ID. 0x3 is reserved for T-Bears. |
| keyStore | string | Keystore file path. |
| from | string | From address. It is ignored if 'keyStore' is set. |
| to | string | To address. |
| stepLimit | string | \(optional\) stepLimit value. |
| deploy | dict | Options for deploy command. |
| deploy.mode | string | Deploy mode. install: new SCORE deployment. update: update the SCORE that was previously deployed. |
| deploy.scoreParams | dict | Parameters to be passed to on\_install\(\) or on\_update\(\) |
| deploy.from | string | Address of the SCORE deployer Optional. This value will override "from" value. If not given, "from" value will be used. |
| deploy.to | string | Used when update SCORE \(The address of the SCORE being updated\). In the case of "install" mode, the address should be 'cx0000~'. Optional. This value will override "to" value. If not given, "to" value will be used. |
| txresult | dict | Options for txresult command. You can define command options in a dict. |
| transfer | dict | Options for transfer command. You can define command options in a dict. |

Following CLI commands and options can be defined in the configuration file.

| Command | Options |
| :--- | :--- |
| deploy | uri, nid, keyStore, from, to, mode, scoreParams, stepLimit |
| transfer | uri, nid, keyStore, from, stepLimit |
| sendtx | uri, nid, keyStore, from, stepLimit |
| txresult | uri |
| balance | uri |
| totalsupply | uri |
| scoreapi | uri |
| txbyhash | uri |
| lastblock | uri |
| blockbyhash | uri |
| blockbyheight | uri |
| call | uri |

### keystore\_test1

Keystore file for a test account. The password of this keystore file is `test1_Account`. You will find the test account `test1` in `tbears_server_config.json` and see the account has enough ICX balance \(0x2961fff8ca4a62327800000\) to test.

**Do not transfer any ICX or tokens to 'test1' account.**

```javascript
// keystore_test1
{
    "address": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
    "crypto": {
        "cipher": "aes-128-ctr",
        "cipherparams": {
            "iv": "dc0762c56ca56cd06038df5051c9e23e"
        },
        "ciphertext": "7cc40efac0b14eaf56f951c9c9620f9f34bac548175e85052aa9f753423dc984",
        "kdf": "scrypt",
        "kdfparams": {
            "dklen": 32,
            "n": 16384,
            "r": 1,
            "p": 8,
            "salt": "380c00457be5fd1c244f5745c322b21f"
        },
        "mac": "157dda6fb7092df62ff93411bed54e5a64dbf06c1aae3b375d356061a9c3dfd1"
    },
    "id": "e2ca66c6-b8de-4413-82cb-52c2a2200b8d",
    "version": 3,
    "coinType": "icx"
}
```

### Reference

* [CLI Commands](doc:how-to-use-t-bears) 
* [ICON JSON-RPC API v3](icon-json-rpc-v3)
* [earlgrey](https://github.com/icon-project/earlgrey)
* [ICON Commons](https://github.com/icon-project/icon-commons)
* [ICON RPC Server](https://github.com/icon-project/icon-rpc-server)
* [ICON Service](https://github.com/icon-project/icon-service)

### License

This project follows the Apache 2.0 License. Please refer to [LICENSE](https://www.apache.org/licenses/LICENSE-2.0) for details.

