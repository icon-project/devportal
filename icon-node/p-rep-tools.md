# P-Rep Tools

## P-Rep tools (preptools) Tutorial

This tutorial is intended to give an introduction to using preptools. This guide will walk through the basics of setting up the development environment and the usage of preptools CLI commands.

### Building from source

First, clone this project. Then go to the project directory, create a virtualenv environment, and run the build script. Then install preptools with the .whl file.

```bash
$ python -m venv venv             # Create a virtual environment.
$ source venv/bin/activate        # Enter the virtual environment.
(venv)$ ./build.sh                # run build script.
(venv)$ ls dist                   # check result wheel file.
preptools-1.0.6-py3-none-any.whl
```

### Installation

This chapter explains how to install P-Rep Tools on your system.

#### Requirements

* OS: MacOS or Linux
* Windows is not supported.
* Python
  * Make a virtualenv for Python 3.6.5+ (3.7 is also supported)
  *   Check your Python version

      ```bash
      $ python3 -V
      ```

#### Setup

**Install dependencies**

Some native tools and libraries are needed to install preptools without any errors.

```bash
$ sudo apt-get install -y libssl-dev build-essential automake pkg-config libtool libffi-dev libgmp-dev libyaml-cpp-dev
$ sudo apt-get install -y python3.7-dev libsecp256k1-dev python3-pip
```

**Install preptools**

Install the preptools with the .whl file as below.

```bash
(venv) $ pip install dist/preptools-1.0.6-py3-none-any.whl
```

Install the preptools with pypi

```bash
(venv) $ pip install preptools
```

### How to use P-Rep tools

#### Command-line Interfaces (CLIs)

**Overview**

preptools provides several commands. Here is the list of the available commands.

**Usage**

```
usage: preptools [-h]
                 ...

P-Rep management command line interface v1.0.6

optional arguments:
  -h, --help            show this help message and exit

subcommands:
  {registerPRep,unregisterPRep,setPRep,setGovernanceVariables,registerProposal,cancelProposal,voteProposal,getPRep,getPReps,getProposal,getProposals,txresult,txbyhash,keystore,genconf}
    registerPRep        Register P-Rep
    unregisterPRep      Unregister P-Rep
    setPRep             Change enrolled P-Rep information
    setGovernanceVariables
                        Change Governance variables used in network operation
    registerProposal    Register Proposal
    cancelProposal      Cancel Proposal
    voteProposal        Vote Proposal
    getPRep             Inquire P-Rep information
    getPReps            Get live status of all registered P-Rep candidates
    getProposal         Inquire Proposal information using transaction hash
    getProposals        Inquire all of network proposal list.
    txresult            Get transaction result by hash
    txbyhash            Get transaction by hash
    keystore            Create keystore file in the specified path.
    genconf             Create config file in the specified path.
```

**Options**

| shorthand, Name | default | Description                     |
| --------------- | ------- | ------------------------------- |
| -h, --help      |         | Show this help message and exit |

#### Setting PRep commands

There are four commands to set up the P-Rep information: `preptools registerPRep`, `preptools unregisterPRep`, `preptools setPRep`, and `preptools setGovernanceVariables`. Whenever the commands are called, they load the configuration from `preptools_config.json`. In order to use other configuration file, please specify the file location with the `-c` option.

**registerPRep**

**Description**

Register P-Rep.\
There are two ways of registering a P-Rep.

* Using command line option\
  Input P-Rep information with --\[OPT_NAME] OPT_VALUE.\
  The order of priority is command line > json
* Using json file\
  Input P-Rep information with --prep-json JSON_PATH.

**Usage**

```bash
usage: preptools registerPRep [-h] [--url URL] [--nid NID] [--config CONFIG]
                              [--yes] [--verbose] [--password PASSWORD]
                              [--keystore KEYSTORE]
                              [--step-limit-s STEP_LIMIT] [--name [NAME]]
                              [--country COUNTRY] [--city CITY]
                              [--email EMAIL] [--website WEBSITE]
                              [--details DETAILS] [--p2p-endpoint P2PENDPOINT]
                              [--node-address NODEADDRESS]
                              [--prep-json [PREP_JSON]]

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
  --yes, -y             Don't want to ask send transaction.
  --verbose, -v         Verbose mode
  --password PASSWORD, -p PASSWORD
                        keystore password
  --keystore KEYSTORE, -k KEYSTORE
                        keystore file path
  --step-limit-s STEP_LIMIT
                        step limit to set
  --name [NAME]         P-Rep name
  --country COUNTRY     P-Rep's country
  --city CITY           P-Rep's city
  --email EMAIL         P-Rep's email
  --website WEBSITE     P-Rep's homepage url
  --details DETAILS     json url including P-Rep detailed information
  --p2p-endpoint P2PENDPOINT
                        Network info used for connecting among P-Rep nodes
  --node-address NODEADDRESS
                        PRep Node Key
  --prep-json [PREP_JSON]
                        json file having P-Rep information
```

**Options**

| shorthand, Name  | default                                            | Description                                                                                                      |
| ---------------- | -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| -h, --help       |                                                    | show this help message and exit                                                                                  |
| -u, --url        | [http://127.0.0.1/api/v3](http://127.0.0.1/api/v3) | node url                                                                                                         |
| -n, --nid        | 3                                                  | network id                                                                                                       |
| -c, --config     | ./preptools_config.json                            | preptools config file path                                                                                       |
| -p, --password   |                                                    | keystore password                                                                                                |
| -k, --keystore   |                                                    | keystore file path                                                                                               |
| -s, --step-limit | 0x50000000                                         | step limit to set                                                                                                |
| --name           |                                                    | P-Rep name                                                                                                       |
| --country        |                                                    | P-Rep's country. This require [ISO 3166-1 alpha-3](https://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-3) standard. |
| --city           |                                                    | P-Rep's city.                                                                                                    |
| --email          |                                                    | P-Rep's email.                ex) "example@iconloop.com"                                                         |
| --website        |                                                    | P-Rep's homepage url.         ex) "[https://node.example.com/](https://node.example.com)"                        |
| --details        |                                                    | json url including P-Rep detailed information                                                                    |
|                  |                                                    | ex) "[https://node.example.com/json](https://node.example.com/json)"                                             |
| --p2p-endpoint   |                                                    | Network info used for connection among P-Rep nodes.                                                              |
|                  |                                                    | ex) “123.45.67.89:7100” or “node.example.com:7100”                                                               |
| --node-address   |                                                    | PRep Node Key (default: Operator Key)                                                                            |
| --prep-json      |                                                    | json file having P-Rep information                                                                               |

