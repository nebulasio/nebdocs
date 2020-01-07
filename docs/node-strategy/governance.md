# 3. Governance

**This chapter will introduce the Governance Mechanism of PoD mechanism, follow here:**

* [3.1 Governance Committee](#governance-committee)
* [3.2 Governance Scope](#governance-scope)
	* [3.2.1 Community Collaboration](#community-collaboration)
	* [3.2.2 Community Assets](#community-assets)
	* [3.2.3 Penalties for Consensus Mechanism](#penalties-for-consensus-mechanism)
* [3.3 Governance Method: Vote](#governance-method-vote)
	* [3.3.1 Voting Cycle](#voting-cycle)
	* [3.3.2 Voting Methods](#voting-methods)
	* [3.3.3 Processing of Voting Results](#processing-of-voting-results)
* [3.4 Penalty Mechanism](#penalty-mechanism)
	* [3.4.1 Individual Governance Node Penalties](#individual-governance-node-penalties)
	* [3.4.2 Governance Failure](#governance-failure)


Nebulas focuses on the contribution of different roles to the diverse ecosystem via decentralized collaboration and the utilized governance mechanism is an important portion of PoD mechanism. 

The governance mechanisms are a range of tools for community self-governmenance via the organization of community collaboration and management of community assets by the governance committee.


## 3.1 Governance Committee

The implementation of governance mechanisms is managed by the Governance Committee and is made up of governance nodes.

**Governance cycle:** One governance cycle will occur every 820 consensus node polling cycles (approximately 1 month).

**Governance node selection:** Governance nodes are selected from the consensus committee and the selected 51 consensus nodes where the largest number of block generators for the past 820 consensus node polling cycles are eligible to become governance nodes in the governance cycle. If there are equally qualified nodes available to become governance nodes and not enough spaces are left, the node(s) which have achieved the number of generated blocks first will be selected.


## 3.2 Governance Scope


### 3.2.1 Community Collaboration

The proposal operation of the Nebulas community is an important part of the continuation of the Autonomous Metanet. All proposals and projects of the Nebulas community are public information which are displayed and managed via the Go Nebulas collaboration platform ([go.nebulas.io](https://go.nebulas.io)). All community members can put forward their own ideas, opinions and suggestions on the future development of the Nebulas via this platform. Ideas and suggestions include but are not limited to <sup id="a1">[[1]](#f1)</sup>:



1. Research and development of the Nebulas mainnet;
2. Community collaboration process optimization, governance recommendations, etc…;
3. Improvement suggestions and bug reports for existing Nebulas community products;
4. Development and maintenance of community eco-products;
5. Community operations and market expansion.

For a proposals to go from idea to implementation, it will go through multiple steps including: 



*   proposal;
*   Project establishment;
*   project execution;
*   project acceptance.

Each step needs to be voted for and approved by the Governance Committee. The Governance Committee has three types of voting tasks in each governance cycle:



1. **Proposal voting:** Vote on the proposals submitted from the Nebulas community and decide whether to approve the project to the next phase.
2. **Project establishment voting:** Vote on the establishment and budget of projects that have successfully passed the proposal process.  
3. **Project acceptance voting:** Review and vote on projects that have been established, completed and issue funding.

**The Governance Committee process is as follows:**



![](../resources/node/Nebulas-PoD-3-1.png "Figure 3.1 Governance Committee Voting Process")


*Figure 3.1 Governance Committee Voting Process*

**Proposal period:** All community members are welcome to create and share proposals on Go Nebulas ([go.nebulas.io](https://go.nebulas.io)).

**Project establishment period:** Projects that successfully complete the proposal period will proceed to the establishment period. All projects are separated into two categories:



1. **No budget required for this project:** For example, proposals that include the discussion of improving existing Nebulas projects. These include suggestions on adjusting the structure of the governance organization and the adjustment of the mainnet parameters. After a proposal is voted on and approved, the relevant person in charge may accelerate the implementation of this proposal.
2. **Proposals that require a budget:** This process is facilitated by the Go Nebulas Operations team, which handles project budgets and fund release. Project creators can submit budgets, project objectives, execution steps and expected duration. Creators can also apply to be the project owner or elect a community member to operate the project. Projects should be submitted in accordance with the standard template available on Go Nebulas.

**Project execution and acceptance period:** The execution and acceptance period is an internal operational process of Go Nebulas; the governance Committee does not directly participate in this process. The process is divided into three stages:


1. **Set budget period:** The project creator or the Go Nebulas operation team can be set as the project creator. Project creators set the reward for successful completion of the project and members of the community are welcome to participate in projects;
2. **Execution period:** The project creator confirms the project owner/manager. At this point, the project owner begins to execute the project and once progressing and upon completion, submits the project results;
3. **Project Review Period:** Once a project is marked as completed, the project creator and the Go Nebulas Operations Team will review project and its results. Afterwords, a recommendation on whether the project has been successfully completed or not will be given to the Governance Committee. The committee then decides what further action, if any is required. If none is required, the project will receive its funding as decided in the budget period.


### 3.2.2 Community Assets

The Governance Committee is responsible for managing the use of the public community assets. Public community assets include:



1. **Use and distribution of the Go Nebulas Community Collaboration Fund:** The primary source of this fund is the DPoS revenue generated from Nebulas since the launch of the Nebulas mainnet on March 30, 2018. Some of these assets have been used for programs such as the Nebulas Incentive Program. The remaining assets will be used for the Go Nebulas Community Collaboration Fund after node decentralization. \
Since the maximum amount of NAS issued per governance cycle is capped at no more than $30,000 USDT, the actual use of the governance mechanism within six months of its launch is $180,000 USDT equivalent NAS.
2. **Incentive allocation of the Nebulas PoD Node decentralization strategy:** The incentive for Nebulas PoD Node Decentralization Strategy includes two parts: consensus incentive and governance incentive. The source is 8,219.1744 NAS revenue generated daily via DPoS. For the specific allocation method, review section [1.3 Incentive allocation](overview.html#incentive-allocation).

The use of public assets, changes to the allocation program, etc... will require the use of the proposal process and can be implemented only after the adoption of the resolution by the Governance Committee.


### 3.2.3 Penalties for Consensus Mechanism

During the voting phase of governance cycle, the governance committee will also need to vote on the results of medium and severe security violations.



1. If the governance committee votes that the punishment is justified, the NAS that has been frozen will be donated to the Go Nebulas Community Collaboration Fund.
2. If the governance committee votes that the node did not cause intentional harm to the network, the block generation stability index S<sub>(i)</sub> of the node will be restored to the level prior to the punishment and the NAS will be unfrozen.


## 3.3 Governance Method: Vote


### 3.3.1 Voting Cycle

Governance nodes must vote within 120 polling cycles (about 5 days) after the end of the previous governance cycle.  Not participating in voting is considered an Abstain vote.


### 3.3.2 Voting Methods

The voting of governance nodes is conducted on the public chain with the results viewable to all. All governance nodes are expected to participate in **all** governance periods. All votable items will have the following options (must choose one): 



*   For
*   Against
*   Abstain

Each proposal can only be voted for once by each governance node by utilizing 1 NAX per item being voted. NAX used for voting is destroyed and will not be returned.


### 3.3.3 Processing of Voting Results

The adoption of a proposal or item requires the following conditions:


<table>
  <tr>
   <td><strong>Type</strong>
   </td>
   <td><strong>Governance node participation rate</strong>
   </td>
   <td><strong>Required approval rate</strong>
   </td>
   <td><strong>Budget constraints</strong>
   </td>
  </tr>
  <tr>
   <td>Proposal Voting
   </td>
   <td>Minimum of 26 nodes
   </td>
   <td>50% or greater
   </td>
   <td>Not-applicable 
   </td>
  </tr>
  <tr>
   <td>Project Establishment Voting
   </td>
   <td>Minimum of 26 nodes
   </td>
   <td>67% or greater
   </td>
   <td>A single project budget must not exceed $15,000 USDT <sup>*</sup>;
<p>
The total for all approved projects during the governance cycle must not exceed $30,000 USDT <sup>**</sup>
   </td>
  </tr>
  <tr>
   <td>Project Acceptance Voting
   </td>
   <td>Minimum of 26 nodes
   </td>
   <td>67% or greater
   </td>
   <td>/
   </td>
  </tr>
  <tr>
   <td>Penalties for consensus mechanism
   </td>
   <td>Minimum of 26 nodes
   </td>
   <td>67% or greater
   </td>
   <td>/
   </td>
  </tr>
</table>


*Table 3.1: Processing of Voting Results Table*

\* If a single project budget will exceed the maximum dollar value, it is suggested to split the proposed project into a multi-phase project. 

** If the total amount of all approved projects during the governance cycle exceeds the maximum budget, projects are ranked by their support rate. Any proposal that is approved but funding is not available for the current governance cycle is deferred to the next governance cycle.


## 3.4 Penalty Mechanism


### 3.4.1 Individual Governance Node Penalties

If a consensus node becomes a governance node for two consecutive governance cycles without taking part in governance voting, the node will not be able to be selected as a governance node for 820 consensus polling cycles (approximately one month).


### 3.4.2 Governance Failure



1. If there are fewer than 26 (of the 51 selected) governance nodes participating in the voting during a governance cycle, the cycle will be declared invalid; no decision made will be executed and all governance incentives will be donated to the Go Nebulas Community Collaboration Fund (See [3.2.2 Community Assets](#community-assets)).
2. If there is no proposal or project in a governance cycle, i.e. there is nothing to vote on, the cycle is declared invalid and all governance incentives will be donated to the Go Nebulas Community Collaboration Fund.

***

<span id="f1">[[1]](#a1)</span> [Go.nebulas.io help Documentation](https://www.notion.so/Go-nebulas-io-Help-Documentation-cbdeb02e0ff547b2b7cf59f2d249aafb)



