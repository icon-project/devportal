# How to write a smart contract

A smart contract is a self-contained program that is stored and replicated on a blockchain network. When a contract is deployed to the blockchain, it becomes a part of the blockchain and is stored on every node in the network.

In the following guide we are going to explain in detail the process of:

* Setting up the development environment for writing a smart contract on ICON
* Writing a sample smart contract that will work as a poll for people to cast a vote “Yes” or “No”.&#x20;
* Compile, optimize and deploy the smart contract.
* And finally we are going to interact with the smart contract via RPC calls.

### Prerequisites

For setting the development environment we need to install the following programs:

* Install OpenJDK, go, and goloop. Follow [these instructions to install goloop](https://docs.icon.community/concepts/computational-utilities/goloop/setup) (this has instructions for OpenJDK, go and goloop).
* [Install gradle](https://docs.gradle.org/current/userguide/installation.html)

For the smart contract deployment you can either choose to deploy on a testnet in the ICON Blockchain or run a local network.

If you want to deploy to a testnet you will need to have ICX in your selected testnet, for that you can use the following faucet:

[https://faucet.iconosphere.io/](https://faucet.iconosphere.io/)

For setting up a local network, you can follow this guide:

{% embed url="https://docs.icon.community/getting-started/how-to-run-a-local-network" %}

### Creating the project workspace with gradle

We are going to be working on a folder named `poll-contract` inside our home folder, you can use any folder of your choice in your computer.

```bash
mkdir  ~/poll-contract
cd ~/poll-contract
```

Inside the folder we are going to initialize a project using gradle. If you are unfamiliar with gradle please refer to [their documentation](https://docs.gradle.org/current/userguide/what\_is\_gradle.html).

```bash
$gradle init
```

The selected options are the following:

* Select type of project to generate: 2
* Select implementation language: 3
* Split functionality across multiple sub-projects?: 1
* Select build script DSL: 1
* Generate build using new APIs and behavior (some features may change in the next minor release)? (default: no) \[yes, no]  yes
* Select test framework: 4&#x20;
* Project name (default: poll-contract): poll-contract
* Source package (default: poll.contract): poll.contract

The following folder structure will be created:

```bash
.
├── app
│   ├── build.gradle
│   └── src
│   ├── main
│   │   ├── java
│   │   │   └── poll
│   │   │   └── contract
│   │   │       └── App.java
│   │   └── resources
│   └── test
│       ├── java
│       │   └── poll
│       │   └── contract
│       │       └── AppTest.java
│       └── resources
├── gradle
│   └── wrapper
│   ├── gradle-wrapper.jar
│   └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
└── settings.gradle


14 directories, 8 files
```

The `build.gradle` file is used to create [build scripts](https://docs.gradle.org/current/userguide/writing\_build\_scripts.html). In ICON, we use it to specify dependencies and write tasks.

After creating the workspace, you need to create one build.gradle file at the root of the project with the following code.

<pre class="language-groovy"><code class="lang-groovy">buildscript {
  repositories {
    mavenCentral()
  }
  dependencies {
<strong>    classpath 'foundation.icon:gradle-javaee-plugin:0.8.1'
</strong>  }
}

subprojects {
  repositories {
    mavenCentral()
  }

  apply plugin: 'java'
  apply plugin: 'foundation.icon.javaee'

  java {
    sourceCompatibility = JavaVersion.VERSION_11
    targetCompatibility = JavaVersion.VERSION_11
  }

  compileJava {
    options.compilerArgs += ['-parameters']
  }
}
</code></pre>

This specifies the project to use the [gradle javaee plugin](https://github.com/icon-project/gradle-javaee-plugin) which is specifically created for smart contract development in ICON. Each subproject (in above tree structure, app folder) would have its own `build.gradle` file. There are 2 additional tasks you would need to add to optimize and deploy the jar file.\
\
Edit the `app/build.gradle` file to have the following data:

```groovy
dependencies {
     compileOnly 'foundation.icon:javaee-api:0.9.3'

    	testImplementation 'foundation.icon:javaee-unittest:0.10.0'
    	testImplementation 'org.mockito:mockito-core:4.8.0'
    	testImplementation 'org.junit.jupiter:junit-jupiter-api:5.9.0'
    	testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.9.0'
}

optimizedJar {
     mainClassName = 'poll.contract.App'
     archivesBaseName = "poll-contract-" + archiveVersion.get() + ".jar"
	from {
    	    configurations.runtimeClasspath.collect {it.isDirectory() ? it : zipTree(it) }
	}

}

deployJar {
    endpoints {
        local {
            uri = 'http://localhost:9080/api/v3'
            nid = 0x3
        }
    }

    keystore = rootProject.hasProperty('keystoreName') ? "$keystoreName" : ''
    password = rootProject.hasProperty('keystorePass') ? "$keystorePass" : ''
}


test {
     useJUnitPlatform()
}
```

In the `optimizedJar` task, `mainClassName` refers to the class which would be the entry point for the smart contract.

To avoid errors during compilation edit the `app/src/test/java/poll/contract/AppTest.java` file to have the following code:

```java
package poll.contract;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class AppTest {
}
```

### Writing the smart contract

Edit the `app/src/main/java/poll/contract/App.java` to have the following code:

```java
package poll.contract; 
                                                                             	 
import score.Address;                                                        	 
import score.Context;                                                        	 
import score.VarDB;                                                          	 
import score.annotation.External;                                            	 
import score.DictDB;                                                         	 
                                                                             	 
import java.math.BigInteger;                                                 	 
                                                                             	 
public class App {                                                           	 
    private static final String VOTER_ADDRESS = "voterAddress";   
    // Dict of votes per each address           	 
    final static DictDB<String, String> voters = Context.newDictDB(VOTER_ADDRESS, String.class);
    // Counter of “no” votes                                                               	 
    private final VarDB<BigInteger> countOfNo = Context.newVarDB("countOfNo", BigInteger.class);      
    // Counter of “yes” votes                                                         	 
    private final VarDB<BigInteger> countOfYes = Context.newVarDB("countOfYes", BigInteger.class);                                                             	 
                    
    /*
    * Adds a vote of “yes” with the caller address
    */                                                         	 
    @External                                                                	 
    public String addVoteYes() {                                             	 
        Address _caller = Context.getCaller();                               	 
        String _addressVote = voters.getOrDefault(_caller.toString(), "null");    
        String result = "Account already voted";
        if (_addressVote == "null") {
            result = "Voted Yes";
        	voters.set(_caller.toString(), "yes");
        	countOfYes.set(countOfYes.getOrDefault(BigInteger.ZERO).add(BigInteger.ONE));
        }
        return result;
    }

    /*
    * Adds a vote of “no” with the caller address
    */     
    @External
    public String addVoteNo() {
        Address _caller = Context.getCaller();
        String _addressVote = voters.getOrDefault(_caller.toString(), "null");
        String result = "Account already voted";
        if (_addressVote == "null") {
            result = "Voted No";
            voters.set(_caller.toString(), "no");
            countOfNo.set(countOfNo.getOrDefault(BigInteger.ZERO).add(BigInteger.ONE));
        }
        return result;
    }

    /*
    * Checks the vote of the specified address
    */     
    @External(readonly=true)
    public String checkVote(String _address) {
        return voters.get(_address);
    }

    /*
    * Gets the count of all votes
    */     
    @External(readonly=true)
    public BigInteger getVotesResult() {
        return countOfNo.getOrDefault(BigInteger.ZERO).add(countOfYes.getOrDefault(BigInteger.ZERO));
    }
}
```

### Compiling the smart contract

To build the project and have it ready to deploy into the ICON Blockchain we run the following commands:

```bash
$ ./gradlew build

BUILD SUCCESSFUL in 432ms
4 actionable tasks: 1 executed, 3 up-to-date
```

```bash
$ ./gradlew optimize

BUILD SUCCESSFUL in 410ms
3 actionable tasks: 1 executed, 2 up-to-date
```

After running these commands the folder `./app/build/libs/` (along with an assortment of other files and folders) will be created by gradle with the following content:

```bash
$ tree -I 'classes|generated|report|gradle|src|tmp|reports|test-results|Makefile|*.gradle|gradle*'.
└── app
     └── build
          └── libs
          ├── poll-contract-optimized.jar
          └── poll-contract.jar


3 directories, 2 files
```

The file that we are going to deploy into the ICON Blockchain is `poll-contract-optimized.jar`.

### Deploying the smart contract

Deployment of the smart contract on an ICON Network (Mainnet, testnets or local networks) is done using a special contract creation transaction sent to the [contract creation address](https://tracker.icon.community/contract/cx0000000000000000000000000000000000000000).

```json
{
    "jsonrpc": "2.0",
    "method": "icx_sendTransaction",
    "id": 1234,
    "params": {
        "version": "0x3",
        "from": "hxbe258ceb872e08851f1f59694dac2558708ece11",
        "to": "cx0000000000000000000000000000000000000000", // address 0 means SCORE install
        "stepLimit": "0x12345",
        "timestamp": "0x563a6cf330136",
        "nid": "0x3",
        "nonce": "0x1",
        "signature": "VAia7YZ2Ji6igKWzjR2YsGa2m53nKPrfK7uXYW78QLE+ATehAVZPC40szvAiA6NEU5gCYB4c4qaQzqDh2ugcHgA=",
        "dataType": "deploy",
        "data": {
            "contentType": "application/java",
            "content": "0x1867291283973610982301923812873419826abcdef91827319263187263a7326e...", // compressed SCORE data
            "params": {  // parameters to be passed to on_install()
                "name": "ABCToken",
                "symbol": "abc",
                "decimals": "0x12"
            }
        }
    }
}
```

Smart contracts are only executed when they are called by a transaction, either directly or as part of a chain of contract calls. They do not run in the background or parallel, and they are single-threaded.

### Deploying the smart contract on the terminal using goloop CLI

As shown in the previous RPC JSON example for the deployment of the smart contract we need to encode the compiled smart contract (jar file) into a hex string and also sign the RPC JSON with the private key of the wallet that we are going to assign as the owner of the smart contract.

The easiest way to do this in the terminal is with the goloop CLI tool.

The following command can be used to deploy the smart contract with the goloop CLI:

```bash
$ goloop rpc sendtx deploy ./app/build/libs/poll-contract-optimized.jar --uri http://localhost:9080/api/v3 --key_store /path/to/keystore.json --key_password WALLET_PASSWORD --nid 3 --step_limit=20000000000 --content_type application/java
```

### Deploy using icon-sdk-js

We can also deploy the smart contract using the `icon-sdk-js` , for this example we are going to create a `nodejs` script to deploy our smart contract.

Inside our project root folder (`~/poll-contract`) lets create a `nodejs` project and install the `icon-sdk-js` package running the following commands:

```bash
$ npm init -y
$ npm install --save-dev icon-sdk-js
```

Create an `index.js` file and add the following code in it:

```javascript
const IconService = require("icon-sdk-js");                                          
const fs = require("fs");                                                            
                                                                                     
const {                                                                              
  IconWallet,                                                                        
  IconBuilder,                                                                       
  SignedTransaction,                                                                 
  IconConverter,                                                                     
  HttpProvider,                                                                      
} = IconService.default;                                                             
                                                                                     
const { DeployTransactionBuilder } = IconBuilder;                                    
                                                                                     
// add the path to the keystore file                                                 
const keystorePath = "/path/to/keystore.json";                                       
// add the password of the keystore file                                             
const keystorePWD = "gochain";                                                       
// port to the local network                                                         
const port = 9080;                                                                   
// hostname of the local network                                                     
const hostname = "localhost";                                                        
// url of the local node                                                             
const apiNode = `http://${hostname}:${port}/api/v3`;                                 
// select the correct NID depending on the network                                   
// https://docs.icon.community/icon-stack/icon-networks/main-network                 
const nid = 3;                                                                       
// instantiate httProvider and iconService                                           
const httpProvider = new HttpProvider(apiNode);                                      
const iconService = new IconService.default(httpProvider);                           
// path to the compiled contract                                                     
const scorePath = "./app/build/libs/poll-contract-optimized.jar";                    
                                                                                     
// Function to deploy the smart contract                                             
function deployContract(keystore, pwd, content) {                                    
  const walletKs = getKeystore(keystore);                                            
  const walletLoaded = IconWallet.loadKeystore(walletKs, pwd);
  // Create tx object for contract deployment
  const txObj = new DeployTransactionBuilder()
    .from(walletLoaded.getAddress())
    .to("cx0000000000000000000000000000000000000000")
    .stepLimit(IconConverter.toBigNumber("2500000000"))
    .nid(IconConverter.toBigNumber(nid))
    .nonce(IconConverter.toBigNumber("1")) 
    .version(IconConverter.toBigNumber("3"))
    .timestamp(new Date().getTime() * 1000)
    .contentType("application/java")
    .content(content) 
    .build();

  // Sign transaction with wallet
  const signedTx = new SignedTransaction(txObj, walletLoaded);
  return signedTx;
}

// Get keystore from file
function getKeystore(path) {
  const ks = fs.readFileSync(path, "utf-8");
  return ks;
}

async function main() {
  try {
    // Read smart contract and encode into hex string
    const scoreContentInHex = "0x" + fs.readFileSync(scorePath).toString("hex");
     
    // get signed transaction
    const signedTx = deployContract(keystorePath, keystorePWD, scoreContentInHex);
    console.log(`signed tx: ${JSON.stringify(signedTx.getProperties())}`);

    const tx = await iconService.sendTransaction(signedTx).execute();
    console.log(tx);
  } catch (err) {
    console.log("Unexpected error signing transaction");
    console.log(err); 
    return null;
  }
}

main();
```

Execute the file running `node index.js` and your contract will be deployed to the network.

### Post deployment

The RPC call to deploy the contract will return a transaction hash, we need to query the network about the transaction information of that hash to verify that the transaction was processed correctly and get the contract address.

This command will output a transaction hash that we can use to make a readonly call using the `icx_getTransactionResult` method and obtain the contract address:

```bash
$ curl -X POST --data '{"jsonrpc":"2.0","method":"icx_getTransactionByHash","id":121,"params":{"txHash":"0x2cec2d2b0823e73354ac42f93c9126db3572c5043e66a3e8a8e7432911feb48f"}}' http://localhost:9080/api/v3
```

Result of the call:

```json
{
  "jsonrpc": "2.0",
  "result": {
"blockHash": "0xf40ed11bdd98a34bb24e6f59d90c2bdeadd08f2ca0ad6d5bcbd3a189b14fcb45",
"blockHeight": "0x50ef2",
"cumulativeStepUsed": "0x3cfd64f1",
"eventLogs": [],
"logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
"scoreAddress": "cx367b34a401e2df975d6d122360e23815e021ce4a",
"status": "0x1",
"stepPrice": "0x2e90edd00",
"stepUsed": "0x3cfd64f1",
"to": "cx0000000000000000000000000000000000000000",
"txHash": "0x2cec2d2b0823e73354ac42f93c9126db3572c5043e66a3e8a8e7432911feb48f",
"txIndex": "0x1"
  },
  "id": 121
}
```

When interacting with a smart contract a very useful method is the `icx_getScoreApi`. This readonly method will return the contract ABI which is a JSON formatted object that shows the methods of the contract with the inputs needed when calling each method and the resulting outputs after calling a method.

```bash
$ curl -X POST --data '{"jsonrpc":"2.0","method":"icx_getScoreApi","id":121,"params":{"address":"cx62fbf5e0e1eec28282beea38253c068123ddd429"}}' http://localhost:9080/api/v3

```

The result of calling `icx_getScoreApi` to the contract we have just created would be the following:

```json
{                                                                    
  "jsonrpc": "2.0",                                                           
  "result": [                                                                 
{                                                                         
   "inputs": [],                                                           
   "name": "addVoteYes",                                                   
   "outputs": [                                                            
     {                                                                     
       "type": "str"                                                       
     }                                                                     
   ],                                                                      
   "type": "function"                                                      
},                                                                        
{                                                                         
   "inputs": [],                                                           
   "name": "addVoteNo",                                                    
   "outputs": [                                                            
     {                                                                     
       "type": "str"
     }
   ],
   "type": "function"
},
{
   "inputs": [
     {
       "name": "_address",
       "type": "str"
     }
   ],
   "name": "checkVote",
   "outputs": [
     {
       "type": "str"
     }
   ],
   "readonly": "0x1",
   "type": "function"
},
{
   "inputs": [],
   "name": "getVotesResult",
   "outputs": [
     {
       "type": "int"
     }
   ],
   "readonly": "0x1",
   "type": "function"
}
  ],
  "id": 121
}
```

For calling the methods in a smart contract we have 2 main RPC JSON methods to use:

* &#x20;`icx_call`: for readonly methods ([link](https://docs.icon.community/icon-stack/client-apis/json-rpc-api/v3#icx\_call)).
* &#x20;`icx_sendTransaction`: for write methods. These calls require the RPC json to be signed using the private key. ([link](https://docs.icon.community/icon-stack/client-apis/json-rpc-api/v3#icx\_sendtransaction))

### Resources:

* Java smart contract tutorials part 1. [https://icon.community/tutorials/java-tutorial-part-1-setting-development-environment-and-writing-smart-contract/](https://icon.community/tutorials/java-tutorial-part-1-setting-development-environment-and-writing-smart-contract/)
* Java smart contract tutorials part 2. [https://icon.community/tutorials/java-tutorial-part-2-deploying-the-smart-contract-and-interacting-with-the-smart-contract-onchain/](https://icon.community/tutorials/java-tutorial-part-2-deploying-the-smart-contract-and-interacting-with-the-smart-contract-onchain/)
* Java smart contract tutorials part 3. [https://icon.community/tutorials/java-tutorial-part-3-unit-testing/](https://icon.community/tutorials/java-tutorial-part-3-unit-testing/)
* [ICON Stack - Smart Contracts](../../icon-stack/smart-contracts/)