**Examples**

```bash
(venv) $ cat registerPRep.json 
{
    "name": "banana node",
    "country": "USA",
    "city": "New York",
    "email": "banana@example.com",
    "website": "https://icon.banana.com",
    "details": "https://icon.banana.com/json",
    "p2pEndpoint": "node.example.com:7100",
    "nodeAddress": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6"
}

(venv) $ preptools registerPRep -k test_keystore --prep-json registerPRep.json 
> Password: 
[Request] ======================================================================
{
    "from_": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6",
    "to": "cx0000000000000000000000000000000000000000",
    "value": 2000000000000000000000,
    "step_limit": 268435456,
    "nid": 3,
    "nonce": null,
    "version": 3,
    "timestamp": null,
    "method": "registerPRep",
    "data_type": "call",
    "params": {
        "name": "banana node",
        "country": "USA",
        "city": "New York",
        "email": "banana@example.com",
        "website": "https://icon.banana.com",
        "details": "https://icon.banana.com/json",
        "p2pEndpoint": "node.example.com:7100",
        "nodeAddress": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d400"
    }
}

> Continue? [Y/n]
request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": "0xe667b8de967e4c5e2cc5f4fc2775766f87517935e0875a8c4d0b9c8c2ce01846",
    "id": 1234
}

(venv) $ cat registerPRep.json 
{
    "email": "banana@example.com",
    "website": "https://icon.banana.com",
    "details": "https://icon.banana.com/json",
    "p2pEndpoint": "node.example.com:7100",
    "nodeAddress": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d400"
}

(venv) preptools registerPRep -k test_keystore --prep-json registerPRep.json --name "kokoa node"
> Password: 
 > country : USA
 > city : New York
[Request] ======================================================================
{
    "from_": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6",
    "to": "cx0000000000000000000000000000000000000000",
    "value": 2000000000000000000000,
    "step_limit": 268435456,
    "nid": 3,
    "nonce": null,
    "version": 3,
    "timestamp": null,
    "method": "registerPRep",
    "data_type": "call",
    "params": {
        "email": "banana@example.com",
        "website": "https://icon.banana.com",
        "details": "https://icon.banana.com/json",
        "p2pEndpoint": "node.example.com:7100",
        "name": "kokoa node",
        "country": "USA",
        "city": "New York"
    }
}

> Continue? [Y/n]
request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": "0xeb00ea0ad9ee155067f37015d2403067904649a76a35dd06197600e408d30e3e",
    "id": 1234
}
```

**unregisterPRep**

**Description**

Unregister P-Rep.

**Usage**

```bash
usage: preptools unregisterPRep [-h] [--url URL] [--nid NID] [--config CONFIG]
                                [--yes] [--verbose] [--password PASSWORD]
                                [--keystore KEYSTORE]
                                [--step-limit-s STEP_LIMIT]

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
  --yes, -y             Don't want to ask send transaction.
  --verbose, -v         Verbose mode
  --password PASSWORD, -p PASSWORD
                        keystore password
  --keystore KEYSTORE, -k KEYSTORE
                        keystore file path
  --step-limit-s STEP_LIMIT
                        step limit to set
```

**Options**

| shorthand, Name  | default                                            | Description                     |
| ---------------- | -------------------------------------------------- | ------------------------------- |
| -h, --help       |                                                    | show this help message and exit |
| -u, --url        | [http://127.0.0.1/api/v3](http://127.0.0.1/api/v3) | node url                        |
| -n, --nid        | 3                                                  | network id                      |
| -c, --config     | ./preptools_config.json                            | preptools config file path      |
| -p, --password   |                                                    | keystore password               |
| -k, --keystore   |                                                    | keystore file path              |
| -s, --step-limit | 0x50000000                                         | step limit to set               |

**Examples**

```bash
(venv) $ preptools unregisterPRep -k test_keystore 
> Password: 
[Request] ======================================================================
{
    "from_": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6",
    "to": "cx0000000000000000000000000000000000000000",
    "value": 0,
    "step_limit": 268435456,
    "nid": 3,
    "nonce": null,
    "version": 3,
    "timestamp": null,
    "method": "unregisterPRep",
    "data_type": "call",
    "params": {}
}

> Continue? [Y/n]
request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": "0x027038296f595aedd1bfa680de2e20c3fd133816f9c74807e440bf6c548fb9aa",
    "id": 1234
}
```

**setPRep**

**Description**

Change enrolled P-Rep information.\
There are three way of set P-Rep.

* Using command line option\
  You can input P-Rep information with --\[OPT_NAME] OPT_VALUE.\
  The order of priority is command line > json.
* Using json file\
  You can input P-Rep information with --prep-json JSON_PATH.
* Using interactive mode \[--i]\
  Activate interactive mode and input P-Rep info what you want.\
  If you don't want to input, just enter.

**Usage**

```bash
usage: preptools setPRep [-h] [--url URL] [--nid NID] [--config CONFIG]
                         [--yes] [--verbose] [--password PASSWORD]
                         [--keystore KEYSTORE] [--step-limit-s STEP_LIMIT]
                         [-i] [--name NAME] [--country COUNTRY] [--city CITY]
                         [--email EMAIL] [--website WEBSITE]
                         [--details DETAILS] [--p2p-endpoint P2PENDPOINT]
                         [--node-address NODEADDRESS]
                         [--prep-json PREP_JSON]

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
  --yes, -y             Don't want to ask send transaction.
  --verbose, -v         Verbose mode
  --password PASSWORD, -p PASSWORD
                        keystore password
  --keystore KEYSTORE, -k KEYSTORE
                        keystore file path
  --step-limit-s STEP_LIMIT
                        step limit to set
  -i, --interactive     Activate interactive mode when prep fields are blank.
  --name NAME           PRep name
  --country COUNTRY     P-Rep's country
  --city CITY           P-Rep's city
  --email EMAIL         P-Rep's email
  --website WEBSITE     P-Rep's homepage url
  --details DETAILS     json url including P-Rep details information
  --p2p-endpoint P2PENDPOINT
                        Network info used for connecting among P-Rep nodes
  --node-address NODEADDRESS
                        PRep Node Key (Default: Own Address)
  --prep-json PREP_JSON
                        json file including P-Rep information
```

**Options**

| shorthand, Name   | default                                            | Description                                                                                                      |
| ----------------- | -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| -h, --help        |                                                    | show this help message and exit                                                                                  |
| -u, --url         | [http://127.0.0.1/api/v3](http://127.0.0.1/api/v3) | node url                                                                                                         |
| -n, --nid         | 3                                                  | network id                                                                                                       |
| -c, --config      | ./preptools_config.json                            | preptools config file path                                                                                       |
| -p, --password    |                                                    | keystore password                                                                                                |
| -k, --keystore    |                                                    | keystore file path                                                                                               |
| -s, --step-limit  | 0x50000000                                         | step limit to set                                                                                                |
| -i, --interactive |                                                    | Activate interactive mode when prep fields are blank.                                                            |
| --name            |                                                    | P-Rep name                                                                                                       |
| --country         |                                                    | P-Rep's country. This require [ISO 3166-1 alpha-3](https://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-3) standard. |
| --city            |                                                    | P-Rep's city.                                                                                                    |
| --email           |                                                    | P-Rep's email.                ex) "example@iconloop.com"                                                         |
| --website         |                                                    | P-Rep's homepage url.         ex) "[https://node.example.com/](https://node.example.com)"                        |
| --details         |                                                    | json url including P-Rep detailed information                                                                    |
|                   |                                                    | ex) "[https://node.example.com/json](https://node.example.com/json)"                                             |
| --p2p-endpoint    |                                                    | Network info used for connection among P-Rep nodes.                                                              |
|                   |                                                    | ex) “123.45.67.89:7100” or “node.example.com:7100”                                                               |
| --node-address    |                                                    | PRep Node Key (Default: Own Address)                                                                             |
| --prep-json       |                                                    | json file having P-Rep information                                                                               |

