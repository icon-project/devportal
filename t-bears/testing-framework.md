# Testing Framework

### Functions Provided by **ScoreTestCase**

SCORE unit test class must inherit `ScoreTestCase`. Deploying SCORE after every code modification is a hassle. `ScoreTestCase` helps you test SCORE logic itself in an isolated environment without network deployment. `ScoreTestCase` provides following functions.

* Instantiate SCORE
  * Instantiate SCORE. You can access the attributes and methods of the SCORE as if it a general object.
* Set SCORE properties
  * Provide the ability to set the property values in the SCORE instance. You can set `tx`, `msg`, and the current block height and timestamp in your test code to mimic the precondition of a test.
* Mock state DB
  * Test code can set / get the state variables of the SCORE as if it is executed on the blockchain. The state is stored in memory, not in the file system. Unit-test code execution does not affect on local T-Bears server state \(Level DB\) in any way. 
* Assert eventlog invocation
  * Provides a way to verify that the evenlog function is called in your SCORE implementation as per the method specification. 
* Patch & assert InternalCall \(invoking external functions in other SCORE\).
  * Provides a way to assert that the external function has been called with specific arguments. It assumes that external modules behave as per the specification. 

### Functions Provided by **IconIntegrateTestBase**

Every SCORE integration test class must inherit `IconIntegrateTestBase`. `IconIntegrateTestBase` class provides three functions

* Support Python unittest
  * You can write and run the test method with prefix `test_`
  * You can initialize and finalize the test by overriding `setUp` and `tearDown`.
* Emulate ICON service for testing
  * Initialize ICON service and confirm the genesis block
  * Create accounts for test
    * self.\_test1 : Account with 1,000,000 ICX
    * self.\_wallet\_array\[\] : 10 empty accounts in a list
* Provide APIs for SCORE integration test
  * process\_transaction\(\)

    Invoke transaction and return the transaction result

  * process\_call\(\)

    Call SCORE's read-only external function and return the result

SCORE integration test code is implemented as follows.

* Deploy the SCORE to be tested. 
* Create an ICON JSON-RPC API request for the SCORE API you want to test
* If necessary, sign the ICON JSON-RPC API request
* Invoke the ICON JSON-RPC API request and get the result
* Check the result

### Writing Tests

Please follow the below guidelines to learn about testing SCOREs on T-Bears.

* [How-to write SCORE unit test](doc:how-to-write-score-unit-test) 
* [How-to write SCORE integration test](doc:how-to-write-score-integration-test)

