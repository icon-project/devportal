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
    - [Deploy ICON Node](./BTP-Development-Instructions.md#1-deploy-icon-node)
    - [Deploy Moonriver Node](./BTP-Development-Instructions.md#2-deploy-moonriver-node)
  - [Deploy Smart Contracts (ICON)](./Smart-Contracts-ICON.md#smart-contracts-on-icon-deployment)
    - [Preparation](./Smart-Contracts-ICON.md#1-preparation-compiling-requiring-java-files)
    - [Deploy BMC SCORE](./Smart-Contracts-ICON.md#2-deploy-bmc-score-contract)
    - [Deploy BMV SCORE](./Smart-Contracts-ICON.md#3-deploy-bmv-score-contract)
      - [Deploy Kusama and Moonriver Event Decoder](./Smart-Contracts-ICON.md#deploy-kusama-and-moonriver-event-decoder)
      - [Deploy BMV](./Smart-Contracts-ICON.md#deploy-bmv-contract-on-icon-network)
    - [Deploy IRC31Token](./Smart-Contracts-ICON.md#4-deploy-irc31token-contract)
    - [Deploy NativeCoinBSH](./Smart-Contracts-ICON.md#5-deploy-nativecoinbsh-contract)
    - [Deploy FeeAggregation](./Smart-Contracts-ICON.md#6-deploy-feeaggregation-contract)
    - [ICON-BMC Configurations](./Smart-Contracts-ICON.md#7-icon-bmc-configuration)
      - [Add Link to Moonriver-BMC](./Smart-Contracts-ICON.md#add-connection-link-to-moonriver-bmc)
      - [Set Link Configuration](./Smart-Contracts-ICON.md#set-link-configuration)
      - [Add BSH Service](./Smart-Contracts-ICON.md#add-bsh-service-contract)
      - [Register Relays](./Smart-Contracts-ICON.md#register-relay-to-icon-bmc)
    - [NativeCoinBSH Configuration](./Smart-Contracts-ICON.md#8-config-nativecoin-bsh)
  - [Deploy Smart Contracts (Moonriver)](./Smart-Contracts-PRA.md)
    - [Preparation](./Smart-Contracts-PRA.md#smart-contracts-on-moonriver-deployment)
    - [Deploy BMC PRA](./Smart-Contracts-PRA.md#1.deploy-bmc-contracts-on-moonriver-network)
      - [Deploy BMCs](./Smart-Contracts-ICON.md#deploy-bmc-contracts)
      - [Get Contract and BTP Address of BMCs](./Smart-Contracts-ICON.md#deploy-bmc-contracts)
    - [Deploy BSH PRA](./Smart-Contracts-PRA.md#2.deploy-bsh-contracts-on-moonriver-network)
    - [Deploy BMV PRA](./Smart-Contracts-PRA.md#3.deploy-bmv-contracts-on-moonriver-network)
      - [Deploy BMVs](./Smart-Contracts-ICON.md#deploy-bmv-contracts)
      - [Get Contract and status of BMV](./Smart-Contracts-ICON.md#get-address-and-check-status-of-bmv-contract)
    - [Moonriver-BMC Configuration](./Smart-Contracts-PRA.md#4.config-moonriver-bmc)
    - [Moonriver-BSH Configuration](./Smart-Contracts-PRA.md#5.config-moonriver-bsh)
  - [Deploy BMRs](./BMR-Deployment.md)
    - [Build Executable Files](./BMR-Deployment.md#1-build-executable-files)
    - [Generate Keystore and Configuration Files](./BMR-Deployment.md#2-generate-keystores-and-configuration-files)
      - [Generate Keystores](./BMR-Deployment.md#create-keystore-files)
      - [Query Offset and BMRs](./BMR-Deployment.md#query-offset-and-list-of-Relays-from-BMC)
      - [Create Configuration Files](./BMR-Deployment.md#create-configuration-files)
    - [Start BMRs](./BMR-Deployment.md#3-start-bmrs)