# Java

The Java execution engine is the main execution environment for smart contracts on ICON. This environment is equivalent to the [Java Virtual Machine](https://en.wikipedia.org/wiki/Java\_virtual\_machine) developed by the [OpenJDK](http://openjdk.java.net/) project with a translation and auditing layer wrapped around it to provide features supporting a cryptocurrency distributed computing network. The translation and auditing layer is based off of the [Aion](https://aion.theoan.com/) Virtual Machine. The purpose of this layer is to provide cryptocurrency-specific features and address specific security concerns. A short overview of the features and security considerations in the overall Java execution environment for Java are described below.

### Features

#### Multi-threading

ICON supports multi-threaded code due to support from the JVM. This means that multiple transactions can be executed in parallel on a single node.

#### Data  storage

The Java execution environment uses a combination of the [Java Object Graph](https://www.tutorialspoint.com/object-graph-in-java-serialization) as well as the typical [Merkle Patricia Tree](https://eth.wiki/fundamentals/patricia-tree). All objects must be serializable. The contract developer can choose to place objects into the object graph or the tree. If the object is specified to be associated with either VarDB, ArrayDB, or DictDB, then it will be stored as a key-value pair in the merkle tree.

Else, it will go into the object graph. Storing data in the object graph is very expensive, as the entire object must be copied and stored each time that the object is modified. For those who are curious, the object graph is stored as a [heap](https://en.wikipedia.org/wiki/Heap\_\(data\_structure\)).

#### Fees per instruction

The ICON translation and audit layer modifies the written smart contract code to include cryptocurrency-specific instructions for accumulating fees related for each computational instruction. Objects in the graph are lazy-loaded and incur fees related to the amount of I/O required to load them.

The step of adding per-instruction fees happens after smart contracts are deployed to the blockchain.

#### Aion Virtual Machine

ICON's usage of the [Aion Virtual Machine](https://github.com/aionnetwork/AVM) includes modified versions of the following features:

* Data storage layer
* Java class library restrictions
  * E.g. prevention of non-deterministic behavior
* Insertion of per-instruction fees
* Internal error-handling

Here are some articles where you can learn more about the Aion Virtual Machine:

* [Why Java?](https://aion.theoan.com/blog/aion-virtual-machine-avm-why-java-and-the-jvm/)
* [Why not WASM?](https://aion.theoan.com/blog/aion-virtual-machine-avm-why-not-wasm/)
* [AVM Hybrid Storage](https://aion.theoan.com/blog/avm-hybrid-storage/)

#### Language Support

As the smart contract code ultimately runs on the Java Virtual Machine, theoretically there is support for development using the other JVM languages, such as [Scala](https://scala-lang.org/), [Kotlin](https://kotlinlang.org), or [Groovy](https://groovy-lang.org/). However, this is highly untested, and is probably an area that deserves more attention. If you have interest, feel free to experiment and organize a working group through the [ICON Technical Forum](https://forum.icon.community/) or [ICON Community Development Organization](https://github.com/icon-community).

### Security

This section details the security features implemented in the Java execution environment. It is separated from the prior _Features_ section in order to highlight security-specific components of the execution environment.

#### Blacklisting Java features & classes

Among the blacklisted features and classes are the following:

* Random operation
* Floating point operation
* Network calls
* System calls (incl. file management)
* Long-running operations&#x20;

#### Type-safe heap for data in the object graph

[Type-safety](https://en.wikipedia.org/wiki/Type\_safety) is a Java feature that ensures that the only functions allowed to operate on an object are the functions that are defined for that object type. This prevents some types of attacks from occurring on objects in the graph.

&#x20;
