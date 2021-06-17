# Part 1. HelloWorld on local emulated environment

### Install T-Bears \(Docker\)

* Install Docker \[[Get Started with Docker](https://www.docker.com/get-started)\]
* Install T-Bears and run the container.

Below command will download tbears docker image, create a container, start the container, and attach your stdin/stderr to the container.

```text
$ docker run -it --name local-tbears -p 9000:9000 iconloop/tbears:mainnet
 * Starting RabbitMQ Messaging Server rabbitmq-server                    [ OK ]
Made tbears_cli_config.json, tbears_server_config.json, keystore_test1 successfully
Started tbears service successfully
root@c5b81f9874ee:/tbears#
```

Exit and stop the container.

```text
root@07dfee84208e:/tbears# exit
```

Show the container list.

```text
$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS                    NAMES
c5b81f9874ee        iconloop/tbears     "entry.sh"          6 minutes ago       Up 6 minutes               0.0.0.0:9000->9000/tcp   local-tbears
```

Start and attach to the container, or attach to a running container.

```text
$ docker container start -a local-tbears
root@07dfee84208e:/tbears#

$ docker container attach local-tbears
root@07dfee84208e:/tbears#
```

### Test account

Test keystore file, `keystore_test1`, is provided for your convenience. Be careful not to use this keystore file to sign your transaction to any network other than T-Bears, because the password is open to public.

* Address : hxe7af5fcfd8dfc67530a01a0e403882687528dfcb
* Password : test1\_Account
* ICX balance : 0x2961fff8ca4a62327800000

### Check your account balance

Let's issue a `tbears balance` command to check your balance.

```text
root@07dfee84208e:/tbears# tbears balance hxe7af5fcfd8dfc67530a01a0e403882687528dfcb
balance in hex: 0x2961fff8ca4a62327800000
balance in decimal: 800460000000000000000000000
```

### Create HelloWorld contract and deploy it

`tbears init` command will initialize a project.

```text
root@07dfee84208e:/tbears# tbears init hello_world HelloWorld
Initialized tbears successfully

root@07dfee84208e:/tbears# ls hello_world
hello_world.py  __init__.py  package.json
```

`hello_world.py` has the SCORE implementation template which is fully functional.

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

Let's deploy the contract without modification and get the contract address. For every transaction, you need to check the execution result using txhash. Transaction result contains the SCORE address if the deployment was successful.

```text
root@07dfee84208e:/tbears# tbears deploy hello_world
Send deploy request successfully.
If you want to check SCORE deployed successfully, execute txresult command
transaction hash: 0xc40cbbf2b89cd1e2890132145e6d86ad61835edaca0bcc3a4c34b5cb22b8be28

root@07dfee84208e:/tbears# tbears txresult 0xc40cbbf2b89cd1e2890132145e6d86ad61835edaca0bcc3a4c34b5cb22b8be28
Transaction result: {
    "jsonrpc": "2.0",
    "result": {
        "txHash": "0xc40cbbf2b89cd1e2890132145e6d86ad61835edaca0bcc3a4c34b5cb22b8be28",
        "blockHeight": "0x3158",
        "blockHash": "0x9e0c1385128bf0d425773f9f9130d683d327a058a9c8dc0a6c4df71bb98195e1",
        "txIndex": "0x0",
        "to": "cx3176b5d6cae66a1abbc3ca9070423a5c708834a9",
        "scoreAddress": "cx3176b5d6cae66a1abbc3ca9070423a5c708834a9", <-- SCORE address
        "stepUsed": "0x3cdba380",
        "stepPrice": "0x2540be400",
        "cumulativeStepUsed": "0x3cdba380",
        "eventLogs": [],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1"
    },
    "id": 1
}
```

