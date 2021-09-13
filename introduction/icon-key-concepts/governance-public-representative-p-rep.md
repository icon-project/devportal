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

#### 

<table>
  <thead>
    <tr>
      <th style="text-align:left">L0 (Item)</th>
      <th style="text-align:left">L1 (Detailed Item)</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Network Proposal Actions</td>
      <td style="text-align:left">Proposal Submitting</td>
      <td style="text-align:left">
        <ul>
          <li>All P-Reps in the validator set can set and submit the type of Network
            Proposal</li>
          <li>Network Proposal is able to get votes from validators immediately upon
            submission</li>
          <li>All P-Reps in the validator set must pay 100 ICX as fee when they submit
            the network proposal</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Proposal Cancellation</td>
      <td style="text-align:left">
        <ul>
          <li>The P-Rep that submitted the proposal can cancel the proposal</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Proposal Voting Period</td>
      <td style="text-align:left">
        <ul>
          <li>All P-Reps in the validator set can exercise their voting rights on Approve/Disapprove
            as much as Voting Weight they own</li>
          <li>Voting period is only activated within 5 terms, and after 5 terms, it
            will be automatically disapproved</li>
          <li>Voting period ends when the term ends</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Proposal Approval</td>
      <td style="text-align:left">
        <ul>
          <li>Network Proposal will be immediately approved if 66% of total votes of
            validators and 66% of quorums approve</li>
          <li>Once the proposal is approved, the proposal will be executed immediately</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Proposal Rejection</td>
      <td style="text-align:left">
        <ul>
          <li>Network Proposal will be immediately rejected if 33% of total votes of
            validators and 33% quorums reject</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Types of Network Proposal</td>
      <td style="text-align:left">Text Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>Text Proposals are open-ended proposals submitted by P-Reps and, if approved,
            are executed in good faith</li>
          <li>All P-Reps in the validator set can write and submit proposals on-chain.
            Anything can be proposed as a Text Proposal so that P-Reps can raise discussion
            topics</li>
          <li>Text proposals are different from all others in that they do not provide
            autonomous execution.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Malicious SCORE Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>Malicious SCORE Proposal is a proposal that validators can submit to freeze
            and unfreeze a SCORE. Once the SCORE is frozen, will not function until
            unfrozen.</li>
          <li>Freeze</li>
          <li>Unfreeze</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Revision Update Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>Revision Proposals, if approved, give validators the ability to autonomously
            update their software version</li>
          <li>Some updates are only activated after approving in the Revision Proposal</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">P-Rep Disqualification Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>P-Rep Disqualification Proposal is a proposal that validators can submit
            a specific P-Rep&#x2019;s address to disqualify a malicious P-Rep</li>
          <li>It gives validators the ability to autonomously disqualify a P-Rep. Disqualified
            P-Reps are not able to register as a P-Rep again</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Step Adjustment Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>Step Adjustment Proposals, if approved, give validators the ability to
            autonomously adjust the Step Price and Step Costs, which is the base transaction
            fee on the ICON Network</li>
          <li>This Step price can be adjusted within 25% of existing Step Price</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Governance SCORE Update Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>This proposal is to update the code of the Governance SCORE</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Network SCORE Deployment Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>Network SCORE Deployment Proposals, if approved, will deploy a SCORE owned
            by the validators</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Network SCORE Update Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>Network SCORE Updater Proposal, if approved, will update the code of a
            Network SCORE</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Network SCORE Designation Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>Network SCORE Designation Proposal, if approved, will assign a specific
            role to a Network SCORE</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Monthly Reward Fund Setting Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>All P-Reps in the validator set can submit the proposal to set the iglobal</li>
          <li>When validators submit the proposal, if they doesn&#x2019;t meet this
            condition, the transaction to create the proposal can be rejected
            <ul>
              <li>iglobal can be submitted within a range of 25%
                <ul>
                  <li>current iglobal-25%&lt;submitted iglobal&lt;current iglobal+25%</li>
                </ul>
              </li>
              <li>Inflation determined by the iglobal cannot exceed 115% of the current
                total supply</li>
              <li>iglobal12total supply115%</li>
            </ul>
          </li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Monthly Reward Fund Allocation Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>All P-Reps in the validator set can submit the iprep, icps, irelay, and
            ivoter value to determine the allocation of the monthly reward fund</li>
          <li>Constraint: i_prep + i_cps +... i_n = 100%</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

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

