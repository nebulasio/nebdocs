# Appendix


* [Appendix A. Recommended Hardware Configuration for Node Operation](#appendix-a-recommended-hardware-configuration-for-node-operation)
* [Appendix B. Node Multi-User Participation](#appendix-b-node-multi-user-participation)
* [Appendix C. Earnings Simulation](#appendix-c-earnings-simulation)
* [Appendix D. Parameter Table](#appendix-d-parameter-table)
* [Appendix E. Addresses](#appendix-e-addresses)
* [Appendix F. Changelog](#appendix-f-changelog)

## Appendix A. Recommended Hardware Configuration for Node Operation

Monthly recommended configuration server expenditure is approximately: $150 USDT/month



*   CPU：>=4-Core minimum (Recommended 8-Core)
*   RAM：>=16G
*   Disk: 	>= 600G SSD
*   NTP: NTP service is required on the server to ensure correct time synchronization for all operational nodes.

**Node Installation Tutorial** - review the [Nebulas Technical Documentation: Nebulas 101 - 01 Compile Installation](../go-nebulas/tutorials/01-installation).


It’s recommended to build and deploy nodes via docker:



*   Install [docker](https://docs.docker.com/get-started/) and [docker-compose](https://docs.docker.com/compose/install/)
*   Execute the following docker command via [root](https://github.com/nebulasio/go-nebulas)

```
sudo docker-compose build

sudo docker-compose up -d

```



## Appendix B. Node Multi-User Participation

Nodes can be operated by an individual, business entity or even a group of individuals acting as a single entity. **The distribution of node incentives is determined by the primary node operator.**

Supporting a node participant is the autonomous ideology of the community members and those who choose to support node operators should only make this decision after fully examining the operation of the node. PoD can only guarantee the pledging and withdrawal of any pledged NAX to a node and is not responsible for the commitment of the node operator to their supporters.

In order to facilitate the participation of community users, the Nebulas Foundation will form a demonstration multi-user participation node. This node will be operated and maintained by the Nebulas Foundation. All community members can support this node by pledging NAX to its existence and the benefits (minus the basic cost of server operation) of the node will be equally distributed to those who are involved in its co-construction based on NAX pledge quantity.


## Appendix C. Earnings Simulation

Assuming that a node is among the 51 candidate nodes every day for a month, the maximum consensus incentive for the month is approximately 9,920 NAS. 

There are over 700 polling cycles per month and considering the existence of random factors in the selection algorithm, the average revenue per node is expected to be about 3,307 NAS. This however can vary greatly depending on multiple factors as detailed in this paper.

Assuming that all 51 governance nodes selected each month participate, the nodes incentive is estimated to be 816 NAS per month per node.


## Appendix D. Parameter Table


### D.1 Basic Parameters



*   Average block time: 15 seconds
*   Polling cycle: 210 block height, approx. 52.5 minutes
*   Governance cycle: 820 polling cycles (approximately 1 month)
*   Consensus nodes: 21
*   Candidate nodes: 51 (with 21 consensus nodes)
*   Governance nodes: 51 (consensus nodes who generated the largest number of valid blocks per governance cycle)
*   Deposit: 20,000 NAS
*   Candidate node minimum pledge (vote): 100,000 NAX
*   NAS Pledge return time (Once the exit request has been issued): 820 polling cycles (approximately 1 month)
*   NAX return time (Once the withdrawal request or the exit request has been issued): 120 polling cycles (approximately 5 days)


### D.2 Parameters Related to the Consensus Mechanism



*   Number of blocks generated per node within each polling cycle: 10
*   Initial Value of Block Generation Stability Index S<sub>(i)</sub>: 0.8
*   Max Value of Block Generation Stability Index S<sub>(i)</sub>: 1
*   Trigger threshold for penalty mechanism via Block Generation Stability Index S<sub>(i)</sub>: 0.5
*   Penalty (Medium): 5% of NAS deposit
*   Penalty (Severe): All NAS deposit plus all NAX pledged to that node
*   Consensus mechanism penalty duration (low and medium security level): Candidate node cannot be selected for 20 polling cycles (approximately 1 day)
*   Consensus mechanism penalty duration (severe security level): Permanent


### D.3 Governance Mechanism Parameters



*   Governance node voting time: 120 polling cycles (approximately 5 days)
*   Governance node minimum participation: 26
*   Required proposal approval rate: Greater than 50%
*   Required approval rate for the project establishment voting, project acceptance voting, and penalties for consensus mechanism voting: Greater than 67%
*   Governance penalty trigger: Not participating in ALL voting proposals (at minimum level) for two governance cycles constantly
*   Governance penalty duration: 820 polling cycles (approximately 1 month)


### D.4 Incentive Allocation Parameters



*   Daily bookkeeping Income (entire network): 8,219.1744 NAS
*   Annual bookkeeping income (entire network): 2,999,941 NAS
*   Total annual consensus mechanism incentives (entire network): 2,499,951 NAS
*   Total annual incentives for governance mechanisms (entire network): 499,999 NAS
*   Single project budget: Cannot be greater than $15,000 USDT
*   Maximum amount of funds released per governance cycle: Cannot greater than $30,000 USDT


## Appendix E. Addresses

* **Sign-in address:** This address is the only one that you can sign in to the node platform with. Please use the Nebulas Chrome Extension to sign in and manage your node on this node platform.

* **Minner address:** Only used for creating the block, signature, polling check. The keystore is on your server.

* **Incentive address:** Your consensus incentive will be sent to this address. We recommend a cold storage wallet for security. The incentive address can be modified via the server configuration.

* **Governance address:** If your node is selected as a governance node, your vote for proposals and projects will be via this address. In addition, your governance incentive will be sent to this address. To partake in governance and to vote, we recommend using a hot wallet such as NAS nano Pro.

The default governance address is the same as the sign-in address. For security reasons, it is recommended to use a different address and allocate hot and cold wallets according to our recommendations.


## Appendix F. Changelog

* Nov 20, 2019 - v1.0
* June 2, 2020 - v1.0.1 - PoD Penalty Rule Adjustment ([NP289](https://go.nebulas.io/project/289)).
* June 26, 2020 - v1.0.2 -  Modification to the Stability Index for Nebulas Nodes ([NP294](https://go.nebulas.io/project/294)), Addition of a lucky node in the consensus node selection process([NP296](https://go.nebulas.io/project/296)), Node Governance: Remove "abstain" from calculations ([NP295](https://go.nebulas.io/project/295)).
