## Governor Delta
An EVM (Ethereum virtual machine) modular governance system, successor to [Governor Bravo](https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/GovernorBravoDelegateG2.sol).

### Voting Modules
Arbitrary voting model implementations, enabling extensibility for alternative coordination mechansims without restructuring.

#### Strategies
* [`WeightedVotingStrategy`](https://github.com/focaldotorg/governor-delta/blob/main/src/modules/strategies/WeightedVotingStrategy.sol): Traditional shareholder voting (one share, one vote)
* [`TenureVotingStrategy`](https://github.com/focaldotorg/governor-delta/blob/main/src/modules/strategies/TenureVotingStrategy.sol)*: Linear time-weighted voting (loyalty shares)
* [`PolycentricVotingStrategy`](https://github.com/focaldotorg/governor-delta/blob/main/src/modules/strategies/PolycentricVotingStrategy.sol)*: Path-dependent voting ([paper](https://focal.org/polycentric-voting.pdf))

_`*` - Virtualised strategies ([learn more](./SPEC.md#virtualisation))_

#### Extensions
Feature enhancements that strategy standards can integrate for example the [bootstrapped extension](https://github.com/focaldotorg/governor-delta/blob/main/src/modules/strategies/derivatives/BootstrappedTenureVotingStrategy.sol) enables issuers to specify preconfigured time weights for set addresses with apprioriate expiration dates, allowing the engineering of power dynamics explicitly for virtualised strategies.

---

### Native Delegation
Delta does not require [`ERC20Votes`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/ERC20Votes.sol) or a checkpoint system on the underlying governing token, delegation now being facilitated through the introduction of a lock and commit system. Supporting any token that follows the [ERC20 standard](https://eips.ethereum.org/EIPS/eip-20) the ability to be used in governance without the need for token migration or wrapping. Additionally two new dynamics are introduced: 

**Expirations**: Delegations are configured under a maximum duration, after which authority returns to the delegator, preventing idle delegations from accruing significant voting power overtime that they may become unaccustomed with.

**Revocation**: Delegators retain the right to revoke or redirect voting power except when it is utilised in an active ballot, on the contrary to virtualised voting strategies where revocation can happen without restriction. Providing a conflict resolution mechanism in response to delegated voting power concentration.

---

### Veto Mechanism
Bravo has a critical flaw regarding proposal cancellation, given that it can be only initiated by **a) the proposer** or **b) a counterparty if the proposer's balance falls under the proposal threshold**. This becomes problematic in the scenario where an malicious proposal is submitted, succeeds and then queued **it becomes impossible to dispute**.

Thus veto powers give stakeholders the agency to contest proposals subject to the timelock - all without requiring a new proposal to be cast - creating a more balanced solution for fighting adversarial capture while retaining the original proposal lifecycle. Contesting a proposal can only be done within a select time window of the timelock's process cycle until it is marked as valid for execution.

---

### Graduated Proposals
Delta introduces a tiered proposal framework, each level configured with an independent quorum, quota and voting duration parameters. This mediates short term voting power advantages on high tier critical decisions through extended voting periods, while enabling agile and broad decision-making for lower tiered proposals. An example proposal configuration given total supply equal to 100,000 shares:

| Tier | Tier | Quorum | Quota | Duration |
|------|----------|--------|-------|----------|
| 0 | Low | 5,000 | 500 | 7 days |
| 1 | Medium | 15,000 | 3,000 | 14 days |
| 2 | High | 33,000 | 10,00 | 44 days |

All tiers are configurable and adjustable through governance, by default **all tiers are assigned equal parameters**.

---

### Guard System 

Delta provides an open framework for defining an organisation's policies, achieved through a module system for execution invariant checks using preimage check before proposal execution and a snapshot after. If a policy (guard) is failed to be met, proposal execution will be reverted. This provides robust and explict safeguards that was previously lackluster in subsequent governance standards. Some examples being; restricting the amount of assets that can be transferred, contracts which can be called and even the functions permitted in any proposal. 

#### Modules
* [`TransferGuard`](https://github.com/focaldotorg/governor-delta/blob/main/src/modules/guards/TransferGuard.sol): Index by asset, restrict transfers that exceed limits and budgets  
* [`WhitelistGuard`](https://github.com/focaldotorg/governor-delta/blob/main/src/modules/guards/WhitelistGuard.sol): Indexed by address, permit calls to select targets
* [`FunctionSelectorGuard`](https://github.com/focaldotorg/governor-delta/blob/main/src/modules/guards/FunctionSelectorGuard.sol): Restrict function types that can be executed 

Guards work complimentary to the graduated proposal framework, where they are configurable by tier, with more strict safeguards assigned to lower tiers and more lenient for higher tiers. Consider that on deployment, **only the [`StakingGuard`](https://github.com/focaldotorg/governor-delta/blob/main/src/modules/guards/StakingGuard.sol) is configured**.

---

### Migrating

#### From Alpha

Given that the first implementation of the Governor standard, Alpha, is a standalone contract it must be depreciated.

1. Deploy Delta
2. Transfer all peripheral contract permissions from Bravo to Delta
3. Transfer timelock admin rights to Delta
4. Configure delegation, voting strategy and proposal configurations (optional)

#### From Bravo

Given that Delta is designed as an extension of Bravo, existing deployments can be upgraded using its delegator (proxy) without reconfiguring permissions to perhipheral contracts as the target address does not change:

#### From Bravo
1. Deploy Delta
2. Propose migration by assigning Delta as the new implementation on the Bravo delegator
3. Call the `initialize` function on the delegator to migrate
4. Configure delegation, voting strategy and proposal configurations (optional)

---

### Security
Report vulnerabilities via [research@focal.org](mailto:research@focal.org). Do not open public issues for security disclosures.

---

### Contributing
Open a well documented issue referencing the relevant areas of the architecture of any problem statement. Pull requests (PR) should include test coverage and a clear description of the intent and design tradeoffs. Issues and PRs should follow title capitalisiation when labeling and commit messages should always be lowercase. 


