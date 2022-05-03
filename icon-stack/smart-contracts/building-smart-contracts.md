# Building smart contracts

### The ICON execution environment

ICON can only run your smart contract after it has been compiled down into bytecode.

You need to install JDK 11 or later version. Visit [OpenJDK.net](http://openjdk.java.net) for prebuilt binaries. Or you can install a proper OpenJDK package from your OS vendors.

In macOS:

```
$ brew tap AdoptOpenJDK/openjdk
$ brew cask install adoptopenjdk11
```

In Linux (Ubuntu 18.04):

```
$ sudo apt install openjdk-11-jdk
```

For example, using the hello-world sample in the ICON smart contract examples:

#### 1. Build the project

```
$ ./gradlew build
```

The compiled jar bundle will be generated at `./hello-world/build/libs/hello-world-0.1.0.jar`.

#### 2. Optimize the jar

You need to optimize your jar bundle before you deploy it to local or ICON networks. This involves some pre-processing to ensure the actual deployment successful.

`gradle-javaee-plugin` is a Gradle plugin to automate the process of generating the optimized jar bundle. Run the `optimizedJar` task to generate the optimized jar bundle.

```
$ ./gradlew optimizedJar
```

The output jar will be located at `./hello-world/build/libs/hello-world-0.1.0-optimized.jar`.

### Web applications

The smart contract will be associated with an _Application Binary Interface (ABI)_, which is a markup list of the public-facing, callable endpoints of the contract. A web application, such as a [Javascript](../client-apis/javascript-sdk/) client or a [Python](../client-apis/python-sdk/) service, can read the ABI so that the application can make calls to interact with the smart contract.

You can get an ICON smart contract's ABI by using the [`icx_getScoreApi`](../client-apis/json-rpc-api/v3.md#icx\_getscoreapi) function from the JSON-RPC API. See the ABI specification listed there in the _**Returns**_ descriptor.

### Resources

* [Smart contract API document](https://www.javadoc.io/doc/foundation.icon/javaee-api)
* [Gradle plugin for JavaEE](https://github.com/icon-project/gradle-javaee-plugin)
