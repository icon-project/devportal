# Blockchain Transmission Protocol (BTP)

The blockchain transmission protocol (BTP) is the ICON project's bespoke cross-chain communication protocol for heterogenous blockchains.

dApps interact with BTP through the xCall contract. Testnet xCall contract addresses are listed below. For more information on xCall, see the [xCall section](../xcall/) and [standard](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md)

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
* Start Height (Block):
  * BSC: `6229774`
  * ETH2: `6229184`

Contracts

| Contract  | Address                                    | Note        |
| --------- | ------------------------------------------ | ----------- |
| BMC       | cxf1b0808f09138fffdb890772315aeabb37072a8a |             |
| BSC BMV   | cx57b9817bfa313afeffc459b19e8dab973f7b4e82 | Bridge Mode |
| ETH2 BMV  | cx495e239a7a365459926e1f2c20b3cb6d42fe15e2 | Bridge Mode |
| XCALL     | cxf4958b242a264fc11d7d8d95f79035e35b21c1bb |             |
| Demo DAPP | cx92283a47a95164bd3d604da08128886125593545 |             |

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
| BMV       | 0xAfDa56289E92338514B5aBF0bf22A7dcA60d7e09 | Trustless |
| XCALL     | 0x6193c0b12116c4963594761d859571b9950a8686 |           |
| Demo DAPP | 0xb072b7d60C143944fdBb45E22A3ae04DcD0b7432 |           |

#### ETH2 Testnet Testnet

Domain

* RPC URL: `https://sepolia.infura.io/v3/ffbf8ebe228f4758ae82e175640275e0`
* Network: `0xaa36a7.eth2`
* Start Height (Block): 3185472

Contracts

| Contract  | Address                                    | Note      |
| --------- | ------------------------------------------ | --------- |
| BMCM      | 0x39FBbE3AeCbe6ED08baf16e13eFE4aA31550CaA2 |           |
| BMCS      | 0xd6298BBB8b8B8EA273C3CB470B273A1cef552Ef3 |           |
| BMC       | 0x50DD9479c45085dC64c6F0a0796040C7768f25CE |           |
| BMV       | 0x684ba8F34f9481f7F02aCd4F143506E11AC19E3E | Trustless |
| XCALL     | 0x9B68bd3a04Ff138CaFfFe6D96Bc330c699F34901 |           |
| Demo DAPP | 0x0FcBB34A8468CaA65d3f81CAef8C42E43043687c |           |

Reference link: [https://github.com/iconloop/btp2-testnet](https://github.com/iconloop/btp2-testnet)

### Resources

* BTP Litepaper: [https://icon.community/assets/btp-litepaper.pdf](https://icon.community/assets/btp-litepaper.pdf)
* ICON BTP Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md)
* ICON BTP Fee Gathering Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-35.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-35.md)
* ICON BTP Message Fragmentation Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-40.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-40.md)
* ICON BTP Arbitrary Call Service Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md)
