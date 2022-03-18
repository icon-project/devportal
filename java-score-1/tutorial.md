# Tutorial

## Goals

* Deploy sample java SCORE to [Lisbon](https://icondev.io/introduction/the-icon-network/testnet#lisbon) using goloop CLI binary.

## Requirements

*   You need to install OpenJDK 11 version. Visit [OpenJDK.net](http://openjdk.java.net) for prebuilt binaries. Or you can install a proper OpenJDK package from your OS vendors.

    In macOS:

    ```
    $ brew tap AdoptOpenJDK/openjdk
    $ brew install --cask adoptopenjdk11
    ```

    In Linux (Ubuntu 18.04):

    ```
    $ sudo apt install openjdk-11-jdk
    ```
*   Download and Install GoLang

    In macOS:

    ```
    $ brew install go
    ```

    In Linux (Ubuntu 18.04):

    ```
    $sudo wget -c https://dl.google.com/go/go1.14.2.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local
    $export PATH=$PATH:/usr/local/go/bin
    ```

    Verify installation

    ```
    go version
    ```
*   Download and Install Python + VM

    In macOS:

    ```
    brew install python3
    pip3 install virtualenv setuptools wheel
    ```

    In Linux (Ubuntu 18.04):

    ```
    $ sudo apt install python3
    $ pip3 install virtualenv setuptools wheel
    ```
*   Download and Install rocksdb

    In macOS:

    ```
    $ brew install rocksdb
    ```

    In Linux (Ubuntu 18.04):

    ```
    git clone https://github.com/facebook/rocksdb.git
    cd rocksdb

    DEBUG_LEVEL=0 make shared_lib install-shared

    export LD_LIBRARY_PATH=/usr/local/lib
    ```

## Step 1. Source checkout

You need to checkout the `goloop` repository for building goloop CLI

```
$ git clone git@github.com:icon-project/goloop.git
$ GOLOOP_ROOT=/path/to/goloop
```

Then you need to checkout the `java-score-examples` repository for sample java SCORE.

```
$ git clone git@github.com:icon-project/java-score-examples.git
$ JAVA_SCORE_EXAMPLES_ROOT=/path/to/java-score-examples
```

## Step 2. Build goloop CLI

First of all, you need checkout git specific branch.

```
$ cd ${GOLOOP_ROOT}
$ git checkout master  # use the latest stable release
```

Then, you need build & copy built goloop binary file to`GOLOOP_ROOT`.

```
$ make goloop
$ cp ./bin/goloop ${GOLOOP_ROOT}/goloop
```

## Step 3. Build sample java SCORE

```
$ cd ${JAVA_SCORE_EXAMPLES_ROOT}
```

First of all, edit `java-score-example/hello-world/build.gradle` as below.

```
...
dependencies {
    # use the local api jar for testing
    compile files('api-0.8.7-SNAPSHOT.jar')

    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.6.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.6.0'
}
...
```

Prepare `hello-world/src/main/java/com/iconloop/score/example/HelloWorld.java` file as below.

```
/*
 * Copyright 2021 ICONLOOP Inc.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package com.iconloop.score.example;

import score.Context;
import score.ObjectReader;
import score.ByteArrayObjectWriter;
import score.annotation.External;
import score.annotation.Payable;

public class HelloWorld {
    private final String name;

    public HelloWorld(String name) {
        this.name = name;
    }

    @External(readonly=true)
    public String name() {
        return name;
    }

    @External(readonly=true)
    public String getGreeting() {
        String msg = "Hello " + name + "!";
        Context.println(msg);
        return msg;
    }

    @Payable
    public void fallback() {
        // just receive incoming funds
    }

    @External
    public void testRlp() {
        var codec = "RLPn";
        var msg = "testRLP";

        ByteArrayObjectWriter w = Context.newByteArrayObjectWriter(codec);
        w.write(msg.getBytes());
        ObjectReader r = Context.newByteArrayObjectReader(codec, msg.getBytes());
    }
}
```

### Build the SCORE

```
$ ./gradlew build
```

The compiled jar bundle will be generated at `./hello-world/build/libs/hello-world-0.1.0.jar`.

### Optimize the jar

You need to optimize your jar bundle before you deploy it to local or ICON networks. This involves some pre-processing to ensure the actual deployment successful.

gradle-javaee-plugin is a Gradle plugin to automate the process of generating the optimized jar bundle. Run the optimizedJar task to generate the optimized jar bundle.

```
$ ./gradlew optimizedJar
```

The output jar will be located at `./hello-world/build/libs/hello-world-0.1.0-optimized.jar`.

### Copy example SCORE to GOLOOP\_ROOT

```
$ cp ./hello-world/build/libs/hello-world-0.1.0-optimized.jar ${GOLOOP_ROOT}
```

## Step 4. Generate keystore and get test ICX

Generate keystore.

```
$ cd ${GOLOOP_ROOT}
$ ./goloop ks gen --out kestore.json --password gochain
hxd8fefc57e0d358e0cf338d684c3e438190d64145 ==> kestore.json
```

Then get test ICX in Lisbon using the [faucets](https://icondev.io/introduction/the-icon-network/testnet#faucets).&#x20;

## Step 5. Deploy the optimized jar (hello-world SCORE with RLP)



You can deploy the optimized jar by using goloop CLI binary.

```
$ cd ${GOLOOP_ROOT}
$ ./goloop rpc sendtx deploy ./hello-world-0.1.0-optimized.jar \
    --uri https://lisbon.net.solidwallet.io/api/v3 \
    --key_store keystore.json --key_password gochain \
    --nid 2 --step_limit 10000000000 \
    --content_type application/java \
    --param name=GoLoop
"0xfee1e31e3ecb88106e785a6cb8b0b957e42f5f908a7c2c66a0c19aebd659f7ef"
```

Check the deployed SCORE address first using the txresult command.

```
$ ./goloop rpc txresult 0xfee1e31e3ecb88106e785a6cb8b0b957e42f5f908a7c2c66a0c19aebd659f7ef \
    --uri https://lisbon.net.solidwallet.io/api/v3
{
  "to": "cx0000000000000000000000000000000000000000",
  "cumulativeStepUsed": "0x3d70a5c3",
  "stepUsed": "0x3d70a5c3",
  "stepPrice": "0x2e90edd00",
  "eventLogs": [],
  "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  "status": "0x1",
  "scoreAddress": "cxd1f5d12e92459a4fcdf2678a14b572687471a70e",
  "blockHash": "0xe678a4a19d43c54c709c16b4d794e59a3214af64d9efd860eb8f97fc6bdece7d",
  "blockHeight": "0x1b6",
  "txIndex": "0x0",
  "txHash": "0xfee1e31e3ecb88106e785a6cb8b0b957e42f5f908a7c2c66a0c19aebd659f7ef"
}
```

Then you can query getGreeting method via the following call command.

```
$ ./goloop rpc call --to cxd1f5d12e92459a4fcdf2678a14b572687471a70e \
    --method getGreeting \
    --uri https://lisbon.net.solidwallet.io/api/v3
"Hello GoLoop!"
```

And you can invoke testRlp method via the following sendtx command.

```
$ ./goloop rpc sendtx call --to cxd1f5d12e92459a4fcdf2678a14b572687471a70e \
    --method testRlp \
    --uri https://lisbon.net.solidwallet.io/api/v3 \
    --key_store keystore.json --key_password gochain \
    --nid 2 --step_limit 10000000000
"0x07868af25c42e0d201073eae9d490d895e0922a431918199aa3bd461d6d9e65f"
```

Check the called SCORE address using the txresult command.

```
$ ./goloop rpc txresult 0x07868af25c42e0d201073eae9d490d895e0922a431918199aa3bd461d6d9e65f \
    --uri https://lisbon.net.solidwallet.io/api/v3
{
  "to": "cxd1f5d12e92459a4fcdf2678a14b572687471a70e",
  "cumulativeStepUsed": "0x1fe85",
  "stepUsed": "0x1fe85",
  "stepPrice": "0x2e90edd00",
  "eventLogs": [],
  "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  "status": "0x1",
  "blockHash": "0xae2be6e87b715f786c2ba599f5f570f9a4f018a20338cfb1d055ae3463d65571",
  "blockHeight": "0x2bc",
  "txIndex": "0x0",
  "txHash": "0x07868af25c42e0d201073eae9d490d895e0922a431918199aa3bd461d6d9e65f"
}
```
