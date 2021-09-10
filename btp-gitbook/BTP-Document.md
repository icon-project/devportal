# Table of Contents

## Overview

____

- [Blokchain Transmission Protocol (BTP)](./overview.md#overview)
- [BTP Example](./overview.md#btp-interoperability-example)

## BTP Development Resources

____

- [Prerequisites](./btp-development-resources.md#prerequisites)
- [BTP Service Handler (BSH) Interfaces](./btp-development-resources.md#btp-service-handler-bsh)
- [BTP Message Center (BMC) Interfaces](./btp-development-resources.md#btp-message-center-bmc)
- [BTP Message Verifier (BMV) Interfaces](./btp-development-resources.md#btp-message-verifier-bmv)
- [Contract Upgradeability - Solidity](./btp-development-resources.md#contract-upgradeability---solidity)

## BTP Development Instructions

____

- [Setup and Installation Requirements](./btp-development-instructions.md#setup-and-installation)
- [Deployment Instructions](./btp-development-instructions.md#deployment-instructions)
  - [Deploy Nodes](./btp-development-instructions.md#create-blockchain-nodes)
    - [1. Deploy ICON Node](./btp-development-instructions.md#1-deploy-icon-node)
    - [2. Deploy Moonriver Node](./btp-development-instructions.md#2-deploy-moonriver-node)
  - [Deploy Smart Contracts (ICON)](./deploy-smart-contracts-icon.md#deploy-smart-contracts-icon)
    - [1. Deploy BMC SCORE](./deploy-smart-contracts-icon.md#1-deploy-bmc-score-contract)
    - [2. Deploy BMV SCORE](./deploy-smart-contracts-icon.md#2-deploy-bmv-score-contract)
      - [Deploy Kusama and Moonriver Event Decoder](./deploy-smart-contracts-icon.md#deploy-kusama-and-moonriver-event-decoder)
      - [Deploy BMV](./deploy-smart-contracts-icon.md#deploy-bmv-contract-on-icon-network)
    - [3. Deploy IRC31Token](./deploy-smart-contracts-icon.md#3-deploy-irc31token-contract)
    - [4. Deploy NativeCoinBSH](./deploy-smart-contracts-icon.md#4-deploy-nativecoinbsh-contract)
    - [5. Deploy FeeAggregation](./deploy-smart-contracts-icon.md#5-deploy-feeaggregation-contract)
  - [Deploy Smart Contracts (Moonriver)](./deploy-smart-contracts-moonriver.md#deploy-smart-contracts-moonriver)
    - [1. Deploy BMC PRA](./deploy-smart-contracts-moonriver.md#1-deploy-bmc-contracts-on-moonriver-network)
    - [2. Deploy BSH PRA](./deploy-smart-contracts-moonriver.md#2-deploy-bsh-contracts-on-moonriver-network)
    - [3. Deploy BMV PRA](./deploy-smart-contracts-moonriver.md#3-deploy-bmv-contracts-on-moonriver-network)
  - [BMRs Setup](./relays-setup.md#relays-setup)
    - [Generate Keystores](./relays-setup.md#generate-keystores)
  - [Smart Contracts Configuration](./smart-contracts-configuration.md#smart-contracts-configuration)
    - [1. ICON-BMC Configurations](./smart-contracts-configuration.md#1-icon-bmc-configuration)
    - [2. ICON-NativeCoinBSH Configurations](./smart-contracts-configuration.md#2-config-nativecoin-bsh)
    - [3. Moonriver-BMC Configuration](./smart-contracts-configuration.md#3-config-moonriver-bmc)
    - [4. Moonriver-BSH Configuration](./smart-contracts-configuration.md#4-config-moonriver-bsh)
  - [Deploy BMRs](./deploy-relays.md#deploy-relays)
    - [1. Create Configuration Files](./deploy-relays.md#1-create-configuration-files)
    - [2. Start BMRs](./deploy-relays.md#2-start-bmrs)

## Transfer Example

____

- [Transfer DEV](./transfer-example.md#transfer-dev-to-icon-network)
- [Transfer ICX](./transfer-example.md#transfer-icx-to-moonriver-network)

## Appendix

____

- [ICON-BSH commands](./appendix.md#icon-bsh)
- [ICON-BMC commands](./appendix.md#icon-bmc)
- [ICON-BMV commands](./appendix.md#icon-bmv)
- [MOON-BSH commands](./appendix.md#moon-bsh)
- [MOON-BMC commands](./appendix.md#moon-bmc)
- [MOON-BMV commands](./appendix.md#moon-bmv)
- [ICON Error Response Codes](./appendix.md#icon-error-response-codes)
