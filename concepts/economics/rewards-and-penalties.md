# Rewards & Penalties

### Rewards

There are 7 reward types for earning ICX based on different types of participation in the ICON ecosystem. The reward system is bilateral in the relationship between [delegates](../governance/delegates.md) and general community members. Notice that both the delegates and general community members benefit from the same performance-based outcomes, but they are rewarded at different rates. The principle here is that delegates maintain more responsibility and are both rewarded and penalized as such:

* Block validation reward
* Delegate reward
* Delegate delegation reward
* Ecosystem expansion project reward
* Ecosystem expansion project delegation reward
* Decentralized application reward
* Decentralized application delegation reward

#### Block validation reward

The _Block validation reward_ is given to the top 22 delegates for proving their contribution by producing and validating blocks. This top tier of delegates is given roles in block production and validation, and the delegates can acquire a certain amount of ICX reward per block for providing computational resources. This system provides an economic incentive for block production and validation, thus maintaining the distributed network.

#### Delegate reward

The _Delegate reward_ rewards the top 100 delegates for proving their contribution by registering a node and receiving network-stake delegation from ICON community members. All delegates acquire a certain amount of ICX per block, depending on the amount of tokens staked (i.e. delegated) to them by ICON community members. The more tokens that are staked to them, the more rewards they earn. Therefore, this system provides an economic incentive for delegates to work harder to receive more tokens staked to them.

#### Delegate delegation reward

_Delegate delegation reward_ is specified for ICON community members in exchange for staking their ICX to a delegate. All community members can acquire a certain amount of ICX per block, based on how much they staked.

#### Ecosystem expansion project reward

_Ecosystem expansion project reward_ is the reward for the top 100 ecosystem expansion projects. All ecosystem expansion projects can prove their contribution by receiving staked ICX from community members. Only the top 100 ecosystem expansion projects acquire a certain amount of ICX per block. As a result of this, ecosystem expansion projects have an incentive to perform better projects to receive more staked tokens from community members.

#### Ecosystem expansion project delegation reward

_Ecosystem expansion project delegation reward_ is specified for community members in exchange for delegating their ICX to an ecosystem expansion project, thus proving the contribution of the ecosystem expansion project. All community members can delegate ICX to ecosystem expansion project and can acquire a certain amount of ICX per block.

#### Decentralized application reward

The 'Decentralized Application (dApp) Reward’ is the reward for the top 100 dApps according to the amount of ICX staked to that dApp. All dApps can receive staked ICX from community members. Only the top 100 dApps acquire a certain amount of ICX per block, depending on the amount of ICX staked. As a result of this, dApps have an incentive to develop a popular dApp to receive more staked tokens from community members.

#### Decentralized application delegation reward

_Decentralized application delegation reward_ is the reward specified for community members in exchange for staking their ICX to a dApp, thus proving the  contribution of the  dApp. All community members can delegate  ICX to dApps and can acquire a certain amount of ICX per block.

### Penalties

There are 3 types of penalties that result in forfeiting ICX or the opportunity to earn ICX based on participation patterns and community agreements within the ICON ecosystem. Delegates are the only community members who are penalized in the following ways based on their performance:

* Validation penalty
* Low productivity penalty
* Disqualification penalty

#### Validation penalty

This occurs when a specific delegate fails to validate blocks successively for 660 blocks

The penalty is to exclude such delegates from block production during the term. Block production and validation opportunities are forfeited until the next term, creating an opportunity cost.

#### Low productivity penalty

This applies when the Productivity Ratio of a specific delegate has dropped below 85%. The Productivity Ratio is the ratio of actual blocks produced and validated  divided by the number of opportunities to produce and validate a block. Delegates will not be subject to this penalty for their first 86,240 blocks as a block-producing delegate.

The penalty is to disqualify the delegate and burn 6% of the ICX staked to them.

#### Disqualification penalty

This applies when a specific delegate has been disqualified via a _Delegate Disqualification Proposal._

The penalty is to disqualify the delegate in question and burn 6% of the ICX staked to them.

### Reward Calculations

$$
Block\ validation\ reward = \beta_1 * 0.5 * (boolean(produces\ block) + boolean(validates\ block)/n_{del})
$$

$$
\beta_1 = (i_{del} * 0.5) * 22 * 1/1,296,000
$$

$$
i_{del} = determined\ by\ stake-weighted\ vote\ of \ top\ 22\ delegates
$$

$$
Delegate\ reward = \beta_2 * ICX_{del,staked}/ICX_{top100del,staked}
$$



$$
\beta_2=(i_{del} * 0.5) * 100 * 1/1,296,000
$$

$$
Delegate\ delegation\ reward = \beta_3 * ICX_{self,staked}/ICX_{all,staked}
$$

$$
\beta_3=r_{del} * ICX_{del,staked} * 1/15,552,000
$$

$$
r_{del} = (r_{max} - r_{min}) / (r_{point})^2 * ((ICX_{del,staked}/ICX_{tot}) - r_{point})^2 + r_{min}
$$

$$
r_{max} = 12\%
$$

$$
r_{min} = 2\%
$$

$$
r_{point} = 70\%
$$

$$
Ecosystem\ expansion\ project\ reward = \beta_4 * ICX_{del, staked} / ICX_{top100eep,staked}
$$

$$
\beta_4 = i_{eep} * 100 * 1/1,296,000
$$

$$
i_{eep} = i_{del} * 0.25
$$

$$
Ecosystem\ expansion\ project\ delegation\ reward = \beta_5 * ICX_{self,staked} / ICX_{top100eep,staked}
$$

$$
\beta_5 = r_{eep} * ICX_{eep,staked} * 1/15,552,000
$$

$$
r_{eep} = (r_{max} - r_{min}) / (r_{point})^2 * ((ICX_{eep,staked}/ICX_{tot}) - r_{point})^2 + r_{min}
$$

$$
Decentralized\ application\ reward = \beta_6 * ICX_{dapp, staked} / ICX_{top100dapp,staked}
$$

$$
\beta_6 = i_{dapp} * 100 * 1/1,296,000
$$

$$
i_{dapp} = i_{del} * 0.25
$$

$$
Decentralized\ application\ delegation\ reward = \beta_7 * ICX_{self,staked} / ICX_{top100dapp,staked}
$$

$$
\beta_7 = r_{dapp} * ICX_{dapp,staked} * 1/15,552,000
$$

$$
r_{dapp} = (r_{max} - r_{min}) / (r_{point})^2 * ((ICX_{dapp,staked}/ICX_{tot}) - r_{point})^2 + r_{min}
$$
