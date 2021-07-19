---
title: Node Management API
language_tabs: []
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2
---

# API

## Node Management API v0.1.0 <a id="node-management-api"></a>

> Scroll down for example requests and responses.

goloop management

Base URLs:

* [http://localhost:9080/admin](http://localhost:9080/admin)

## node <a id="node-management-api-node"></a>

Node Management

### View system

> Code samples

`GET /system`

Return system information.

#### Parameters <a id="view-system-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| format | query | string | false | Format the output using the given Go template |

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

#### Responses <a id="view-system-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | [System](goloop_admin_api.md#schemasystem) |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### View system configuration <a id="opIdgetSystem"></a>

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

#### Responses <a id="view-system-configuration-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | [SystemConfig](goloop_admin_api.md#schemasystemconfig) |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Configure system <a id="opIdgetSystemConfiguration"></a>

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

#### Parameters <a id="configure-system-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| body | body | [ConfigureParam](goloop_admin_api.md#schemaconfigureparam) | true | key-value to configure |

#### Responses <a id="configure-system-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### List Backups <a id="opIdconfigureSystem"></a>

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

#### Responses <a id="list-backups-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | [BackupList](goloop_admin_api.md#schemabackuplist) |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Restore Status <a id="opIdgetBackups"></a>

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

#### Responses <a id="restore-status-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | [RestoreStatus](goloop_admin_api.md#schemarestorestatus) |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Start Restore <a id="opIdgetRestoreStatus"></a>

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

#### Parameters <a id="start-restore-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| body | body | [RestoreParam](goloop_admin_api.md#schemarestoreparam) | true | Name of backup and options |

#### Responses <a id="start-restore-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Stop Restore <a id="opIdstartRestore"></a>

> Code samples

`DELETE /system/restore`

Stop restoring operation

#### Responses <a id="stop-restore-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

## chain <a id="node-management-api-chain"></a>

Chain Management

### List Chains <a id="opIdstopRestore"></a>

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

#### Responses <a id="list-chains-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | Inline |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

#### Response Schema <a id="list-chains-responseschema"></a>

Status Code **200**

_array of chains_

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| _anonymous_ | \[[Chain](goloop_admin_api.md#schemachain)\] | false | none | array of chains |
| » cid | string\("0x" + lowercase HEX string\) | false | none | chain-id of chain |
| » nid | string\("0x" + lowercase HEX string\) | false | none | network-id of chain |
| » channel | string | false | none | chain-alias of node |
| » height | integer\(int64\) | false | none | block height of chain |
| » state | string | false | none | state of chain |
| » lastError | string | false | none | last error of chain |

 This operation does not require authentication

### Join Chain <a id="opIdlistChain"></a>

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

#### Parameters <a id="join-chain-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| body | body | object | true | Genesis-Storage zip file and json encoded chain-configuration for join chain using multipart |
| » json | body | [ChainConfig](goloop_admin_api.md#schemachainconfig) | true | json encoded chain-configuration, using multipart 'Content-Disposition: name=json' |
| »» dbType | body | string | false | Name of database system, ReadOnly |
| »» seedAddress | body | string | false | List of Seed ip-port, Comma separated string, Runtime-Configurable |
| »» role | body | integer | false | Role: |
| »» concurrencyLevel | body | integer | false | Maximum number of executors to use for concurrency |
| »» normalTxPool | body | integer | false | Size of normal transaction pool |
| »» patchTxPool | body | integer | false | Size of patch transaction pool |
| »» maxBlockTxBytes | body | integer | false | Max size of transactions in a block |
| »» nodeCache | body | string | false | Node cache: |
| »» channel | body | string | false | Chain-alias of node |
| »» secureSuites | body | string | false | Supported Secure suites with order \(none,tls,ecdhe\) - Comma separated string |
| »» secureAeads | body | string | false | Supported Secure AEAD with order \(chacha,aes128,aes256\) - Comma separated string |
| »» defaultWaitTimeout | body | integer | false | Default wait timeout in milli-second\(0:disable\) |
| »» maxWaitTimeout | body | integer | false | Max wait timeout in milli-second\(0:uses same value of defaultWaitTimeout\) |
| »» txTimeout | body | integer | false | Transaction timeout in milli-second\(0:uses system default value\) |
| »» autoStart | body | boolean | false | Start the chain automatically on node start |
| » genesisZip | body | string\(binary\) | true | Genesis-Storage zip file, using multipart 'Content-Disposition: name=genesisZip' |

**Detailed descriptions**

**»» role**: Role:

* `0` - None
* `1` - Seed
* `2` - Validator
* `3` - Seed and Validator

  Runtime-Configurable

**»» nodeCache**: Node cache:

* `none` - No cache
* `small` - Memory Lv1 ~ Lv5 for all
* `large` - Memory Lv1 ~ Lv5 for all and File Lv6 for store

**Enumerated Values**

| Parameter | Value |
| :--- | :--- |
| »» dbType | badgerdb |
| »» dbType | goleveldb |
| »» dbType | boltdb |
| »» dbType | mapdb |
| »» role | 0 |
| »» role | 1 |
| »» role | 2 |
| »» role | 3 |
| »» nodeCache | none |
| »» nodeCache | small |
| »» nodeCache | large |

> Example responses
>
> 200 Response

#### Responses <a id="join-chain-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | [ChainID](goloop_admin_api.md#schemachainid) |
| 409 | [Conflict](https://tools.ietf.org/html/rfc7231#section-6.5.8) | Conflict | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Inspect Chain

> Code samples

`GET /chain/{cid}`

Return low-level information about a chain.

#### Parameters <a id="inspect-chain-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | path | string\("0x" + lowercase HEX string\) | true | chain-id of chain |
| format | query | string | false | Format the output using the given Go template |
| informal | query | boolean | false | Inspect with informal data |

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

#### Responses <a id="inspect-chain-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | [ChainInspect](goloop_admin_api.md#schemachaininspect) |
| 404 | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) | Not Found | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Leave Chain <a id="opIdgetChain"></a>

> Code samples

`DELETE /chain/{cid}`

Leave Chain.

#### Parameters <a id="leave-chain-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | path | string\("0x" + lowercase HEX string\) | true | chain-id of chain |

#### Responses <a id="leave-chain-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | None |
| 404 | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) | Not Found | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Start Chain <a id="opIdleaveChain"></a>

> Code samples

`POST /chain/{cid}/start`

Start Chain.

#### Parameters <a id="start-chain-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | path | string\("0x" + lowercase HEX string\) | true | chain-id of chain |

#### Responses <a id="start-chain-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | None |
| 404 | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) | Not Found | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Stop Chain <a id="opIdstartChain"></a>

> Code samples

`POST /chain/{cid}/stop`

Stop Chain.

#### Parameters <a id="stop-chain-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | path | string\("0x" + lowercase HEX string\) | true | chain-id of chain |

#### Responses <a id="stop-chain-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | None |
| 404 | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) | Not Found | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Reset Chain <a id="opIdstopChain"></a>

> Code samples

`POST /chain/{cid}/reset`

Reset Chain.

#### Parameters <a id="reset-chain-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | path | string\("0x" + lowercase HEX string\) | true | chain-id of chain |

#### Responses <a id="reset-chain-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | None |
| 404 | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) | Not Found | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Import Chain <a id="opIdresetChain"></a>

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

