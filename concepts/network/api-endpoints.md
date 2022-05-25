# API Endpoints

### What is an API Endpoint? <a href="#why-should-i-run-an-ethereum-node" id="why-should-i-run-an-ethereum-node"></a>

From the glossary, an [API Endpoint](https://icon.community/glossary/api-endpoint/) is defined as follows:

> Transaction/ABI endpoints for ICON network. Used to get information or relay information to Delegates. Typically representing an end-user that has services running on or in support of the ICON network

In short, an API Endpoint is how users access the ICON blockchain. Public API Endpoints can be accessed directly, and private API Endpoints can be accessed by the corresponding user that runs the endpoint.

API Endpoints only read from the blockchain, they do not write to it. If a user wants a transaction to be processed by the ICON network, then it will request it to an API Endpoint, then if the transaction requires writing new data, the API Endpoint will forward the request to a [Validator node](validator-nodes.md), and then receive the results and display them to the user.

### Why run an API Endpoint?

API Endpoints increase the bandwidth of the ICON network. This allows users to more easily access information from the blockchain. There are many [_decentralized applications (dApps)_](../../projects/decentralized-applications-dapps/) built on ICON, and they all need to access an API Endpoint in some way.

A public API Endpoint allows anyone to access the ICON network. When and API Endpoint is public, it lowers the barrier to entry for people to engage with the network. This functions like a public utility and can also be used as the basis for a [_software-as-a-service_](https://en.wikipedia.org/wiki/Software\_as\_a\_service) business.

A private API Endpoint can be built in support of a specific service. Such an endpoint can be hidden for a specific user or user group to lower the complexity of running this type of node.

### Running an API Endpoint

See the [Getting Started](../../getting-started/how-to-run-an-api-endpoint.md) guide
