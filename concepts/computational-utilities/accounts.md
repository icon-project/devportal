# Accounts

### What is an account?

An ICON account is a component representing either a [user](https://icon.community/glossary/user/) or a [smart contract](https://icon.community/glossary/smart-contract/). ICON accounts can send transactions on the ICON network. All accounts have associated addresses, but they look different depending on whether it is associated with a user account or a smart contract account. The account address is also known as a [_public key_](https://www.investopedia.com/terms/p/public-key.asp). This is a common term used in cryptography meaning a string of characters that represent an identity, similar to a driver's license number or a social security code. The account password is also known as a [_private key_](https://www.investopedia.com/terms/p/private-key.asp). The private key is also a string of characters, but it is usually much longer and is nearly impossible to guess, even if you have the public key. It is extremely important to keep track of both the public and private keys. The public key is where you will want to receive transactions. he private key is your password that you can use to verify your identity, for example, if you want to send a transaction.

On the ICON network, there are some special, predefined accounts, such as the _Governance contract_ and the _Public treasury_.

#### User account

A user account has an associated private key, can sign and send a transaction, therefore can be the owner of transactions. User accounts are necessary in order to deploy a smart contract.

User accounts are 20 bytes long, consist of [hexadecimal characters](https://www.tutorialspoint.com/hexadecimal-number-system), and are prepended with `hx`.

#### Smart contract account

A smart contract account does not have a private key, and cannot initiate a transaction. A smart contract account address can only be the destination of transactions.

Smart contract accounts are 20 bytes long, consist of [hexadecimal characters](https://www.tutorialspoint.com/hexadecimal-number-system), and are prepended with `cx`.

### Creating an account

The simplest way to create an account is through the officially supported [Hana wallet](https://chrome.google.com/webstore/detail/hana/jfdlamikmbghhapbgfoogdffldioobgl/related). Install the wallet as an extension to the [Google Chrome](https://www.google.com/chrome/), [Chromium](https://www.chromium.org/Home), [Brave](https://brave.com/) or other Blink-based browsers. Click on the extension to open it in a new tab, and the user interface will guide you through the process of setting up an account.
