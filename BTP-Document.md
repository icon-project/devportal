# Table of Contents

## Overview

____

- [Blokchain Transmission Protocol (BTP)](./btp-document/Overview.md#crosschain-interoperability-blockchain-transmission-Protocol-(btp))
- [BTP Example](./btp-document/Overview.md#btp-interoperability-example)

## BTP Development Resources

____

- [Prerequisites](./btp-document/BTP-Development-Resources.md#prerequisites)
- [BTP Service Handler (BSH) Interfaces](./btp-document/BTP-Development-Resources.md#btp-service-handler-(bsh))
- [BTP Message Center (BMC) Interfaces](./btp-document/BTP-Development-Resources.md#btp-message-center-(bmc))
- [BTP Message Verifier (BMV) Interfaces](./btp-document/BTP-Development-Resources.md#btp-message-verifier-(bmv))

## BTP Development Instructions

____

- [Setup and Installation Requirements](./btp-document/BTP-Development-Instructions.md#setup-and-installation)
- [Deployment Instructions](./btp-document/BTP-Development-Instructions.md#deployment-instructions)
  - [Deploy Nodes](./btp-document/BTP-Development-Instructions.md#create-blockchain-nodes)
    - [Deploy ICON Node](./btp-document/BTP-Development-Instructions.md#1-deploy-icon-node)
    - [Deploy Moonriver Node](./btp-document/BTP-Development-Instructions.md#2-deploy-moonriver-node)
  - [Deploy Smart Contracts (ICON)](./btp-document/Smart-Contracts-ICON.md#smart-contracts-on-icon-deployment)
    - [Preparation](./btp-document/Smart-Contracts-ICON.md#1-preparation-compiling-requiring-java-files)
    - [Deploy BMC SCORE](./btp-document/Smart-Contracts-ICON.md#2-deploy-bmc-score-contract)
    - [Deploy BMV SCORE](./btp-document/Smart-Contracts-ICON.md#3-deploy-bmv-score-contract)
      - [Deploy Kusama and Moonriver Event Decoder](./btp-document/Smart-Contracts-ICON.md#deploy-kusama-and-moonriver-event-decoder)
      - [Deploy BMV](./btp-document/Smart-Contracts-ICON.md#deploy-bmv-contract-on-icon-network)
    - [Deploy IRC31Token](./btp-document/Smart-Contracts-ICON.md#4-deploy-irc31token-contract)
    - [Deploy NativeCoinBSH](./btp-document/Smart-Contracts-ICON.md#5-deploy-nativecoinbsh-contract)
    - [Deploy FeeAggregation](./btp-document/Smart-Contracts-ICON.md#6-deploy-feeaggregation-contract)
    - [ICON-BMC Configurations](./btp-document/Smart-Contracts-ICON.md#7-icon-bmc-configuration)
      - [Add Link to Moonriver-BMC](./btp-document/Smart-Contracts-ICON.md#add-connection-link-to-moonriver-bmc)
      - [Set Link Configuration](./btp-document/Smart-Contracts-ICON.md#set-link-configuration)
      - [Add BSH Service](./btp-document/Smart-Contracts-ICON.md#add-bsh-service-contract)
      - [Register Relays](./btp-document/Smart-Contracts-ICON.md#register-relay-to-icon-bmc)
    - [NativeCoinBSH Configuration](./btp-document/Smart-Contracts-ICON.md#8-config-nativecoin-bsh)
  - [Deploy Smart Contracts (Moonriver)](./btp-document/Smart-Contracts-PRA.md)
    - [Preparation](./btp-document/Smart-Contracts-PRA.md#smart-contracts-on-moonriver-deployment)
    - [Deploy BMC PRA](./btp-document/Smart-Contracts-PRA.md#1.deploy-bmc-contracts-on-moonriver-network)
      - [Deploy BMCs](./btp-document/Smart-Contracts-ICON.md#deploy-bmc-contracts)
      - [Get Contract and BTP Address of BMCs](./btp-document/Smart-Contracts-ICON.md#deploy-bmc-contracts)
    - [Deploy BSH PRA](./btp-document/Smart-Contracts-PRA.md#2.deploy-bsh-contracts-on-moonriver-network)
    - [Deploy BMV PRA](./btp-document/Smart-Contracts-PRA.md#3.deploy-bmv-contracts-on-moonriver-network)
      - [Deploy BMVs](./btp-document/Smart-Contracts-ICON.md#deploy-bmv-contracts)
      - [Get Contract and status of BMV](./btp-document/Smart-Contracts-ICON.md#get-address-and-check-status-of-bmv-contract)
    - [Moonriver-BMC Configuration](./btp-document/Smart-Contracts-PRA.md#4.config-moonriver-bmc)
    - [Moonriver-BSH Configuration](./btp-document/Smart-Contracts-PRA.md#5.config-moonriver-bsh)
  - [Deploy BMRs](./btp-document/BMR-Deployment.md)
    - [Build Executable Files](./btp-document/BMR-Deployment.md#1-build-executable-files)
    - [Generate Keystore and Configuration Files](./btp-document/BMR-Deployment.md#2-generate-keystores-and-configuration-files)
      - [CLI Tools](./btp-document/BMR-Deployment.md#download-cli-tools)
      - [Generate Keystores](./btp-document/BMR-Deployment.md#create-keystore-files)
      - [Query Offset and BMRs](./btp-document/BMR-Deployment.md#query-offset-and-list-of-Relays-from-BMC)
      - [Create Configuration Files](./btp-document/BMR-Deployment.md#create-configuration-files)
    - [Start BMRs](./btp-document/BMR-Deployment.md#3-start-bmrs)