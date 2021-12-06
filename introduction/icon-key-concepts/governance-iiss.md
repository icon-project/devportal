---
description: 'Yellow paper: https://icon.foundation/download/IISS_Paper_v2.0_EN.pdf'
---

# Governance - IISS

## Basic IISS Overview

* ICON Incentives Scoring System (hereinafter referred to as "IISS") is an evaluation system for accurately measuring and compensating the contributions of ICONists within the ICON Network.
* The ICON Network defines 7 contribution items based on the 'Delegated Proof of Contribution' incentive protocol to identify and promote significant contribution activities to the decentralized network. Each ICONists can demonstrate its contribution to the ecosystem via delegation.
* I-Score is the ecosystem contribution score. IISS calculates and distributes I-Score for each contributing item every 43,200 blocks and determines the ICX issuance accordingly. After then, I-Score can be converted to ICX via Public Treasury. Based on the advanced on-chain governance system, IISS determines the issuance amount of ICX for the ecosystem.
* ICON Network's on-chain governance-based crypto economy is designed to work flexibly depending on the network situation that the network can develop successfully in a decentralized environment.

## Delegated Proof of Contribution

* Delegated Proof of Contribution (DPoC) is the incentive protocol to determine the contributions in the ecosystem. IISS works based on DPoC mechanism. 7 types of contribution items can be demonstrated by IISS.
* Representative
  * A Public Representative (P-Rep) is a full node on the ICON Network that participates in consensus and governance.
  * P-Reps consist of the top 22 Main P-Reps and 78 Sub P-Reps, and are elected by ICONists.
  * Block Validation Reward
    * Block Validation Rewards are given to the top 22 Main P-Reps for block production and verification.
  * Representative Reward
    * Representative Rewards are given to all 100 P-Reps according to the delegated amount of ICX received.
* ICONist
  * All ICONists can receive the reward in return for delegating their ICX to P-Reps
* Contribution Items

## Bond requirement

The bond requirement is a new concept that was introduced with IISS 3.0, and [approved by P-Rep through a network proposal](https://tracker.icon.foundation/proposal/0x16dbc932b601821b08450ad6f228a6a8e1bfd9cf5a361f0bf42ccf4b0b29be7b).

Before IISS 3.0, P-Reps were ranked by the amount of delegation they receive from ICONists. The more vote they get (the more ICX people are delegating to them), the better their rank and the bigger their reward was.

The problem with this approach is that P-Reps actually don't have any stake in the network. If a P-Rep did not behave properly according to the network rules (the node is not working properly, or they do not participate in network governance, etc...), they did not get directly penalized. Sure, their reward was not distributed, but their funds were never at risk.

With the introduction of the bond requirements, P-Rep rank will now be determined by the _bonded delegation_. It is calculated using the following formula: `bondedDelegation = min(bonded * 20, bonded + delegated)`.

What this means in practice is that in order for a P-Rep to maximize their rewards, they must post a bond representing 5% of their total votes. So for example is a P-Rep receive 1,000,000 votes from  other users, they must post a bond of 50,000 ICX in order to keep their rank and receive the same reward as before (this example is simplified to illustrate the main idea).

In IISS 3.1, if a P-Rep does not behave properly, the bond will be _slashed,_ meaning that part of the P-Rep bond will be burned and lost forever. It is similar to a fine in real life, with the main difference is that the slashed bond is burned (it reduces ICX supply). This is a very strong incentive for P-Reps to work hard to provide a stable node operation and participate in governance actively.

Finally, this also creates a competition between P-Rep, where P-Reps with the most efficient operations will gain higher ranks over time.

P-Reps can whitelist up to 10 differents bonder addresses. ICONists can post a bond to up to 100 P-Reps at a time.

## [Governance Variables](governance-public-representative-p-rep.md#inflation-controls-and-reward-distribution)
