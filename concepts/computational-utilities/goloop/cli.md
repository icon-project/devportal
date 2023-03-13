# CLI

## CLI

### goloop

#### Description

Goloop CLI

#### Usage

`goloop`

#### Child commands

| Command                                                                             | Description                                        |
| ----------------------------------------------------------------------------------- | -------------------------------------------------- |
| [goloop chain](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain)     | Manage chains                                      |
| [goloop debug](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-debug)     | DEBUG API                                          |
| [goloop gn](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-gn)           | Genesis transaction manipulation                   |
| [goloop gs](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-gs)           | Genesis storage manipulation                       |
| [goloop ks](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-ks)           | Keystore manipulation                              |
| [goloop rpc](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc)         | JSON-RPC API                                       |
| [goloop server](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-server)   | Server management                                  |
| [goloop stats](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-stats)     | Display a live streams of chains metric-statistics |
| [goloop system](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-system)   | System info                                        |
| [goloop user](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-user)       | User management                                    |
| [goloop version](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-version) | Print goloop version                               |

### goloop chain

#### Description

Manage chains

#### Usage

`goloop chain TASK CID PARAM`

#### Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

#### Child commands

| Command                                                                                         | Description                                     |
| ----------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| [goloop chain backup](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-backup)   | Start to backup the channel                     |
| [goloop chain config](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-config)   | Configure chain                                 |
| [goloop chain genesis](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-genesis) | Download chain genesis file                     |
| [goloop chain import](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-import)   | Start to import legacy database                 |
| [goloop chain inspect](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-inspect) | Inspect chain                                   |
| [goloop chain join](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-join)       | Join chain                                      |
| [goloop chain leave](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-leave)     | Leave chain                                     |
| [goloop chain ls](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-ls)           | List chains                                     |
| [goloop chain prune](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-prune)     | Start to prune the database based on the height |
| [goloop chain reset](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-reset)     | Chain data reset                                |
| [goloop chain start](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-start)     | Chain start                                     |
| [goloop chain stop](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-stop)       | Chain stop                                      |
| [goloop chain verify](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-chain-verify)   | Chain data verify                               |

### goloop chain backup

#### Description

Start to backup the channel

#### Usage

`goloop chain backup CID`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop chain config

#### Description

Configure chain

#### Usage

`goloop chain config CID KEY VALUE`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop chain genesis

#### Description

Download chain genesis file

#### Usage

`goloop chain genesis CID FILE`

#### Inherited Options

### goloop chain import

#### Description

Start to import legacy database

#### Usage

`goloop chain import CID [flags]`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description   |
| -------------- | -------------------- | -------- | ------- | ------------- |
| --db\_path     |                      | true     |         | Database path |
| --height       |                      | true     | 0       | Block Height  |

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop chain inspect

#### Description

Inspect chain

#### Usage

`goloop chain inspect CID [flags]`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description                                   |
| -------------- | -------------------- | -------- | ------- | --------------------------------------------- |
| --format, -f   |                      | false    |         | Format the output using the given Go template |
| --informal     |                      | false    | false   | Inspect with informal data                    |

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop chain join

#### Description

Join chain

#### Usage

`goloop chain join [flags]`

#### Options

| Name,shorthand           | Environment Variable | Required | Default              | Description                                                                      |
| ------------------------ | -------------------- | -------- | -------------------- | -------------------------------------------------------------------------------- |
| --auto\_start            |                      | false    | false                | Auto start                                                                       |
| --channel                |                      | false    |                      | Channel                                                                          |
| --concurrency            |                      | false    | 1                    | Maximum number of executors to be used for concurrency                           |
| --db\_type               |                      | false    | goleveldb            | Name of database system(\*badgerdb, goleveldb, boltdb, mapdb)                    |
| --default\_wait\_timeout |                      | false    | 0                    | Default wait timeout in milli-second (0: disable)                                |
| --genesis                |                      | false    |                      | Genesis storage path                                                             |
| --genesis\_template      |                      | false    |                      | Genesis template directory or file                                               |
| --max\_block\_tx\_bytes  |                      | false    | 0                    | Max size of transactions in a block                                              |
| --max\_wait\_timeout     |                      | false    | 0                    | Max wait timeout in milli-second (0: uses same value of default\_wait\_timeout)  |
| --node\_cache            |                      | false    | none                 | Node cache (none,small,large)                                                    |
| --normal\_tx\_pool       |                      | false    | 0                    | Size of normal transaction pool                                                  |
| --patch\_tx\_pool        |                      | false    | 0                    | Size of patch transaction pool                                                   |
| --platform               |                      | false    |                      | Name of service platform                                                         |
| --role                   |                      | false    | 3                    | \[0:None, 1:Seed, 2:Validator, 3:Both]                                           |
| --secure\_aeads          |                      | false    | chacha,aes128,aes256 | Supported Secure AEAD with order (chacha,aes128,aes256) - Comma separated string |
| --secure\_suites         |                      | false    | none,tls,ecdhe       | Supported Secure suites with order (none,tls,ecdhe) - Comma separated string     |
| --seed                   |                      | false    |                      | List of trust-seed ip-port, Comma separated string                               |
| --tx\_timeout            |                      | false    | 0                    | Transaction timeout in milli-second (0: uses system default value)               |

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop chain leave

#### Description

Leave chain

#### Usage

`goloop chain leave CID`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop chain ls

#### Description

List chains

#### Usage

`goloop chain ls`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop chain prune

#### Description

Start to prune the database based on the height

#### Usage

`goloop chain prune CID [flags]`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description                                   |
| -------------- | -------------------- | -------- | ------- | --------------------------------------------- |
| --db\_type     |                      | false    |         | Database type(default:original database type) |
| --height       |                      | true     | 0       | Block Height                                  |

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop chain reset

