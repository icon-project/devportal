# BTP & ICON Bridge

### What is interoperability?

Interoperability, in the context of the blockchain, is the ability to use multiple chains to facilitate the operation of a single application. Typically, this would mean that a smart contract from one chain could use logic from a contract on another chain while keeping the same guarantees about trustlessness throughout the entire process.

#### Cross-chain messaging

One important component of interoperability is _cross-chain messaging_. This is the system that allows for information to be sent and received between the multiple interacting blockchains.

#### Relays

By design, blockchains cannot perform ourward networking operations, such as http requests, to external services. If a smart contract were able to perform networking operations, then it would become nearly impossible to form a consensus on the events that transpired. Because of this issue, it is necessary to have a service outside of the blockchain that can perform networking operations.

Of course, the natural limit for the operations that can be performed is that the facilitator, referred to here as a _relay_, must be able to send messages back and forth that allow the different interacting blockchains to hold each other accountable. In other words, the relay must provide proofs between the multiple chains, and the chains must keep those proofs so that they can present an argument about the validity of a transaction upon request.

### Decentralized & centralized solutions

The main components of the ICON interoperability solutions are mostly the same between the decentralized version, BTP, and the centralized version, Bridge. Those components are as follows:

* Service handler
* Message broker
* Relay
* Verifier (decentralized only)

#### BTP - Decentralized

In this process, first the user sends a request to service handler to send from one blockchain to another.

Then the service handler passes this to the message broker.

The message broker encodes this request as a BTP message and adds it to the queue.

After this, the relay reads from the queue and sends it to the other blockchain's message broker.&#x20;

The other blockchain's message broker forwards BTP message to the message verifier.

The message verifier then decodes the BTP message and sends it back to the message broker.

The message broker sends the decoded message to service handler

This is trustless because it encodes and decodes messages using an on chain verifier.&#x20;

#### Bridge - Centralized

The centralized version assumes that the relays are run in a responsible and robust way. In this system, the messages are assumed to be correct, and the message verification step is skipped.

### Further resources

[Cosmos Inter-blockchain Communication](https://ibcprotocol.org/)

[Polkadot Network](https://polkadot.network/technology/)

[Avalanche Network](https://www.avax.network/)

[Axelar Network](https://docs.axelar.dev/dev/gmp/overview)

[Solana Wormhole Network](https://docs.wormholenetwork.com/wormhole/)

[Layer0](https://layerzero.gitbook.io/docs)