#### Parameters <a id="import-chain-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | path | string\("0x" + lowercase HEX string\) | true | chain-id of chain |
| body | body | [ChainImportParam](goloop_admin_api.md#schemachainimportparam) | true | none |

#### Responses <a id="import-chain-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | None |
| 404 | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) | Not Found | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Prune Chain <a id="opIdimportChain"></a>

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

#### Parameters <a id="prune-chain-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | path | string\("0x" + lowercase HEX string\) | true | chain-id of chain |
| body | body | [PruneParam](goloop_admin_api.md#schemapruneparam) | true | none |

#### Responses <a id="prune-chain-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | None |
| 404 | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) | Not Found | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Backup Chain <a id="opIdpruneChain"></a>

> Code samples

`POST /chain/{cid}/backup`

Backup chain data to the specific file

#### Parameters <a id="backup-chain-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | path | string\("0x" + lowercase HEX string\) | true | chain-id of chain |

#### Responses <a id="backup-chain-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | None |
| 404 | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) | Not Found | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Download Genesis-Storage <a id="opIdbackupChain"></a>

> Code samples

`GET /chain/{cid}/genesis`

Download Genesis-Storage zip file

#### Parameters <a id="download-genesis-storage-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | path | string\("0x" + lowercase HEX string\) | true | chain-id of chain |