#### Description

Chain data reset

#### Usage

`goloop chain reset CID`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop chain start

#### Description

Chain start

#### Usage

`goloop chain start CID`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop chain stop

#### Description

Chain stop

#### Usage

`goloop chain stop CID`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop chain verify

#### Description

Chain data verify

#### Usage

`goloop chain verify CID`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop debug

#### Description

DEBUG API

#### Usage

`goloop debug`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description      |
| -------------- | -------------------- | -------- | ------- | ---------------- |
| --uri          | GOLOOP\_DEBUG\_URI   | true     |         | URI of DEBUG API |

#### Child commands

| Command                                                                                     | Description                  |
| ------------------------------------------------------------------------------------------- | ---------------------------- |
| [goloop debug trace](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-debug-trace) | Get trace of the transaction |

### goloop debug trace

#### Description

Get trace of the transaction

#### Usage

`goloop debug trace HASH`

#### Inherited Options

| Name,shorthand | Environment Variable | Required | Default | Description      |
| -------------- | -------------------- | -------- | ------- | ---------------- |
| --uri          | GOLOOP\_DEBUG\_URI   | true     |         | URI of DEBUG API |

### goloop gn

#### Description

Genesis transaction manipulation

#### Usage

`goloop gn`

#### Child commands

| Command                                                                             | Description                  |
| ----------------------------------------------------------------------------------- | ---------------------------- |
| [goloop gn edit](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-gn-edit) | Edit genesis transaction     |
| [goloop gn gen](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-gn-gen)   | Generate genesis transaction |

### goloop gn edit

#### Description

Edit genesis transaction

#### Usage

`goloop gn edit [genesis file]`

#### Options

| Name,shorthand  | Environment Variable | Required | Default | Description                                       |
| --------------- | -------------------- | -------- | ------- | ------------------------------------------------- |
| --god, -g       |                      | false    |         | Address or keystore of GOD                        |
| --validator, -v |                      | false    | \[]     | Address or keystore of Validator, \[Validator...] |

### goloop gn gen

#### Description

Generate genesis transaction

#### Usage

`goloop gn gen [address or keystore...]`

#### Options

| Name,shorthand | Environment Variable | Required | Default                                    | Description                   |
| -------------- | -------------------- | -------- | ------------------------------------------ | ----------------------------- |
| --config, -c   |                      | false    | \[]                                        | Chain configuration           |
| --fee          |                      | false    | none                                       | Fee configuration (none,icon) |
| --god, -g      |                      | false    |                                            | Address or keystore of GOD    |
| --out, -o      |                      | false    | genesis.json                               | Output file path              |
| --supply, -s   |                      | false    | 0x2961fff8ca4a62327800000                  | Total supply of the chain     |
| --treasury, -t |                      | false    | hx1000000000000000000000000000000000000000 | Treasury address              |

### goloop gs

#### Description

Genesis storage manipulation

#### Usage

`goloop gs`

#### Child commands

| Command                                                                             | Description                              |
| ----------------------------------------------------------------------------------- | ---------------------------------------- |
| [goloop gs gen](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-gs-gen)   | Create genesis storage from the template |
| [goloop gs info](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-gs-info) | Show genesis storage information         |

### goloop gs gen

#### Description

Create genesis storage from the template

#### Usage

`goloop gs gen`

#### Options

| Name,shorthand | Environment Variable | Required | Default      | Description                  |
| -------------- | -------------------- | -------- | ------------ | ---------------------------- |
| --input, -i    |                      | false    | genesis.json | Input file or directory path |
| --out, -o      |                      | false    | gs.zip       | Output file path             |

### goloop gs info

#### Description

Show genesis storage information

#### Usage

`goloop gs info genesis_storage.zip [flags]`

#### Options

| Name,shorthand  | Environment Variable | Required | Default | Description             |
| --------------- | -------------------- | -------- | ------- | ----------------------- |
| --cid\_only, -c |                      | false    | false   | Showing chain ID only   |
| --nid\_only, -n |                      | false    | false   | Showing network ID only |

### goloop ks

#### Description

Keystore manipulation

#### Usage

`goloop ks`

#### Child commands

| Command                                                                           | Description       |
| --------------------------------------------------------------------------------- | ----------------- |
| [goloop ks gen](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-ks-gen) | Generate keystore |

### goloop ks gen

#### Description

Generate keystore

#### Usage

`goloop ks gen`

#### Options

| Name,shorthand | Environment Variable | Required | Default       | Description               |
| -------------- | -------------------- | -------- | ------------- | ------------------------- |
| --out, -o      |                      | false    | keystore.json | Output file path          |
| --password, -p |                      | false    | gochain       | Password for the keystore |

### goloop rpc

#### Description

JSON-RPC AP

#### Usage

`goloop rpc`

#### Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

#### Child commands

| Command                                                                                                             | Description            |
| ------------------------------------------------------------------------------------------------------------------- | ---------------------- |
| [goloop rpc balance](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-balance)                         | GetBalance             |
| [goloop rpc blockbyhash](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-blockbyhash)                 | GetBlockByHash         |
| [goloop rpc blockbyheight](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-blockbyheight)             | GetBlockByHeight       |
| [goloop rpc blockheaderbyheight](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-blockheaderbyheight) | GetBlockHeaderByHeight |
| [goloop rpc call](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-call)                               | Call                   |
| [goloop rpc databyhash](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-databyhash)                   | GetDataByHash          |
| [goloop rpc lastblock](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-lastblock)                     | GetLastBlock           |
| [goloop rpc monitor](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-monitor)                         | Monitor                |
| [goloop rpc proofforevents](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-proofforevents)           | GetProofForEvents      |
| [goloop rpc proofforresult](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-proofforresult)           | GetProofForResult      |
| [goloop rpc raw](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-raw)                                 | Rpc with raw json file |
| [goloop rpc scoreapi](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-scoreapi)                       | GetScoreApi            |
| [goloop rpc sendtx](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-sendtx)                           | SendTransaction        |
| [goloop rpc totalsupply](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-totalsupply)                 | GetTotalSupply         |
| [goloop rpc txbyhash](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-txbyhash)                       | GetTransactionByHash   |
| [goloop rpc txresult](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-txresult)                       | GetTransactionResult   |
| [goloop rpc votesbyheight](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-votesbyheight)             | GetVotesByHeight       |

