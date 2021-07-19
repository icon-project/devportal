---
description: 'The source code is found on GitHub at https://github.com/icon-project/samples'
---

# HelloWorld

The simplest yet completely legitimate SCORE that returns "Hello" in the response message.

```python
from iconservice import *

class HelloWorld(IconScoreBase):

    def __init__(self, db: IconScoreDatabase) -> None:
        super().__init__(db)

    def on_install(self) -> None:
        super().on_install()

    def on_update(self) -> None:
        super().on_update()

    @external(readonly=True)
    def hello(self) -> str:
        return "Hello"
```

We will add a little tweak to our HelloWorld.

* Give it a name, "HelloWorld"
* Print logs. 
* Allow it to accept ICX and other tokens.

```python
from iconservice import *

TAG = 'HelloWorld'

class HelloWorld(IconScoreBase):

    def __init__(self, db: IconScoreDatabase) -> None:
        super().__init__(db)

    def on_install(self) -> None:
        super().on_install()

    def on_update(self) -> None:
        super().on_update()

    @external(readonly=True)
    def name(self) -> str:
        return "HelloWorld"

    @external(readonly=True)
    def hello(self) -> str:
        Logger.info('Hello, world!', TAG)
        return "Hello"

    @payable
    def fallback(self):
        Logger.info('fallback is called', TAG)

    @external
    def tokenFallback(self, _from: Address, _value: int, _data: bytes):
        Logger.info('tokenFallabck is called', TAG)
```

`fallback` function is added to accept ICX. `fallback` function is executed when the contract receives a transaction request without `data` part. Not having `data` in the transaction means no method name is specified. In such a case, `fallback` function is invoked if the function is provided. If `fallback` function is not given, the transaction will fail.

ICX transfer request message does not have `data` part. So, the default behavior of any contract is rejecting incoming ICX. This design is to prevent any accidental ICX transfer to a contract.

Only functions with `@payable` decorator are permitted to receive ICX coins, therefore, our `fallback` function should be also `@payable` because we want the contract to receive ICX. You don't need to implement anything in the fallback function to handle the ICX transfer. Having payable fallback function declares that the contract is designed to accept ICX coin transfer.

`tokenFallback` method is added to accept IRC2 tokens. The [IRC2](https://github.com/icon-project/IIPs/blob/master/IIPS/iip-2.md#token-fallback) standard mandates the `tokenFallback` method in the receiving SCORE. Token contract will call the `tokenFallback` function whenever it transfers token to a contract address.

### GitHub

Please refer to [Github](https://github.com/icon-project/samples/) for complete source code.

#### Invoke a method from CLI

Below is the T-Bears CLI command to invoke the SCORE's external read-only method. Before issuing the command, don't forget to change the "to" value in the JSON request with an actual SCORE address.

```bash
root@07dfee84208e:/tbears# cat call.json 
{
    "jsonrpc": "2.0",
    "method": "icx_call",
    "id": 1,
    "params": {
        "to": "cx3176b5d6cae66a1abbc3ca9070423a5c708834a9", 
        "dataType": "call",
        "data": {
            "method": "hello"
        }
    }
}
root@07dfee84208e:/tbears# tbears call call.json 
response : {
    "jsonrpc": "2.0",
    "result": "Hello",
    "id": 1
}
```

#### Run test

* Testcase uses python sdk. You need to install python sdk to run the test.

```bash
$ pip3 install iconsdk
```

* Go to the `tests` folder, open `test.py`, and change the global variables.

```bash
$ tree hello_world
hello_world
├── README.md
├── hello_world
│   ├── __init__.py
│   ├── hello_world.py
│   └── package.json
└── tests
    ├── keystore_test1
    └── test.py
```

Use the actual SCORE addresses. To run the test, `keystore_test1` should have a positive amount of IRC2 tokens. Deploy the [IRC2 token contract](https://github.com/sink772/IRC2-token-standard) with `-k keystore_test1` option, then all the initial token supply will go to the account. If you test on T-Bears, use the default `node_uri`. If test on other networks, change the `node_uri` and `network_id` accordingly.

```python
node_uri = "http://localhost:9000/api/v3"
network_id = 3
hello_world_address = "cx3176b5d6cae66a1abbc3ca9070423a5c708834a9"
token_address = "cx9a4c4229ab2cbd61a5cc051fbbb6ee7e3e3adfac"
```

* Run the test.

```bash
$ python3 test.py
....
----------------------------------------------------------------------
Ran 4 tests in 20.369s

OK
```

If you tailed a log, you would see the logs printing.

```bash
$ tail -f tbears.log | grep '\[HelloWorld\]'
[INFO|logger.py:332] 2018-10-07 18:10:48,974 > [HelloWorld] Hello, world!
[INFO|logger.py:332] 2018-10-07 18:10:58,194 > [HelloWorld] fallback is called
[INFO|logger.py:332] 2018-10-07 18:10:58,219 > [HelloWorld] tokenFallabck is called
```

