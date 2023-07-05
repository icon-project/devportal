# How to send a cross-chain message

### Introduction <a href="#resources" id="resources"></a>

In this tutorial, you’ll learn how to setup a local cross-chain environment and send cross-chain messages. The cross-chain messaging system is called the Blockchain Transmission Protocol (BTP) and the contract interface to interact with the system is called xCall.

### Blockchain Transmission Protocol <a href="#resources" id="resources"></a>

Blockchain Transmission Protocol (BTP) is ICON’s chain-agnostic, scalable, and secure interoperability protocol. BTP’s chain-agnostic design allows it to be integrated with any smart contract-enabled blockchain. Unlike traditional bridging solutions that rely on handpicked validators to relay cross-chain messages and custody funds, BTP uses a more secure model with fully-decentralized incentivized relays and on-chain verification of messages.

### xCall <a href="#resources" id="resources"></a>

xCall is a standard interface to make permission-less calls between different blockchain networks. Each network that ICON is interoperable with has an xCall contract address that dApps can call to transfer data across chains.

Currently, permission-less calls are done using the blockchain transmission protocol (BTP). The testnet contract addresses for xCall can be found in the BTP section.

The ICON project plans to extend the interoperability protocols that work with xCall. For example, there is active development for [new integrations](https://github.com/icon-project/IBC-Integration) with the Cosmos inter-blockchain communication protocol (IBC) as smart contracts on ICON.

### Resources <a href="#resources" id="resources"></a>

* BTP Github repository: [https://github.com/icon-project/btp2](https://github.com/icon-project/btp2)
* BTP documentation: [https://docs.icon.community/cross-chain-communication/blockchain-transmission-protocol-btp](https://docs.icon.community/cross-chain-communication/blockchain-transmission-protocol-btp)
* BTP Litepaper: [https://icon.community/assets/btp-litepaper.pdf](https://icon.community/assets/btp-litepaper.pdf)
* ICON BTP Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md)
* ICON BTP Fee Gathering Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-35.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-35.md)
* ICON BTP Message Fragmentation Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-40.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-40.md)
* ICON BTP Arbitrary Call Service Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md)
