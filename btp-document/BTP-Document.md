# Table of Contents

## Overview

____

- [Blokchain Transmission Protocol (BTP)](./Overview.md#crosschain-interoperability-blockchain-transmission-Protocol-(btp))
- [BTP Example](./Overview.md#btp-interoperability-example)

## BTP Development Resources

____

- [Prerequisites](./BTP-Development-Resources.md#prerequisites)
- [BTP Service Handler (BSH) Interfaces](./BTP-Development-Resources.md#btp-service-handler-(bsh))
- [BTP Message Center (BMC) Interfaces](./BTP-Development-Resources.md#btp-message-center-(bmc))
- [BTP Message Verifier (BMV) Interfaces](./BTP-Development-Resources.md#btp-message-verifier-(bmv))
- [Contract Upgradeability - Solidity](./BTP-Development-Resources.md#contract-upgradeability-solidity)

## BTP Development Instructions

____

- [Setup and Installation Requirements](./BTP-Development-Instructions.md#setup-and-installation)
- [Deployment Instructions](./BTP-Development-Instructions.md#deployment-instructions)
  - [Deploy Nodes](./BTP-Development-Instructions.md#create-blockchain-nodes)
    - [1. Deploy ICON Node](./BTP-Development-Instructions.md#1-deploy-icon-node)
    - [2. Deploy Moonriver Node](./BTP-Development-Instructions.md#2-deploy-moonriver-node)
  - [Deploy Smart Contracts (ICON)](./Smart-Contracts-ICON.md#smart-contracts-on-icon-deployment)
    - [1. Preparation](./Smart-Contracts-ICON.md#1-preparation-compiling-requiring-java-files)
    - [2. Deploy BMC SCORE](./Smart-Contracts-ICON.md#2-deploy-bmc-score-contract)
    - [3. Deploy BMV SCORE](./Smart-Contracts-ICON.md#3-deploy-bmv-score-contract)
      - [Deploy Kusama and Moonriver Event Decoder](./Smart-Contracts-ICON.md#deploy-kusama-and-moonriver-event-decoder)
      - [Deploy BMV](./Smart-Contracts-ICON.md#deploy-bmv-contract-on-icon-network)
    - [4. Deploy IRC31Token](./Smart-Contracts-ICON.md#4-deploy-irc31token-contract)
    - [5. Deploy NativeCoinBSH](./Smart-Contracts-ICON.md#5-deploy-nativecoinbsh-contract)
    - [6. Deploy FeeAggregation](./Smart-Contracts-ICON.md#6-deploy-feeaggregation-contract)
  - [Deploy Smart Contracts (Moonriver)](./Smart-Contracts-PRA.md#smart-contracts-on-moonriver-deployment)
    - [1. Deploy BMC PRA](./Smart-Contracts-PRA.md#1.deploy-bmc-contracts-on-moonriver-network)
    - [2. Deploy BSH PRA](./Smart-Contracts-PRA.md#2.deploy-bsh-contracts-on-moonriver-network)
    - [3. Deploy BMV PRA](./Smart-Contracts-PRA.md#3.deploy-bmv-contracts-on-moonriver-network)
  - [BMRs Setup](./BMR-Setup.md)
    - [Build Executable Files](./BMR-Setup.md#1-build-executable-files)
    - [Generate Keystores](./BMR-Setup.md#2-generate-keystores)
  - [Smart Contracts Configuration](./Smart-Contracts-Configuration.md#smart-contracts-configuration)
    - [1. ICON-BMC Configurations](./Smart-Contracts-Configuration.md#1-icon-bmc-configuration)
    - [2. ICON-NativeCoinBSH Configurations](./Smart-Contracts-Configuration.md#2-config-nativecoin-bsh)
    - [3. Moonriver-BMC Configuration](./Smart-Contracts-Configuration.md#3.config-moonriver-bmc)
    - [4. Moonriver-BSH Configuration](./Smart-Contracts-Configuration.md#4.config-moonriver-bsh)
  - [Deploy BMRs](./BMR-Deployment.md)
    - [1. Create Configuration Files](./BMR-Deployment.md#1-generate-keystores-and-configuration-files)
    - [2. Start BMRs](./BMR-Deployment.md#2-start-bmrs)

## Transfer Example

____

- [Transfer ICX](./NativeCoin-Transfer-Example.md#transfer-icx-to-moonriver-network)
- [Transfer DEV](./NativeCoin-Transfer-Example.md#transfer-dev-to-icon-network)

## Appendix

____

- [ICON-BSH commands](./Appendix.md#icon-bsh)
- [ICON-BMC commands](./Appendix.md#icon-bmc)
- [ICON-BMV commands](./Appendix.md#icon-bmv)
- [MOON-BSH commands](./Appendix.md#moon-bsh)
- [MOON-BMC commands](./Appendix.md#moon-bmc)
- [MOON-BMV commands](./Appendix.md#moon-bmv)
- [ICON Error Response Codes](./Appendix.md#icon-error-response-codes)