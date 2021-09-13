# Governance - Public Representative \(P-Rep\)

## What is a Public Representative?

* A Public Representative \(P-Rep\) is a full node on the ICON Network that participates in consensus and governance.
* P-Reps consist of the top 22 Main P-Reps and 78 Sub P-Reps, and are elected by ICONists.

## Reward for Representative

Representatives can receive compensation through IISS. See [IISS](governance-iiss.md) for more information.

## On-chain Governance System

* Representatives can submit governance variables, network proposals through an on-chain governance system. All ICONists can delegate their ICX to the representatives to participate in decision making. It is important to delegate the ICX to reflect each ICONists’ intention.
* Governance proposals are submitted by P-Rep

### Governance Methods

<table>
  <thead>
    <tr>
      <th style="text-align:left">Governance type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
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
      <td style="text-align:left">Malicious SCORE Propoosal</td>
      <td style="text-align:left">
        <ul>
          <li>Malicious SCORE Proposal is a proposal that validators can submit to freeze
            and unfreeze a SCORE. Once the SCORE is frozen, will not function until
            unfrozen.</li>
          <li>Freeze</li>
          <li>Unfreeze</li>
        </ul>
      </td>
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
    </tr>
    <tr>
      <td style="text-align:left">Governance SCORE Update Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>This proposal is to update the code of the Governance SCORE</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Network SCORE Deployment Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>Network SCORE Deployment Proposals, if approved, will deploy a SCORE owned
            by the validators</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Network SCORE Update Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>Network SCORE Updater Proposal, if approved, will update the code of a
            Network SCORE</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Network SCORE Designation Proposal</td>
      <td style="text-align:left">
        <ul>
          <li>Network SCORE Designation Proposal, if approved, will assign a specific
            role to a Network SCORE</li>
        </ul>
      </td>
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
    </tr>
  </tbody>
</table>

### Proposal processes

<table>
  <thead>
    <tr>
      <th style="text-align:left">Method</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
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
    </tr>
    <tr>
      <td style="text-align:left">Proposal Rejection</td>
      <td style="text-align:left">
        <ul>
          <li>Network Proposal will be immediately rejected if 33% of total votes of
            validators and 33% quorums reject</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

### Inflation controls and reward distribution

<table>
  <thead>
    <tr>
      <th style="text-align:left">Network variable</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">iglobal</td>
      <td style="text-align:left">
        <ul>
          <li>iglobal is the total amount of monthly reward fund that is determined
            by Monthly Reward Fund Setting Proposal</li>
          <li>iglobal can be submitted within a range of 25%
            <ul>
              <li>current iglobal-25%&lt;submitted iglobal&lt;current iglobal+25%</li>
            </ul>
          </li>
          <li>Denominated in ICX (10^18 loop)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">iprep</td>
      <td style="text-align:left">
        <ul>
          <li>iprep is the percentage allocated to the P-Rep from the monthly reward
            fund</li>
          <li>A 4 digit integer</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">icps</td>
      <td style="text-align:left">
        <ul>
          <li>icps is the percentage allocated to the CPS from the monthly reward fund</li>
          <li>A 4 digit integer</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">irelay</td>
      <td style="text-align:left">
        <ul>
          <li>irelay is the percentage allocated to the Relay from the monthly reward
            fund</li>
          <li>A 4 digit integer</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ivoter</td>
      <td style="text-align:left">
        <ul>
          <li>ivoter is the percentage allocated to the Voter from the monthly reward
            fund</li>
          <li>A 4 digit integer</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

