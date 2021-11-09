# Bond management

## How to post a bond

Posting a bond is done in 3 steps:

1. P-Rep whitelist up to 10 addresses which can post the bond. This is done using the following RPC command [#setbonderlist](json-rpc/iiss\_extension.md#setbonderlist "mention")
2. Whitelisted bonder then must stake ICX to the P-Rep using [#setstake](json-rpc/iiss\_extension.md#setstake "mention")
3. Whitelisted bonder then must post the bond to the P-Rep using [#setbond](json-rpc/iiss\_extension.md#setbond "mention")

{% hint style="info" %}
ICONists can post a bond to up to 100 P-reps, if they are whitelisted by them

A P-Rep can only receive a bond from 10 different ICONists
{% endhint %}

## Penalty

IISS 3 introduced the concept of `bond` (you can see the network proposal [here](https://tracker.icon.foundation/transaction/0xcd7055153f777c3392f05c83c17e66207045f68b8504a36c07a459c706992dfe)). When a P-Rep does not behave as expected, this bond will be slashed (ICX will be burn) following those specific rules.

![](../../.gitbook/assets/f8d977c64b14a38161633f22f3b027b90c35366b.jpeg)
