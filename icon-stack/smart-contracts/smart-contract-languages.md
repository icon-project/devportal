# Smart contract languages

### Java

One of the design decisions of ICON is to develop on top of existing, robust, and well-used technologies to aid in the developer experience. ICON supports smart contract development in Java, which is consistently listed as a [top 5 programming language](https://www.tiobe.com/tiobe-index/) for quality and usability.

Some key points about Java:

* Object-oriented, high-level
* Curly-brace notation
* Statically-typed
* Supports inheritance, libraries, user-defined types, etc

There is a tremendous amount of learning material available for Java. See the [_Further Resources_](smart-contract-languages.md#further-resources) section for more information.

#### Example contract

The following is the _hello world_ contract sample taken from the [ICON smart contract examples](https://github.com/icon-project/java-score-examples) repository on Github.

```
/*
 * Copyright 2020 ICONLOOP Inc.
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
import score.annotation.External;
import score.annotation.Payable;

public class HelloWorld {
    private String name;

    public HelloWorld(String name) {
        this.name = name;
    }

    @External()
    public void setName(String name) {
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
}

```

#### Further resources

[ICON Smart contract API documentations](https://www.javadoc.io/doc/foundation.icon/javaee-api/latest/index.html)

[ICON Smart contract examples](https://github.com/icon-project/java-score-examples)

[W3 Java tutorial](https://www.w3schools.com/java/)

[Java -- Wikipedia](https://en.wikipedia.org/wiki/Java\_\(programming\_language\))

[Java -- Codecademy](https://www.codecademy.com/resources/docs/java)
