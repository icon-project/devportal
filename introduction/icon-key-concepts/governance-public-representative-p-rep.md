# Governance - Public Representative \(P-Rep\)

### What is a Public Representative?

* A Public Representative \(P-Rep\) is a full node on the ICON Network that participates in consensus and governance.
* P-Reps consist of the top 22 Main P-Reps and 78 Sub P-Reps, and are elected by ICONists.

### Reward for Representative

Representatives can receive compensation through IISS. See [IISS](governance-iiss.md) for more information.

### On-chain Governance System

* Representatives can submit governance variables, network proposals through an on-chain governance system. All ICONists can delegate their ICX to the representatives to participate in decision making. It is important to delegate the ICX to reflect each ICONists’ intention.
* Governance Variable submitted by P-Rep

#### Governance Variables submitted by P-Rep

| Variables | Name | Explanation |
| :--- | :--- | :--- |
| i\_rep | Expected Monthly Reward per Representative | Expected reward amount for representative is determined by weighted average of each representative suggestion. Each P-Reps \(22 Main P-Reps and 79 Sub P-Reps\) suggest this amount based on their expenditure and profit. i\_rep must be equal to or greater than 10,000, and the suggested value should not increase or decrease more than 20% from the previous submission. i\_rep can be updated daily. |
| s | Step | Minimum transaction fee in the ICON network which is determined by weighted average of each representative suggestion. When updating the Step, the suggested value change should not exceed more than 5% from the previous value. |

#### Network Proposal

* Text Proposal
  * Text Proposal is an on-chain proposal to align each P-Rep’s opinions about governance.  
  * P-Rep can propose a Text Proposal on the ICON Network and they can vote Yes/No on on-chain. 
  * The scope of the proposal is very comprehensive; for example, the issue of off-chain governance can be discussed, such as hard forks, economic policies, and new functions.
* Malicious SCORE Proposal \(Temporarily\)
  * Malicious SCORE Proposal is an on-chain proposal to freeze the operation of a SCORE on the ICON Network.
  * P-Rep can propose a Malicious SCORE Proposal on the ICON Network and they can vote Yes/No on on-chain. 
  * SCORE can be frozen for up to 3 days. 
* P-Rep Disqualification Proposal
  * P-Rep Disqualification Proposal is an on-chain proposal to disqualify the P-Rep on the ICON Network.
  * P-Rep can propose a P-Rep Disqualification Proposal on the ICON Network and they can vote Yes/No on on-chain. 
  * This proposal prevents P-Rep who makes a problem that is difficult to identify on on-chain.