### goloop rpc balance

#### Description

GetBalance

#### Usage

`goloop rpc balance ADDRESS`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc blockbyhash

#### Description

GetBlockByHash

#### Usage

`goloop rpc blockbyhash HASH`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc blockbyheight

#### Description

GetBlockByHeight

#### Usage

`goloop rpc blockbyheight HEIGHT`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc blockheaderbyheight

#### Description

GetBlockHeaderByHeight

#### Usage

`goloop rpc blockheaderbyheight HEIGHT`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc call

#### Description

Call

#### Usage

`goloop rpc call [flags]`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description                                                              |
| -------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------ |
| --from         |                      | false    |         | FromAddress                                                              |
| --method       |                      | false    |         | Name of the function to invoke in SCORE, if '--raw' used, will overwrite |
| --param        |                      | false    | \[]     | key=value, Function parameters, if '--raw' used, will overwrite          |
| --raw          |                      | false    |         | call with 'data' using raw json file or json-string                      |
| --to           |                      | true     |         | ToAddress                                                                |

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc databyhash

#### Description

GetDataByHash

#### Usage

`goloop rpc databyhash HASH`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc lastblock

#### Description

GetLastBlock

#### Usage

`goloop rpc lastblock`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc monitor

#### Description

Monitor

#### Usage

`goloop rpc monitor`

#### Options

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

#### Child commands

| Command                                                                                                 | Description  |
| ------------------------------------------------------------------------------------------------------- | ------------ |
| [goloop rpc monitor block](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-monitor-block) | MonitorBlock |
| [goloop rpc monitor event](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-monitor-event) | MonitorEvent |

### goloop rpc monitor block

#### Description

MonitorBlock

#### Usage

`goloop rpc monitor block HEIGHT [flags]`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description                              |
| -------------- | -------------------- | -------- | ------- | ---------------------------------------- |
| --filter       |                      | false    | \[]     | EventFilter raw json file or json string |

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc monitor event

#### Description

MonitorEvent

#### Usage

`goloop rpc monitor event HEIGHT [flags]`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description                                            |
| -------------- | -------------------- | -------- | ------- | ------------------------------------------------------ |
| --addr         |                      | false    |         | SCORE Address                                          |
| --data         |                      | false    | \[]     | Not indexed Arguments of Event, comma-separated string |
| --event        |                      | false    |         | Signature of Event                                     |
| --indexed      |                      | false    | \[]     | Indexed Arguments of Event, comma-separated string     |
| --raw          |                      | false    |         | EventFilter raw json file or json-string               |

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc proofforevents

#### Description

GetProofForEvents

#### Usage

`goloop rpc proofforevents BLOCK_HASH TX_INDEX EVENT_INDEXES`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc proofforresult

#### Description

GetProofForResult

#### Usage

`goloop rpc proofforresult HASH INDEX`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc raw

#### Description

Rpc with raw json file

#### Usage

`goloop rpc raw FILE`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc scoreapi

#### Description

GetScoreApi

#### Usage

`goloop rpc scoreapi ADDRESS`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc sendtx

#### Description

SendTransaction

#### Usage

`goloop rpc sendtx`

#### Options

| Name,shorthand  | Environment Variable       | Required | Default | Description                        |
| --------------- | -------------------------- | -------- | ------- | ---------------------------------- |
| --estimate      | GOLOOP\_RPC\_ESTIMATE      | false    | false   | Just estimate steps for the tx     |
| --key\_password | GOLOOP\_RPC\_KEY\_PASSWORD | false    |         | Password for the KeyStore file     |
| --key\_secret   | GOLOOP\_RPC\_KEY\_SECRET   | false    |         | Secret(password) file for KeyStore |
| --key\_store    | GOLOOP\_RPC\_KEY\_STORE    | true     |         | KeyStore file for wallet           |
| --nid           | GOLOOP\_RPC\_NID           | true     |         | Network ID                         |
| --step\_limit   | GOLOOP\_RPC\_STEP\_LIMIT   | true     | 0       | StepLimit                          |

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

#### Child commands

| Command                                                                                                     | Description                                           |
| ----------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| [goloop rpc sendtx call](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-sendtx-call)         | SmartContract Call Transaction                        |
| [goloop rpc sendtx deploy](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-sendtx-deploy)     | Deploy Transaction                                    |
| [goloop rpc sendtx raw](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-sendtx-raw)           | Send transaction with json file                       |
| [goloop rpc sendtx raw2](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-sendtx-raw2)         | Send transaction with json file just timestamp & sign |
| [goloop rpc sendtx transfer](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-rpc-sendtx-transfer) | Coin Transfer Transaction                             |

### goloop rpc sendtx call

#### Description

SmartContract Call Transaction

#### Usage

`goloop rpc sendtx call [flags]`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description                                                              |
| -------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------ |
| --method       |                      | true     |         | Name of the function to invoke in SCORE, if '--raw' used, will overwrite |
| --param        |                      | false    | \[]     | key=value, Function parameters, if '--raw' used, will overwrite          |
| --raw          |                      | false    |         | call with 'data' using raw json file or json-string                      |
| --to           |                      | true     |         | ToAddress                                                                |
| --value        |                      | false    |         | Value of transfer                                                        |

