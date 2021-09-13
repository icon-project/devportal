#Context Class

Every score has an associated Context which allows the application to interface with the 
environment the SCORE is running. It includes the transaction and block context and other 
blockchain functionality.

```java
java.lang.Object score.Context
public final class Context extends java.lang.Object
```

## Methods

**getTransactionHash**

Returns the hash of the transaction.
```java
public static byte[] getTransactionHash()
```

**getTransactionIndex**

Returns the timestamp of a transaction request.
```java
public static int getTransactionIndex()
```

**getTransactionTimestamp**

Returns the timestamp of a transaction request.
```java
public static long getTransactionTimestamp()
```

**getTransactionNonce**

Returns the nonce of a transaction request.

```java
public static java.math.BigInteger getTransactionNonce()
```

**getAddress**

Returns the address of the currently-running SCORE.
```java
public static Address getAddress()
```

**getCaller**

Returns the caller's address.

```java
public static Address getCaller()

```

The caller and the origin might be same but differ in cross-calls. The origin
is the sender of the "first" invocation in the chain while the caller is whoever directly 
called the current SCORE.

**getOrigin**

Returns the originator's address.

```java
public static Address getOrigin()
```

The caller and the origin may be the same but differ in cross-calls. The 
origin is the sender of the "first" invocation in the chain while the
caller is whoever directly called the SCORE. The origin never has associated
code

**getOwner**

Returns the address of the account who initially deployed the contract.

```java
public static Address getOwner()
```

**getValue**

Returns the value being transferred to this SCORE.

```java
public static java.math.BigInteger getValue()
```

**getBlockTimestamp**

Returns the timestamp of the current block in microseconds.

```java
public static long getBlockTimestamp()

```

**getBlockHeight**

Returns the height of the block in which the transaction is included.

```java
public static long getBlockHeight()
```

**getBalance**

Returns the balance of an account.

```java
public static java.math.BigInteger getBalance(Address address) 
        throws java.lang.IllegalArgumentException
```
    Prameters:      address - the account address
    Returns:        the account balance, or 0 if the account does not exist
    Throws:         java.lang.IllegalArgumentException - if the address is invalid, e.g. NULL address

**call**

Calls the method of the given account address with the value.
```java
public static <T> T call​(java.lang.Class<T> cls, java.math.BigInteger value, 
        Address targetAddress, java.lang.String method, java.lang.Object... params)
```
    Type parameters:    T - return type
    Prameters:          cls- class of return type
    value:              the value in loop to transfer
    targetAddress:      the account address
    method:             method
    params:             parameters
    Returns:            the invocation result
    Throws:             java.lang.IllegalArgumentException - if the arguments are invalid, e.g. insufficient balance, NULL address
                        RevertedException - if call target reverts the newly created frame
                        UserRevertedException - if call target reverts the newly created frame by calling 
                        revert(int, java.lang.String)


Calls the method of the given account address with the value.
```java
public static java.lang.Object call​(java.math.BigInteger value, 
        Address targetAddress, java.lang.String method, java.lang.Object... params)
```
    Parameters: value - the value in loop to transfer
                targetAddress - the account address
                method - method
                params - parameters
    Returns:    the invocation result
    Throws:     java.lang.IllegalArgumentException - if the arguments are invalid, e.g. insufficient balance, NULL address
                RevertedException - if call target reverts the newly created frame
                UserRevertedException - if call target reverts the newly created frame by calling  
                revert(int, java.lang.String)

Calls the method of the account designated by the targetAddress.

```java
public static <T> T call​(java.lang.Class<T> cls, Address targetAddress, 
        java.lang.String method, java.lang.Object... params)
```
    Type Parameters:    T - return type
    Parameters:         cls- class of return type
                        targetAddress - the account address
                        method - method
                        params - parameters
    Returns:            the invocation result
    Throws:             java.lang.IllegalArgumentException - if the arguments are invalid, e.g. insufficient balance, NULL address
                        RevertedException - if call target reverts the newly created frame
                        UserRevertedException - if call target reverts the newly created frame by calling 
                        revert(int, java.lang.String)

Calls the method of the account designated by the targetAddress.

```java
public static java.lang.Object call(Address targetAddress, 
        java.lang.String method, java.lang.Object... params)
```

    Parameters: targetAddress - the account address
                method - method
                params - parameters
    Returns:    the invocation result
    Throws:     java.lang.IllegalArgumentException - if the arguments are invalid, e.g. insufficient balance, NULL address
                RevertedException - if call target reverts the newly created frame
                UserRevertedException - if call target reverts the newly created frame by calling 
                revert(int, java.lang.String)

**transfer**

Transfers the value to the given target address from this SCORE's account.

```java
public static void transfer(Address targetAddress, java.math.BigInteger value)

```
    Parameters: targetAddress - the account address
                value - the value in loop to transfer
    Throws:     java.lang.IllegalArgumentException - if the arguments are invalid, e.g. insufficient balance, NULL address

**deploy**

Deploys a SCORE with the given byte streams.

```java
public static Address deploy(byte[] content, java.lang.Object... params)
```

    Parameters: content - the byte streams of the SCORE
                params - parameters
    Returns:    the newly created SCORE address
    Throws:     java.lang.IllegalArgumentException - if the arguments are invalid, e.g. corrupted content, etc.

Deploys a SCORE with the given byte streams to the target address.

