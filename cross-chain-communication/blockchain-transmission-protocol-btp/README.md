# Blockchain Transmission Protocol (BTP)

The blockchain transmission protocol (BTP) is the ICON project's bespoke cross-chain communication protocol for heterogenous blockchains.

dApps interact with BTP through the xCall contract. Testnet xCall contract addresses are listed below. For more information on xCall, see the [xCall section](broken-reference) and [standard](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md)

A sample dApp is provided for demonstration purposes.

### BTP Testnet Network Information

This section contains BTP's official deployment information.

> BTP is currently under active development and is subject to significant outages. If you want to develop with BTP at this time, we recommend using the local setup found here [https://github.com/icon-project/btp2/tree/main/e2edemo](https://github.com/icon-project/btp2/tree/main/e2edemo)

BTP2 Network Status Monitor - [Link](https://testnet.btp2.24x365.online/)

#### ICON Berlin Testnet

Domain

* RPC URL: `https://berlin.net.solidwallet.io/api/v3/icon_dex`
* Network: `0x7.icon`
* Tracker URL: [Berlin Tracker](https://berlin.tracker.solidwallet.io/)
* BTP Network ID:
  * BSC: `0x4`
  * ETH2: `0x6`
  * Havah: `0x7`
* Start Height (Block):
  * BSC: `9929680`
  * ETH2: `10349992`
  * Havah: `10361781`

Contracts

| Contract  | Address                                    | Note                            |
| --------- | ------------------------------------------ | ------------------------------- |
| BMC       | cxf1b0808f09138fffdb890772315aeabb37072a8a |                                 |
| BSC BMV   | cx6810e0a7d6c0bc53eef006c221dcd731c8903a95 | Bride Mode                      |
| ETH2 BMV  | cxdd91f194673553097745f33dd464a39740075735 | Trustless Mode Supports Capella |
| Havah BMV | cx90b6dca89aa45388b24a6c158eb9d21d51263037 | Trustless Mode                  |
| XCALL     | cxf4958b242a264fc11d7d8d95f79035e35b21c1bb |                                 |
| Demo DAPP | cx92283a47a95164bd3d604da08128886125593545 |                                 |

#### BSC Testnet Testnet

Domain

* RPC URL: `https://data-seed-prebsc-1-s1.binance.org:8545`
* Tracker URL: [BSC Testnet Tracker](https://testnet.bscscan.com/)
* Network: `0x61.bsc`
* Start Height (Block): `31022641`

Contracts

| Contract             | Address                                    | Note      |
| -------------------- | ------------------------------------------ | --------- |
| BMC                  | 0x9Fd9e050682A8795dEa6eE70870A82a513d390Ac |           |
| BMC Implementation   | 0x8E3eb11031b6316C2A8e85040ecb204040Dc2A69 |           |
| BMCM                 | 0x41CD95F16f9bbF2bEB5479C00CF249A8b0A076bF |           |
| BMCS                 | 0xCC98F0736ec2ef32B8A64251BB89aF14E27043b6 |           |
| BMV                  | 0x0a42d5c21EF16aec1c31c4511EdCaA9648a9538C | Trustless |
| XCALL                | 0x5Ebb7aCB7bCaf7C1ADeFcF9660D39AC07d432904 |           |
| XCALL Implementation | 0x3CFBbB1c3a83b1c5a8E94eC3e2229Ba7a03f3EAd |           |
| Demo DAPP            | 0x4EaC1CDBE8131de10FE1AE969397d02d47D21082 |           |

#### ETH2 Sepolia Testnet

Domain

* RPC URL: `https://sepolia.infura.io/v3/ffbf8ebe228f4758ae82e175640275e0`
* Tracker URL: [Sepolia Tracker](https://sepolia.etherscan.io/)
* Network: `0xaa36a7.eth2`
* Start Height (Block): `3813345`

Contracts

| Contract             | Address                                    | Note           |
| -------------------- | ------------------------------------------ | -------------- |
| BMC                  | 0xE602326106f5E1d436a3CCEB2A408759925f81ff |                |
| BMC Implementation   | 0x23810745feC53c8D80E9A3E30508458E408d3EB7 |                |
| BMCM                 | 0x9fb595461023f9A920B276A4b289972c4aFF114F |                |
| BMCS                 | 0xEe94cBA4C4d138fb4de1F4bcfA1CEeF062eE8251 |                |
| BMV                  | 0x1592F432Dde573341BaFe14d5FAbe4A299b2E721 | Trustless Mode |
| XCALL                | 0x694C1f5Fb4b81e730428490a1cE3dE6e32428637 |                |
| XCALL Implementation | 0x7Fc0f3807ECAD54eFCCb6ED686a788955fe0958f |                |
| Demo DAPP            | 0x597F73bfb3124B6145151E7a8A30b781C41FF2B0 |                |

#### Havah Testnet

Domain

* RPC URL: `https://ctz.altair.havah.io/api/v3/icon_dex`
* Network: `0x111.icon`
* Tracker URL: [Havah Testnet Tracker](https://scan.altair.havah.io/)
* BTP NetworkId:
  * Berlin: `0x1`
* Start Height (Block):
  * Berlin: `9443`

Contracts

| Contract  | Address                                    | Note           |
| --------- | ------------------------------------------ | -------------- |
| BMC       | cxf36efc770627067c41949a16688a0246ea6428e8 |                |
| BMV       | cx90f9d84e0b757ba02599fabcbda30e46105cb89c | Trustless Mode |
| XCALL     | cxce3a72cc1defaf07b23e05b595840c00d5a80b0c |                |
| Demo DAPP | cxc9f0ba03c4a39a43c5dc0a5ae5d2d1327a065d62 |                |

Reference link: [https://github.com/iconloop/btp2-testnet](https://github.com/iconloop/btp2-testnet)

### Resources

* BTP Litepaper: [https://icon.community/assets/btp-litepaper.pdf](https://icon.community/assets/btp-litepaper.pdf)
* ICON BTP Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md)
* ICON BTP Fee Gathering Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-35.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-35.md)
* ICON BTP Message Fragmentation Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-40.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-40.md)
* ICON BTP Arbitrary Call Service Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md)
