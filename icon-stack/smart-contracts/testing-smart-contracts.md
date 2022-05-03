# Testing smart contracts

### Testing tools and libraries



Two testing frameworks are provided as to be used for different purposes: one is for unit testing and the other is for integration testing.

####

#### Unit testing

The [`javaee-unittest`](https://github.com/icon-project/javaee-unittest) artifact is used to perform unit testing.

Here are the sample unit test cases.

* [HelloWorld](https://github.com/icon-project/java-score-examples/blob/master/hello-world/src/test/java/com/iconloop/score/example/AppTest.java)
* [MultisigWallet](https://github.com/icon-project/java-score-examples/blob/master/multisig-wallet/src/test/java/com/iconloop/score/example/MultiSigWalletTest.java)
* [Crowdsale](https://github.com/icon-project/java-score-examples/blob/master/sample-crowdsale/src/test/java/com/iconloop/score/example/SampleCrowdsaleTest.java)
* [IRC3Token (NFT)](https://github.com/icon-project/java-score-examples/blob/master/irc3-token/src/test/java/com/iconloop/score/example/IRC3BasicTest.java)
* [IRC2BurnableToken](https://github.com/icon-project/java-score-examples/blob/master/irc2-token/src/test/java/com/iconloop/score/example/IRC2BurnableTest.java)
* [SampleToken](https://github.com/icon-project/java-score-examples/blob/master/sample-token/src/test/java/com/iconloop/score/example/SampleTokenTest.java)

#### Integration testing

The `testinteg` subproject can be used for the integration testing. It assumes there is a running ICON network (either local or remote) that can be connected for the testing. It uses the ICON Java SDK (`foundation.icon:icon-sdk:2.0.0`) to interact with the network. The [default configuration](https://github.com/icon-project/java-score-examples/blob/master/testinteg/conf/env.props) is for [gochain-local](https://github.com/icon-project/gochain-local) network. If you want to change this configuration, either modify the configuration file directly or set the proper system property (`env.props`) when you run the integration testing (see [example](https://github.com/icon-project/java-score-examples/blob/14c4df50b146c12c27a040410411271e87efa94a/multisig-wallet/build.gradle#L69)).

Here are the sample integration test cases.

* [MultisigWallet](https://github.com/icon-project/java-score-examples/blob/master/multisig-wallet/src/intTest/java/foundation/icon/test/cases/MultiSigWalletTest.java)
* [Crowdsale](https://github.com/icon-project/java-score-examples/blob/master/sample-crowdsale/src/intTest/java/foundation/icon/test/cases/CrowdsaleTest.java)
* [IRC3Token (NFT)](https://github.com/icon-project/java-score-examples/blob/master/irc3-token/src/intTest/java/foundation/icon/test/cases/IRC3TokenTest.java)

Use `integrationTest` task to run the integration testing. Here is the example of invoking the MultisigWallet integration testing.

```
$ ./gradlew multisig-wallet:integrationTest
```
