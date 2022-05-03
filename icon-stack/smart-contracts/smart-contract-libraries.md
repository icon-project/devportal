# Smart contract libraries

### What's in a library <a href="#whats-in-a-library" id="whats-in-a-library"></a>

A library typically contains resources that are meant to be highly reusable. The two most common examples of this are patterns of execution behavior and implementations of standards. In ICON, other contracts may be used as libraries as well as traditional Java libraries.

[Information about Java libraries](https://happycoding.io/tutorials/java/libraries)

[ICON standards](https://github.com/icon-project/IIPs)

[Smart contract composability](composability.md)

### Example of how to add a library <a href="#how-to" id="how-to"></a>

```
// From https://github.com/icon-project/java-score-examples/blob/master/multisig-wallet/src/main/java/com/iconloop/score/example/MultiSigWallet.java
package com.iconloop.score.example;

// Adding official icon-java libraries
import score.Address;
import score.ArrayDB;
import score.BranchDB;
import score.Context;
import score.DictDB;
import score.VarDB;
import score.annotation.EventLog;
import score.annotation.External;
import score.annotation.Optional;
import score.annotation.Payable;
import scorex.util.StringTokenizer;

// adding java core libraries
import java.math.BigInteger;
import java.util.List;
import java.util.Map;

// Add whatever other libraries are useful as well

public class MultiSigWallet
{
...
}
```

### When to use <a href="#when-to-use" id="when-to-use"></a>

Using libraries is very useful to enhance the development process. For one, it can add increase security if you choose a library that is well audited and maintained. Additionally, it can speed up the process of developing and debugging code if you choose a library that is well documented and uses common patterns.

One major concern that a developer should be aware of is that a library may do things that you are not expecting. If you are unfamiliar with a certain aspect of a library, then you may introduce issues into your code. Make sure to review and test your code before deploying, and to be receptive and responsive to issues that may pop up after deploying.

One good rule of thumb for deciding whether or not to use a library is to check if the library is well-used. A well-used library may have a lot of stars on Github or a lot of references to usage in common developer forums. If a library is well-used, then it is less likely to have bugs, more likely to have a community that can help debug, and more likely to be maintained into the future.

### Related tools <a href="#related-tools" id="related-tools"></a>

[An ICON smart contract library for standard tokens](https://github.com/sink772/javaee-tokens)

[Some useful Java classes that can be used as substitutes for some Java standard I/O and collections frameworks](https://github.com/icon-project/javaee-scorex)

[A fast and small JSON parser and writer for Java](https://github.com/sink772/minimal-json)