```java
public static Address deploy(Address targetAddress, byte[] content, java.lang.Object... params)
```
    Parameters: targetAddress - the SCORE address that is to be updated
                content - the byte streams of the SCORE
                params - parameters
    Returns:    the target SCORE address
    Throws:     java.lang.IllegalArgumentException - if the arguments are invalid, e.g. corrupted content, NULL address

**revert**

Stops the current execution and rolls back all state changes. In case of cross-calls, UserRevertedException 
would be raised to the caller with the given code and message data.

```java
public static void revert(int code, java.lang.String message)
```
    Parameters: code - an arbitrary user-defined code
                message - a message to be delivered to the caller

**require**

Checks that the provided condition is true and if it is false, triggers a revert.
If the condition == true, the method does nothing.

```java
public static void require(boolean condition)
```
    Parameters: condition - the condition that is required to be true.

**println**

Prints a message, for debugging purpose.

```java
public static void println(java.lang.String message)
```
    Parameters: message - the message to print

**hash**

Returns hash value of the given message.

```java
public static byte[] hash(java.lang.String alg, byte[] msg)
```
    Parameters: alg - hash algorithm. One of sha-256, sha3-256, keccak-256, xxhash-128, blake2b-128 and blake2b-256.
                msg - message
    Returns:    hash value
    Throws:     java.lang.IllegalArgumentException - if the algorithm is unsupported.

**verifySignature**

Returns true if the given signature for the given message by the given public key is correct.

```java
public static boolean verifySignature(java.lang.String alg, byte[] msg, byte[] sig, byte[] pubKey)

```
    Parameters: alg - signature algorithm. One of ed25519 and ecdsa-secp256k1
                msg - message
                sig - signature
                pubKey - public key
    Returns:    true if the given signature for the given message by the given public key is correct.
    Throws:     java.lang.IllegalArgumentException - if the algorithm is unsupported.

**recoverKey**

Recovers the public key from the message and the recoverable signature.

```java
public static byte[] recoverKey(java.lang.String alg, byte[] msg, byte[] sig, boolean compressed)
```
    Parameters: alg - signature algorithm. ecdsa-secp256k1 is supported.
                msg - message
                sig - signature
                compressed - the type of public key to be returned
    Returns:    the public key recovered from message and signature
    Throws:     java.lang.IllegalArgumentException - if the algorithm is unsupported.

**getAddressFromKey**

Returns the address that is associated with the given public key.

```java
public static Address getAddressFromKey(byte[] pubKey)
```
    Parameters: pubKey - a byte array that represents the public key
    Returns:    the address that is associated with the public key

**getFeeSharingProportion**

Returns the current fee sharing proportion of the SCORE. 100 means the SCORE will pay 
100% of transaction fees on behalf of the transaction sender.

```java
public static int getFeeSharingProportion()
```

**setFeeSharingProportion**

Sets the proportion of transaction fees that the SCORE will pay. proportion should be between 0 and 100. If this method 
is invoked multiple times, the last proportion value will be used.

```java
public static void setFeeSharingProportion(int proportion)
```
    Parameters: proportion - the desired proportion of transaction fees that the SCORE will pay
    Throws:     java.lang.IllegalArgumentException - if the proportion is not betwee

**newBranchDB**

Returns a new branch DB.

```java

public static <K,V> BranchDB<K,V> newBranchDB(java.lang.String id, java.lang.Class<?> leafValueClass)
```
    Type Parameters:K - key type
                    V - sub-DB type
    Parameters:     id - DB ID
                    leafValueClass - class of leaf value. For example, a branch DB of type BranchDB<BigInteger, DictDB<Address, Boolean>> has Boolean.class as its leaf value class.
    Returns:        new branch DB

**newDictDB**

Returns a new dictionary DB

```java
public static <K,V> DictDB<K,V> newDictDB(java.lang.String id, java.lang.Class<V> valueClass)
```
    Type Parameters:K - key type
                    V - value type
    Parameters:     id - DB ID
                    valueClass - class of V
    Returns:        new dictionary DB

**newArrayDB**

Returns a new array DB.

```java
public static <E> ArrayDB<E> newArrayDB(java.lang.String id, java.lang.Class<E> valueClass)
```
    Type Parameters:E - element type
    Parameters:     id - DB ID
                    valueClass - class of E
    Returns:        new array DB

**newVarDB**

Returns a new variable DB.

```java
public static <E> VarDB<E> newVarDB(java.lang.String id, java.lang.Class<E> valueClass)
```
    Type Parameters:E - variable type
    Parameters:     id - DB ID
    valueClass:     class of E
    Returns:        new variable DB

**logEvent**

Records a log on the blockchain. It is recommended to use EventLog annotation rather than this method directly.

```java
public static void logEvent(java.lang.Object[] indexed, java.lang.Object[] data)
```
    Parameters: indexed - indexed data
    data: extra data

**newByteArrayObjectReader**

Returns a new object reader reading from a byte array.

```java
public static ObjectReader newByteArrayObjectReader(java.lang.String codec, byte[] byteArray)
```
    Parameters: codec - codec. (Currently "RLPn" is supported)
                byteArray - byte array.
    Returns:    object reader.
    Throws:     java.lang.IllegalArgumentException - if the codec is unsupported.

**newByteArrayObjectWriter**

Returns a new object writer writing to a byte array.

```java
public static ByteArrayObjectWriter newByteArrayObjectWriter(java.lang.String codec)
```
    Parameters: codec - codec. (Currently "RLPn" is supported)
    Returns:    byte array object writer
    Throws:     java.lang.IllegalArgumentException - if the codec is unsupported.
