# xCall

xCall is a standard interface to make permission-less calls between different blockchain networks. Each network that ICON is interoperable with has an xCall contract address that dApps can call to transfer data across chains.

Currently, permission-less calls are done using the blockchain transmission protocol (BTP). The testnet contract addresses for xCall can be found in the BTP section.

The ICON project plans to extend the interoperability protocols that work with xCall. For example, there is active development for [new integrations](https://github.com/icon-project/IBC-Integration) with the Cosmos inter-blockchain communication protocol (IBC) as smart contracts on ICON.

### xCall for developers

The following links showcase in detail the xCall implementation of the BTP (Blockchain Transmission Protocol) for developers to interact and use it to send arbitrary messages between blockchains.

* [Sending a message with xCall without rollback.](sending-a-message-with-xcall.md)

To showcase the functionality of xCall these examples use a sample DApp that will make use of the BTP protocol in a local environment consistent of a local ICON network and a local ganache blockchain that are going to simulate the ICON Network and any EVM network (Ethereum, BSC, etc).

as a prerequisite, to setup these networks please follow the tutorial in the following link:

* [BTP Tutorial: Setting up BTP locally for testing](https://icon.community/tutorials/btp-tutorial-setting-up-btp-locally-for-testing/)&#x20;

#### Resources

* End-to-end demonstration of BTP containing xCall: [https://github.com/icon-project/btp2/tree/main/e2edemo](https://github.com/icon-project/btp2/tree/main/e2edemo)
* xCall Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md)
* IBC on ICON: [https://github.com/icon-project/IBC-Integration](https://github.com/icon-project/IBC-Integration)
* [xCall testnet contracts](../blockchain-transmission-protocol-btp/)
