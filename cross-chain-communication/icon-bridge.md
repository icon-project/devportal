# ICON Bridge

The ICON Bridge is a centralized bridge for Blockchain Transmission Protocol(BTP) Relay System which can be used to transfer tokens across multiple chains. Currently, it supports cross chain transfer from ICON and Binance Smart Chain (BSC).

The main components of icon bridge are:

* **BTP Message Relay (BMR)**: It serves to relay BTP Message across connected chains and monitor BTP events
* **Contracts**:
  * **BTP Message Center (BMC)**
    * Receive BTP messages through transactions.
    * Send BTP messages through events.
  * **BTP Service Handler (BSH)**
    * Services that can be serviced by ICON-Bridge
    * BTP Token Service (BTS) is a BSH that is responsible for token transfers cross chain.
    * Currently, BTS is the only service handler for icon bridge
    * Handle service messages related to the service
    * Send service messages through the BMC

The main distinction between the blockchain transmission protocol (BTP) and the ICON Bridge is that the ICON Bridge does not verify state on-chain. Instead, the BTP Message Verifier contract is replaced with off-chain verification on the relay. This means that the relay is operated centrally and trusted.

The official ICON Bridge relay is privately operated by Parameta. However, the code is open-source so anyone can deploy their own set of contracts and host their own relay to connect those contracts.

### ICON Bridge Source Code Audit

