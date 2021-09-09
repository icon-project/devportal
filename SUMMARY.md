# Table of contents

* [Get Started](README.md)

## BTP <a id="btp-gitbook"></a>

* [Overview](btp-gitbook/overview/README.md)
  * [Blokchain Transmission Protocol \(BTP\)](btp-gitbook/overview/blokchain-transmission-protocol-btp.md)
  * [BTP Example](btp-gitbook/overview/btp-example.md)
* [BTP Development Resources](btp-gitbook/btp-development-resources/README.md)
  * [BTP Service Handler \(BSH\) Interfaces](btp-gitbook/btp-development-resources/btp-service-handler-bsh-interfaces.md)
  * [Prerequisites](btp-gitbook/btp-development-resources/prerequisites.md)
  * [BTP Service Handler \(BSH\) Interfaces](btp-gitbook/btp-development-resources/btp-service-handler-bsh-interfaces-1.md)
  * [BTP Message Center \(BMC\) Interfaces](btp-gitbook/btp-development-resources/btp-message-center-bmc-interfaces.md)
  * [BTP Message Verifier \(BMV\) Interfaces](btp-gitbook/btp-development-resources/btp-message-verifier-bmv-interfaces.md)
* [BTP Development Instructions](btp-gitbook/btp-development-instructions/README.md)
  * [Setup and Installation Requirements](btp-gitbook/btp-development-instructions/setup-and-installation-requirements.md)
  * [Deployment Instructions](btp-gitbook/btp-development-instructions/deployment-instructions/README.md)
    * [Deploy Nodes](btp-gitbook/btp-development-instructions/deployment-instructions/deploy-nodes/README.md)
      * [1. Deploy ICON Node](btp-gitbook/btp-development-instructions/deployment-instructions/deploy-nodes/1.-deploy-icon-node.md)
      * [2. Deploy Moonriver Node](btp-gitbook/btp-development-instructions/deployment-instructions/deploy-nodes/2.-deploy-moonriver-node.md)
    * [Deploy Smart Contracts \(ICON\)](btp-gitbook/btp-development-instructions/deployment-instructions/deploy-smart-contracts-icon.md)
    * [Deploy Smart Contracts \(Moonriver\)](btp-gitbook/btp-development-instructions/deployment-instructions/deploy-smart-contracts-moonriver.md)
    * [Relays Setup](btp-gitbook/btp-development-instructions/deployment-instructions/relays-setup.md)
    * [Smart Contracts Configuration](btp-gitbook/btp-development-instructions/deployment-instructions/smart-contracts-configuration.md)
    * [Deploy Relays](btp-gitbook/btp-development-instructions/deployment-instructions/deploy-relays.md)
  * [Transfer Example](btp-gitbook/btp-development-instructions/transfer-example.md)
  * [Appendix](btp-gitbook/btp-development-instructions/appendix.md)

## Introduction

* [What Is ICON Network?](introduction/what-is-icon-network.md)
* [ICON Key Concepts](introduction/icon-key-concepts/README.md)
  * [Accounts](introduction/icon-key-concepts/accounts.md)
  * [Transactions](introduction/icon-key-concepts/transactions.md)
  * [Transactions Fees](introduction/icon-key-concepts/transactions-fees.md)
  * [ICON Nodes](introduction/icon-key-concepts/icon-nodes.md)
  * [Governance - Public Representative \(P-Rep\)](introduction/icon-key-concepts/governance-public-representative-p-rep.md)
  * [Governance - IISS](introduction/icon-key-concepts/governance-iiss.md)
* [The ICON Network](introduction/the-icon-network/README.md)
  * [Testnet](introduction/the-icon-network/testnet.md)
  * [Mainnet](introduction/the-icon-network/mainnet.md)

## Python SCORE

* [Overview](python-score/overview.md)
* [Quickstart](python-score/quickstart/README.md)
  * [Part 1. HelloWorld on local emulated environment](python-score/quickstart/part-1.-helloworld-on-local-emulated-environment.md)
  * [Part 2. HelloWorld on testnet](python-score/quickstart/part-2.-helloworld-on-testnet.md)
