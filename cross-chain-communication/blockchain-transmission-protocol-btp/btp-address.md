# BTP Address

A BTP Address is a string of URL for locating an account of the blockchain network.

Example:

```
btp://<Network Address>/<Account Identifier>
```

### Network Identifier

A string to identify blockchain network

```
<NID>.<Network System>
```

**NID**: ID of the network in the blockchain network system.

**Network System**: Short name of the blockchain network system.

| Name     | Description           |
| -------- | --------------------- |
| icon     | ICON Mainnet (goloop) |
| moonbeam | Moonbeam              |

**Examples:**

| Network Address | Description                                           |
| --------------- | ----------------------------------------------------- |
| `0x1.icon`      | ICON Network with nid="0x1" (mainnet)                 |
| `0x2.icon`      | ICON sub network with nid="0x2" (sub network on ICON) |
| `0x5.moonbeam`  | Moonbeam network with nid="0x5"                       |

### Account Identifier

**Account Identifier**: Identifier of the account including smart contract. It should be composed of URL safe characters except "."(dot).

```
btp://0x1.icon/hxc0007b426f8880f9afbab72fd8c7817f0d3fd5c0
btp://0x2.icon/hx7ab72fd8c7812680f9afbab72fd8c7817f0d3fd5
btp://0x5.moonbeam/0x5425F5d4ba2B7dcb277C369cCbCb5f0E7185FB41
```

### Reference:

[https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md#btp-address](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md#btp-address)
