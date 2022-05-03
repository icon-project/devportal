# Smart contract anatomy

A [smart contract](./) runs a piece of code and maintains the state of the interactions with that code by different accounts. The three components of a smart contract are the [Data](smart-contract-anatomy.md#data), [Functions](smart-contract-anatomy.md#functions), and [Events + Logs](smart-contract-anatomy.md#events-and-logs)

### Data

Smart contract data can be stored either permanently or temporarily. The placement of variables is decided by the programmer. Permanent variables are placed into _storage_, while temporary variables are placed into _memory_.

#### Storage

Permanent storage is much more expensive than temporary memory. See more information about the [ICON execution environments](../icon-execution-environments/) to understand exactly how data gets permanently stored, including information on the data structures and repercussions associated with them.

A permanent variable is defined as a member of a class.

```
public class HelloWorld {
    private String name; // Permanent variable
}
```

#### Memory

Temporary memory is, conversely, much cheaper than permanent storage.

Temporary variables are defined as variables inside of functions.

```
public class HelloWorld {
    private String name;

    public HelloWorld(String name) {
        this.name = name;
    }


    @External(readonly=true)
    public String getGreeting() {
        String msg = "Hello " + name + "!"; // `msg` is a temporary variable
        Context.println(msg);
        return msg;
    }
}
```

### Functions

Functions on ICON are equivalent to functions in Java. ICON adds additional annotations that are parsed by the [Java execution environment](../icon-execution-environments/java.md) to do things like make a function available as a part of the public API. These annotations can be found [here](https://www.javadoc.io/doc/foundation.icon/javaee-api/latest/score/annotation/package-summary.html).

[Java classes](https://www.w3schools.com/java/java\_classes.asp)

[Java methods](https://www.w3schools.com/java/java\_methods.asp)

#### Writing functions

Here is an example of a function that is made available for the public API of a smart contract:

```
@External(readonly=true)
public String getGreeting() {
    String msg = "Hello " + name + "!";
    Context.println(msg);
    return msg;
}
```

### Events and logs

Events are useful to track that some action has occurred within the processing of a smart contract. They are written to the blockchain as logs, which can then be used by external interfaces. For example, they can be used in user interfaces to provide updates on the status of an operation:

```
    // From https://github.com/icon-project/java-score-examples/blob/master/sample-crowdsale/src/main/java/com/iconloop/score/example/SampleCrowdsale.java
public class SampleCrowdsale
{
    ...
    @External
    public void tokenFallback(Address _from, BigInteger _value, byte[] _data) {
        // check if the caller is a token SCORE address that this SCORE is interested in
        Context.require(Context.getCaller().equals(this.tokenScore));

        // depositing tokens can only be done by owner
        Context.require(Context.getOwner().equals(_from));

        // value should be greater than zero
        Context.require(_value.compareTo(BigInteger.ZERO) >= 0);

        // start Crowdsale hereafter
        Context.require(this.crowdsaleClosed);
        this.crowdsaleClosed = false;
        // emit eventlog
        CrowdsaleStarted(this.fundingGoal, this.deadline);
    }
    ...
    
    @EventLog
    protected void CrowdsaleStarted(BigInteger fundingGoal, long deadline) {}
    ...
}
```