#### Inherited Options

| Name,shorthand  | Environment Variable       | Required | Default | Description                               |
| --------------- | -------------------------- | -------- | ------- | ----------------------------------------- |
| --debug         | GOLOOP\_RPC\_DEBUG         | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri    | GOLOOP\_RPC\_DEBUG\_URI    | false    |         | URI of JSON-RPC Debug API                 |
| --estimate      | GOLOOP\_RPC\_ESTIMATE      | false    | false   | Just estimate steps for the tx            |
| --key\_password | GOLOOP\_RPC\_KEY\_PASSWORD | false    |         | Password for the KeyStore file            |
| --key\_secret   | GOLOOP\_RPC\_KEY\_SECRET   | false    |         | Secret(password) file for KeyStore        |
| --key\_store    | GOLOOP\_RPC\_KEY\_STORE    | true     |         | KeyStore file for wallet                  |
| --nid           | GOLOOP\_RPC\_NID           | true     |         | Network ID                                |
| --step\_limit   | GOLOOP\_RPC\_STEP\_LIMIT   | true     | 0       | StepLimit                                 |
| --uri           | GOLOOP\_RPC\_URI           | true     |         | URI of JSON-RPC API                       |

### goloop rpc sendtx deploy

#### Description

Deploy Transaction

#### Usage

`goloop rpc sendtx deploy SCORE_ZIP_FILE [flags]`

#### Options

| Name,shorthand  | Environment Variable | Required | Default                                    | Description                                                                       |
| --------------- | -------------------- | -------- | ------------------------------------------ | --------------------------------------------------------------------------------- |
| --content\_type |                      | false    | application/zip                            | Mime-type of the content                                                          |
| --param         |                      | false    | \[]                                        | key=value, Function parameters will be delivered to on\_install() or on\_update() |
| --to            |                      | false    | cx0000000000000000000000000000000000000000 | ToAddress                                                                         |

#### Inherited Options

| Name,shorthand  | Environment Variable       | Required | Default | Description                               |
| --------------- | -------------------------- | -------- | ------- | ----------------------------------------- |
| --debug         | GOLOOP\_RPC\_DEBUG         | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri    | GOLOOP\_RPC\_DEBUG\_URI    | false    |         | URI of JSON-RPC Debug API                 |
| --estimate      | GOLOOP\_RPC\_ESTIMATE      | false    | false   | Just estimate steps for the tx            |
| --key\_password | GOLOOP\_RPC\_KEY\_PASSWORD | false    |         | Password for the KeyStore file            |
| --key\_secret   | GOLOOP\_RPC\_KEY\_SECRET   | false    |         | Secret(password) file for KeyStore        |
| --key\_store    | GOLOOP\_RPC\_KEY\_STORE    | true     |         | KeyStore file for wallet                  |
| --nid           | GOLOOP\_RPC\_NID           | true     |         | Network ID                                |
| --step\_limit   | GOLOOP\_RPC\_STEP\_LIMIT   | true     | 0       | StepLimit                                 |
| --uri           | GOLOOP\_RPC\_URI           | true     |         | URI of JSON-RPC API                       |

### goloop rpc sendtx raw

#### Description

Send transaction with json file

#### Usage

`goloop rpc sendtx raw FILE`

#### Inherited Options

| Name,shorthand  | Environment Variable       | Required | Default | Description                               |
| --------------- | -------------------------- | -------- | ------- | ----------------------------------------- |
| --debug         | GOLOOP\_RPC\_DEBUG         | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri    | GOLOOP\_RPC\_DEBUG\_URI    | false    |         | URI of JSON-RPC Debug API                 |
| --estimate      | GOLOOP\_RPC\_ESTIMATE      | false    | false   | Just estimate steps for the tx            |
| --key\_password | GOLOOP\_RPC\_KEY\_PASSWORD | false    |         | Password for the KeyStore file            |
| --key\_secret   | GOLOOP\_RPC\_KEY\_SECRET   | false    |         | Secret(password) file for KeyStore        |
| --key\_store    | GOLOOP\_RPC\_KEY\_STORE    | true     |         | KeyStore file for wallet                  |
| --nid           | GOLOOP\_RPC\_NID           | true     |         | Network ID                                |
| --step\_limit   | GOLOOP\_RPC\_STEP\_LIMIT   | true     | 0       | StepLimit                                 |
| --uri           | GOLOOP\_RPC\_URI           | true     |         | URI of JSON-RPC API                       |

### goloop rpc sendtx raw2

#### Description

Send transaction with json file just timestamp & sign

#### Usage

`goloop rpc sendtx raw2 FILE`

#### Inherited Options

| Name,shorthand  | Environment Variable       | Required | Default | Description                               |
| --------------- | -------------------------- | -------- | ------- | ----------------------------------------- |
| --debug         | GOLOOP\_RPC\_DEBUG         | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri    | GOLOOP\_RPC\_DEBUG\_URI    | false    |         | URI of JSON-RPC Debug API                 |
| --estimate      | GOLOOP\_RPC\_ESTIMATE      | false    | false   | Just estimate steps for the tx            |
| --key\_password | GOLOOP\_RPC\_KEY\_PASSWORD | false    |         | Password for the KeyStore file            |
| --key\_secret   | GOLOOP\_RPC\_KEY\_SECRET   | false    |         | Secret(password) file for KeyStore        |
| --key\_store    | GOLOOP\_RPC\_KEY\_STORE    | true     |         | KeyStore file for wallet                  |
| --nid           | GOLOOP\_RPC\_NID           | true     |         | Network ID                                |
| --step\_limit   | GOLOOP\_RPC\_STEP\_LIMIT   | true     | 0       | StepLimit                                 |
| --uri           | GOLOOP\_RPC\_URI           | true     |         | URI of JSON-RPC API                       |

