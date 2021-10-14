# API

## Node Management API v0.1.0 <a href="node-management-api" id="node-management-api"></a>

> Scroll down for example requests and responses.

goloop management

Base URLs:

* [http://localhost:9080/admin](http://localhost:9080/admin)

## node <a href="node-management-api-node" id="node-management-api-node"></a>

Node Management

### View system

> Code samples

`GET /system`

Return system information.

#### Parameters <a href="view-system-parameters" id="view-system-parameters"></a>

| Name   | In    | Type   | Required | Description                                   |
| ------ | ----- | ------ | -------- | --------------------------------------------- |
| format | query | string | false    | Format the output using the given Go template |

> Example responses
>
> 200 Response

```javascript
{
  "buildVersion": "v0.1.7",
  "buildTags": "linux/amd64 tags()-2019-08-20-09:39:15",
  "setting": {
    "address": "hx4208599c8f58fed475db747504a80a311a3af63b",
    "p2p": "localhost:8080",
    "p2pListen": "localhost:8080",
    "rpcAddr": ":9080",
    "rpcDump": false
  },
  "config": {
    "eeInstances": 1,
    "rpcDefaultChannel": "",
    "rpcIncludeDebug": false
  }
}
```

#### Responses <a href="view-system-responses" id="view-system-responses"></a>

| Status | Meaning                                                                    | Description           | Schema                                     |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------------------------------------------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | [System](goloop_admin_api.md#schemasystem) |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None                                       |

 This operation does not require authentication

### View system configuration <a href="opidgetsystem" id="opidgetsystem"></a>

> Code samples

`GET /system/configure`

Return system configuration.

> Example responses
>
> 200 Response

```javascript
{
  "eeInstances": 1,
  "rpcDefaultChannel": "",
  "rpcIncludeDebug": false
}
```

#### Responses <a href="view-system-configuration-responses" id="view-system-configuration-responses"></a>

| Status | Meaning                                                                    | Description           | Schema                                                 |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------------------------------------------------------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | [SystemConfig](goloop_admin_api.md#schemasystemconfig) |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None                                                   |

 This operation does not require authentication

### Configure system <a href="opidgetsystemconfiguration" id="opidgetsystemconfiguration"></a>

> Code samples

`POST /system/configure`

Configure system, configurable properties refer to [SystemConfig](goloop_admin_api.md#schemasystemconfig)

> Body parameter

```javascript
{
  "key": "string",
  "value": "string"
}
```

#### Parameters <a href="configure-system-parameters" id="configure-system-parameters"></a>

| Name | In   | Type                                                       | Required | Description            |
| ---- | ---- | ---------------------------------------------------------- | -------- | ---------------------- |
| body | body | [ConfigureParam](goloop_admin_api.md#schemaconfigureparam) | true     | key-value to configure |

#### Responses <a href="configure-system-responses" id="configure-system-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

### List Backups <a href="opidconfiguresystem" id="opidconfiguresystem"></a>

> Code samples

`GET /system/backup`

Return list of backups

> Example responses
>
> 200 Response

```javascript
[
  {
    "name": "0x178977_0x1_1_20200715-111057.zip",
    "cid": "0x178977",
    "nid": "0x1",
    "channel": "1",
    "height": 2021,
    "codec": "rlp"
  }
]
```

#### Responses <a href="list-backups-responses" id="list-backups-responses"></a>

| Status | Meaning                                                                    | Description           | Schema                                             |
| ------ | -------------------------------------------------------------------------- | --------------------- | -------------------------------------------------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | [BackupList](goloop_admin_api.md#schemabackuplist) |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None                                               |

 This operation does not require authentication

### Restore Status <a href="opidgetbackups" id="opidgetbackups"></a>

> Code samples

`GET /system/restore`

View the status of restoring

> Example responses
>
> 200 Response

```javascript
{
  "name": "0x178977_0x1_1_20200715-111057.zip",
  "overwrite": true,
  "state": "started 23/128"
}
```

#### Responses <a href="restore-status-responses" id="restore-status-responses"></a>

| Status | Meaning                                                                    | Description           | Schema                                                   |
| ------ | -------------------------------------------------------------------------- | --------------------- | -------------------------------------------------------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | [RestoreStatus](goloop_admin_api.md#schemarestorestatus) |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None                                                     |

 This operation does not require authentication

### Start Restore <a href="opidgetrestorestatus" id="opidgetrestorestatus"></a>

> Code samples

`POST /system/restore`

Start to restore chain from the backup

> Body parameter

```javascript
{
  "name": "0x178977_0x1_1_20200715-111057.zip",
  "overwrite": true
}
```

#### Parameters <a href="start-restore-parameters" id="start-restore-parameters"></a>

| Name | In   | Type                                                   | Required | Description                |
| ---- | ---- | ------------------------------------------------------ | -------- | -------------------------- |
| body | body | [RestoreParam](goloop_admin_api.md#schemarestoreparam) | true     | Name of backup and options |

#### Responses <a href="start-restore-responses" id="start-restore-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

### Stop Restore <a href="opidstartrestore" id="opidstartrestore"></a>

> Code samples

`DELETE /system/restore`

Stop restoring operation

#### Responses <a href="stop-restore-responses" id="stop-restore-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

## chain <a href="node-management-api-chain" id="node-management-api-chain"></a>

Chain Management

### List Chains <a href="opidstoprestore" id="opidstoprestore"></a>

> Code samples

`GET /chain`

Returns a list of chains

> Example responses
>
> 200 Response

```javascript
[
  {
    "cid": "0x782b03",
    "nid": "0x000000",
    "channel": "000000",
    "state": "started",
    "height": 100,
    "lastError": ""
  }
]
```

#### Responses <a href="list-chains-responses" id="list-chains-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | Inline |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

#### Response Schema <a href="list-chains-responseschema" id="list-chains-responseschema"></a>

Status Code **200**

_array of chains_

| Name        | Type                                        | Required | Restrictions | Description           |
| ----------- | ------------------------------------------- | -------- | ------------ | --------------------- |
| _anonymous_ | \[[Chain](goloop_admin_api.md#schemachain)] | false    | none         | array of chains       |
| » cid       | string("0x" + lowercase HEX string)         | false    | none         | chain-id of chain     |
| » nid       | string("0x" + lowercase HEX string)         | false    | none         | network-id of chain   |
| » channel   | string                                      | false    | none         | chain-alias of node   |
| » height    | integer(int64)                              | false    | none         | block height of chain |
| » state     | string                                      | false    | none         | state of chain        |
| » lastError | string                                      | false    | none         | last error of chain   |

 This operation does not require authentication

### Join Chain <a href="opidlistchain" id="opidlistchain"></a>

> Code samples

`POST /chain`

Join Chain

> Body parameter

```yaml
json:
  dbType: goleveldb
  seedAddress: 'localhost:8080'
  role: 3
  concurrencyLevel: 1
  normalTxPool: 5000
  patchTxPool: 1000
  maxBlockTxBytes: 1048576
  nodeCache: none
  channel: '000000'
  secureSuites: 'none,tls,ecdhe'
  secureAeads: 'chacha,aes128,aes256'
  defaultWaitTimeout: 0
  txTimeout: 0
  maxWaitTimeout: 0
  autoStart: false
genesisZip: string
```

#### Parameters <a href="join-chain-parameters" id="join-chain-parameters"></a>

| Name                  | In   | Type                                                 | Required | Description                                                                                  |
| --------------------- | ---- | ---------------------------------------------------- | -------- | -------------------------------------------------------------------------------------------- |
| body                  | body | object                                               | true     | Genesis-Storage zip file and json encoded chain-configuration for join chain using multipart |
| » json                | body | [ChainConfig](goloop_admin_api.md#schemachainconfig) | true     | json encoded chain-configuration, using multipart 'Content-Disposition: name=json'           |
| »» dbType             | body | string                                               | false    | Name of database system, ReadOnly                                                            |
| »» seedAddress        | body | string                                               | false    | List of Seed ip-port, Comma separated string, Runtime-Configurable                           |
| »» role               | body | integer                                              | false    | Role:                                                                                        |
| »» concurrencyLevel   | body | integer                                              | false    | Maximum number of executors to use for concurrency                                           |
| »» normalTxPool       | body | integer                                              | false    | Size of normal transaction pool                                                              |
| »» patchTxPool        | body | integer                                              | false    | Size of patch transaction pool                                                               |
| »» maxBlockTxBytes    | body | integer                                              | false    | Max size of transactions in a block                                                          |
| »» nodeCache          | body | string                                               | false    | Node cache:                                                                                  |
| »» channel            | body | string                                               | false    | Chain-alias of node                                                                          |
| »» secureSuites       | body | string                                               | false    | Supported Secure suites with order (none,tls,ecdhe) - Comma separated string                 |
| »» secureAeads        | body | string                                               | false    | Supported Secure AEAD with order (chacha,aes128,aes256) - Comma separated string             |
| »» defaultWaitTimeout | body | integer                                              | false    | Default wait timeout in milli-second(0:disable)                                              |
| »» maxWaitTimeout     | body | integer                                              | false    | Max wait timeout in milli-second(0:uses same value of defaultWaitTimeout)                    |
| »» txTimeout          | body | integer                                              | false    | Transaction timeout in milli-second(0:uses system default value)                             |
| »» autoStart          | body | boolean                                              | false    | Start the chain automatically on node start                                                  |
| » genesisZip          | body | string(binary)                                       | true     | Genesis-Storage zip file, using multipart 'Content-Disposition: name=genesisZip'             |

**Detailed descriptions**

**»» role**: Role:

* `0` - None
* `1` - Seed
* `2` - Validator
*   `3` - Seed and Validator

    Runtime-Configurable

**»» nodeCache**: Node cache:

* `none` - No cache
* `small` - Memory Lv1 \~ Lv5 for all
* `large` - Memory Lv1 \~ Lv5 for all and File Lv6 for store

**Enumerated Values**

| Parameter    | Value     |
| ------------ | --------- |
| »» dbType    | badgerdb  |
| »» dbType    | goleveldb |
| »» dbType    | boltdb    |
| »» dbType    | mapdb     |
| »» role      | 0         |
| »» role      | 1         |
| »» role      | 2         |
| »» role      | 3         |
| »» nodeCache | none      |
| »» nodeCache | small     |
| »» nodeCache | large     |

> Example responses
>
> 200 Response

#### Responses <a href="join-chain-responses" id="join-chain-responses"></a>

| Status | Meaning                                                                    | Description           | Schema                                       |
| ------ | -------------------------------------------------------------------------- | --------------------- | -------------------------------------------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | [ChainID](goloop_admin_api.md#schemachainid) |
| 409    | [Conflict](https://tools.ietf.org/html/rfc7231#section-6.5.8)              | Conflict              | None                                         |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None                                         |

 This operation does not require authentication

### Inspect Chain

> Code samples

`GET /chain/{cid}`

Return low-level information about a chain.

#### Parameters <a href="inspect-chain-parameters" id="inspect-chain-parameters"></a>

| Name     | In    | Type                                | Required | Description                                   |
| -------- | ----- | ----------------------------------- | -------- | --------------------------------------------- |
| cid      | path  | string("0x" + lowercase HEX string) | true     | chain-id of chain                             |
| format   | query | string                              | false    | Format the output using the given Go template |
| informal | query | boolean                             | false    | Inspect with informal data                    |

> Example responses
>
> 200 Response

```javascript
{
  "cid": "0x782b03",
  "nid": "0x000000",
  "channel": "000000",
  "state": "started",
  "height": 100,
  "lastError": "",
  "genesisTx": {},
  "config": {
    "dbType": "goleveldb",
    "seedAddress": "localhost:8080",
    "role": 3,
    "concurrencyLevel": 1,
    "normalTxPool": 5000,
    "patchTxPool": 1000,
    "maxBlockTxBytes": 1048576,
    "nodeCache": "none",
    "channel": "000000",
    "secureSuites": "none,tls,ecdhe",
    "secureAeads": "chacha,aes128,aes256",
    "defaultWaitTimeout": 0,
    "txTimeout": 0,
    "maxWaitTimeout": 0,
    "autoStart": false
  },
  "module": {
    "property1": {},
    "property2": {}
  }
}
```

#### Responses <a href="inspect-chain-responses" id="inspect-chain-responses"></a>

| Status | Meaning                                                                    | Description           | Schema                                                 |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------------------------------------------------------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | [ChainInspect](goloop_admin_api.md#schemachaininspect) |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Not Found             | None                                                   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None                                                   |

 This operation does not require authentication

### Leave Chain <a href="opidgetchain" id="opidgetchain"></a>

> Code samples

`DELETE /chain/{cid}`

Leave Chain.

#### Parameters <a href="leave-chain-parameters" id="leave-chain-parameters"></a>

| Name | In   | Type                                | Required | Description       |
| ---- | ---- | ----------------------------------- | -------- | ----------------- |
| cid  | path | string("0x" + lowercase HEX string) | true     | chain-id of chain |

#### Responses <a href="leave-chain-responses" id="leave-chain-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | None   |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Not Found             | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

### Start Chain <a href="opidleavechain" id="opidleavechain"></a>

> Code samples

`POST /chain/{cid}/start`

Start Chain.

#### Parameters <a href="start-chain-parameters" id="start-chain-parameters"></a>

| Name | In   | Type                                | Required | Description       |
| ---- | ---- | ----------------------------------- | -------- | ----------------- |
| cid  | path | string("0x" + lowercase HEX string) | true     | chain-id of chain |

#### Responses <a href="start-chain-responses" id="start-chain-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | None   |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Not Found             | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

### Stop Chain <a href="opidstartchain" id="opidstartchain"></a>

> Code samples

`POST /chain/{cid}/stop`

Stop Chain.

#### Parameters <a href="stop-chain-parameters" id="stop-chain-parameters"></a>

| Name | In   | Type                                | Required | Description       |
| ---- | ---- | ----------------------------------- | -------- | ----------------- |
| cid  | path | string("0x" + lowercase HEX string) | true     | chain-id of chain |

#### Responses <a href="stop-chain-responses" id="stop-chain-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | None   |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Not Found             | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

### Reset Chain <a href="opidstopchain" id="opidstopchain"></a>

> Code samples

`POST /chain/{cid}/reset`

Reset Chain.

#### Parameters <a href="reset-chain-parameters" id="reset-chain-parameters"></a>

| Name | In   | Type                                | Required | Description       |
| ---- | ---- | ----------------------------------- | -------- | ----------------- |
| cid  | path | string("0x" + lowercase HEX string) | true     | chain-id of chain |

#### Responses <a href="reset-chain-responses" id="reset-chain-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | None   |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Not Found             | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

### Import Chain <a href="opidresetchain" id="opidresetchain"></a>

> Code samples

`POST /chain/{cid}/import`

Import a chain from legacy database.

> Body parameter

```javascript
{
  "dbPath": "/path/to/database",
  "height": 1
}
```

#### Parameters <a href="import-chain-parameters" id="import-chain-parameters"></a>

| Name | In   | Type                                                           | Required | Description       |
| ---- | ---- | -------------------------------------------------------------- | -------- | ----------------- |
| cid  | path | string("0x" + lowercase HEX string)                            | true     | chain-id of chain |
| body | body | [ChainImportParam](goloop_admin_api.md#schemachainimportparam) | true     | none              |

#### Responses <a href="import-chain-responses" id="import-chain-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | None   |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Not Found             | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

### Prune Chain <a href="opidimportchain" id="opidimportchain"></a>

> Code samples

`POST /chain/{cid}/prune`

Prune chain data from the specific height

> Body parameter

```javascript
{
  "dbType": "goleveldb",
  "height": 1
}
```

#### Parameters <a href="prune-chain-parameters" id="prune-chain-parameters"></a>

| Name | In   | Type                                               | Required | Description       |
| ---- | ---- | -------------------------------------------------- | -------- | ----------------- |
| cid  | path | string("0x" + lowercase HEX string)                | true     | chain-id of chain |
| body | body | [PruneParam](goloop_admin_api.md#schemapruneparam) | true     | none              |

#### Responses <a href="prune-chain-responses" id="prune-chain-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | None   |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Not Found             | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

### Backup Chain <a href="opidprunechain" id="opidprunechain"></a>

> Code samples

`POST /chain/{cid}/backup`

Backup chain data to the specific file

#### Parameters <a href="backup-chain-parameters" id="backup-chain-parameters"></a>

| Name | In   | Type                                | Required | Description       |
| ---- | ---- | ----------------------------------- | -------- | ----------------- |
| cid  | path | string("0x" + lowercase HEX string) | true     | chain-id of chain |

#### Responses <a href="backup-chain-responses" id="backup-chain-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | None   |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Not Found             | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

### Download Genesis-Storage <a href="opidbackupchain" id="opidbackupchain"></a>

> Code samples

`GET /chain/{cid}/genesis`

Download Genesis-Storage zip file

#### Parameters <a href="download-genesis-storage-parameters" id="download-genesis-storage-parameters"></a>

| Name | In   | Type                                | Required | Description       |
| ---- | ---- | ----------------------------------- | -------- | ----------------- |
| cid  | path | string("0x" + lowercase HEX string) | true     | chain-id of chain |

> Example responses
>
> 200 Response

#### Responses <a href="download-genesis-storage-responses" id="download-genesis-storage-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | string |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Not Found             | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

### View chain configuration <a href="opidgetchaingenesis" id="opidgetchaingenesis"></a>

> Code samples

`GET /chain/{cid}/configure`

Return chain configuration.

#### Parameters <a href="view-chain-configuration-parameters" id="view-chain-configuration-parameters"></a>

| Name | In   | Type                                | Required | Description       |
| ---- | ---- | ----------------------------------- | -------- | ----------------- |
| cid  | path | string("0x" + lowercase HEX string) | true     | chain-id of chain |

> Example responses
>
> 200 Response

```javascript
{
  "dbType": "goleveldb",
  "seedAddress": "localhost:8080",
  "role": 3,
  "concurrencyLevel": 1,
  "normalTxPool": 5000,
  "patchTxPool": 1000,
  "maxBlockTxBytes": 1048576,
  "nodeCache": "none",
  "channel": "000000",
  "secureSuites": "none,tls,ecdhe",
  "secureAeads": "chacha,aes128,aes256",
  "defaultWaitTimeout": 0,
  "txTimeout": 0,
  "maxWaitTimeout": 0,
  "autoStart": false
}
```

#### Responses <a href="view-chain-configuration-responses" id="view-chain-configuration-responses"></a>

| Status | Meaning                                                                    | Description           | Schema                                               |
| ------ | -------------------------------------------------------------------------- | --------------------- | ---------------------------------------------------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | [ChainConfig](goloop_admin_api.md#schemachainconfig) |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Not Found             | None                                                 |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None                                                 |

 This operation does not require authentication

### Configure chain <a href="opidgetchainconfiguration" id="opidgetchainconfiguration"></a>

> Code samples

`POST /chain/{cid}/configure`

Configure chain, configurable properties refer to [ChainConfig](goloop_admin_api.md#schemachainconfig)

> Body parameter

```javascript
{
  "key": "string",
  "value": "string"
}
```

#### Parameters <a href="configure-chain-parameters" id="configure-chain-parameters"></a>

| Name | In   | Type                                                       | Required | Description            |
| ---- | ---- | ---------------------------------------------------------- | -------- | ---------------------- |
| cid  | path | string("0x" + lowercase HEX string)                        | true     | chain-id of chain      |
| body | body | [ConfigureParam](goloop_admin_api.md#schemaconfigureparam) | true     | key-value to configure |

#### Responses <a href="configure-chain-responses" id="configure-chain-responses"></a>

| Status | Meaning                                                                    | Description           | Schema |
| ------ | -------------------------------------------------------------------------- | --------------------- | ------ |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | Success               | None   |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Not Found             | None   |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None   |

 This operation does not require authentication

## Schemas <a href="opidconfigurechain" id="opidconfigurechain"></a>

### ChainID <a href="tocschainid" id="tocschainid"></a>

```javascript
"0x782b03"
```

_chain-id of chain, "0x" + lowercase HEX string_

#### Properties <a href="schemachainid" id="schemachainid"></a>

| Name        | Type   | Required | Restrictions | Description                                    |
| ----------- | ------ | -------- | ------------ | ---------------------------------------------- |
| _anonymous_ | string | false    | none         | chain-id of chain, "0x" + lowercase HEX string |

### Chain <a href="tocschain" id="tocschain"></a>

```javascript
{
  "cid": "0x782b03",
  "nid": "0x000000",
  "channel": "000000",
  "state": "started",
  "height": 100,
  "lastError": ""
}
```

#### Properties <a href="schemachain" id="schemachain"></a>

| Name      | Type                                | Required | Restrictions | Description           |
| --------- | ----------------------------------- | -------- | ------------ | --------------------- |
| cid       | string("0x" + lowercase HEX string) | false    | none         | chain-id of chain     |
| nid       | string("0x" + lowercase HEX string) | false    | none         | network-id of chain   |
| channel   | string                              | false    | none         | chain-alias of node   |
| height    | integer(int64)                      | false    | none         | block height of chain |
| state     | string                              | false    | none         | state of chain        |
| lastError | string                              | false    | none         | last error of chain   |

### ChainInspect <a href="tocschaininspect" id="tocschaininspect"></a>

```javascript
{
  "cid": "0x782b03",
  "nid": "0x000000",
  "channel": "000000",
  "state": "started",
  "height": 100,
  "lastError": "",
  "genesisTx": {},
  "config": {
    "dbType": "goleveldb",
    "seedAddress": "localhost:8080",
    "role": 3,
    "concurrencyLevel": 1,
    "normalTxPool": 5000,
    "patchTxPool": 1000,
    "maxBlockTxBytes": 1048576,
    "nodeCache": "none",
    "channel": "000000",
    "secureSuites": "none,tls,ecdhe",
    "secureAeads": "chacha,aes128,aes256",
    "defaultWaitTimeout": 0,
    "txTimeout": 0,
    "maxWaitTimeout": 0,
    "autoStart": false
  },
  "module": {
    "property1": {},
    "property2": {}
  }
}
```

#### Properties <a href="schemachaininspect" id="schemachaininspect"></a>

_allOf_

| Name        | Type                                     | Required | Restrictions | Description |
| ----------- | ---------------------------------------- | -------- | ------------ | ----------- |
| _anonymous_ | [Chain](goloop_admin_api.md#schemachain) | false    | none         | none        |

_and_

| Name                        | Type                                                 | Required | Restrictions | Description         |
| --------------------------- | ---------------------------------------------------- | -------- | ------------ | ------------------- |
| _anonymous_                 | object                                               | false    | none         | none                |
| » genesisTx                 | object                                               | false    | none         | Genesis Transaction |
| » config                    | [ChainConfig](goloop_admin_api.md#schemachainconfig) | false    | none         | none                |
| » module                    | object                                               | false    | none         | none                |
| »» **additionalProperties** | object                                               | false    | none         | none                |

### ChainConfig <a href="tocschainconfig" id="tocschainconfig"></a>

```javascript
{
  "dbType": "goleveldb",
  "seedAddress": "localhost:8080",
  "role": 3,
  "concurrencyLevel": 1,
  "normalTxPool": 5000,
  "patchTxPool": 1000,
  "maxBlockTxBytes": 1048576,
  "nodeCache": "none",
  "channel": "000000",
  "secureSuites": "none,tls,ecdhe",
  "secureAeads": "chacha,aes128,aes256",
  "defaultWaitTimeout": 0,
  "txTimeout": 0,
  "maxWaitTimeout": 0,
  "autoStart": false
}
```

#### Properties <a href="schemachainconfig" id="schemachainconfig"></a>

| Name               | Type    | Required | Restrictions | Description                                                                                                                            |
| ------------------ | ------- | -------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| dbType             | string  | false    | none         | Name of database system, ReadOnly                                                                                                      |
| seedAddress        | string  | false    | none         | List of Seed ip-port, Comma separated string, Runtime-Configurable                                                                     |
| role               | integer | false    | none         | Role:  _ `0` - None  _ `1` - Seed  _ `2` - Validator  _ `3` - Seed and Validator Runtime-Configurable                                  |
| concurrencyLevel   | integer | false    | none         | Maximum number of executors to use for concurrency                                                                                     |
| normalTxPool       | integer | false    | none         | Size of normal transaction pool                                                                                                        |
| patchTxPool        | integer | false    | none         | Size of patch transaction pool                                                                                                         |
| maxBlockTxBytes    | integer | false    | none         | Max size of transactions in a block                                                                                                    |
| nodeCache          | string  | false    | none         | Node cache:  _ `none` - No cache  _ `small` - Memory Lv1 \~ Lv5 for all  \* `large` - Memory Lv1 \~ Lv5 for all and File Lv6 for store |
| channel            | string  | false    | none         | Chain-alias of node                                                                                                                    |
| secureSuites       | string  | false    | none         | Supported Secure suites with order (none,tls,ecdhe) - Comma separated string                                                           |
| secureAeads        | string  | false    | none         | Supported Secure AEAD with order (chacha,aes128,aes256) - Comma separated string                                                       |
| defaultWaitTimeout | integer | false    | none         | Default wait timeout in milli-second(0:disable)                                                                                        |
| maxWaitTimeout     | integer | false    | none         | Max wait timeout in milli-second(0:uses same value of defaultWaitTimeout)                                                              |
| txTimeout          | integer | false    | none         | Transaction timeout in milli-second(0:uses system default value)                                                                       |
| autoStart          | boolean | false    | none         | Start the chain automatically on node start                                                                                            |

**Enumerated Values**

| Property  | Value     |
| --------- | --------- |
| dbType    | badgerdb  |
| dbType    | goleveldb |
| dbType    | boltdb    |
| dbType    | mapdb     |
| role      | 0         |
| role      | 1         |
| role      | 2         |
| role      | 3         |
| nodeCache | none      |
| nodeCache | small     |
| nodeCache | large     |

### ChainImportParam <a href="tocschainimportparam" id="tocschainimportparam"></a>

```javascript
{
  "dbPath": "/path/to/database",
  "height": 1
}
```

#### Properties <a href="schemachainimportparam" id="schemachainimportparam"></a>

| Name   | Type   | Required | Restrictions | Description   |
| ------ | ------ | -------- | ------------ | ------------- |
| dbPath | string | true     | none         | Database path |
| height | int64  | true     | none         | Block Height  |

### System <a href="tocssystem" id="tocssystem"></a>

```javascript
{
  "buildVersion": "v0.1.7",
  "buildTags": "linux/amd64 tags()-2019-08-20-09:39:15",
  "setting": {
    "address": "hx4208599c8f58fed475db747504a80a311a3af63b",
    "p2p": "localhost:8080",
    "p2pListen": "localhost:8080",
    "rpcAddr": ":9080",
    "rpcDump": false
  },
  "config": {
    "eeInstances": 1,
    "rpcDefaultChannel": "",
    "rpcIncludeDebug": false
  }
}
```

#### Properties <a href="schemasystem" id="schemasystem"></a>

| Name         | Type                                                   | Required | Restrictions | Description                          |
| ------------ | ------------------------------------------------------ | -------- | ------------ | ------------------------------------ |
| buildVersion | string                                                 | false    | none         | build version                        |
| buildTags    | string                                                 | false    | none         | buildTags                            |
| setting      | object                                                 | false    | none         | none                                 |
| » address    | string                                                 | false    | none         | wallet address                       |
| » p2p        | string                                                 | false    | none         | p2p address                          |
| » p2pListen  | string                                                 | false    | none         | p2p listen address                   |
| » rpcAddr    | string                                                 | false    | none         | Listen ip-port of JSON-RPC           |
| » rpcDump    | boolean                                                | false    | none         | JSON-RPC Request, Response Dump flag |
| config       | [SystemConfig](goloop_admin_api.md#schemasystemconfig) | false    | none         | none                                 |

### SystemConfig <a href="tocssystemconfig" id="tocssystemconfig"></a>

```javascript
{
  "eeInstances": 1,
  "rpcDefaultChannel": "",
  "rpcIncludeDebug": false
}
```

#### Properties <a href="schemasystemconfig" id="schemasystemconfig"></a>

| Name              | Type    | Required | Restrictions | Description                               |
| ----------------- | ------- | -------- | ------------ | ----------------------------------------- |
| eeInstances       | integer | false    | none         | eeInstances                               |
| rpcDefaultChannel | string  | false    | none         | default channel for legacy api            |
| rpcIncludeDebug   | boolean | false    | none         | JSON-RPC Response with detail information |

### ConfigureParam <a href="tocsconfigureparam" id="tocsconfigureparam"></a>

```javascript
{
  "key": "string",
  "value": "string"
}
```

#### Properties <a href="schemaconfigureparam" id="schemaconfigureparam"></a>

| Name  | Type   | Required | Restrictions | Description              |
| ----- | ------ | -------- | ------------ | ------------------------ |
| key   | string | true     | none         | configuration field name |
| value | string | true     | none         | configuration value      |

### PruneParam <a href="tocspruneparam" id="tocspruneparam"></a>

```javascript
{
  "dbType": "goleveldb",
  "height": 1
}
```

#### Properties <a href="schemapruneparam" id="schemapruneparam"></a>

| Name   | Type   | Required | Restrictions | Description   |
| ------ | ------ | -------- | ------------ | ------------- |
| dbType | string | false    | none         | Database type |
| height | int64  | true     | none         | Block Height  |

### BackupList <a href="tocsbackuplist" id="tocsbackuplist"></a>

```javascript
[
  {
    "name": "0x178977_0x1_1_20200715-111057.zip",
    "cid": "0x178977",
    "nid": "0x1",
    "channel": "1",
    "height": 2021,
    "codec": "rlp"
  }
]
```

#### Properties <a href="schemabackuplist" id="schemabackuplist"></a>

_None_

### RestoreStatus <a href="tocsrestorestatus" id="tocsrestorestatus"></a>

```javascript
{
  "name": "0x178977_0x1_1_20200715-111057.zip",
  "overwrite": true,
  "state": "started 23/128"
}
```

#### Properties <a href="schemarestorestatus" id="schemarestorestatus"></a>

| Name      | Type    | Required | Restrictions | Description                                                        |
| --------- | ------- | -------- | ------------ | ------------------------------------------------------------------ |
| state     | string  | true     | none         | State of the job (stopped, started N/T, stopping, failed, success) |
| name      | string  | false    | none         | Name of backup                                                     |
| overwrite | boolean | false    | none         | Whether it replaces existing chain data                            |

### RestoreParam <a href="tocsrestoreparam" id="tocsrestoreparam"></a>

```javascript
{
  "name": "0x178977_0x1_1_20200715-111057.zip",
  "overwrite": true
}
```

#### Properties <a href="schemarestoreparam" id="schemarestoreparam"></a>

| Name      | Type    | Required | Restrictions | Description                        |
| --------- | ------- | -------- | ------------ | ---------------------------------- |
| name      | string  | true     | none         | Name of the backup to restore      |
| overwrite | boolean | false    | none         | Whether it replaces existing chain |
