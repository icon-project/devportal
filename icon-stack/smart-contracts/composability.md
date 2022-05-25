# Composability

### A brief introduction <a href="#a-brief-introduction" id="a-brief-introduction"></a>

Smart contracts on ICON contain public interfaces. Other smart contracts that may want to make use of those interfaces can call them as if they were [libraries](smart-contract-libraries.md). You can write some very complicated [decentralized applications (dApps)](../../projects/decentralized-applications-dapps/) with some very simple code by using public interfaces of useful smart contracts on ICON.

### How to invoke a external method of another smart contract

One smart contract can invoke an external method of another using the following APIs.

```
// [package score.Context]
public static Object call(Address targetAddress, String method, Object... params);

public static Object call(BigInteger value,
                          Address targetAddress, String method, Object... params);
```

The following example is for calling `tokenFallback`.

```
if (_to.isContract()) {
    Context.call(_to, "tokenFallback", _from, _value, dataBytes);
}
```

### Further resources

[Composability: What is it and why should you care?](https://mrgnd.io/posts/composability-21072021/)
