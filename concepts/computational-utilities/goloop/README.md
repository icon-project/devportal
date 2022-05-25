# Goloop

### What is goloop?

Goloop is the [client](../../network/clients.md) for the ICON network officially supported by the ICON Foundation. Its purpose is to maintain the state of the decentralized blockchain in a robust, failsafe way.

#### Structure

There are 6 main layers to the goloop client:

* Transaction handler for interaction with users
* Transaction handler for internal usage
* Communication with the execution environment
* Execution environment
* Data storage
* Peer-to-peer communication

You can learn more about the transaction-processing system in the section entitled [_Transaction processing in ICON_](../../../icon-stack/icon-execution-environments/).

The goloop program is built using the [_go_](https://go.dev/) programming language, which is popular for system-level and cloud-native applications. It is very modular and can be repackaged to modified versions of the existing components, for example to use a different consensus protocol.

The goloop project is open-sourced, and we welcome collaboration. Check it out [here on GitHub](https://github.com/icon-project/goloop), and contribute something or experiment with it if you would like.

### Using goloop

See the goloop CLI documentation for information on how to use it.
