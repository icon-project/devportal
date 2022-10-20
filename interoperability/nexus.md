# Nexus

## What is Nexus?

Nexus - [https://nexusportal.io/](https://nexusportal.io/)

Nexus is an application that facilitiates token transfers between ICON and BNB Smart Chain. To do this, Nexus provides users with an intuitive frontend for interfacing with ICON Bridge, a cross-chain bridging solution developed by ICON and ICONLOOP.

When a user submits a token transfer transaction with Nexus, ICON Bridge locks the tokens on the source blockchain, and mints an equal amount of the token on the destination blockchain. For example, if a user transfers 10 sICX from ICON to BNB Smart Chain, ICON Bridge will lock 10 sICX (IRC-2) on ICON and mint 10 sICX (BEP-20 on BNB Smart Chain.

## An Overview of Nexus

The Nexus user interface has four primary points of interaction:

1. Transfer
2. History
3. Network
4. Connect a Wallet

### Transfer

The "Transfer" page is where you can send tokens between blockchains.

### History

The "History" page is where you can browse a historical overview of Nexus transfers. The page also supports sorting and filtering (assets, networks, and status), which makes it easy to find information about a specific class of transaction (e.g. all successful sICX transfers from ICON to BNB Smart Chain).

### Network

The "Network" page lists all the blockchains that are supported by Nexus.

### Connect a Wallet

The "Connect a Wallet" button brings up a modal for connecting a wallet to Nexus. Nexus currently supports **Hana on ICON**, and **MetaMask for EVM chains** like BNB Smart Chain.



## Testnet

In addition to mainnet, Nexus is also deployed on ICON's Lisbon testnet and the BNB Smart Chain testnet. To use Nexus on testnet, you'll need to complete two prerequisites.

1. [Configure Hana and MetaMask for testnet](https://icon.community/blog/2022/nexus-beta-now-available-on-lisbon-testnet/#how-to-configure-wallets-for-nexus).
2. [Acquire testnet ICX and BNB to pay for transaction fees](https://icon.community/blog/2022/nexus-beta-now-available-on-lisbon-testnet/#how-to-get-testnet-icx-and-bnb).

## Supported Assets

Nexus currently supports the assets below on ICON and BNB Smart Chain.

### ICON (Mainnet)

| Name  | Source | Contract Address                             |
| ----- | ------ | -------------------------------------------- |
| ICX   | ICON   | `N/A`                                        |
| bnUSD | ICON   | `cx88fd7df7ddff82f7cc735c871dc519838cb235bb` |
| sICX  | ICON   | `cx2609b924e33ef00b648a409245c7ea394c467824` |
| BNB   | BSC    | `cx077807f2322aeb42ea19a1fcc0c9f3d3f35e1461` |
| BTCB  | BSC    | `cx5b5a03cb525a1845d0af3a872d525b18a810acb0` |
| BUSD  | BSC    | `cxb49d82c46be6b61cab62aaf9824b597c6cf8a25d` |
| ETH   | BSC    | `cx288d13e1b63563459a2ac6179f237711f6851cb5` |
| USDC  | BSC    | `cx532e4235f9004c233604c1be98ca839cd777d58c` |
| USDT  | BSC    | `cx8e4d9b4164618f796d493a8154f1f17ad75f11bb` |

### ICON (Lisbon Testnet)

| Name  | Source | Contract Address                             |
| ----- | ------ | -------------------------------------------- |
| ICX   | ICON   | `N/A`                                        |
| bnUSD | ICON   | `cx26ad29e85087ade6dc3418a17275f5e99851abf1` |
| sICX  | ICON   | `cx2609b924e33ef00b648a409245c7ea394c467824` |
| BNB   | BSC    | `cxbfe1004c4eb9094e59f157e9cdfeefc6bf004955` |
| BTCB  | BSC    | `cx1a7b11239a1e78132eb87c27b9efde3258cfe461` |
| BUSD  | BSC    | `cx43f55441db4e2d24e1781020465b212ec7627e68` |
| ETH   | BSC    | `cxa86504ccff5a2e759b180650b2fba53975ac2484` |
| USDC  | BSC    | `cx7cb84e3da09e51c61aca5b7bb8a1f70b4e4af31a` |
| USDT  | BSC    | `cx8c53539b68d2634607be11b323a33ec3392aac1b` |

### BNB Smart Chain (Mainnet)

| Name  | Source | Contract Address                             |
| ----- | ------ | -------------------------------------------- |
| BNB   | BSC    | `N/A`                                        |
| BTCB  | BSC    | `0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c` |
| BUSD  | BSC    | `0xe9e7C`EA3DedcA5984780Bafc599bD69ADd087D56 |
| ETH   | BSC    | `0x2170Ed0880ac9A755fd29B2688956BD959F933F8` |
| USDC  | BSC    | `0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d` |
| USDT  | BSC    | `cx8e4d9b4164618f796d493a8154f1f17ad75f11bb` |
| bnUSD | ICON   | `0xa804D2e9221057099eF331AE1c0D6616cC27d770` |
| ICX   | ICON   | `0x9b7b6A964f8870699Ae74744941663D257b0ec1f` |
| sICX  | ICON   | `0x33acDF0Fe57C531095F6bf5a992bF5aA81c94Acf` |

### BNB Smart Chain (Testnet)

| Name  | Source | Contract Address                             |
| ----- | ------ | -------------------------------------------- |
| BNB   | BSC    | `N/A`                                        |
| BTCB  | BSC    | `0x466B9c44771de5529D201fCCb6f7511B62Cb1A13` |
| BUSD  | BSC    | `0x001bA2DF00bB5BCA9E184dB9B1FaCDB58c22F48A` |
| ETH   | BSC    | `0xA849E49271A97F782142D6c759F395e42F1e3Fe9` |
| USDC  | BSC    | `0x1d44Ee3867FF08C23e016dF8B3c0e23ca127550C` |
| USDT  | BSC    | `0x7f661A255e4dbeDF3795957b35A3F1EdB4F0C7eC` |
| bnUSD | ICON   | `0x5e1E7c2E24FE558907Bdd6D56432D4843055891F` |
| ICX   | ICON   | `0x890E000C8771e0A3D855b9768219Ac733e04509b` |
| sICX  | ICON   | `0x83462D89C3563FbC0BAa4523aD49e4Cfef6657C9` |

## How to Submit Bug Reports for Nexus

As an open-source project, the Nexus codebase is fully transparent and can be viewed anytime on the [icon-project/Nexus GitHub repo](https://github.com/icon-project/Nexus). If you come across any bugs while using Nexus, we encourage you to **submit an issue directly on GitHub** – this helps streamline the bug fixing process for the Nexus developers. If you’re unsure about how to raise an issue on GitHub, [check out this guide](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue) to learn more.



