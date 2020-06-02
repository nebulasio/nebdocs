# 2. Consensus

**This chapter will introduce the Consensus Mechanism of PoD mechanism, follow here:**

* [2.1 Minimum Requirements for Node Selection](#minimum-requirements-for-node-selection)
* [2.2 Node Selection Rules](#node-selection-rules)
	* [2.2.1 Candidate Node Comprehensive Ranking Algorithm](#candidate-node-comprehensive-ranking-algorithm)
		* [2.2.1.1 NAX Votes](#nax-votes)
		* [2.2.1.2 Block Generation Stability Index](#block-generation-stability-index)
	* [2.2.2 Consensus Node Selection Algorithm](#consensus-node-selection-algorithm)
* [2.3 Consensus Algorithm](#consensus-algorithm)
	* [2.3.1 Block Generation Order](#block-generation-order)
	* [2.3.2 Packaging of Generated Blocks](#packaging-of-generated-blocks)
	* [2.3.3 On-chain Confirmation](#on-chain-confirmation)
* [2.4 Exit Mechanism](#exit-mechanism)
	* [2.4.1 Withdrawal of NAX Support for a Node (votes)](#withdrawal-of-nax-support-for-a-node-votes)
	* [2.4.2 Exiting the Node Pool](#exiting-the-node-pool)
* [2.5 Penalties and Emergency Response](#penalties-and-emergency-response)
	* [2.5.1 Penalties](#penalties)
		* [2.5.1.1 Block Generation Penalties](#block-generation-penalties)
		* [2.5.1.2 Governance Penalties](#governance-penalties)
	* [2.5.2 Emergency Response](#emergency-response)



The consensus mechanism utilizes smart contract management which is primarily comprised of node selection rules and the consensus algorithm. This smart contract jointly completes the block generation and ensures the normal operation of the mainnet.

The average block time on the Nebulas mainnet is 15 seconds. During each polling period, the 21 selected consensus nodes take turns generating 10 blocks each. As a result, one polling cycle is 210 blocks which takes about 52.5 minutes. The consensus mechanism execution process during each polling cycle is shown in the following figure:


![](../resources/node/Nebulas-PoD-2-1.png "Figure 2.1 The consensus mechanism execution process for each polling cycle")


*Figure 2.1 The consensus mechanism execution process for each polling cycle*


## 2.1 Minimum Requirements for Node Selection 

Any individual or organization can apply to become a consensus node and must meet all of the following eligibility requirements to participate in the candidate selection process:



*   The server meets the minimum requirements (see [Appendix A - recommended hardware configuration](appendix#appendix-a-recommended-hardware-configuration));
*   The server is guaranteed to be in operation;
*   The node pledge (vote) is not less than 100,000 NAX;
*   Pledge of 20,000 NAS as deposit;
*   No record of severe level abuse or manipulation on the network (see [2.5.1 penalties](#penalties))


## 2.2 Node Selection Rules

The node selection rule consists of two steps:



1. **Candidate node selection:** During each polling cycle, among all nodes that meet the minimum selection requirements, a total of 51 nodes are selected according to the comprehensive candidate node ranking algorithm via smart contract;
2. **Consensus node selection:** During each polling cycle, the algorithm selects 21 consensus nodes which are selected in a consistent method and best represents the user's rights and interests in a group of candidate nodes which is based on the consensus node selection algorithm via smart contract. The consensus nodes are responsible for block generation and can obtain consensus incentives as long as they participate in the process of block generation (online, creating blocks, not manipulating the network, etc…).

The candidate node and the consensus node together constitute the consensus committee. The selection process is shown in the following figure:




![](../resources/node/Nebulas-PoD-2-2.png "Figure 2.2 Node Selection Process")


*Figure 2.2 Node Selection Process*


### 2.2.1 Candidate Node Comprehensive Ranking Algorithm

Under the premise of meeting the minimum requirements of becoming a candidate node ([2.1 Minimum requirements for node selection](#minimum-requirements-for-node-selection)), all nodes are ranked by the comprehensive candidate node ranking algorithm; the top 51 nodes selected via the ranking algorithm will be selected as candidate nodes. 

The candidate node ranking algorithm references to two primary factors: NAX poll number V<sub>(i) </sub>and block stabilization index S<sub>(i)</sub>. The final candidate node ranking index “R<sub>(i)</sub>” is:

> R<sub>(i)</sub> = V<sub>(i)</sub> × S<sub>(i)</sub>

Assuming that S<sub>(i)</sub> is the same for multiple nodes and the NAX vote V<sub>(i)</sub> is also the same for multiple nodes, the first node to reach the required NAX vote V<sub>(i)</sub> is selected.


#### 2.2.1.1 NAX Votes

**Number of NAX votes V<sub>(i)</sub>:** All community members and node operators can pledge NAX to further support the activation of a node which helps the node improve their overall ranking.


#### 2.2.1.2 Block Generation Stability Index

**Block generation stability index S<sub>(i)</sub>:** This rating is determined by the ratio of successful block generation when it’s chosen as a consensus node. If this node has not yet had the chance to be a consensus node, initial value of S<sub>(i)</sub> is 0.8. During each polling cycle, a candidate node has three possible values: 



1. Not participating in block generation;
2. Successful/accepted block generation;
3. Invalid block generation.

**A. Not participating in block generation**

The S<sub>(i+1) </sub>of the following cycle of candidate nodes that have not participated in block generation is:

> S<sub>(i+1) </sub>= S<sub>(i) </sub>+ 0.01

If the node continues to not participate in the generation of blocks, the S<sub>(i)</sub> rating is reduced to the initial value S=0.8 at its lowest level.

**B. Successful block generation**

Consensus nodes need to generate 10 blocks per polling cycle (polling cycles consist of 210 blocks). If a node generates 10 blocks successfully during the cycle, the S<sub>(i+1)</sub> of the next cycle is:

> S<sub>(i+1) </sub>= S<sub>(i) </sub>+ 0.1 (S<=1)

S<sub>(i)</sub> is gradually increased to the maximum level of 1. In general, functioning nodes with stable block generation will reach the maximum level of S<sub>(i)</sub>=1.

**C. Invalid block generation**

If the node generates a invalid block, the S<sub>(i+1) </sub> of the next cycle is:

> S<sub>(i+1) </sub>= S<sub>(i) </sub>× (10 - C) / 10

Where C is the number of invalid blocks. The larger C, the lower S<sub>(i)</sub> value. If S<sub>(i)</sub> falls to the K threshold (K initial value is 0.5), the consensus node cannot be selected as a candidate node for the next 20 polling cycles, as detailed in [2.5.1 Penalties](#penalties).


### 2.2.2 Consensus Node Selection Algorithm

Consensus nodes are selected from the 51 candidate nodes that are initially selected during the polling cycle. The selection method is as follows:



1. The top 6 candidate nodes are selected automatically as per their score (detailed above).
2. The remaining 15 candidate nodes are selected from the candidate pool containing the 45 additional nodes according to the following formula:

> R<sub>Consensus</sub> = (R<sub>(i)</sub> / Sum(R)) × Random()

The formula explanation is as follows:


**R<sub>Consensus</sub>**：Consensus node ranking index.


**R<sub>(i)</sub>**：Candidate nodes ranking index. R<sub>(i)</sub> is the derived score of two primary factors: NAX poll number V<sub>(i)</sub> and block stabilization index S<sub>(i)</sub>; as a result, R<sub>(i)</sub> is treated as the community support rate of the node and its contribution of historical blocks generation.


**Sum(R)**: The sum score of 51 candidate nodes ranking index; as a result, R<sub>(i)</sub>/Sum(R) can be treated as an individual node contribution ratio within the 51 candidate nodes.


**Random()**：A random probability.


## 2.3 Consensus Algorithm

The consensus algorithm is based on the well understood and mature DPoS consensus mechanism where the block generation of the following polling cycle is scheduled to be produced by the nodes within the consensus committee; the selected 21 consensus nodes take turns to generate blocks. After the polling cycle is complete, the next selected 21 consensus nodes take turns to generate blocks in the following cycle.

Byzantine fault tolerant BFT<sup id="a1">[[1]](#f1)</sup> operation is used to ensure the consistency and stability of the blockchain and the PoD mechanism.


### 2.3.1 Block Generation Order

The order of block generation of the 21 consensus nodes is randomly selected via a Verifiable Random Function (VRF<sup id="a2">[[2]](#f2)</sup>) in one polling cycle. The consensus nodes and the order responsible for the block generation remain unchanged during each polling cycle.


### 2.3.2 Packaging of Generated Blocks

Consensus nodes package transactions that are contained within the transaction cache pool when it is time to generate a new block. The specific methods is as follows:



1. Consensus nodes package blocks strictly according to the predefined order and duration of the polling cycle.
2. Package as many transactions as possible within packing time-frame;
3. Transactions with a higher overall GasPrice (when compared to other pending transactions) will take priority;
4. A non-verifiable transaction is disregarded when it's execution fails.


### 2.3.3 On-chain Confirmation

The on-chain confirmation for consensus nodes guarantees the consistency and security of the chain as well as penalizing any nodes that may harm the integrity of the blockchain. The Nebulas blockchain utilizes the following rules:



1. The longest subchain is chosen as the optimal chain.
2. The optimal chain is selected according to hash order of previous blocks if the subchains are with the same length.
3. The use of BFT operation across the network for irreversible transactions requires the confirmation from ⅔+1 of consensus nodes within the network;
4. The penalty mechanism should be adopted for attacks such as generating blocks when unexpected and attempting double-spends (see [2.5.1 Penalties](#penalties)).


## 2.4 Exit Mechanism

Voting for PoD nodes is a fair and free service. All members of the community can withdraw support via NAX for a node or apply to exit the node pool at any time.


### 2.4.1 Withdrawal of NAX Support for a Node (votes)

All members or organizations within the community may at any time withdraw their support for a community operated node. When support/votes are withdrawn, the node operators NAX support level is immediately reduced ([2.2.1.1 NAX votes](#nax-votes), V<sub>(i)</sub>) and will affect their ranking in following rounds of node selection. As stated in the minimum requirements ([2.1 Minimum requirements for node selection](#minimum-requirements-for-node-selection)), if the total amount of NAX support for a node drops under 100,000 NAX, they cannot be selected as a consensus node.

**Quantity of votes to withdraw:** The voters may choose how much NAX to withdraw for their support. Community members or organizations can only apply to revoke their own NAX.

**NAX return time-frame:** Once a withdrawal request has been issued, NAX is returned to the voter’s original address after 120 polling cycles (approximately 5 days) has passed.


### 2.4.2 Exiting the Node Pool

All nodes can exit the pool at any time. Once the exit request has been issued, the node will immediately lose its candidacy for following cycles.

**Return of NAS security deposit:** All NAS deposit required for candidacy is returned in one sum (partial refund is not an option).

**NAS security deposit return time-frame:** Once the exit request has been issued, the security deposit is returned after 820 polling cycles (approximately 1 month) and will be returned to the original address.

**Return of NAX deposit:** Any NAX that has been voted/issued for a node which is exiting the pool will be returned to the corresponding address after 120 polling cycles (approximately 5 days).


## 2.5 Penalties and Emergency Response


### 2.5.1 Penalties


#### 2.5.1.1 Block Generation Penalties

In order to maintain the security of the PoD system, the corresponding Penalties are carried out according to the situation; the more malicious act of a node, the higher the punishment. 

**The three block generation penalties for the consensus node are as follows:**

<table>
  <tr>
   <td rowspan="2" ><strong>Security level</strong>
   </td>
   <td rowspan="2" ><strong>Damage</strong>
   </td>
   <td rowspan="2" ><strong>Examples</strong>
   </td>
   <td colspan="2" ><strong>Punishment</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Limits</strong>
   </td>
   <td><strong>Penalty</strong>
   </td>
  </tr>
  <tr>
   <td>Low
   </td>
   <td>Causing instability to the network
   </td>
   <td>Intermittent participation in consensus such as failure to generate a block when expected.
   </td>
   <td>Block stability index S<sub>(i) </sub>(<a href="#block-generation-stability-index">2.2.1.2</a>) decline; if reduced to 0.5 or less, the node cannot participate in the candidate node selection for 20 polling cycles (approximately 1 day).
   </td>
   <td>No penalty
   </td>
  </tr>
  <tr>
   <td>Medium
   </td>
   <td>Causing instability to the network
   </td>
   <td>No blocks generated for an entire cycle.
   </td>
   <td>The node cannot participate in the candidate node selection for 20 polling cycles (approximately 1 day).
   </td>
   <td>Freeze 5% of NAS deposit
   </td>
  </tr>
  <tr>
   <td>Severe
   </td>
   <td>Threat to the security of the system or assets
   </td>
   <td>Multiple blocks generated at the same height or failure to revoke a "bad" block.
   </td>
   <td>Permanent removal from the Governance pool
   </td>
   <td>Freeze all NAS deposit and all NAX for this node (include the votes from community)
   </td>
  </tr>
</table>


*Table 2.1: Consensus Mechanism Safety Rating Table*

**Medium and Severe punishment process:**



1. Once a medium and severe penalty occurs, restrictions are automatically executed, and the node will be observed to see if it generates a minimum of one block within 200 polling cycles (approximately 7 days) after the incident.
    1. If the node proceeds to generate at least one block, the penalty will be disregarded.
    2. If there is still no block, it is deemed that the problem has not been resolved, the node will have about 1,000 NAS (5% of NAS deposit) frozen.
    3. If after a penalty occurs,  the node can also exit the node strategy. Afterward, NAX will be returned to the original address after 120 polling cycles (approximately 5 days) after the successful submission of the node exit application.

2. During the voting phase of the next governance cycle, the governance committee votes to determine whether the node punishment is justified. 
    1. If the governance committee votes that the punishment is justified, the NAS that has been frozen will be donated to the Go Nebulas Community Collaboration Fund (See [3.2.2 Community Assets](governance.html#community-assets)).
    2. If the governance committee votes that the node did not cause intentional harm to the network, the block generation stability index S<sub>(i)</sub> of the node will be restored to the level prior to the punishment and the NAS will be unfrozen.

See the [3.2.3 Penalties for consensus mechanism](governance.html#penalties-for-consensus-mechanism) and [3.3.3 Processing of voting results](governance.html#processing-of-voting-results) of [3.2 Governance scope](governance.html#governance-scope).


#### 2.5.1.2 Governance Penalties

In addition to the block generation penalties (listed above), when a consensus node is selected as a governance node, the governance node must complete all governance tasks (taking part in votes). If the governance node does not take part in the governance process for two consecutive governance cycles, it cannot be selected for the next 820 polling cycles (approximately one month). [See 3.4.1 Individual governance node penalties](governance.html#individual-governance-node-penalties).


### 2.5.2 Emergency Response

In the event of an attack on the Nebulas mainnet from a hacker or other unforeseen threats/emergencies and in order to ensure that the network can quickly respond to these attacks and reduce the harm of them, the Nebulas Foundation has reserved emergency smart contract management methods. The Nebulas Foundation can immediately blacklist the address in question and prohibit transfers from blacklisted addresses.

The entire process is open and transparent. The Nebulas Foundation will thoroughly review the incident and openly accept the supervision from the community.


***


<span id="f1">[[1]](#a1)</span> **BFT (Byzantine Fault Tolerance):** It is a fault-tolerant technique in the field of distributed computing. Byzantine fault-tolerant comes from the Byzantine Fault problem. The Byzantine Fault problem models the real world, where computers and networks can behave unpredictably due to hardware errors, network congestion or disruption, and malevolence. Byzantine fault-tolerant techniques are designed to handle real-world abnormal behavior and meet the specification requirements of the problems to be addressed.

<span id="f2">[[2]](#a2)</span> **VRF (Verifiable Random Function):** Verifiable random functions: It is an encryption scheme that maps the input to a verifiable pseudo-random output. The program was proposed by Micali (the founder of Algorand), Rabin and Vadhan in 1999. To date, VRF has been widely used in various encryption scenarios, protocols and systems.