> Example responses
>
> 200 Response

#### Responses <a id="download-genesis-storage-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | string |
| 404 | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) | Not Found | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### View chain configuration <a id="opIdgetChainGenesis"></a>

> Code samples

`GET /chain/{cid}/configure`

Return chain configuration.

#### Parameters <a id="view-chain-configuration-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | path | string\("0x" + lowercase HEX string\) | true | chain-id of chain |

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

#### Responses <a id="view-chain-configuration-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | [ChainConfig](goloop_admin_api.md#schemachainconfig) |
| 404 | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) | Not Found | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

### Configure chain <a id="opIdgetChainConfiguration"></a>

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

#### Parameters <a id="configure-chain-parameters"></a>

| Name | In | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | path | string\("0x" + lowercase HEX string\) | true | chain-id of chain |
| body | body | [ConfigureParam](goloop_admin_api.md#schemaconfigureparam) | true | key-value to configure |

#### Responses <a id="configure-chain-responses"></a>

| Status | Meaning | Description | Schema |
| :--- | :--- | :--- | :--- |
| 200 | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success | None |
| 404 | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) | Not Found | None |
| 500 | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error | None |

 This operation does not require authentication

## Schemas <a id="opIdconfigureChain"></a>

### ChainID <a id="tocSchainid"></a>

```javascript
"0x782b03"
```

_chain-id of chain, "0x" + lowercase HEX string_

#### Properties <a id="schemachainid"></a>

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| _anonymous_ | string | false | none | chain-id of chain, "0x" + lowercase HEX string |

### Chain <a id="tocSchain"></a>

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

#### Properties <a id="schemachain"></a>

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| cid | string\("0x" + lowercase HEX string\) | false | none | chain-id of chain |
| nid | string\("0x" + lowercase HEX string\) | false | none | network-id of chain |
| channel | string | false | none | chain-alias of node |
| height | integer\(int64\) | false | none | block height of chain |
| state | string | false | none | state of chain |
| lastError | string | false | none | last error of chain |

### ChainInspect <a id="tocSchaininspect"></a>

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

#### Properties <a id="schemachaininspect"></a>