**Examples**

```bash
(venv) $ cat setPRep.json 
{
    "name": "kokoa node",
    "country": "USA",
    "website": "https://icon.kokoa.com",
    "details": "https://icon.kokoa.com/json",
    "p2pEndpoint": "node.example.com:7100",
    "nodeAddress": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d400"
}

(venv) $ preptools setPRep -k test_keystore --prep-json setPRep.json 
> Password: 
[Request] ======================================================================
{
    "from_": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6",
    "to": "cx0000000000000000000000000000000000000000",
    "value": 0,
    "step_limit": 268435456,
    "nid": 3,
    "nonce": null,
    "version": 3,
    "timestamp": null,
    "method": "setPRep",
    "data_type": "call",
    "params": {
        "name": "kokoa node",
        "country": "USA",
        "website": "https://icon.kokoa.com",
        "details": "https://icon.kokoa.com/json",
        "p2pEndpoint": "node.example.com:7100",
    "nodeAddress": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d400"
    }
}

> Continue? [Y/n]
request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": "0xc8456053128897a0941dab4c79428db91dda5a2899e3813698146ac25808c4c9",
    "id": 1234
}

(venv) $ preptools setPRep -k test_keystore --prep-json setPRep.json -i
> Password: 
 > city : New York
 > email : 
[Request] ======================================================================
{
    "from_": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6",
    "to": "cx0000000000000000000000000000000000000000",
    "value": 0,
    "step_limit": 268435456,
    "nid": 3,
    "nonce": null,
    "version": 3,
    "timestamp": null,
    "method": "setPRep",
    "data_type": "call",
    "params": {
        "name": "kokoa node",
        "country": "USA",
        "website": "https://icon.kokoa.com",
        "details": "https://icon.kokoa.com/json",
        "p2pEndpoint": "node.example.com:7100",
        "city": "New York",
    "nodeAddress": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d400"
    }
}

> Continue? [Y/n]
request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": "0xff0c4b603a2ae5ba50f658e0d0188210a5afeec559e44df29b55806342fa4563",
    "id": 1234
}
```

**setGovernanceVariables**

**Description**

Change Governance variables used in network operation.\
You can only change it once per term.\
Other items besides irep may be added later.

**Usage**

```bash
usage: preptools setGovernanceVariables [-h] [--url URL] [--nid NID]
                                        [--config CONFIG] [--yes] [--verbose]
                                        [--password PASSWORD]
                                        [--keystore KEYSTORE]
                                        [--step-limit-s STEP_LIMIT] --irep
                                        IREP

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
  --yes, -y             Don't want to ask send transaction.
  --verbose, -v         Verbose mode
  --password PASSWORD, -p PASSWORD
                        keystore password
  --keystore KEYSTORE, -k KEYSTORE
                        keystore file path
  --step-limit-s STEP_LIMIT
                        step limit to set
  --irep IREP           amounts of irep
```

**Options**