* [SCORE Development Guide](python-score/score-development-guide/README.md)
  * [Development Tools](python-score/score-development-guide/development-tools.md)
  * [Writing SCORE](python-score/score-development-guide/writing-score.md)
  * [Iconservice API References](https://iconservice.readthedocs.io/en/latest)
  * [Deploying your SCORE](python-score/score-development-guide/deploying-your-score.md)
  * [Invoking SCORE Functions](python-score/score-development-guide/invoking-score-functions.md)
  * [Step Estimation](python-score/score-development-guide/step-estimation.md)
  * [Testing your SCORE](python-score/score-development-guide/testing-your-score.md)
  * [Fee Sharing and Virtual Step](python-score/score-development-guide/fee-sharing-and-virtual-step.md)
* [SCORE Audit](python-score/score-audit/README.md)
  * [Audit Checklist](python-score/score-audit/audit-checklist.md)
  * [Deployment Process](python-score/score-audit/deployment-process.md)
* [Sample SCOREs](python-score/sample-scores/README.md)
  * [HelloWorld](python-score/sample-scores/helloworld.md)
  * [Token & Crowdsale](python-score/sample-scores/token-and-crowdsale.md)
  * [Multisig Wallet](python-score/sample-scores/multisig-wallet.md)
  * [DEX](python-score/sample-scores/dex.md)

## T-Bears for Python SCORE <a id="tbears"></a>

* [Overview](tbears/overview.md)
* [Installation](tbears/installation.md)
* [CLI Commands](tbears/cli-commands.md)
* [Testing Framework](tbears/testing-framework.md)
* [Emulated Node Environment](tbears/emulated-node-environment.md)
* [Configuration](tbears/configuration.md)

## ICON SDKs

* [Overview](icon-sdks/overview.md)
* [Java SDK](icon-sdks/java-sdk/README.md)
  * [javadoc](http://www.javadoc.io/doc/foundation.icon/icon-sdk)
* [Python SDK](icon-sdks/python-sdk/README.md)
  * [Python API Reference](icon-sdks/python-sdk/python-api-reference.md)
* [Javascript SDK](icon-sdks/javascript/README.md)
  * [Javascript API Reference](icon-sdks/javascript/javascript-api-reference.md)
* [Swift SDK](icon-sdks/swift-sdk/README.md)
  * [Swift API Reference](icon-sdks/swift-sdk/swift-api-reference.md)

## ICONex Connect

* [Chrome Extension](iconex-connect/chrome-extension.md)
* [iOS](iconex-connect/ios.md)
* [Android](iconex-connect/android.md)

## ICON Node

* [Overview](icon-node/overview.md)
* [Quickstart](icon-node/quickstart.md)
* [P-Rep Tools](icon-node/p-rep-tools.md)
* [Maintenance](icon-node/maintenance/README.md)
  * [Node operation and configuration](icon-node/maintenance/node-operation-and-configuration.md)
  * [Understanding log files](icon-node/maintenance/understanding-log-files.md)
  * [How to collect centralized logging](icon-node/maintenance/how-to-collect-centralized-logging.md)
  * [Backup and restore DB guide](icon-node/maintenance/backup-and-restore-db-guide.md)

## Oracles

* [Band Protocol](oracles/band-protocol.md)

## References

* [How-to](references/how-to/README.md)
  * [Set up a Keystore file on a P-Rep Node](references/how-to/set-up-a-keystore-file-on-a-p-rep-node.md)
  * [Create an account](references/how-to/create-an-account.md)
  * [Change network in ICONex](references/how-to/change-network-in-iconex.md)
  * [Deploy a SCORE](references/how-to/deploy-a-score.md)
  * [Estimate required step](references/how-to/estimate-required-step.md)
  * [Write SCORE unit test](references/how-to/write-score-unit-test.md)
  * [Write SCORE integration test](references/how-to/write-score-integration-test.md)
  * [Generate a transaction signature](references/how-to/generate-a-transaction-signature.md)
  * [Create own blockchain network from AWS marketplace](references/how-to/create-own-blockchain-network-from-aws-marketplace.md)
* [Reference Manuals](references/reference-manuals/README.md)
  * [ICON JSON-RPC API v3 Specification](references/reference-manuals/icon-json-rpc-api-v3-specification.md)
  * [JSON Standard for P-Rep Detailed Information](references/reference-manuals/json-standard-for-p-rep-detailed-information.md)
  * [ICON Governance SCORE APIs](references/reference-manuals/icon-governance-score-apis.md)

## ICON 2.0

* [Goloop](icon-2.0/goloop/README.md)
  * [Get Started](icon-2.0/goloop/get-started/README.md)
    * [How to build](icon-2.0/goloop/get-started/build.md)
    * [Local Network](icon-2.0/goloop/get-started/local-network.md)
  * [Genesis](icon-2.0/goloop/genesis/README.md)
    * [Transaction](icon-2.0/goloop/genesis/genesis_tx.md)
    * [Storage](icon-2.0/goloop/genesis/genesis_storage.md)
  * [JSON-RPC](icon-2.0/goloop/json-rpc/README.md)
    * [Goloop JSON-RPC API v3](icon-2.0/goloop/json-rpc/jsonrpc_v3.md)
    * [BTP Extension](icon-2.0/goloop/json-rpc/btp_extension.md)
    * [IISS Extension](icon-2.0/goloop/json-rpc/iiss_extension.md)
  * [Node Management](icon-2.0/goloop/management/README.md)
    * [API](icon-2.0/goloop/management/goloop_admin_api.md)
    * [CLI](icon-2.0/goloop/management/goloop_cli.md)
    * [Metrics](icon-2.0/goloop/management/metric.md)
* [Java SCORE](icon-2.0/java-score/README.md)
  * [Examples](https://github.com/icon-project/java-score-examples)
  * [Tutorial](icon-2.0/java-score/tutorial.md)

---

* [Support](support.md)
* [Developer Forum](https://forum.icon.community/c/d/33)

