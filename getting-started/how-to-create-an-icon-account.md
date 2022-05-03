# How to create an ICON account

### Prerequisites

Install the [Hana Wallet extension](https://chrome.google.com/webstore/detail/hana/jfdlamikmbghhapbgfoogdffldioobgl) for a Google Chrome-based browser ([Chrome](https://www.google.com/chrome/index.html), [Chromium](https://www.chromium.org/getting-involved/download-chromium/), [Brave](https://brave.com/download/), [Edge](https://www.microsoft.com/en-us/edge)). If you intend to use [Ledger](https://www.ledger.com/start) hardware wallet, then be sure to set that up as well.

If you intend to use the [goloop](../concepts/computational-utilities/goloop/) cli directly, then you must have that installed and setup. See the [instructions on how to setup goloop](../concepts/computational-utilities/goloop/setup.md).

### Using Hana Wallet

Click on the extension to open it in a new tab, and the user interface will guide you through the process of setting up an account. Be sure to keep your password and recovery material secure.

### Using goloop CLI

Generate a keystore.

```
$ goloop ks gen --out keystore.json --password <your_password>
hxd8fefc57e0d358e0cf338d684c3e438190d64145 ==> keystore.json
```

Then get test ICX in Lisbon using the [faucets](https://icondev.io/introduction/the-icon-network/testnet#faucets).

### Further resources

[Learn more about ICON accounts](../concepts/computational-utilities/accounts.md)
