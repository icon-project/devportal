# Intro to the stack

### Level 1: ICON execution environments <a href="#ethereum-virtual-machine" id="ethereum-virtual-machine"></a>

ICON execution environments manage the runtime of contracts on the ICON network. These environments are responsible for handling the execution of [transactions](../concepts/computational-utilities/transactions.md), which are the basic functions of ICON to change the state of the blockchain.

The execution environments create an abstraction layer between the three types of operators on ICON: [_smart contracts_](smart-contracts/), [_nodes (i.e. machines)_](../concepts/network/), and [_users_](broken-reference).

You can learn more about the execution environments in the [corresponding documentation section](icon-execution-environments/).

### Level 2: Smart contracts <a href="#smart-contracts" id="smart-contracts"></a>

[Smart contracts](https://icon.community/glossary/smart-contract/) are defined as follows by the ICON glossary:

> Blockchain-integrated code for maintaining and interacting with data in a trusted, distributed database

Smart contracts are written in supported [programming languages](smart-contracts/smart-contract-languages.md) and are compiled into bytecode or interpreted and then executed in the proper execution environment.

Once stored on ICON, a smart contract functions as a public interface to some part of an [application](../projects/decentralized-applications-dapps.md) and it will always run as long as the network is running. Such a service is sometimes referred to as an [Application Binary Interface (ABI)](https://icon.community/glossary/abi/). A single application may connect to multiple smart contracts, and a smart contract may connect to other smart contracts as well. This concept is referred to as [_composability_](broken-reference).

Developing and using smart contracts are powerful ways to integrate with ICON. You can make the blockchain, an already powerful tool, work in specific and custom ways to experiment with ideas or build a business. You can engage with the various open cryptocurrency communities to aid you to get started or solve problems during the development process, including using the ICON [discord](https://discord.com/invite/7a75Hf3cFm) and [community forum](https://forum.icon.community/).

### Level 3: ICON nodes <a href="#ethereum-nodes" id="ethereum-nodes"></a>

An application must connect to an ICON [node](../concepts/network/clients.md) in order to interact with the blockchain. This allows you to read from the blockchain and send transactions to be written on the blockchain.

It is important to note that, as an open, decentralized network, the blockchain itself is just the nodes working together. They are responsible for keeping copies of the blockchain state and for reaching consensus on new blocks to add to the blockchain.

You can use the [JSON-RPC API](client-apis/) to connect to the blockchain in order to read from it or write to it.

### Level 4: ICON client SDKs <a href="#ethereum-client-apis" id="ethereum-client-apis"></a>

There are a number of official and community-supported developer kits that allow you to access the ICON blockchain. These are referred to as the [_client standard developer kits (client SDKs)_](broken-reference). There are a number of different versions in different programming languages, for example [Javascript](client-apis/javascript-sdk/), [Python](client-apis/python-sdk/), and [Java](client-apis/java-sdk.md).

A developer may want to use these methods over the JSON-RPC API directly because they either prefer to work in a particular language or because they want to have some abstractions away from the often complex message specifications. Such SDK libraries may include functions for simplifying the process of creating transactions, swapping currency types, processing user account information, etc.

If you would like to see an integration for ICON in another programming language, engage with the technical community and see if you can collaborate with some other community members to do so.

### Level 5: End-user applications <a href="#end-user-applications" id="end-user-applications"></a>

The level of the stack that most people will interact with is the end-user applications. These appear as web or mobile applications such as those you use to check the weather or manage your calendar.

This level is often the most accessible for people and requires the simplest types of interactions with a computer in order to do something potentially very complicated.
