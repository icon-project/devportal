---
description: A standard interface for arbitrary cross-chain contract calls
---

# xCall

### Introduction <a href="#resources" id="resources"></a>

`xcall` streamlines the process of executing arbitrary cross-chain contract calls. In essence, it works as an interpreter, allowing different cross-chain message protocols to "speak the same language," even when their underlying technical architectures differ.

The service acts as an aggregator by accumulating and managing cross-chain message protocols. This aggregation model not only simplifies the implementation of cross-chain functionalities for dApps, but it also enhances scalability and flexibility for developers by abstracting the complexities associated with handling various cross-chain protocols.

Currently, the Blockchain Transmission Protocol (BTP) is the only cross-chain message protocol on ICON. As additional cross-chain message protocols come online, developers will be able to access them with the same entry point, the `xcall` interface.

To learn how to send a cross-chain message with `xcall`, see the [how-to-send-a-cross-chain-message](../getting-started/how-to-send-a-cross-chain-message/ "mention") section.

### Resources <a href="#resources" id="resources"></a>

* xCall Java API: [https://www.javadoc.io/doc/foundation.icon/btp2-xcall/latest/foundation/icon/btp/xcall/package-summary.html](https://www.javadoc.io/doc/foundation.icon/btp2-xcall/latest/foundation/icon/btp/xcall/package-summary.html)
* xCall Solidity Sender API: [https://github.com/icon-project/btp2-solidity/blob/main/library/contracts/interfaces/ICallService.sol](https://github.com/icon-project/btp2-solidity/blob/main/library/contracts/interfaces/ICallService.sol)
* xCall Solidity Receiver API: [https://github.com/icon-project/btp2-solidity/blob/main/library/contracts/interfaces/ICallServiceReceiver.sol](https://github.com/icon-project/btp2-solidity/blob/main/library/contracts/interfaces/ICallServiceReceiver.sol)
* xCall specification: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-52.md)
* xCall home page: [https://icon.community/xcall/](https://icon.community/xcall/)
* awesome-icon: [https://github.com/icon-community/awesome-icon#interoperability](https://github.com/icon-community/awesome-icon#interoperability)
* BTP Github repository: [https://github.com/icon-project/btp2](https://github.com/icon-project/btp2)
* BTP documentation: [https://docs.icon.community/cross-chain-communication/blockchain-transmission-protocol-btp](https://docs.icon.community/cross-chain-communication/blockchain-transmission-protocol-btp)
* BTP Litepaper: [https://icon.community/assets/btp-litepaper.pdf](https://icon.community/assets/btp-litepaper.pdf)
* ICON BTP Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-25.md)
* ICON BTP Fee Gathering Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-35.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-35.md)
* ICON BTP Message Fragmentation Standard: [https://github.com/icon-project/IIPs/blob/master/IIPS/iip-40.md](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-40.md)

\
