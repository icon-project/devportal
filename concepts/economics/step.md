# STEP

### What is STEP? <a href="#what-is-gas" id="what-is-gas"></a>

STEP refers to the unit that measures the amount of computational effort required to execute specific operations on the ICON network.

Each ICON [transaction](../blockchain-components/transactions.md) requires computational resources to execute. Because this is a financial system, each transaction thus requires a fee. In the [ICON network](../../), these fees are referred to as STEP.

STEP fees are paid in [ICX](icx.md), the ICON network's native currency. The initial STEP price is approximately as follows:

$$
STEP=1*10^{-8} ICX
$$

However, the [ICON network delegates](../governance/delegates.md) can vote to change the STEP fee as supply and demand for computational power in the ICON network changes.

### Why can STEP fees get so high? <a href="#why-can-gas-fees-get-so-high" id="why-can-gas-fees-get-so-high"></a>

STEP fees rise with the increased usage of the ICON network. An [smart contract](../../icon-stack/smart-contracts/) developer should be aware of two aspects of ICON network usage:

1. How complicated is an individual transaction?
2. How many applications are attempting to perform transactions during each new block?

For point 1., work to make each transaction efficient so that they can minimize their own costs or the cost to their users.

For point 2., note that as more applications conduct transactions, the block validators will have to use more computational power. If there is a high demand for computational power, then ICON network delegates can vote to increase the STEP fee.
