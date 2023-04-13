# Clients

### What are nodes and clients? <a href="#what-are-nodes-and-clients" id="what-are-nodes-and-clients"></a>

The [Ethereum documentation](https://ethereum.org/en/developers/docs/nodes-and-clients/#what-are-nodes-and-clients) refers to a node as "a running piece of client software.", where a client is then an implementation of the blockchain network. From the [ICON glossary](https://icon.community/glossary/node/), a node is defined for this network as follows:

> Computational service running in support of the ICON network that can either be an API Endpoint or a Delegate / Validator

[_Goloop_](../computational-utilities/goloop/) is the officially supported client for the ICON blockchain. You can learn more about it in the linked documentation subsection under the [_computational utilities_](../computational-utilities/) section.

### Node types <a href="#node-types" id="node-types"></a>

Due to the delegated nature of the ICON network, there are currently two different types of nodes: [API Endpoints](api-endpoints.md) and [Validators](validator-nodes.md).

#### API Endpoints

* Stores full blockchain state
* Read capabilities
* Provides data to users

#### Validators

* Stores full blockchain state
* Read / write capabilities
* Provides data to API Endpoints and other validators

### Why should I run an ICON node? <a href="#why-should-i-run-an-ethereum-node" id="why-should-i-run-an-ethereum-node"></a>

By running a node, you will be able to privately and trustlessly access the ICON network. Additionally, you will be supporting the ecosystem. Running a node helps to create diversity and increase computational bandwidth for the ICON ecosystem. This helps to prevent network attacks by malicious nodes and users.

There are financial benefits to running a Validator node if you are voted in as a Delegate. This is elaborated upon in the section on [_Rewards & Penalties_](../economics/rewards-and-penalties.md).

