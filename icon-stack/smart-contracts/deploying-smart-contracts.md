# Deploying smart contracts

### How to deploy a smart contract <a href="#how-to-deploy-a-smart-contract" id="how-to-deploy-a-smart-contract"></a>

**Using `goloop` CLI command**

Now you can deploy the optimized jar to ICON networks that support the [Java smart contract execution environment](../icon-execution-environments/java.md). Assuming you are running a local network that is listening on port 9082 for incoming requests, you can create a deploy transaction with the optimized jar and deploy it to the local network as follows.

```
$ goloop rpc sendtx deploy ./hello-world/build/libs/hello-world-0.1.0-optimized.jar \
    --uri http://localhost:9082/api/v3 \
    --key_store <your_wallet_json> --key_password <password> \
    --nid 3 --step_limit=1000000 \
    --content_type application/java \
    --param name=Alice
```

**\[Note]** The content type should be `application/java` instead of `application/zip` to differentiate it with the Python smart contract deployment.



**Using `deployJar` extension**

Starting with version `0.7.2` of `gradle-javaee-plugin`, you can also use the `deployJar` extension to specify all the information required for deployment.

```
deployJar {
    endpoints {
        local {
            uri = 'http://localhost:9082/api/v3'
            nid = 3
        }
    }
    keystore = rootProject.hasProperty('keystoreName') ? "$keystoreName" : ''
    password = rootProject.hasProperty('keystorePass') ? "$keystorePass" : ''
    parameters {
        arg('name', 'Alice')
    }
}
```

Now you can run `deployToLocal` task as follows.

```
$ ./gradlew hello-world:deployToLocal -PkeystoreName=<your_wallet_json> -PkeystorePass=<password>

> Task :hello-world:deployToLocal
>>> deploy to http://localhost:9082/api/v3
>>> optimizedJar = ./hello-world/build/libs/hello-world-0.1.0-optimized.jar
>>> keystore = <your_wallet_json>
Succeeded to deploy: 0x699534c9f5277539e1b572420819141c7cf3e52a6904a34b2a2cdb05b95ab0a3
SCORE address: cxd6d044b01db068cded47bde12ed4f15a6da9f1d8
```

**\[Note]** If you want to deploy to Lisbon testnet, use the following configuration for the endpoint and run `deployToLisbon` task.

```
deployJar {
    endpoints {
        lisbon {
            uri = 'https://lisbon.net.solidwallet.io/api/v3'
            nid = 0x2
        }
        ...
    }
}
```

####

#### 4. Verify the execution

Check the deployed smart contract address first using the `txresult` command.

```
$ goloop rpc txresult <tx_hash> --uri http://localhost:9082/api/v3
{
  ...
  "scoreAddress": "cxaa736426a9caed44c59520e94da2d64888d9241b",
  ...
}
```

Then you can invoke `getGreeting` method via the following `call` command.

```
$ goloop rpc call --to <score_address> --method getGreeting --uri http://localhost:9082/api/v3
"Hello Alice!"
```

### Related tools <a href="#related-tools" id="related-tools"></a>