_allOf_

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| _anonymous_ | [Chain](goloop_admin_api.md#schemachain) | false | none | none |

_and_

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| _anonymous_ | object | false | none | none |
| » genesisTx | object | false | none | Genesis Transaction |
| » config | [ChainConfig](goloop_admin_api.md#schemachainconfig) | false | none | none |
| » module | object | false | none | none |
| »» **additionalProperties** | object | false | none | none |

### ChainConfig <a id="tocSchainconfig"></a>

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

#### Properties <a id="schemachainconfig"></a>

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| dbType | string | false | none | Name of database system, ReadOnly |
| seedAddress | string | false | none | List of Seed ip-port, Comma separated string, Runtime-Configurable |
| role | integer | false | none | Role:   _`0` - None_   `1` - Seed   _`2` - Validator_   `3` - Seed and Validator Runtime-Configurable |
| concurrencyLevel | integer | false | none | Maximum number of executors to use for concurrency |
| normalTxPool | integer | false | none | Size of normal transaction pool |
| patchTxPool | integer | false | none | Size of patch transaction pool |
| maxBlockTxBytes | integer | false | none | Max size of transactions in a block |
| nodeCache | string | false | none | Node cache:   _`none` - No cache_   `small` - Memory Lv1 ~ Lv5 for all  \* `large` - Memory Lv1 ~ Lv5 for all and File Lv6 for store |
| channel | string | false | none | Chain-alias of node |
| secureSuites | string | false | none | Supported Secure suites with order \(none,tls,ecdhe\) - Comma separated string |
| secureAeads | string | false | none | Supported Secure AEAD with order \(chacha,aes128,aes256\) - Comma separated string |
| defaultWaitTimeout | integer | false | none | Default wait timeout in milli-second\(0:disable\) |
| maxWaitTimeout | integer | false | none | Max wait timeout in milli-second\(0:uses same value of defaultWaitTimeout\) |
| txTimeout | integer | false | none | Transaction timeout in milli-second\(0:uses system default value\) |
| autoStart | boolean | false | none | Start the chain automatically on node start |

**Enumerated Values**

| Property | Value |
| :--- | :--- |
| dbType | badgerdb |
| dbType | goleveldb |
| dbType | boltdb |
| dbType | mapdb |
| role | 0 |
| role | 1 |
| role | 2 |
| role | 3 |
| nodeCache | none |
| nodeCache | small |
| nodeCache | large |

### ChainImportParam <a id="tocSchainimportparam"></a>

```javascript
{
  "dbPath": "/path/to/database",
  "height": 1
}
```

#### Properties <a id="schemachainimportparam"></a>

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| dbPath | string | true | none | Database path |
| height | int64 | true | none | Block Height |

### System <a id="tocSsystem"></a>

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

#### Properties <a id="schemasystem"></a>

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| buildVersion | string | false | none | build version |
| buildTags | string | false | none | buildTags |
| setting | object | false | none | none |
| » address | string | false | none | wallet address |
| » p2p | string | false | none | p2p address |
| » p2pListen | string | false | none | p2p listen address |
| » rpcAddr | string | false | none | Listen ip-port of JSON-RPC |
| » rpcDump | boolean | false | none | JSON-RPC Request, Response Dump flag |
| config | [SystemConfig](goloop_admin_api.md#schemasystemconfig) | false | none | none |

### SystemConfig <a id="tocSsystemconfig"></a>

```javascript
{
  "eeInstances": 1,
  "rpcDefaultChannel": "",
  "rpcIncludeDebug": false
}
```

#### Properties <a id="schemasystemconfig"></a>

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| eeInstances | integer | false | none | eeInstances |
| rpcDefaultChannel | string | false | none | default channel for legacy api |
| rpcIncludeDebug | boolean | false | none | JSON-RPC Response with detail information |

### ConfigureParam <a id="tocSconfigureparam"></a>

```javascript
{
  "key": "string",
  "value": "string"
}
```

#### Properties <a id="schemaconfigureparam"></a>

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| key | string | true | none | configuration field name |
| value | string | true | none | configuration value |

### PruneParam <a id="tocSpruneparam"></a>

```javascript
{
  "dbType": "goleveldb",
  "height": 1
}
```

#### Properties <a id="schemapruneparam"></a>

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| dbType | string | false | none | Database type |
| height | int64 | true | none | Block Height |

### BackupList <a id="tocSbackuplist"></a>

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

#### Properties <a id="schemabackuplist"></a>

_None_

### RestoreStatus <a id="tocSrestorestatus"></a>

```javascript
{
  "name": "0x178977_0x1_1_20200715-111057.zip",
  "overwrite": true,
  "state": "started 23/128"
}
```

#### Properties <a id="schemarestorestatus"></a>

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| state | string | true | none | State of the job \(stopped, started N/T, stopping, failed, success\) |
| name | string | false | none | Name of backup |
| overwrite | boolean | false | none | Whether it replaces existing chain data |

### RestoreParam <a id="tocSrestoreparam"></a>

```javascript
{
  "name": "0x178977_0x1_1_20200715-111057.zip",
  "overwrite": true
}
```

#### Properties <a id="schemarestoreparam"></a>

| Name | Type | Required | Restrictions | Description |
| :--- | :--- | :--- | :--- | :--- |
| name | string | true | none | Name of the backup to restore |
| overwrite | boolean | false | none | Whether it replaces existing chain |

