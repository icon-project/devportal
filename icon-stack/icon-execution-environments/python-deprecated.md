# Python (deprecated)

The Python execution engine is the former execution environment for smart contracts on ICON. It is deprecated as of ICON 2. It is useful to document this information, as it includes design decisions that form the basis of the current architecture of ICON. This execution environment is based off of the standard [CPython](https://en.wikipedia.org/wiki/CPython), which is the standard version for most operating systems.

### Features

#### Multi-threading & multi-processing

Because this execution environment runs on CPython, it supports multi-threading. This means that multiple transactions can be executed in parallel on a single node. As in the typical CPython environment, multi-threading provides few computational bandwidth benefits. This is because of the limitations in place from the [Python Global Interpreter Lock](https://towardsdatascience.com/python-gil-e63f18a08c65). In order to make use of multiple CPU cores, you can use multi-processing, which starts multiple interpreters.

#### Data  storage

ICON's Python execution environment library includes the following data structures to aid in the construction of merkle proofs:

* VarDB
* ArrayDB
* DictDB

Data stored in these structures gets serialized into the unified global state machine that makes up the ICON blockchain.

#### Fees per instruction

The ICON base class and annotations modify the written smart contract code to include cryptocurrency-specific instructions for accumulating fees related for each computational instruction.

The step of adding per-instruction fees occurs during writing the smart contract code and is only accepted onto the network if the correct base class is inherited and correct annotations are present.

#### Development suite

The [ICON development suite for Python smart contracts](https://github.com/icon-project/t-bears) includes a library describing how to correctly structure a smart contract such that it would be correctly deployed onto ICON. It also includes convenience tools for interfacing with the blockchain in various ways, such as:

* Deploy contract
* Create boilerplate application
* Check account information
* Check block information

### Security

This section details the security features implemented in the Java execution environment. It is separated from the prior _Features_ section in order to highlight security-specific components of the execution environment.

#### Blacklisting Python features & classes

Among the blacklisted features and classes are the following:

* Random operation
* Floating point operation
* Network calls
* System calls (incl. file management)
* Long-running operations&#x20;
