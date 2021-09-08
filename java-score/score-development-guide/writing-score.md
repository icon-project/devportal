# Writing Score

The document presents how to write a SCORE, smart contract of the ICON network. Through the series of documents, you will learn from setting the workspace to deploying a SCORE.

SCORE is the platform for ICON’s smart contract. But it is also used to refer to the ICON’s smart contract itself.

### Intended Audience

The intended audience is the developers who have basic Java programming knowledge.

### Purpose

After reading this document, you will understand the structure of SCORE and learn the basic syntax of writing SCORE.

### Prerequisite

We assume you have already set up a local environment to deploy java score using gloop CLI library. While setting up the environment you would have to build the docker image and javaee-api.jar for local.
-	[Deploy Java Score](https://www.icondev.io/icon-2.0/java-score/tutorial)

### Creating a Workspace

The score is compiled the following the same structure as any other Java program. `gradle-javaee-plugin` is a Gradle plugin to automate the process of generating the optimized jar bundle. The generated jar bundle can be used for deployment to ICON networks that support the Java SCORE execution environment (a.k.a. JavaEE).

The `build.gradle` file contains the dependencies which contain the dependent packages for building the SCORE.
`optimizedJar` extends from Gradle’s `Jar` type i.e. all attributes and methods available on `Jar` are also available on optimizedJar. In optimizedJar `mainClassName` property is required to indicate the entry point for initial execution. This property is included into the generated jar bundle with the `Main-Class` header in the manifest.

	optimizedJar {
    mainClassName = 'com.iconloop.score.example.HelloWorld'
}

The `deployJar` extension is used to deploy the optimized jar to local or remote ICON networks that support the Java SCORE execution environment.

	deployJar {
    endpoints {
        local {
            uri = 'http://localhost:9082/api/v3'
            nid = 3
        }
    }
    keystore = './mykey.json'
    password = 'keypass'
    parameters {
        arg('name', 'Alice')
    }
}

### Structure of Score

Every SCORE has a main class which is specified in the `build.gradle`. The main class should have more than one external method which is supposed to be invoked by transactions from EOA or other Smart Contracts. Transactions will result in the internal state update.

The following is the main class content for the [Hello world](https://github.com/icon-project/java-score-examples/blob/master/hello-world/src/main/java/com/iconloop/score/example/HelloWorld.java) score:

```java
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

```

Below is the comparison of Java to  Python Score Structure

| Name                | Python Score               | Java Score                  |
|---------------------|----------------------------|-----------------------------|
| External decorator  | @external                  | @External                   |
| -(readonly)         | @external(readonly=True)   | @External(readonly=true)    |
| Payable decorator   | @payable                   | @Payable|
| Eventlog decorator  | @eventlog                  | @EventLog                   |
| - (indexed)         | @eventlog(indexed=1)       | @EventLog(indexed=1)        |
| fallback signature  | def fallback               | void fallback()             |
| SCORE initialize    | override on_install method | define a public constructor |
| Default parameters  | native language support    | @Optional                   |

The details of the tables are discussed further down below.

### Java Score Library for ICON Standard Tokens

By Including the package from [Maven Central](https://search.maven.org/search?q=g:com.github.sink772%20a:javaee-tokens) in the dependency of `build.gradle` you can create ICON standard Tokens like IRC2 and IRC3. Using this you no longer required to write the whole thing from scratch.

	implementation 'com.github.sink772:javaee-tokens:0.6.0'


You need to create an entry Java class to inherit the attributes and methods from the basic token classes. The example below would be the simplest IRC2 token SCORE with a fixed supply.

```java
public class IRC2FixedSupply extends IRC2Basic {
    public IRC2FixedSupply(String _name, String _symbol) {
        super(_name, _symbol, 3);
        _mint(Context.getCaller(), BigInteger.valueOf(1000000));
    	}
	}
```

### Built-In Properties

**Address Class**

Address Class represents an address of an account in the ICON Network. Following is the method summary of the class.

| Modifier and Type | Method        | Description                                                             |
|-------------------|---------------|-------------------------------------------------------------------------|
| boolean           | equals        | Compares this address to the specified object.                          |
| static Address    | fromString    | Creates an address from the hex string format.                          |
| int               | hashCode()    | Returns a hash code for this address.                                   |
| boolean           | isContract()  | Returns true if and only if this address represents a contract address. |
| byte[]            | toByteArray() | Converts this address to a new byte array.                              |
| java.lang.String  | toString()    | Returns a string representation of this address.                        |

**Context Class**

Every score has an associated Context which allows the application to interface with the environment the SCORE is running.
It includes the transaction and block context and other blockchain functionality.
Following is the summary of the methods of the class

| Modifier and Type            | Method                                                                                                                               | Description                                                                                   |
|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| static <T> T                 | call(java.lang.Class<T> cls, java.math.BigInteger value, Address targetAddress, java.lang.String method, java.lang.Object... params) | Calls the method of the given account address with the value.                                 |
| static <T> T                 | call(java.lang.Class<T> cls, Address targetAddress, java.lang.String method, java.lang.Object... params)                             | Calls the method of the account designated by the targetAddress.                              |
| static java.lang.Object      | call(java.math.BigInteger value, Address targetAddress, java.lang.String method, java.lang.Object... params)                         | Calls the method of the given account address with the value.                                 |
| static java.lang.Object      | call(Address targetAddress, java.lang.String method, java.lang.Object... params)                                                     | Calls the method of the account designated by the targetAddress.                              |
| static Address               | deploy(byte[] content, java.lang.Object... params)                                                                                   | Deploys a SCORE with the given byte streams.                                                  |
| static Address               | deploy(Address targetAddress, byte[] content, java.lang.Object... params)                                                            | Deploys a SCORE with the given byte streams to the target address.                            |
| static Address               | getAddress()                                                                                                                         | Returns the address of the currently-running SCORE.                                           |
| static Address               | getAddressFromKey(byte[] pubKey)                                                                                                     | Returns the address that is associated with the given public key.                             |
| static java.math.BigInteger  | getBalance(Address address)                                                                                                          | Returns the balance of an account.                                                            |
| static long                  | getBlockHeight()                                                                                                                     | Returns the block height.                                                                     |
| static long                  | getBlockTimestamp()                                                                                                                  | Returns the block timestamp.                                                                  |
| static Address               | getCaller()                                                                                                                          | Returns the caller's address.                                                                 |
| static int                   | getFeeSharingProportion()                                                                                                            | Returns the current fee sharing proportion of the SCORE.                                      |
| static Address               | getOrigin()                                                                                                                          | Returns the originator's address.                                                             |
| static Address               | getOwner()                                                                                                                           | Returns the address of the account who initially deployed the contract.                       |
| static byte[]                | getTransactionHash()                                                                                                                 | Returns the hash of the transaction.                                                          |
| static int                   | getTransactionIndex()                                                                                                                | Returns the transaction index in a block.                                                     |
| static java.math.BigInteger  | getTransactionNonce()                                                                                                                | Returns the nonce of a transaction request.                                                   |
| static long                  | getTransactionTimestamp()                                                                                                            | Returns the timestamp of a transaction request.                                               |
| static java.math.BigInteger  | getValue()                                                                                                                           | Returns the value being transferred to this SCORE.                                            |
| static byte[]                | hash(java.lang.String alg, byte[] msg)                                                                                               | Returns hash value of the given message.                                                      |
| static void                  | logEvent(java.lang.Object[] indexed, java.lang.Object[] data)                                                                        | Records a log on the blockchain.                                                              |
| static ArrayDB        | newArrayDB(java.lang.String id, java.lang.Class<E> valueClass)                                                                       | Returns a new array DB.                                                                       |
| static <K,V> BranchDB<K,V>   | newBranchDB(java.lang.String id, java.lang.Class<?> leafValueClass)                                                                  | Returns a new branch DB.                                                                      |
| static ObjectReader          | newByteArrayObjectReader(java.lang.String codec, byte[] byteArray)                                                                   | Returns a new object reader reading from a byte array.                                        |
| static ByteArrayObjectWriter | newByteArrayObjectWriter(java.lang.String codec)                                                                                     | Returns a new object writer writing to a byte array.                                          |
| static <K,V> DictDB<K,V>     | newDictDB(java.lang.String id, java.lang.Class<V> valueClass)                                                                        | Returns a new dictionary DB.                                                                  |
| static <E> VarDB<E>          | newVarDB(java.lang.String id, java.lang.Class<E> valueClass)                                                                         | Returns a new variable DB.                                                                    |
| static void                  | println(java.lang.String message)                                                                                                    | Prints a message, for debugging purpose.                                                      |
| static byte[]                | recoverKey(java.lang.String alg, byte[] msg, byte[] sig, boolean compressed)                                                         | Recovers the public key from the message and the recoverable signature.                       |
| static void                  | require(boolean condition)                                                                                                           | Checks that the provided condition is true and if it is false, triggers a revert.             |
| static void                  | require(boolean condition, java.lang.String message)                                                                                 | Checks that the provided condition is true and if it is false, triggers a revert.             |
| static void                  | revert()                                                                                                                             | Stops the current execution and rolls back all state changes.                                 |
| static void                  | revert(int code)                                                                                                                     | Stops the current execution and rolls back all state changes.                                 |
| static void                  | revert(int code, java.lang.String message)                                                                                           | Stops the current execution and rolls back all state changes.                                 |
| static void                  | revert(java.lang.String message)                                                                                                     | Stops the current execution and rolls back all state changes.                                 |
| static void                  | setFeeSharingProportion(int proportion)                                                                                              | Sets the proportion of transaction fees that the SCORE will pay.                              |
| static void                  | transfer(Address targetAddress, java.math.BigInteger value)                                                                          | Transfers the value to the given target address from this SCORE's account.                    |
| static boolean               | verifySignature(java.lang.String alg, byte[] msg, byte[] sig, byte[] pubKey)                                                         | Returns true if the given signature for the given message by the given public key is correct. |


### Implementing Score External Methods

Users can implement methods which are supposed to be involved from outside the blockchain. These methods can be decorated with `@External` and/or `@Payable`. Additionally, users can declare methods decorated with `@Eventlog` which can be used to leave custom event logs in the TxResult.

**External Annotators (@External)**

This annotation can be used to indicate whether the method will be exposed externally or not.
Methods should be annotated as `@External` so that they can be called from outside the contract (EOA or another contract). The annotated methods are registered on the exportable API list.
Any attempt to call a non-external method from outside the contract will fail.
If the `readonly` element is specified and its value is true, i.e. `@External (readonly =true)`, the method will have read-only access to the state database.

**Payable Annotators (@Payable)**

This annotation can be used to indicate whether the method can receive ICX coins. External method annotated with `@Payable` can receive the incoming ICX coins designated in the transaction message and process further works for it. Users can get the value of transferred ICX coins by using `Context.getValue()`.
If ICX coins are passed to a non-payable method, that transaction would fail.

**Eventlog Annotators (@Eventlog)**

This annotation can be used to record logs in its TxResult as eventLogs.

If the value of an element, named `indexed`, is set, the designated number of parameters of the applied method declaration will be indexed in the order and included in the Bloom filter. Indexed parameters and non-indexed parameters are separately stored in the TxResult. At most 3 parameters can be indexed, and the value of indexed cannot exceed the number of parameters. Possible data types for method parameters are `int, boolean, byte[], BigInteger, String, and Address`.

**Optional Annotators (@Optional)**

This annotation can be used to indicate whether the method parameter is optional. If a method parameter is annotated with this Optional, the parameter can be omitted in the transaction message. If optional parameters were omitted when the external method is called, the value of optional parameters would be their zero values.  The zero value is: 0 for numeric types, false for the boolean type, and null for Object types.

**fallback**

`fallback` is a special method that is invoked whenever the contract receives plain ICX coins without data.

```java
@Payable
public void fallback() {
    // just receive incoming funds
    }
```

However, if the `fallback` method is not annotated with `@Payable`, it would not be listed on the SCORE APIs and could not be called as well.
The `fallback` method cannot be annotated with `@External`  (i.e., fallback method cannot be specified in the transaction message as a callee method)

### Storing State Data

The state of SCORE should be stored in the state database in the blockchain. The state of SCORE simply means the values of member variables in the smart contract which are declared as VarDB, DictDB, and ArrayDB. The state of SCORE is changed only by the execution of transactions. All nodes execute transactions and change the state of the smart contract in their own state database independently. To be confirmed, the changed state should be agreed between the ⅔ of nodes after each execution of transactions, and this process is called consensus in the blockchain.

**VarDB(variable type)**

Variable type parameters can be  a readable and writable class. It holds one value.

**DictDB(key type, value type)**

A dictionary DB is a hash from key to value. Only values of the dictionary DB are recorded in the DB. Keys are not recorded.
Key type can be `String`, `byte array`, `Address`, `Byte`, `Short`, `Integer`, `Long`, `Character` or `BigInteger`.
Value Type can be readable and writable class

**ArrayDB(element type)**

An ArrayDB holds a sequence of values. Element type parameters is a readable and writable class.

**BranchDB(key type, value type)**

A branch DB is a hash from keys to sub-DBs.
Key type can be `String`, `byte array`, `Address`, `Byte`, `Short`, `Integer`, `Long`, `Character` or `BigInteger`.
Value type can be `VarDb`, `DictDB`, `ArrayDB` or `BranchDB`.

### Invoking Other Score Methods

One SCORE can invoke an external method of another SCORE using the following APIs.
```java
// [package score.Context]
public static Object call(Address targetAddress, String method, Object... params);

public static Object call(BigInteger value,Address targetAddress, String method, Object... params);

```

The following example is for calling `tokenFallback`.

```java
if (_to.isContract()) {
    Context.call(_to, "tokenFallback", _from, _value, dataBytes);
    }
```

### Exception Handling

**RevertedException**

Signals a failure of an inter-contract call. The construction constructs a new exception on calling.

**UserRevertedException**

Signals a manual reversion from a score. The construction constructs a new exception on calling.

**UserRevertException**

Signals failure of an External method. If an external method is called by an external transaction or an internal transaction and the method throws an exception of this class or subclass of this class during the execution, the transaction fails and the code returned by getCode() is used as the user failure code of the transaction. If the code is out of range, the code is clamped.
