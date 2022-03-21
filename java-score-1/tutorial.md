# Java SCORE Tutorial

## Goals

* Build a sample Java SCORE.
* Deploy the Java SCORE to [Lisbon](https://icondev.io/introduction/the-icon-network/testnet#lisbon).
* Verify the transactions

## Requirements

*   You need to install OpenJDK 11 version. Visit [OpenJDK.net](http://openjdk.java.net) for prebuilt binaries. Or you can install a proper OpenJDK package from your OS vendors.

    In macOS:
    ```
    $ brew tap AdoptOpenJDK/openjdk
    $ brew install --cask adoptopenjdk11
    ```

    In Linux (Ubuntu 20.04):
    ```
    $ sudo apt install openjdk-11-jdk
    ```

*   Download and Install Go

    In macOS:
    ```
    $ brew install go
    ```

    In Linux (Ubuntu 20.04):
    ```
    $ wget -c https://go.dev/dl/go1.16.15.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local
    $ export PATH=$PATH:/usr/local/go/bin
    ```

    Verify installation
    ```
    $ go version
    ```

## Step 1. Build goloop CLI

You need to checkout the "goloop" repository for building the `goloop` CLI.

```
$ git clone git@github.com:icon-project/goloop.git
$ GOLOOP_ROOT=/path/to/goloop
```

Then build the `goloop` CLI binary.

```
$ cd ${GOLOOP_ROOT}
$ GOBUILD_TAGS= make goloop
```

Make sure the binary builds successfully.
```
$ ./bin/goloop version
goloop version v1.2.3-7-g78f7f438 linux/amd64 tags()-2022-03-21-14:09:46
```

## Step 2. Build sample Java SCORE

You need to checkout the "java-score-examples" repository for sample java SCORE.

```
$ git clone git@github.com:icon-project/java-score-examples.git
$ cd java-score-examples
```

### Build the SCORE

```
$ ./gradlew build
```

The compiled jar bundle will be generated at `./hello-world/build/libs/hello-world-0.1.0.jar`.

### Optimize the jar

You need to optimize your jar bundle before you deploy it to local or ICON networks. This involves some pre-processing to ensure the actual deployment successful.

`gradle-javaee-plugin` is a Gradle plugin to automate the process of generating the optimized jar bundle. Run the optimizedJar task to generate the optimized jar bundle.

```
$ ./gradlew optimizedJar
```

The output jar will be located at `./hello-world/build/libs/hello-world-0.1.0-optimized.jar`.


## Step 3. Generate a keystore and get test ICX

Generate a keystore.

```
$ ${GOLOOP_ROOT}/bin/goloop ks gen --out keystore.json --password gochain
hxd8fefc57e0d358e0cf338d684c3e438190d64145 ==> keystore.json
```

Then get test ICX in Lisbon using the [faucets](https://icondev.io/introduction/the-icon-network/testnet#faucets).


## Step 4. Deploy the optimized jar

### Using `goloop` CLI command

You can deploy the optimized jar by using `goloop` CLI binary.

```
$ ${GOLOOP_ROOT}/bin/goloop rpc sendtx deploy ./hello-world/build/libs/hello-world-0.1.0-optimized.jar \
    --uri https://lisbon.net.solidwallet.io/api/v3 \
    --key_store keystore.json --key_password gochain \
    --nid 2 --step_limit 10000000000 \
    --content_type application/java \
    --param name=Alice
"0xfee1e31e3ecb88106e785a6cb8b0b957e42f5f908a7c2c66a0c19aebd659f7ef"
```

### Using `deployJar` extension

Alternatively, you can use `deployToLisbon` task as follows.

```
$ ./gradlew hello-world:deployToLisbon -PkeystoreName=keystore.json -PkeystorePass=gochain

> Task :hello-world:deployToLisbon
>>> deploy to https://lisbon.net.solidwallet.io/api/v3
>>> optimizedJar = ./hello-world/build/libs/hello-world-0.1.0-optimized.jar
>>> keystore = keystore.json
Succeeded to deploy: 0xfee1e31e3ecb88106e785a6cb8b0b957e42f5f908a7c2c66a0c19aebd659f7ef
SCORE address: cxd1f5d12e92459a4fcdf2678a14b572687471a70e
```

## Step 5. Verify the execution

Check the deployed SCORE address first using the `txresult` command.

```
$ ${GOLOOP_ROOT}/bin/goloop rpc txresult 0xfee1e31e3ecb88106e785a6cb8b0b957e42f5f908a7c2c66a0c19aebd659f7ef \
    --uri https://lisbon.net.solidwallet.io/api/v3
{
  "to": "cx0000000000000000000000000000000000000000",
  "cumulativeStepUsed": "0x3d70a5c3",
  "stepUsed": "0x3d70a5c3",
  "stepPrice": "0x2e90edd00",
  "eventLogs": [],
  "logsBloom": "0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000...",
  "status": "0x1",
  "scoreAddress": "cxd1f5d12e92459a4fcdf2678a14b572687471a70e",
  "blockHash": "0xe678a4a19d43c54c709c16b4d794e59a3214af64d9efd860eb8f97fc6bdece7d",
  "blockHeight": "0x1b6",
  "txIndex": "0x0",
  "txHash": "0xfee1e31e3ecb88106e785a6cb8b0b957e42f5f908a7c2c66a0c19aebd659f7ef"
}
```
Note that `scoreAddress` field above represents the newly created SCORE address (you need to use this address for RPC calls hereafter).

Then you can query `getGreeting` method via the following `call` command.

```
$ ${GOLOOP_ROOT}/bin/goloop rpc call --to cxd1f5d12e92459a4fcdf2678a14b572687471a70e \
    --uri https://lisbon.net.solidwallet.io/api/v3 \
    --method getGreeting
"Hello Alice!"
```

And you can invoke `setName` method via the following `sendtx` command.

```
$ ${GOLOOP_ROOT}/bin/goloop rpc sendtx call --to cxd1f5d12e92459a4fcdf2678a14b572687471a70e \
    --uri https://lisbon.net.solidwallet.io/api/v3 \
    --key_store keystore.json --key_password gochain \
    --nid 2 --step_limit 10000000000 \
    --method setName \
    --param name=Bob
"0x07868af25c42e0d201073eae9d490d895e0922a431918199aa3bd461d6d9e65f"
```

Check the transaction result using the `txresult` command.

```
$ ${GOLOOP_ROOT}/bin/goloop rpc txresult 0x07868af25c42e0d201073eae9d490d895e0922a431918199aa3bd461d6d9e65f \
    --uri https://lisbon.net.solidwallet.io/api/v3
{
  "to": "cxd1f5d12e92459a4fcdf2678a14b572687471a70e",
  "cumulativeStepUsed": "0x1fe85",
  "stepUsed": "0x1fe85",
  "stepPrice": "0x2e90edd00",
  "eventLogs": [],
  "logsBloom": "0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000...",
  "status": "0x1",
  "blockHash": "0xae2be6e87b715f786c2ba599f5f570f9a4f018a20338cfb1d055ae3463d65571",
  "blockHeight": "0x2bc",
  "txIndex": "0x0",
  "txHash": "0x07868af25c42e0d201073eae9d490d895e0922a431918199aa3bd461d6d9e65f"
}
```

Query `getGreeting` method again to confirm the name change.

```
$ ${GOLOOP_ROOT}/bin/goloop rpc call --to cxd1f5d12e92459a4fcdf2678a14b572687471a70e \
    --uri https://lisbon.net.solidwallet.io/api/v3 \
    --method getGreeting
"Hello Bob!"
```