### goloop rpc sendtx transfer

#### Description

Coin Transfer Transaction

#### Usage

`goloop rpc sendtx transfer [flags]`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description |
| -------------- | -------------------- | -------- | ------- | ----------- |
| --message      |                      | false    |         | Message     |
| --to           |                      | true     |         | ToAddress   |
| --value        |                      | true     |         | Value       |

#### Inherited Options

| Name,shorthand  | Environment Variable       | Required | Default | Description                               |
| --------------- | -------------------------- | -------- | ------- | ----------------------------------------- |
| --debug         | GOLOOP\_RPC\_DEBUG         | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri    | GOLOOP\_RPC\_DEBUG\_URI    | false    |         | URI of JSON-RPC Debug API                 |
| --estimate      | GOLOOP\_RPC\_ESTIMATE      | false    | false   | Just estimate steps for the tx            |
| --key\_password | GOLOOP\_RPC\_KEY\_PASSWORD | false    |         | Password for the KeyStore file            |
| --key\_secret   | GOLOOP\_RPC\_KEY\_SECRET   | false    |         | Secret(password) file for KeyStore        |
| --key\_store    | GOLOOP\_RPC\_KEY\_STORE    | true     |         | KeyStore file for wallet                  |
| --nid           | GOLOOP\_RPC\_NID           | true     |         | Network ID                                |
| --step\_limit   | GOLOOP\_RPC\_STEP\_LIMIT   | true     | 0       | StepLimit                                 |
| --uri           | GOLOOP\_RPC\_URI           | true     |         | URI of JSON-RPC API                       |

### goloop rpc totalsupply

#### Description

GetTotalSupply

#### Usage

`goloop rpc totalsupply`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc txbyhash

#### Description

GetTransactionByHash

#### Usage

`goloop rpc txbyhash HASH`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc txresult

#### Description

GetTransactionResult

#### Usage

`goloop rpc txresult HASH`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop rpc votesbyheight

#### Description

GetVotesByHeight

#### Usage

`goloop rpc votesbyheight HEIGHT`

#### Inherited Options

| Name,shorthand | Environment Variable    | Required | Default | Description                               |
| -------------- | ----------------------- | -------- | ------- | ----------------------------------------- |
| --debug        | GOLOOP\_RPC\_DEBUG      | false    | false   | JSON-RPC Response with detail information |
| --debug\_uri   | GOLOOP\_RPC\_DEBUG\_URI | false    |         | URI of JSON-RPC Debug API                 |
| --uri          | GOLOOP\_RPC\_URI        | true     |         | URI of JSON-RPC API                       |

### goloop server

#### Description

Server management

#### Usage

`goloop server`

#### Options

