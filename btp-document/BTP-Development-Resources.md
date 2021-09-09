# BTP Development Resources

## Prerequisites

____

There are a couple of essential requirements to fully utilize BTP features:

- The blockchain of an issuer must have finality in its consensus
- The blockchain of a recipient must have a capability of supporting smart contracts

As mentioned earlier, the security and reliability of the Blockchain Transmission Protocol (BTP) entirely rely on a set of three types of smart contracts (BSH, BMV, and BMC) on each connected network. In this section, we would like to provide essential and mandatory interfaces of these three contracts to develop the BTP Network.

## BTP Service Handler (BSH)

____

BTP Service Handlers (BSH) is a smart contract that handles user's requests and sends them to BTP Message Center (BMC). The request can also come from other smart contracts. In addition, BSHs are also responsible for handling messages from other supported BSHs. As long as service smart contracts following BSH standards and meeting the requirements as proposed, anyone can develop their BSH contracts and attempt to request to connect with BMC contracts. Once this request is approved, its service can be officially activated and operated in the BTP Network.

- Interfaces of `BSH` on PRA (Solidity) [[link](BSH.md)]
- Interface of `BSH` on ICON (Java) [[link](https://github.com/icon-project/btp/blob/master/doc/bsh.md)]

## BTP Message Center (BMC)

____

BTP Message Center (BMC) is a smart contract that builds BTP Message before sending to the Relay (BMR) and handles relay messages from other BMRs. In addition, BMC also has features to manage authorization (e.g. Operators of a Contract and a list of allowed Relays), and information of connected networks (i.e. link, route) to make sure communication from the source delivering to the destination network can be operated smoothly and conveniently.

- Interfaces of `BMC` on PRA (Solidity) [[link](BMC.md)]
- Interface of `BMC` on ICON (Java) [[link](https://github.com/icon-project/btp/blob/master/doc/bmc.md)]

## BTP Message Verifier (BMV)

____

BTP Message Verifier (BMV) is a smart contract that extracts a relay message, which is composed of a BTP message with proof of existence, to the BTP message before verifying. For easy verification, it may update trust information for the following events. Most of the implementations may track the hashes of block headers. If the blockchain system provides proof of absence of the BTP messages, then it's enough for the verifier to sustain the last state only. It updates the hash only if it sees proof of absence of further BTP messages in the block. Most blockchain systems don't provide proof of absence for their data. Therefore, it is mandatory to provide methods to verify historical hashes. Merkle Accumulator can be used for verifying old hashes. BMV sustains roots of Merkle Tree Accumulator, and relay will sustain all elements of Merkle Tree Accumulator. The relay may make the proof of any one of old hashes. So, even if byzantine relay updated the trust information with the proof of a new block, a normal relay can send BTP Messages in the past block with the proof.

- Interfaces of `BMV` on PRA (Solidity) [[link](BMV.md)]
- Interface of `BMV` on ICON (Java) [[link](https://github.com/icon-project/btp/blob/master/doc/bmv.md)]

**Note that:** The provided interfaces are mandated to provide general or minimal requirements in building smart contracts so that distinct protocols could have compatible ways to interact and communicate with each other. Depending on the design of your project, a specific logic of implementations can be varied and additional functions/methods can be added

## Contract Upgradeability - Solidity

____

Have you ever wondered what would happen if your logic implementation had some issues/bugs, or you might need to add more features after deploying your contracts to a network? By design, smart contracts are immutable. Even though immutability is a significant design for a blockchain-based software, developers still need a certain degree of mutability. Hence, contract upgradeability (Solidity) was proposed. If you are interested in this feature, please click on links below for more information:

- [Short Introduction of Contract Upgradeability](Contract-Upgradeability-Solidity.md#contract-upgradeability-solidity)
- [Special roles in a context of Contract Upgradeability](Special-Roles.md#special-authorization-roles-in-contract-upgradeability)

&nbsp;

In the next section, we would like to give you a practical utilization of these interfaces and provide instructions in developing BTP Network connecting two blockchain ecosystems to a tee. Click on 'Next' if you are interested

&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;
[<--- Prev](./Overview.md) &emsp; &emsp; &emsp; &emsp; [Next --->](./BTP-Development-Instructions.md#setup-and-installation)

<!--<p align="center">-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/Overview.md" class="button"><--- Prev &emsp; &emsp;</a>-->
<!--  <a href="https://git.baikal.io/icon/btp/-/blob/BTPDocument/BTP-Development-Instructions.md" class="button">&emsp; &emsp; Next </a>-->
<!--</p> -->