The source code for the ICON Bridge has been audited by FYEO. The audit report can be found here [https://github.com/icon-project/icon-bridge/blob/main/docs/ICON\_Foundation\_-\_Security\_Assessment\_of\_ICON\_Bridge\_BTP\_v1.2.pdf](https://github.com/icon-project/icon-bridge/blob/main/docs/ICON\_Foundation\_-\_Security\_Assessment\_of\_ICON\_Bridge\_BTP\_v1.2.pdf).

### ICON Bridge Testnet Network Information

#### **ICON**

| Network Parameter | Value                                                                                                    |
| ----------------- | -------------------------------------------------------------------------------------------------------- |
| URI               | [https://lisbon.net.solidwallet.io/api/v3/icon\_dex](https://lisbon.net.solidwallet.io/api/v3/icon\_dex) |
| Network ID        | 0x2                                                                                                      |
| BTP Network ID    | 0x2.icon                                                                                                 |
| Block Height      | 16561180                                                                                                 |

| Smart Contract Name | Address                                    |
| ------------------- | ------------------------------------------ |
| sICX                | cx3044ad389267b50eb3c57103eade0c5a72261c1a |
| bnUSD               | cx7f45afe9d8ce95e80c1be7c4eef2ea0dd843c4e3 |
| BNB                 | cxcea1078c39e8b887692d3ccdd81bd711a6260ea5 |
| BUSD                | cxea67f5fe1d1f7e1d29d54f185f0585b8262c788e |
| USDT                | cxac717247714a0b8e2b9038fdadfdcc0f033e325b |
| USDC                | cxd840ae3c79c1366895747aa8c228bd7e3459032f |
| BTCB                | cx63be8619af9cdf1cb053ccde7642ae974648a8c1 |
| ETH                 | cx4b9cd9bb520b08d14c19c5035295f7e44003e42f |
| bmc                 | cxcc165238ae0e894835e88f549f22e520c7ad740f |
| bts                 | cx949e9e242305309ed234c19183e9ed6e8f44ee73 |

#### **BSC**

| Network Parameter | Value                                                                |
| ----------------- | -------------------------------------------------------------------- |
| URI               | [https://bsc-dataseed.binance.org](https://bsc-dataseed.binance.org) |
| Network ID        | 0x38                                                                 |
| BTP Network ID    | 0x38.bsc                                                             |
| Block Height      | 20493051                                                             |

| Smart Contract Name | Address                                    |
| ------------------- | ------------------------------------------ |
| sICX                | 0xBBE70cE3dAe164a188a47e6Be898F09D29AFdF74 |
| bnUSD               | 0x4F6f26967a882c12a03DAe27272Ed0fd85A94443 |
| ICX                 | 0x0C8773fa9A67291e089cB8136Abb1bcb0Aae220F |
| BUSD                | 0xED41B3B136a96c867Ee265cC8a79a8ea39eeC9C4 |
| USDT                | 0x8dE8FaF129d5BD9844dbc92024907d48B415987C |
| USDC                | 0x9DDBcf279D1D01C32A2c13efCB6415f37416857F |
| BTCB                | 0x299Fb600FB51A208d3c268Da187539a59bE40041 |
| ETH                 | 0xd49a76cF9a79F13deaAcB789039e3ef76C4c1c5F |
| BMCManagement       | 0x9Ab68EB48423AF80d7BDAffd7Ad976f69aa67e37 |
| BMCPeriphery        | 0x99B22952e4D37d46046c46cD9F91cE1cdfB0605B |
| BTSCore             | 0x9fCBAD6F4C9dC2C0b109408E39cf042B9b2aE65A |
| BTSPeriphery        | 0xd75A671A5196459b13c97424B0C275D51D2C3488 |
| BTSOwnerManager     | 0xa49Cd5E485d63b5A271759DbAa17Fe05d14DAeDe |

### ICON Bridge Mainnet Network Information

#### **ICON**

| Network Parameter | Value                                                                                      |
| ----------------- | ------------------------------------------------------------------------------------------ |
| URI               | [https://ctz.solidwallet.io/api/v3/icon\_dex](https://ctz.solidwallet.io/api/v3/icon\_dex) |
| Network ID        | 0x1                                                                                        |
| BTP Network ID    | 0x1.icon                                                                                   |
| Block Height      | 54062001                                                                                   |

| Smart Contract Name | Address                                    |
| ------------------- | ------------------------------------------ |
| sICX                | cx2609b924e33ef00b648a409245c7ea394c467824 |
| bnUSD               | cx88fd7df7ddff82f7cc735c871dc519838cb235bb |
| BNB                 | cx077807f2322aeb42ea19a1fcc0c9f3d3f35e1461 |
| BUSD                | cxb49d82c46be6b61cab62aaf9824b597c6cf8a25d |
| USDT                | cx8e4d9b4164618f796d493a8154f1f17ad75f11bb |
| USDC                | cx532e4235f9004c233604c1be98ca839cd777d58c |
| BTCB                | cx5b5a03cb525a1845d0af3a872d525b18a810acb0 |
| ETH                 | cx288d13e1b63563459a2ac6179f237711f6851cb5 |
| bmc                 | cx23a91ee3dd290486a9113a6a42429825d813de53 |
| bts                 | cxcef70e92b89f2d8191a0582de966280358713c32 |

#### **BSC**

| Network Parameter | Value                                                                |
| ----------------- | -------------------------------------------------------------------- |
| URI               | [https://bsc-dataseed.binance.org](https://bsc-dataseed.binance.org) |
| Network ID        | 0x38                                                                 |
| BTP Network ID    | 0x38.bsc                                                             |
| Block Height      | 20493051                                                             |

| Smart Contract Name | Address                                    |
| ------------------- | ------------------------------------------ |
| sICX                | 0x33acDF0Fe57C531095F6bf5a992bF5aA81c94Acf |
| bnUSD               | 0xa804D2e9221057099eF331AE1c0D6616cC27d770 |
| ICX                 | 0x9b7b6A964f8870699Ae74744941663D257b0ec1f |
| BUSD                | 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56 |
| USDT                | 0x55d398326f99059fF775485246999027B3197955 |
| USDC                | 0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d |
| BTCB                | 0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c |
| ETH                 | 0x2170Ed0880ac9A755fd29B2688956BD959F933F8 |
| BMCManagement       | 0xe221e50fbe2Ba54b1898b4c02F66bf9598fbD1dB |
| BMCPeriphery        | 0x034AaDE86BF402F023Aa17E5725fABC4ab9E9798 |
| BTSCore             | 0x7A4341Af4995884546Bcf7e09eB98beD3eD26D28 |
| BTSPeriphery        | 0x556CA2d717d366A448c118D14e94a744b3c6578c |
| BTSOwnerManager     | 0x030d1c1B98d15b4d1F9c7C57068E3dAfCFb73dC4 |

For more in-depth information about ICON Bridge please visit the official repository: [https://github.com/icon-project/icon-bridge](https://github.com/icon-project/icon-bridge)

### Resources:

* [https://github.com/icon-project/icon-bridge](https://github.com/icon-project/icon-bridge)
* [https://icon.community/learn/icon-bridge/](https://icon.community/learn/icon-bridge/)
