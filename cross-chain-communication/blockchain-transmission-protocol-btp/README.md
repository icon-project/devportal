# Blockchain Transmission Protocol (BTP)

The blockchain transmission protocol (BTP) is the ICON project's bespoke cross-chain communication protocol for heterogenous blockchains.

dApps interact with BTP through the xCall contract. Testnet xCall contract addresses are listed below. For more information on xCall, see the [xCall section](../../getting-started/how-to-send-a-cross-chain-message/#resources-2) and [standard](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md)

A sample dApp is provided for demonstration purposes.

### BTP Testnet Network Information

This section contains BTP's official deployment information.

> BTP is currently under active development and is subject to significant outages. If you want to develop with BTP at this time, we recommend using the local setup found here [https://github.com/icon-project/btp2/tree/main/e2edemo](https://github.com/icon-project/btp2/tree/main/e2edemo)

#### ICON Berlin Testnet

Domain

* RPC URL: `https://berlin.net.solidwallet.io/api/v3/icon_dex`
* Network: `0x7.icon`
* BTP Network ID:
  * BSC: `0x1`
  * ETH2: `0x2`
  * Havah: `0x3`
* Start Height (Block):
  * BSC: `6229774`
  * ETH2: `6229184`
  * Havah: `8582402`

Contracts

| Contract  | Address                                    | Note                                |
| --------- | ------------------------------------------ | ----------------------------------- |
| BMC       | cxf1b0808f09138fffdb890772315aeabb37072a8a |                                     |
| BSC BMV   | cx7af8b64f516f804258526fa70e12be96d65fb06d | Trustless Mode Supports Planck-fork |
| ETH2 BMV  | cxbab382c24c5e492d45cf11c0b07fd36e8344e96f | Trustless Mode Supports Capella     |
| Havah BMV | cxd1bd0add3c7707fa875bf5370da46875365f44cf | Trustless Mode                      |
| XCALL     | cxf4958b242a264fc11d7d8d95f79035e35b21c1bb |                                     |
| Demo DAPP | cx92283a47a95164bd3d604da08128886125593545 |                                     |

#### BSC Testnet Testnet

Domain

* RPC URL: `https://data-seed-prebsc-1-s1.binance.org:8545`
* Network: `0x61.bsc`
* Start Height (Block): `28463560`

Contracts

|           |                                            |           |
| --------- | ------------------------------------------ | --------- |
| BMCM      | 0xFd82803c9b2E92C628846012c6E5016Ac380f68d |           |
| BMCS      | 0x6AB5fB039ABbEE20bf43F84393E528015686fB04 |           |
| BMC       | 0x193eD92257E0773ccBA097e0ba4110E588eb0F1c |           |
| BMV       | 0xFCDD2AB0D0D98c3f74db20a0913c7e3B264dF8a1 | Trustless |
| XCALL     | 0x6193c0b12116c4963594761d859571b9950a8686 |           |
| Demo DAPP | 0xb072b7d60C143944fdBb45E22A3ae04DcD0b7432 |           |

#### ETH2 Sepolia Testnet

Domain

* RPC URL: `https://sepolia.infura.io/v3/ffbf8ebe228f4758ae82e175640275e0`
* Network: `0xaa36a7.eth2`
* Start Height (Block): `3185472`

Contracts

| Contract  | Address                                    | Note |
| --------- | ------------------------------------------ | ---- |
| BMCM      | 0x39FBbE3AeCbe6ED08baf16e13eFE4aA31550CaA2 |      |
| BMCS      | 0xd6298BBB8b8B8EA273C3CB470B273A1cef552Ef3 |      |
| BMC       | 0x50DD9479c45085dC64c6F0a0796040C7768f25CE |      |
| BMV       | 0x684ba8F34f9481f7F02aCd4F143506E11AC19E3E |      |
| XCALL     | 0x9B68bd3a04Ff138CaFfFe6D96Bc330c699F34901 |      |
| Demo DAPP | 0x0FcBB34A8468CaA65d3f81CAef8C42E43043687c |      |

#### Havah Testnet

Domain

* RPC URL: `https://btp.vega.havah.io/api/v3/icon_dex`
* Network: `0x111.icon`
* BTP NetworkId:
  * Berlin: `0x3`
* Start Height (Block):
  * Berlin: `3185472`

Contracts

| Contract   | Address                                    | Note           |
| ---------- | ------------------------------------------ | -------------- |
| BMC        | cx683a92f72cc2fe9a7a617019a8d6fcba6b6c06b7 |                |
| Berlin BMV | cx8737236f65ef6d41ac33b328bc758e8663366caa | Trustless Mode |
| XCALL      | cx05b5f4e2cb80827d4b53d13549041c66442f327d |                |
| Demo DAPP  | cxd1531ebfc81cb890810bc0e5603c1e27046174e3 |                |

Reference link: [https://github.com/iconloop/btp2-testnet](https://github.com/iconloop/btp2-testnet)

### Resources

* BTP Litepaper: [https://icon.community/assets/btp-litepaper.pdf](https://icon.community/assets/btp-litepaper.pdf)
* ICON BTP Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md)
* ICON BTP Fee Gathering Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-35.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-35.md)
* ICON BTP Message Fragmentation Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-40.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-40.md)
* ICON BTP Arbitrary Call Service Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md)