| shorthand, Name  | default                                            | Description                     |
| ---------------- | -------------------------------------------------- | ------------------------------- |
| -h, --help       |                                                    | show this help message and exit |
| -u, --url        | [http://127.0.0.1/api/v3](http://127.0.0.1/api/v3) | node url                        |
| -n, --nid        | 3                                                  | network id                      |
| -c, --config     | ./preptools_config.json                            | preptools config file path      |
| -p, --password   |                                                    | keystore password               |
| -k, --keystore   |                                                    | keystore file path              |
| -s, --step-limit | 0x50000000                                         | step limit to set               |
| --irep           |                                                    | amounts of irep in loop         |

**Examples**

```bash
(venv) $ preptools setGovernanceVariables --irep 50_000_000_000_000_000_000_000
> Password: 
[Request] ======================================================================
{
    "from_": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6",
    "to": "cx0000000000000000000000000000000000000000",
    "value": 0,
    "step_limit": 268435456,
    "nid": 3,
    "nonce": null,
    "version": 3,
    "timestamp": null,
    "method": "setGovernanceVariables",
    "data_type": "call",
    "params": {
        "irep": "0xa968163f0a57b400000"
    }
}

> Continue? [Y/n]
request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": "0xc15b6989cd39e01b3d4bb65b72e6f7fcbc009020779b7f9fc60d59da4df7b091",
    "id": 1234
}
```

#### Preptools information commands

Commands that show the P-Rep information. There are two commands `preptools getPRep` and `preptools getPReps`.

**getPRep**

**Description**

Inquire P-Rep information

**Usage**

```bash
usage: preptools getPRep [-h] [--url URL] [--nid NID] [--config CONFIG]
                         address

positional arguments:
  address               Address of P-Rep you are looking for

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
```

**Options**

| shorthand, Name | default                                            | Description                          |
| --------------- | -------------------------------------------------- | ------------------------------------ |
| -h, --help      |                                                    | show this help message and exit      |
| -u, --url       | [http://127.0.0.1/api/v3](http://127.0.0.1/api/v3) | node url                             |
| -n, --nid       | 3                                                  | network id                           |
| -c, --config    | ./preptools_config.json                            | preptools config file path           |
| address         |                                                    | Address of P-Rep you are looking for |

**Examples**

```bash
(venv) $ preptools getPRep hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6
[Request] ======================================================================
{
    "from_": "hx1234567890123456789012345678901234567890",
    "to": "cx0000000000000000000000000000000000000000",
    "method": "getPRep",
    "params": {
        "address": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6"
    }
}

request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": {
        "status": "0x1",
        "grade": "0x2",
        "name": "kokoa node",
        "country": "USA",
        "city": "New York",
        "stake": "0x0",
        "delegated": "0x0",
        "totalBlocks": "0x0",
        "validatedBlocks": "0x0",
        "irep": "0xa968163f0a57b400000",
        "irepUpdateBlockHeight": "0x58f",
        "lastGenerateBlockHeight": "-0x1",
        "email": "rhkddnjs99@hotmail.com",
        "website": "https://icon.kokoa.com",
        "details": "https://icon.kokoa.com/json",
        "p2pEndpoint": "node.example.com:7100",
        "nodeAddress": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d400"
    },
    "id": 1234
}
```

**getPReps**

**Description**

Get live status of all registered P-Rep candidates

**Usage**

```bash
usage: preptools getPReps [-h] [--url URL] [--nid NID] [--config CONFIG]
                          [--start-ranking START_RANKING]
                          [--end-ranking END_RANKING]
                          [--block-height BLOCK_HEIGHT]

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
  --start-ranking START_RANKING
                        Get P-Rep list which starts from start ranking
  --end-ranking END_RANKING
                        Get P-Rep list which ends with end ranking, inclusive
  --block-height BLOCK_HEIGHT
                        Block height which ranking formed
```

**Options**

| shorthand, Name | default                                            | Description                                           |
| --------------- | -------------------------------------------------- | ----------------------------------------------------- |
| -h, --help      |                                                    | show this help message and exit                       |
| -u, --url       | [http://127.0.0.1/api/v3](http://127.0.0.1/api/v3) | node url                                              |
| -n, --nid       | 3                                                  | network id                                            |
| -c, --config    | ./preptools_config.json                            | preptools config file path                            |
| --start-ranking |                                                    | Get P-Rep list which starts from start ranking        |
| --end-ranking   |                                                    | Get P-Rep list which ends with end ranking, inclusive |
| --block-height  |                                                    | Block height which ranking formed                     |

**Examples**

```bash
(venv) $ preptools getPReps
[Request] ======================================================================
{
    "from_": "hx1234567890123456789012345678901234567890",
    "to": "cx0000000000000000000000000000000000000000",
    "method": "getPReps",
    "params": null
}

request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": {
        "blockHeight": "0x1e3f3",
        "startRanking": "0x0",
        "totalDelegated": "0x0",
        "totalStake": "0x0",
        "preps": [
            {
                "name": "Banana node",
                "country": "USA",
                "city": "New York",
                "grade": "0x0",
                "address": "hx8f21e5c54f006b6a5d5fe65486908592151a7c57",
                "irep": "0xc350",
                "irepUpdateBlockHeight": "0x1200",
                "lastGenerateBlockHeight": "-0x1",
                "stake": "0x21e19e0c9bab2400000",
                "delegated": "0x204fce5e3e25026110000000",
                "totalBlocks": "0x2710",
                "validatedBlocks": "0x2328"
            },
            ...
        ]
    },
    "id": 1234
}


(venv) $ preptools getPReps --start-ranking "0x1" --end-ranking "0x8" --block-height "0x1234"
[Request] ======================================================================
{
    "from_": "hx1234567890123456789012345678901234567890",
    "to": "cx0000000000000000000000000000000000000000",
    "method": "getPReps",
    "params": {
        "startRanking": "0x1",
        "endRanking": "0x8",
        "blockHeight": "0x1234"
    }
}

request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": {
        "blockHeight": "0x1e452",
        "startRanking": "0x0",
        "totalDelegated": "0x0",
        "totalStake": "0x0",
        "preps": [
            {
                "name": "Banana node",
                "country": "USA",
                "city": "New York",
                "grade": "0x0",
                "address": "hx8f21e5c54f006b6a5d5fe65486908592151a7c57",
                "irep": "0xc350",
                "irepUpdateBlockHeight": "0x1200",
                "lastGenerateBlockHeight": "-0x1",
                "stake": "0x21e19e0c9bab2400000",
                "delegated": "0x204fce5e3e25026110000000",
                "totalBlocks": "0x2710",
                "validatedBlocks": "0x2328"
            },
            ...
        ]
    },
    "id": 1234
}
```

#### Network Proposal commands

There are 3 commands to network-proposal: `registerProposal`, `cancelProposal` and `voteProposal` Whenever the commands are called, they load the configuration from `preptools_config.json`. In order to use other configuration file, please specify the file location with the `-c` option.

**registerProposal**

refer [registerProposal request format](https://github.com/icon-project/governance#registerproposal)

**Description**

Register Network-proposal

**Usage**

```bash
usage: preptools registerProposal [-h] [--url URL] [--nid NID]
                                  [--config CONFIG] [--yes] [--verbose]
                                  [--password PASSWORD] [--keystore KEYSTORE]
                                  [--step-limit-s STEP_LIMIT] --title TITLE
                                  --desc DESC --type TYPE
                                  [--value-value VALUE_VALUE]
                                  [--value-code VALUE_CODE]
                                  [--value-name VALUE_NAME]
                                  [--value-address VALUE_ADDRESS]
                                  [--value-type VALUE_TYPE]

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
  --yes, -y             Don't want to ask send transaction.
  --verbose, -v         Verbose mode
  --password PASSWORD, -p PASSWORD
                        keystore password
  --keystore KEYSTORE, -k KEYSTORE
                        keystore file path
  --step-limit-s STEP_LIMIT
                        step limit to set
  --title TITLE         Proposal title
  --desc DESC           Proposal description
  --type TYPE           type of Proposal
  --value-value VALUE_VALUE
                        type 0:text message, type 4:step price in loop
                        (required when type 0 or 4)
  --value-code VALUE_CODE
                        revision code (required when type 1)
  --value-name VALUE_NAME
                        icon-service version (required when type 1)
  --value-address VALUE_ADDRESS
                        type 2: address of SCORE, type 3: address of main/sub
                        P-Rep (required when type 2 or 3)
  --value-type VALUE_TYPE
                        0 : freeze, 1 : unfreeze (required when type 2)
```

**Options**

| shorthand, Name  | default                                                      | Description                                |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------ |
| -h, --help       |                                                              | show this help message and exit            |
| -u, --url        | [http://127.0.0.1:9000/api/v3](http://127.0.0.1:9000/api/v3) | node url                                   |
| -n, --nid        | 3                                                            | networkId mainnet(1), testnet(2)           |
| --config         |                                                              | configuration file path.                   |
| -y, --yes        |                                                              | Do not confirm if you want to send request |
| -v, --verbose    |                                                              | verbose mode flag                          |
| -p, --password   |                                                              | Keystore file's password                   |
| -k, --keystore   |                                                              | keystore file path                         |
| -s, --step-limit | 0x50000000                                                   | step limit to set                          |
| --title          |                                                              | title of network-proposal                  |
| --desc           |                                                              | description of network-proposal            |
| --type           |                                                              | type of network-proposal(0,1,2,3,4)        |
| --value-value    |                                                              | value of value field                       |
| --value-code     |                                                              | value of code field                        |
| --value-address  |                                                              | value of address field                     |
| --value-type     |                                                              | value of type field                        |

**Examples**

```bash
(venv)$ preptools registerProposal -c preptools_config.json -k prep_keys0 -p qwer1234% --title pro0 --desc "first proposal" --type 4 --value-value 1234
[Value] ========================================================================
{
    "value": "1234"
}
[Request] ======================================================================
{
    "from_": "hxb74e29fba1809a105fdec433040a4e713bbe91fe",
    "to": "cx0000000000000000000000000000000000000001",
    "value": 0,
    "step_limit": 268435456,
    "nid": 3,
    "nonce": null,
    "version": 3,
    "timestamp": null,
    "method": "registerProposal",
    "data_type": "call",
    "params": {
        "title": "pro0",
        "description": "first proposal",
        "type": "0x4",
        "value": "0x7b2276616c7565223a202231323334227d"
    }
}

> Continue? [Y/n]Y
request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": "0x02221f9346f9c9b3322ea33e67a1ca0fbe9491e0ea3aefb5154a43e2ea829fa4",
    "id": 1234
}
```

**voteProposal**

refer [voteProposal request format](https://github.com/icon-project/governance#voteproposal)

**Description**

Vote Network-proposal

**Usage**

```bash
usage: preptools voteProposal [-h] [--url URL] [--nid NID] [--config CONFIG]
                              [--yes] [--verbose] [--password PASSWORD]
                              [--keystore KEYSTORE]
                              [--step-limit-s STEP_LIMIT] --id ID --vote VOTE

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
  --yes, -y             Don't want to ask send transaction.
  --verbose, -v         Verbose mode
  --password PASSWORD, -p PASSWORD
                        keystore password
  --keystore KEYSTORE, -k KEYSTORE
                        keystore file path
  --step-limit-s STEP_LIMIT
                        step limit to set
  --id ID               hash of registerProposal TX
  --vote VOTE           0 : disagree, 1 : agree
```

**Options**

| shorthand, Name  | default                                                      | Description                                |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------ |
| -h, --help       |                                                              | show this help message and exit            |
| -u, --url        | [http://127.0.0.1:9000/api/v3](http://127.0.0.1:9000/api/v3) | node url                                   |
| -n, --nid        | 3                                                            | networkId mainnet(1), testnet(2)           |
| --config         |                                                              | configuration file path.                   |
| -v, --verbose    |                                                              | verbose mode flag                          |
| -y, --yes        |                                                              | Do not confirm if you want to send request |
| -p, --password   |                                                              | password of keystore file                  |
| -k, --keystore   |                                                              | path of keystore file                      |
| -s, --step-limit | 0x50000000                                                   | step limit to set                          |
| --id             |                                                              | id of network-proposal to vote             |
| --vote           |                                                              | voting value(0: disagree, 1: agree)        |

**Examples**

```bash
(venv)$ preptools voteProposal -k prep_keys1 --id 0x515d0c7470e56358a6085ca93d305c4c28d004c10d110b26570dadc34bf2e492 --vote 0
> Password:
[Request] ======================================================================
{
    "from_": "hx85d62b91d70bc2390b636a8d64136a413e671e3a",
    "to": "cx0000000000000000000000000000000000000001",
    "value": 0,
    "step_limit": 268435456,
    "nid": 3,
    "nonce": null,
    "version": 3,
    "timestamp": null,
    "method": "voteProposal",
    "data_type": "call",
    "params": {
        "id": "0x515d0c7470e56358a6085ca93d305c4c28d004c10d110b26570dadc34bf2e492",
        "vote": 0
    }
}

> Continue? [Y/n]Y
request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": "0x22ca6eb228586ed2a00924f18bc57f1819214bf0d5c5d305b03d72a931360cc8",
    "id": 1234
}
```

**cancelProposal**

refer [cancelProposal request format](https://github.com/icon-project/governance#cancelproposal)

**Description**

Cancel Network-proposal

**Usage**

```bash
usage: preptools cancelProposal [-h] [--url URL] [--nid NID] [--config CONFIG]
                                [--yes] [--verbose] [--password PASSWORD]
                                [--keystore KEYSTORE]
                                [--step-limit-s STEP_LIMIT] --id [ID]

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
  --yes, -y             Don't want to ask send transaction.
  --verbose, -v         Verbose mode
  --password PASSWORD, -p PASSWORD
                        keystore password
  --keystore KEYSTORE, -k KEYSTORE
                        keystore file path
  --step-limit-s STEP_LIMIT
                        step limit to set
  --id [ID]             hash of registerProposal TX
```

**Options**

| shorthand, Name  | default                                                      | Description                                |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------ |
| -h, --help       |                                                              | show this help message and exit            |
| -u, --url        | [http://127.0.0.1:9000/api/v3](http://127.0.0.1:9000/api/v3) | node url                                   |
| -n, --nid        | 3                                                            | networkId mainnet(1), testnet(2)           |
| --config         |                                                              | configuration file path.                   |
| -y, --yes        |                                                              | Do not confirm if you want to send request |
| -v, --verbose    |                                                              | verbose mode flag                          |
| -p, --password   |                                                              | password of keystore file                  |
| -k, --keystore   |                                                              | path of keystore file                      |
| -s, --step-limit | 0x50000000                                                   | step limit to set                          |
| --id             |                                                              | id of network-proposal to cancel           |

**Examples**

```bash
(venv)$ preptools cancelProposal -k prep_keys0 --id 0x02221f9346f9c9b3322ea33e67a1ca0fbe9491e0ea3aefb5154a43e2ea829fa4
> Password:
[Request] ======================================================================
{
    "from_": "hxb74e29fba1809a105fdec433040a4e713bbe91fe",
    "to": "cx0000000000000000000000000000000000000001",
    "value": 0,
    "step_limit": 268435456,
    "nid": 3,
    "nonce": null,
    "version": 3,
    "timestamp": null,
    "method": "cancelProposal",
    "data_type": "call",
    "params": {
        "id": "0x02221f9346f9c9b3322ea33e67a1ca0fbe9491e0ea3aefb5154a43e2ea829fa4"
    }
}

> Continue? [Y/n]Y
request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": "0x344887e8b9b30e523991e44602eee51857fd7a55e5437b34a7e8d0f2ede8c019",
    "id": 1234
}
```

#### Querying Network Proposal commands

There are 2 commands to network-proposal: `getProposal` and `getProposals`. Whenever the commands are called, they load the configuration from `preptools_config.json`. In order to use other configuration file, please specify the file location with the `-c` option.

**getProposal**

refer [getProposal request format](https://github.com/icon-project/governance#getproposal)

**Description**

Querying Network-proposal with given proposal-id

**Usage**

```bash
usage: preptools getProposal [-h] [--url URL] [--nid NID] [--config CONFIG]
                             [--yes] [--verbose]
                             transaction_hash

positional arguments:
  transaction_hash      hash of registerProposal transaction

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
  --yes, -y             Don't want to ask send transaction.
  --verbose, -v         Verbose mode
```

**Options**

| shorthand, Name | default                                                      | Description                      |
| --------------- | ------------------------------------------------------------ | -------------------------------- |
| -c, --config    |                                                              | configuration file path.         |
| -h, --help      |                                                              | show this help message and exit  |
| -u, --url       | [http://127.0.0.1:9000/api/v3](http://127.0.0.1:9000/api/v3) | node url                         |
| -n, --nid       | 3                                                            | networkId mainnet(1), testnet(2) |
| -v, --verbose   |                                                              | verbose mode flag                |

**examples**

```bash
(venv)$ preptools getProposal 0x02221f9346f9c9b3322ea33e67a1ca0fbe9491e0ea3aefb5154a43e2ea829fa4
[Request] ======================================================================
{
    "from_": "hx1234567890123456789012345678901234567890",
    "to": "cx0000000000000000000000000000000000000001",
    "method": "getProposal",
    "params": {
        "id": "0x02221f9346f9c9b3322ea33e67a1ca0fbe9491e0ea3aefb5154a43e2ea829fa4"
    }
}

request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": {
        "id": "0x02221f9346f9c9b3322ea33e67a1ca0fbe9491e0ea3aefb5154a43e2ea829fa4",
        "proposer": "hxb74e29fba1809a105fdec433040a4e713bbe91fe",
        "proposerName": "nodehxb74e29fba1809a105fdec433040a4e713bbe91fe",
        "status": "0x0",
        "startBlockHeight": "0x2c",
        "endBlockHeight": "0x2f",
        "contents": {
            "title": "pro0",
            "description": "first proposal",
            "type": "0x4",
            "value": {
                "value": "1234"
            }
        },
        "vote": {
            "agree": {
                "list": [],
                "amount": "0x0"
            },
            "disagree": {
                "list": [],
                "amount": "0x0"
            },
            "noVote": {
                "list": [
                    "hxb74e29fba1809a105fdec433040a4e713bbe91fe",
                    "hx85d62b91d70bc2390b636a8d64136a413e671e3a",
                    "hxd891096dd01c1af790c29d55022a35357684643c",
                    "hx44ae89b457ccfb1bacbe35d278933ba887373b1b",
                    "hxf2c6b56e6dfcfe7c3b9fbd3d1ca1d08973b8d363",
                    "hxe54bdf4b25affa59c8963f1e4a6c45183aee167f",
                    "hx02ccb9e378a35e11a65b5c60e796fded98383b37",
                    "hx064c3d9b4982aae0253b9a8b3dd106823c107c25",
                    "hxcd294f136f39232d97081f2dfa22886c76f45afb",
                    "hxd685153db2a09347115cf0d8d1c5f9ab174bd802",
                    "hx383b555e0301b77b1c326815bccaaaf382ecd238",
                    "hxb0bfb180fb60ac68a0d3dbcadf509af07dd1f501",
                    "hxa5861173d2bd05dd9bfc5c5d74faa654f8d37c7b",
                    "hxee194a44eb4d06fb7c8a9515f74eb41735046be2",
                    "hx6e220a1b6c0fc12b2d3cc6122fccf2e9ec3d1406",
                    "hxa46f74425c0e588be8c93bbabf1be2c67da12066",
                    "hx81c5db07cd6c1c569e0a5abebdb7b108157d80b5",
                    "hx46ca63475e630e7c3a8c3f8c0e2981b675f32919",
                    "hxffa3675581c0209c2adcc598767c77d43f999a33",
                    "hx9259b69bbdea01f32e97d91401ada24d12965ae3",
                    "hx282c3778a572d4d0d1eb8e65ab53daaedea3f68e",
                    "hx25b84c8fe8bfabda4fb30523a1923a79cc304af5"
                ],
                "amount": "0x10658da4dff32a862400000"
            }
        }
    },
    "id": 1234
}
```

**getProposals**

refer [getProposals request format](https://github.com/icon-project/governance#getproposals)

**Description**

Querying all Network-proposal

**Usage**

```bash
usage: preptools getProposals [-h] [--url URL] [--nid NID] [--config CONFIG]
                              [--yes] [--verbose] [--type [TYPE]]
                              [--status [STATUS]]

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
  --yes, -y             Don't want to ask send transaction.
  --verbose, -v         Verbose mode
  --type [TYPE]         type of network proposal to filter
  --status [STATUS]     status of network proposal to filter
```

**Options**

| shorthand, Name | default                                                      | Description                          |
| --------------- | ------------------------------------------------------------ | ------------------------------------ |
| -c, --config    |                                                              | configuration file path.             |
| -h, --help      |                                                              | show this help message and exit      |
| -u, --url       | [http://127.0.0.1:9000/api/v3](http://127.0.0.1:9000/api/v3) | node url                             |
| -n, --nid       | 3                                                            | networkId mainnet(1), testnet(2)     |
| -v, --verbose   |                                                              | verbose mode flag                    |
| --type          |                                                              | Type of network proposal to filter   |
| --status        |                                                              | Status of network proposal to filter |

**examples**

```bash
(venv)$ preptools getProposals
[Request] ======================================================================
{
    "from_": "hx1234567890123456789012345678901234567890",
    "to": "cx0000000000000000000000000000000000000001",
    "method": "getProposals",
    "params": null
}

request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": {
        "proposals": [
            {
                "id": "0x02221f9346f9c9b3322ea33e67a1ca0fbe9491e0ea3aefb5154a43e2ea829fa4",
                "proposer": "hxb74e29fba1809a105fdec433040a4e713bbe91fe",
                "proposerName": "nodehxb74e29fba1809a105fdec433040a4e713bbe91fe",
                "status": "0x0",
                "startBlockHeight": "0x2c",
                "endBlockHeight": "0x2f",
                "contents": {
                    "title": "pro0",
                    "description": "first proposal",
                    "type": "0x4",
                    "value": {
                        "value": "1234"
                    }
                },
                "vote": {
                    "agree": {
                        "count": "0x0",
                        "amount": "0x0"
                    },
                    "disagree": {
                        "count": "0x0",
                        "amount": "0x0"
                    },
                    "noVote": {
                        "count": "0x16",
                        "amount": "0x10658da4dff32a862400000"
                    }
                }
            },
            {
                "id": "0x515d0c7470e56358a6085ca93d305c4c28d004c10d110b26570dadc34bf2e492",
                "proposer": "hxb74e29fba1809a105fdec433040a4e713bbe91fe",
                "proposerName": "nodehxb74e29fba1809a105fdec433040a4e713bbe91fe",
                "status": "0x0",
                "startBlockHeight": "0x2c",
                "endBlockHeight": "0x2f",
                "contents": {
                    "title": "pro1",
                    "description": "second proposal",
                    "type": "0x4",
                    "value": {
                        "value": "1234"
                    }
                },
                "vote": {
                    "agree": {
                        "count": "0x0",
                        "amount": "0x0"
                    },
                    "disagree": {
                        "count": "0x1",
                        "amount": "0xbecc41ad16b07a7600000"
                    },
                    "noVote": {
                        "count": "0x15",
                        "amount": "0xfa6c16332dc7a0bae00000"
                    }
                }
            }
        ]
    },
    "id": 1234
}
```

#### Preptools Common commands

Commands that generate configuration file and keystore file. There are two commands `keystore` and `genconf`.

**keystore**

**Description**

Create a keystore file in the given path.

**Usage**

```bash
usage: preptools keystore [-h] [-p PASSWORD] path

positional arguments:
  path                  Path of keystore file.

optional arguments:
  -h, --help            show this help message and exit
  -p PASSWORD, --password PASSWORD
                        Keystore file's password
```

**Options**

| shorthand, Name | default | Description                                  |
| --------------- | ------- | -------------------------------------------- |
| path            |         | a keystore file path that is to be generated |
| -h, --help      |         | show this help message and exit              |
| -p, --password  |         | Keystore file's password                     |

**Examples**

```bash
(venv) $ preptools keystore keystore_file
Input your keystore password:
Retype your keystore password:
Made file successfully
```

**genconf**

**Description**

Generate P-Rep tools config file.

```bash
usage: preptools genconf [-h] [--path PATH]

optional arguments:
  -h, --help   show this help message and exit
  --path PATH  Path of configue file. default(./preptools_config.json)
```

**Options**

| shorthand, Name | default               | Description                     |
| --------------- | --------------------- | ------------------------------- |
| -h, --help      |                       | show this help message and exit |
| --path          | preptools_config.json | Path of configue file.          |

**Examples**

```bash
(venv) $ preptools genconf
Made ./preptools_config.json successfully
```

#### Preptools Other commands

Commands that are related to transaction. There are two commands `txresult` and `txbyhash`.

**txresult**

**Description**

Get transaction result by transaction hash.

**Usage**

```bash
usage: preptools txresult [-h] [--url URL] [--nid NID] [--config CONFIG]
                          [tx_hash]

positional arguments:
  tx_hash               Enter the transaction hash

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
```

**Options**

| shorthand, Name | default                                                      | Description                           |
| --------------- | ------------------------------------------------------------ | ------------------------------------- |
| tx_hash         |                                                              | Hash of the transaction to be queried |
| -h, --help      |                                                              | show this help message and exit       |
| -u, --url       | [http://127.0.0.1:9000/api/v3](http://127.0.0.1:9000/api/v3) | node url                              |
| -n, --nid       | 3                                                            | network id                            |
| -c, --config    | ./preptools_config.json                                      | preptools config file path            |

**Examples**

```bash
(venv) $ preptools txresult 0xc8456053128897a0941dab4c79428db91dda5a2899e3813698146ac25808c4c9
request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": {
        "txHash": "0xc8456053128897a0941dab4c79428db91dda5a2899e3813698146ac25808c4c9",
        "blockHeight": "0x1d980",
        "blockHash": "0xa7354fb9427308239e56482916dd4e31988ce1f207091cdf81c656f89a066c5f",
        "txIndex": "0x0",
        "to": "cx0000000000000000000000000000000000000000",
        "stepUsed": "0x21340",
        "stepPrice": "0x2540be400",
        "cumulativeStepUsed": "0x21340",
        "eventLogs": [
            {
                "scoreAddress": "cx0000000000000000000000000000000000000000",
                "indexed": [
                    "PRepSet(Address)"
                ],
                "data": [
                    "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6"
                ]
            }
        ],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000000000000000000080000000000000000000000000000000000000000000000000000000000020000000000000008000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1"
    },
    "id": 1234
}
```

**txbyhash**

**Description**

Get transaction by transaction hash

**Usage**

```bash
usage: preptools txbyhash [-h] [--url URL] [--nid NID] [--config CONFIG]
                          [tx_hash]

positional arguments:
  tx_hash               Enter the transaction hash

optional arguments:
  -h, --help            show this help message and exit
  --url URL, -u URL     node url default(http://127.0.0.1:9000/api/v3)
  --nid NID, -n NID     networkId default(3) ex) mainnet(1), testnet(2)
  --config CONFIG, -c CONFIG
                        preptools config file path
```

**Options**

| shorthand, Name | default                                                      | Description                           |
| --------------- | ------------------------------------------------------------ | ------------------------------------- |
| tx_hash         |                                                              | Hash of the transaction to be queried |
| -h, --help      |                                                              | show this help message and exit       |
| -u, --url       | [http://127.0.0.1:9000/api/v3](http://127.0.0.1:9000/api/v3) | node url                              |
| -n, --nid       | 3                                                            | network id                            |
| -c, --config    | ./preptools_config.json                                      | preptools config file path            |

**Examples**

```bash
(venv) $ preptools txbyhash 0xc8456053128897a0941dab4c79428db91dda5a2899e3813698146ac25808c4c9
request success.
[Response] =====================================================================
{
    "jsonrpc": "2.0",
    "result": {
        "version": "0x3",
        "from": "hxef73db5d0ad02eb1fadb37d0041be96bfa56d4e6",
        "to": "cx0000000000000000000000000000000000000000",
        "stepLimit": "0x10000000",
        "timestamp": "0x58fa97a48ad43",
        "nid": "0x3",
        "value": "0x0",
        "dataType": "call",
        "data": {
            "method": "setPRep",
            "params": {
                "name": "kokoa node",
                "country": "USA",
                "website": "https://icon.kokoa.com",
                "details": "https://icon.kokoa.com/json",
                "p2pEndpoint": "node.example.com:7100"
            }
        },
        "signature": "4krblW9KtQr6KNIJOVa22B3JFDQD6vaxepDSjMET91oua3Qeiq3UFMIHiucWiIrKGt3zaSo2K+mVW7Ge5rOtPwE=",
        "txHash": "0xc8456053128897a0941dab4c79428db91dda5a2899e3813698146ac25808c4c9",
        "txIndex": "0x0",
        "blockHeight": "0x1d980",
        "blockHash": "0xa7354fb9427308239e56482916dd4e31988ce1f207091cdf81c656f89a066c5f"
    },
    "id": 1234
}
```

#### Configuration Files

**preptools_config.json**

For every P-Rep tools CLI commands except `genconf` and `keystore`, this file is used to configure the default parameters and initial settings.

In this configuration file, you can define default options values for some CLI commands.

```javascript
{
    "uri": "http://127.0.0.1:9000/api/v3",
    "nid": 3,
    "keyStore": null
}
```

| Field    | Data  type | Description                                |
| -------- | ---------- | ------------------------------------------ |
| uri      | string     | URI to send the request.                   |
| nid      | int        | Network ID. 3 is reserved for P-Rep tools. |
| keyStore | string     | Keystore file path.                        |

### JSON Standard for Public Representative Detailed Information

This is the JSON standard for detailed information about the P-Rep. P-Rep can submit the url of detailed information via the `registerPRep` and `setPRep` action on the ICON Blockchain. We strongly recommend that you register this information.

```javascript
{
    "representative": {
        "logo": {
            "logo_256": "https://icon.foundation/img/img-256.png",
            "logo_1024": "https://icon.foundation/img/img-1024.png",
            "logo_svg": "https://icon.foundation/img/img-logo.svg"
        },
        "media": {
            "steemit": "",
            "twitter": "",
            "youtube": "",
            "facebook": "",
            "github": "",
            "reddit": "",
            "keybase": "",
            "telegram": "",
            "wechat": ""
        }
    },
    "server": {
        "location": {
            "country": "",
            "city": ""
        },
        "server_type": "",
        "api_endpoint": ""
    }
}
```

* representative: Basic information of Public Representative
  * logo: Logo images of P-Rep
    * logo\_256: image 256x256px
    * logo\_1024: image 1024x1024px
    * logo_sgv: image svg
  * media: URL and username of social media
    * steemit: Steemit URL
    * twitter: Twitter URL
    * youtube: Youtube URL
    * Facebook: Facebook URL
    * github: Github URL
    * reddit: Raddit URL
    * keybase: Username
    * telegram: Username
    * wechat: Username
* server: Server information of Public Representative
  * location: Server location
    * name: Node location in human readable format \[City, State]
    * country: Node country code \[XX]
  * server_type: Type of server ‘cloud, on-premise, hybrid’
  * api_endpoint: HTTP endpoint [http://host:port](http://host/:port)

#### How to use

Create a JSON file and upload it to your domain server. When you call the `registerPRep` or `setPRep`function, input the url of this file into the `details` field.

### References

* [ICON JSON-RPC API v3](../references/reference-manuals/icon-json-rpc-api-v3-specification.md)
* [ICON SDK PYTHON](../icon-sdks/python-sdk/)

### License

This project follows the Apache 2.0 License. Please refer to [LICENSE](https://www.apache.org/licenses/LICENSE-2.0) for details.
