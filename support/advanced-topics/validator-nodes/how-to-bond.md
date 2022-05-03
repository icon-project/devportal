# How to bond



## Introduction

For an in depth technical specification of the bond [heads over here](https://docs.google.com/document/d/1WZzbiuMbT7XNKwuGXata20G6B0gnzW7qcGTfCXEpZ1w/edit#heading=h.44sinio).

### Initial settings

When IISS 3.1 will be activated, the **bond requirement will be set to 5%**. The **slashing penalty will be set to 0%** until the network and goloop is stable enough.

## How to post a bond

{% hint style="info" %}
You can find command line example further down this page
{% endhint %}

{% hint style="danger" %}
If you want to remove your bond, there is an unbonding period of 7 terms, during which your bond can still be slashed
{% endhint %}

### Using Keystore

Posting a bond is done in 3 steps:

1. P-Rep whitelist up to 10 addresses which can post the bond. This is done using the following RPC command (howto [Broken link](broken-reference "mention"))
2. Whitelisted bonder then must stake ICX to the P-Rep using (howto [Broken link](broken-reference "mention"))
3. Whitelisted bonder then must post the bond to the P-Rep using (howto [Broken link](broken-reference "mention"))

{% hint style="info" %}
ICONists can post a bond to up to 100 P-reps, if they are whitelisted by them

A P-Rep can only receive a bond from 10 different ICONists
{% endhint %}

You can do this using [https://github.com/icon-project/preptools](https://github.com/icon-project/preptools), the [Broken link](broken-reference "mention")`CallTransactionBuilder` or directly using goloop RPC command[Broken link](broken-reference "mention").

### Using Ledger

You can manage your bond through ICONex and Ledger hardware. Here is a quick video to demonstrate the process.

{% embed url="https://drive.google.com/file/d/1_GLURPIsVI3q1PsIw2vomhNanOvuholi/view?usp=sharing" %}

## Penalty/Slashing

When a P-Rep does not behave as expected, this bond will be slashed (ICX will be burn) following those specific rules.

{% hint style="info" %}
An **opportunity** is defined as **one term** as Main P-Rep. Therefore, 30 opportunities means the last 30 terms as main P-Rep _(**which can be consecutive or not**)_

\_\_\
_For example:_

* You are main prep for term1 to term100, you receive one penalty at term 100
* You are sub prep from term101 to term199
* You become main prep again from term200, **you still have the previous penalty recorded**

\
To get rid of all your previous penalties, make sure your node behave properly for 30 opportunities (30 terms as main prep).
{% endhint %}

![](../../../.gitbook/assets/f8d977c64b14a38161633f22f3b027b90c35366b.jpeg)

## Command line example to post a bond

{% hint style="info" %}
You can use public endpoints instead of a local node `url/uri`
{% endhint %}

### preptools

#### config.json file (used in below commands)

```
{
  "url": "http://localhost:9080/api/v3",
  "nid": 7,
  "keystore": "wallet.json"
}
```

#### setBonderList

```
preptools setBonderList -c config.json --bonder-list hx5be1c0b343698ab2370524b598d48603d7c44a12,hxfbc0a8b4e5e8df8a2e93e8a9cd5325e37289bcd0 -p YOUR_PASSWORD -y
```

#### setBond

```
$ preptools setBond -c config.json hxa8df82e93e8a9cd5325e37289bcd0fbc0a8b4e5e,100 hx047e7de3d2623c008fdf3120180f95919c85bd95,200 -p YOUR_PASSWORD -y
```

### goloop CLI

#### setBond

```
$ cat bond.json
{
  "params": {
    "bonds": [
      {
        "address": "hx047e7de3d2623c008fdf3120180f95919c85bd95",
        "value": "0x64"
      },
      {
        "address": "hxa8df82e93e8a9cd5325e37289bcd0fbc0a8b4e5e",
        "value": "0x123"
      }
    ]
  }
}

$ ./bin/goloop rpc sendtx call --to cx0000000000000000000000000000000000000000 --value 0x0 --key_store wallet.json --nid 1 --uri http://localhost:9080/api/v3 --key_password YOUR_PASSWORD --step_limit 0x1234123 --method setBond --raw bond.json
```

#### setBonderList

```
$ cat bonderList.json
{
  "params": {
    "bonderList": ["hx047e7de3d2623c008fdf3120180f95919c85bd95", "hx5be1c0b343698ab2370524b598d48603d7c44a12"]
  }
}

$ ./bin/goloop rpc sendtx call --to cx0000000000000000000000000000000000000000 --value 0x0 --key_store wallet.json --nid 1 --uri http://localhost:9080/api/v3 --key_password YOUR_PASSWORD --step_limit 0x1234123 --method setBonderList --raw bonderList.json
```