| Name,shorthand            | Environment Variable            | Required | Default        | Description                                                                 |
| ------------------------- | ------------------------------- | -------- | -------------- | --------------------------------------------------------------------------- |
| --backup\_dir             | GOLOOP\_BACKUP\_DIR             | false    |                | Node backup directory (default: \[node\_dir]/backup                         |
| --config, -c              | GOLOOP\_CONFIG                  | false    |                | Parsing configuration file                                                  |
| --console\_level          | GOLOOP\_CONSOLE\_LEVEL          | false    | trace          | Console log level (trace,debug,info,warn,error,fatal,panic)                 |
| --ee\_socket              | GOLOOP\_EE\_SOCKET              | false    |                | Execution engine socket path                                                |
| --engines                 | GOLOOP\_ENGINES                 | false    | python         | Execution engines, comma-separated (python,java)                            |
| --key\_password           | GOLOOP\_KEY\_PASSWORD           | false    |                | Password for the KeyStore file                                              |
| --key\_plugin             | GOLOOP\_KEY\_PLUGIN             | false    |                | KeyPlugin file for wallet                                                   |
| --key\_plugin\_options    | GOLOOP\_KEY\_PLUGIN\_OPTIONS    | false    | \[]            | KeyPlugin options                                                           |
| --key\_secret             | GOLOOP\_KEY\_SECRET             | false    |                | Secret (password) file for KeyStore                                         |
| --key\_store              | GOLOOP\_KEY\_STORE              | false    |                | KeyStore file for wallet                                                    |
| --log\_forwarder\_address | GOLOOP\_LOG\_FORWARDER\_ADDRESS | false    |                | LogForwarder address                                                        |
| --log\_forwarder\_level   | GOLOOP\_LOG\_FORWARDER\_LEVEL   | false    | info           | LogForwarder level                                                          |
| --log\_forwarder\_name    | GOLOOP\_LOG\_FORWARDER\_NAME    | false    |                | LogForwarder name                                                           |
| --log\_forwarder\_options | GOLOOP\_LOG\_FORWARDER\_OPTIONS | false    | \[]            | LogForwarder options, comma-separated 'key=value'                           |
| --log\_forwarder\_vendor  | GOLOOP\_LOG\_FORWARDER\_VENDOR  | false    |                | LogForwarder vendor (fluentd,logstash)                                      |
| --log\_level              | GOLOOP\_LOG\_LEVEL              | false    | debug          | Global log level (trace,debug,info,warn,error,fatal,panic)                  |
| --log\_writer\_compress   | GOLOOP\_LOG\_WRITER\_COMPRESS   | false    | false          | Use gzip on rotated log file                                                |
| --log\_writer\_filename   | GOLOOP\_LOG\_WRITER\_FILENAME   | false    |                | Log filename (rotated files resides in same directory)                      |
| --log\_writer\_localtime  | GOLOOP\_LOG\_WRITER\_LOCALTIME  | false    | false          | Use localtime on rotated log file instead of UTC                            |
| --log\_writer\_maxage     | GOLOOP\_LOG\_WRITER\_MAXAGE     | false    | 0              | Maximum age of log file in day                                              |
| --log\_writer\_maxbackups | GOLOOP\_LOG\_WRITER\_MAXBACKUPS | false    | 0              | Maximum number of backups                                                   |
| --log\_writer\_maxsize    | GOLOOP\_LOG\_WRITER\_MAXSIZE    | false    | 100            | Maximum log file size in MiB                                                |
| --node\_dir               | GOLOOP\_NODE\_DIR               | false    |                | Node data directory (default: \[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s          | GOLOOP\_NODE\_SOCK              | false    |                | Node Command Line Interface socket path (default: \[node\_dir]/cli.sock)    |
| --p2p                     | GOLOOP\_P2P                     | false    | 127.0.0.1:8080 | Advertise ip-port of P2P                                                    |
| --p2p\_listen             | GOLOOP\_P2P\_LISTEN             | false    |                | Listen ip-port of P2P                                                       |
| --rpc\_addr               | GOLOOP\_RPC\_ADDR               | false    | :9080          | Listen ip-port of JSON-RPC                                                  |
| --rpc\_dump               | GOLOOP\_RPC\_DUMP               | false    | false          | JSON-RPC Request, Response Dump flag                                        |

#### Child commands

| Command                                                                                       | Description        |
| --------------------------------------------------------------------------------------------- | ------------------ |
| [goloop server save](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-server-save)   | Save configuration |
| [goloop server start](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-server-start) | Start server       |

### goloop server save

#### Description

Save configuration

#### Usage

`goloop server save [file] [flags]`

#### Options

| Name,shorthand     | Environment Variable | Required | Default | Description                |
| ------------------ | -------------------- | -------- | ------- | -------------------------- |
| --save\_key\_store |                      | false    |         | KeyStore File path to save |

#### Inherited Options

| Name,shorthand            | Environment Variable            | Required | Default        | Description                                                                 |
| ------------------------- | ------------------------------- | -------- | -------------- | --------------------------------------------------------------------------- |
| --backup\_dir             | GOLOOP\_BACKUP\_DIR             | false    |                | Node backup directory (default: \[node\_dir]/backup                         |
| --config, -c              | GOLOOP\_CONFIG                  | false    |                | Parsing configuration file                                                  |
| --console\_level          | GOLOOP\_CONSOLE\_LEVEL          | false    | trace          | Console log level (trace,debug,info,warn,error,fatal,panic)                 |
| --ee\_socket              | GOLOOP\_EE\_SOCKET              | false    |                | Execution engine socket path                                                |
| --engines                 | GOLOOP\_ENGINES                 | false    | python         | Execution engines, comma-separated (python,java)                            |
| --key\_password           | GOLOOP\_KEY\_PASSWORD           | false    |                | Password for the KeyStore file                                              |
| --key\_plugin             | GOLOOP\_KEY\_PLUGIN             | false    |                | KeyPlugin file for wallet                                                   |
| --key\_plugin\_options    | GOLOOP\_KEY\_PLUGIN\_OPTIONS    | false    | \[]            | KeyPlugin options                                                           |
| --key\_secret             | GOLOOP\_KEY\_SECRET             | false    |                | Secret (password) file for KeyStore                                         |
| --key\_store              | GOLOOP\_KEY\_STORE              | false    |                | KeyStore file for wallet                                                    |
| --log\_forwarder\_address | GOLOOP\_LOG\_FORWARDER\_ADDRESS | false    |                | LogForwarder address                                                        |
| --log\_forwarder\_level   | GOLOOP\_LOG\_FORWARDER\_LEVEL   | false    | info           | LogForwarder level                                                          |
| --log\_forwarder\_name    | GOLOOP\_LOG\_FORWARDER\_NAME    | false    |                | LogForwarder name                                                           |
| --log\_forwarder\_options | GOLOOP\_LOG\_FORWARDER\_OPTIONS | false    | \[]            | LogForwarder options, comma-separated 'key=value'                           |
| --log\_forwarder\_vendor  | GOLOOP\_LOG\_FORWARDER\_VENDOR  | false    |                | LogForwarder vendor (fluentd,logstash)                                      |
| --log\_level              | GOLOOP\_LOG\_LEVEL              | false    | debug          | Global log level (trace,debug,info,warn,error,fatal,panic)                  |
| --log\_writer\_compress   | GOLOOP\_LOG\_WRITER\_COMPRESS   | false    | false          | Use gzip on rotated log file                                                |
| --log\_writer\_filename   | GOLOOP\_LOG\_WRITER\_FILENAME   | false    |                | Log filename (rotated files resides in same directory)                      |
| --log\_writer\_localtime  | GOLOOP\_LOG\_WRITER\_LOCALTIME  | false    | false          | Use localtime on rotated log file instead of UTC                            |
| --log\_writer\_maxage     | GOLOOP\_LOG\_WRITER\_MAXAGE     | false    | 0              | Maximum age of log file in day                                              |
| --log\_writer\_maxbackups | GOLOOP\_LOG\_WRITER\_MAXBACKUPS | false    | 0              | Maximum number of backups                                                   |
| --log\_writer\_maxsize    | GOLOOP\_LOG\_WRITER\_MAXSIZE    | false    | 100            | Maximum log file size in MiB                                                |
| --node\_dir               | GOLOOP\_NODE\_DIR               | false    |                | Node data directory (default: \[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s          | GOLOOP\_NODE\_SOCK              | false    |                | Node Command Line Interface socket path (default: \[node\_dir]/cli.sock)    |
| --p2p                     | GOLOOP\_P2P                     | false    | 127.0.0.1:8080 | Advertise ip-port of P2P                                                    |
| --p2p\_listen             | GOLOOP\_P2P\_LISTEN             | false    |                | Listen ip-port of P2P                                                       |
| --rpc\_addr               | GOLOOP\_RPC\_ADDR               | false    | :9080          | Listen ip-port of JSON-RPC                                                  |
| --rpc\_dump               | GOLOOP\_RPC\_DUMP               | false    | false          | JSON-RPC Request, Response Dump flag                                        |

### goloop server start

#### Description

Start server

#### Usage

`goloop server start [flags]`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description                |
| -------------- | -------------------- | -------- | ------- | -------------------------- |
| --cpuprofile   |                      | false    |         | CPU Profiling data file    |
| --memprofile   |                      | false    |         | Memory Profiling data file |

#### Inherited Options

| Name,shorthand            | Environment Variable            | Required | Default        | Description                                                                 |
| ------------------------- | ------------------------------- | -------- | -------------- | --------------------------------------------------------------------------- |
| --backup\_dir             | GOLOOP\_BACKUP\_DIR             | false    |                | Node backup directory (default: \[node\_dir]/backup                         |
| --config, -c              | GOLOOP\_CONFIG                  | false    |                | Parsing configuration file                                                  |
| --console\_level          | GOLOOP\_CONSOLE\_LEVEL          | false    | trace          | Console log level (trace,debug,info,warn,error,fatal,panic)                 |
| --ee\_socket              | GOLOOP\_EE\_SOCKET              | false    |                | Execution engine socket path                                                |
| --engines                 | GOLOOP\_ENGINES                 | false    | python         | Execution engines, comma-separated (python,java)                            |
| --key\_password           | GOLOOP\_KEY\_PASSWORD           | false    |                | Password for the KeyStore file                                              |
| --key\_plugin             | GOLOOP\_KEY\_PLUGIN             | false    |                | KeyPlugin file for wallet                                                   |
| --key\_plugin\_options    | GOLOOP\_KEY\_PLUGIN\_OPTIONS    | false    | \[]            | KeyPlugin options                                                           |
| --key\_secret             | GOLOOP\_KEY\_SECRET             | false    |                | Secret (password) file for KeyStore                                         |
| --key\_store              | GOLOOP\_KEY\_STORE              | false    |                | KeyStore file for wallet                                                    |
| --log\_forwarder\_address | GOLOOP\_LOG\_FORWARDER\_ADDRESS | false    |                | LogForwarder address                                                        |
| --log\_forwarder\_level   | GOLOOP\_LOG\_FORWARDER\_LEVEL   | false    | info           | LogForwarder level                                                          |
| --log\_forwarder\_name    | GOLOOP\_LOG\_FORWARDER\_NAME    | false    |                | LogForwarder name                                                           |
| --log\_forwarder\_options | GOLOOP\_LOG\_FORWARDER\_OPTIONS | false    | \[]            | LogForwarder options, comma-separated 'key=value'                           |
| --log\_forwarder\_vendor  | GOLOOP\_LOG\_FORWARDER\_VENDOR  | false    |                | LogForwarder vendor (fluentd,logstash)                                      |
| --log\_level              | GOLOOP\_LOG\_LEVEL              | false    | debug          | Global log level (trace,debug,info,warn,error,fatal,panic)                  |
| --log\_writer\_compress   | GOLOOP\_LOG\_WRITER\_COMPRESS   | false    | false          | Use gzip on rotated log file                                                |
| --log\_writer\_filename   | GOLOOP\_LOG\_WRITER\_FILENAME   | false    |                | Log filename (rotated files resides in same directory)                      |
| --log\_writer\_localtime  | GOLOOP\_LOG\_WRITER\_LOCALTIME  | false    | false          | Use localtime on rotated log file instead of UTC                            |
| --log\_writer\_maxage     | GOLOOP\_LOG\_WRITER\_MAXAGE     | false    | 0              | Maximum age of log file in day                                              |
| --log\_writer\_maxbackups | GOLOOP\_LOG\_WRITER\_MAXBACKUPS | false    | 0              | Maximum number of backups                                                   |
| --log\_writer\_maxsize    | GOLOOP\_LOG\_WRITER\_MAXSIZE    | false    | 100            | Maximum log file size in MiB                                                |
| --node\_dir               | GOLOOP\_NODE\_DIR               | false    |                | Node data directory (default: \[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s          | GOLOOP\_NODE\_SOCK              | false    |                | Node Command Line Interface socket path (default: \[node\_dir]/cli.sock)    |
| --p2p                     | GOLOOP\_P2P                     | false    | 127.0.0.1:8080 | Advertise ip-port of P2P                                                    |
| --p2p\_listen             | GOLOOP\_P2P\_LISTEN             | false    |                | Listen ip-port of P2P                                                       |
| --rpc\_addr               | GOLOOP\_RPC\_ADDR               | false    | :9080          | Listen ip-port of JSON-RPC                                                  |
| --rpc\_dump               | GOLOOP\_RPC\_DUMP               | false    | false          | JSON-RPC Request, Response Dump flag                                        |

### goloop stats

#### Description

Display a live streams of chains metric-statistics

#### Usage

`goloop stats`

#### Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     | GOLOOP\_CONFIG       | false    |         | Parsing configuration file                                                |
| --interval       | GOLOOP\_INTERVAL     | false    | 1       | Pull interval                                                             |
| --key\_store     | GOLOOP\_KEY\_STORE   | false    |         | KeyStore file for wallet                                                  |
| --no-stream      | GOLOOP\_NO-STREAM    | false    | false   | Only pull the first metric-statistics                                     |
| --node\_dir      | GOLOOP\_NODE\_DIR    | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s | GOLOOP\_NODE\_SOCK   | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop system

#### Description

System info

#### Usage

`goloop system`

#### Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     | GOLOOP\_CONFIG       | false    |         | Parsing configuration file                                                |
| --key\_store     | GOLOOP\_KEY\_STORE   | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      | GOLOOP\_NODE\_DIR    | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s | GOLOOP\_NODE\_SOCK   | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

#### Child commands

| Command                                                                                           | Description                 |
| ------------------------------------------------------------------------------------------------- | --------------------------- |
| [goloop system backup](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-system-backup)   | Manage stored backups       |
| [goloop system config](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-system-config)   | Configure system            |
| [goloop system info](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-system-info)       | Get system information      |
| [goloop system restore](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-system-restore) | Restore chain from a backup |

### goloop system backup

#### Description

Manage stored backups

#### Usage

`goloop system backup`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     | GOLOOP\_CONFIG       | false    |         | Parsing configuration file                                                |
| --key\_store     | GOLOOP\_KEY\_STORE   | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      | GOLOOP\_NODE\_DIR    | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s | GOLOOP\_NODE\_SOCK   | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

####

#### Child commands

| Command                                                                                               | Description          |
| ----------------------------------------------------------------------------------------------------- | -------------------- |
| [goloop system backup ls](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-system-backup-ls) | List current backups |

### goloop system backup ls

#### Description

List current backups

#### Usage

`goloop system backup ls`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop system config

#### Description

Configure system

#### Usage

`goloop system config KEY VALUE`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     | GOLOOP\_CONFIG       | false    |         | Parsing configuration file                                                |
| --key\_store     | GOLOOP\_KEY\_STORE   | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      | GOLOOP\_NODE\_DIR    | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s | GOLOOP\_NODE\_SOCK   | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop system info

#### Description

Get system information

#### Usage

`goloop system info [flags]`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description                                   |
| -------------- | -------------------- | -------- | ------- | --------------------------------------------- |
| --format, -f   |                      | false    |         | Format the output using the given Go template |

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     | GOLOOP\_CONFIG       | false    |         | Parsing configuration file                                                |
| --key\_store     | GOLOOP\_KEY\_STORE   | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      | GOLOOP\_NODE\_DIR    | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s | GOLOOP\_NODE\_SOCK   | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop system restore

#### Description

Restore chain from a backup

#### Usage

`goloop system restore`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     | GOLOOP\_CONFIG       | false    |         | Parsing configuration file                                                |
| --key\_store     | GOLOOP\_KEY\_STORE   | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      | GOLOOP\_NODE\_DIR    | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s | GOLOOP\_NODE\_SOCK   | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

#### Child commands

| Command                                                                                                         | Description                           |
| --------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| [goloop system restore start](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-system-restore-start)   | Start to restore the specified backup |
| [goloop system restore status](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-system-restore-status) | Get restore status                    |
| [goloop system restore stop](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-system-restore-stop)     | Stop current restoring job            |

### goloop system restore start

#### Description

Start to restore the specified backup

#### Usage

`goloop system restore start [NAME]`

#### Options

| Name,shorthand | Environment Variable | Required | Default | Description              |
| -------------- | -------------------- | -------- | ------- | ------------------------ |
| --overwrite    |                      | false    | false   | Overwrite existing chain |

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop system restore status

#### Description

Get restore status

#### Usage

`goloop system restore status`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop system restore stop

#### Description

Stop current restoring job

#### Usage

`goloop system restore stop`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     |                      | false    |         | Parsing configuration file                                                |
| --key\_store     |                      | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      |                      | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s |                      | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop user

#### Description

User management

#### Usage

`goloop user`

#### Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     | GOLOOP\_CONFIG       | false    |         | Parsing configuration file                                                |
| --key\_store     | GOLOOP\_KEY\_STORE   | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      | GOLOOP\_NODE\_DIR    | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s | GOLOOP\_NODE\_SOCK   | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

#### Child commands

| Command                                                                               | Description |
| ------------------------------------------------------------------------------------- | ----------- |
| [goloop user add](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-user-add) | Add user    |
| [goloop user ls](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-user-ls)   | List users  |
| [goloop user rm](../../../icon-2.0/goloop/management/goloop\_cli.md#goloop-user-rm)   | Remove user |

### goloop user add

#### Description

Add user

#### Usage

`goloop user add ADDRESS`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     | GOLOOP\_CONFIG       | false    |         | Parsing configuration file                                                |
| --key\_store     | GOLOOP\_KEY\_STORE   | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      | GOLOOP\_NODE\_DIR    | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s | GOLOOP\_NODE\_SOCK   | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop user ls

#### Description

List users

#### Usage

`goloop user ls`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     | GOLOOP\_CONFIG       | false    |         | Parsing configuration file                                                |
| --key\_store     | GOLOOP\_KEY\_STORE   | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      | GOLOOP\_NODE\_DIR    | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s | GOLOOP\_NODE\_SOCK   | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop user rm

#### Description

Remove user

#### Usage

`goloop user rm ADDRESS`

#### Inherited Options

| Name,shorthand   | Environment Variable | Required | Default | Description                                                               |
| ---------------- | -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| --config, -c     | GOLOOP\_CONFIG       | false    |         | Parsing configuration file                                                |
| --key\_store     | GOLOOP\_KEY\_STORE   | false    |         | KeyStore file for wallet                                                  |
| --node\_dir      | GOLOOP\_NODE\_DIR    | false    |         | Node data directory(default:\[configuration file path]/.chain/\[ADDRESS]) |
| --node\_sock, -s | GOLOOP\_NODE\_SOCK   | true     |         | Node Command Line Interface socket path(default:\[node\_dir]/cli.sock)    |

### goloop version

#### Description

Print goloop version

#### Usage

`goloop version`
