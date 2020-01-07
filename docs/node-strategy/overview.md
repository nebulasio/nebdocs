# 1. PoD Overview

**Proof of Devotion (PoD) Mechanism Overview**

* [1.1 Design Objectives](#design-objectives)
* [1.2 Composition](#composition)
* [1.3 Incentive Allocation](#incentive-allocation)
* [1.4 Contribution Measurement: NAX](#contribution-measurement-nax)



## 1.1 Design Objectives

In order to build a sustainable and beneficial public chain, it is necessary to take into account both the speed and irreversibility of the consensus mechanism as well as the fairness of governance.

At present, we face new application scenarios including simple data interactions to complex, multi-level, on-chain functions. This diverse environment is spawning the creation of new user roles as well as significantly increasing the complexity of the system. Communication scenarios have evolved from in person collaboration to collaboration that ecompasess the world. The goal of collaboration has also changed with the end results going from the physical to the virtual world. This results in time spans for collaborative projects becoming longer and more flexible.<sup id="a1">[[1]](#f1)</sup>

To ensure a fair governance system within these new scenarios, a new approach to collaboration is required. Traditional centralized governance cannot cope with these new and complex scenarios that we face daily in our technologically evolving world. In this new world filled with complex data interaction patterns and expanding user roles, centralized single evaluation options are difficult to be adaptable and comprehensive leading to considerable limitations.

Existing decentralized collaboration method do not take into account the new distribution of benefits caused by the existence of expanded user roles. As a result, there is an uneven distribution of benefits leading to slow development and eventually, an unsustainable ecosystem.

We must protect the interests of all community members so that value comes from the depth of Nebulas' ecosystem which in turn follows our core beliefs. Under the premise of ensuring efficiency and irreversibility first, we have designed PoD to pursue fairness from the perspective of contribution and to protect the interests of the community.


## 1.2 Composition

Nebulas’ Proof of Devotion (PoD) can provide a simple overview of mechanisms built on the basis and magnitude of community contributions which include both consensus mechanisms and governance mechanisms. See Figure 1.1.



![](../resources/node/Nebulas-PoD-1-1.png "Figure 1.1 PoD Composition")


*Figure 1.1 PoD Composition*

The composition of the PoD mechanism will involve two executive committees split into consensus and governance. 



*   The consensus mechanism shall be implemented by the Consensus Committee. The consensus committee is selected from all the available nodes via a comprehensive ranking algorithm. 
*   The governance mechanism shall be implemented by the Governance Committee. The governance committee is composed of the most dedicated contributors of the Consensus Committee.



![](../resources/node/Nebulas-PoD-1-2.png "Figure 1.2 PoD Executive Committee")


*Figure 1.2 PoD Executive Committee*


## 1.3 Incentive Allocation

Since the launch of the Nebulas mainnet on March 31, 2018, DPoS<sup id="a2">[[2]](#f2)</sup> has been used as the interim consensus mechanism until the release of PoD. The DPoS consensus mechanism generates 8,219.1744 NAS in revenue per day; generating 2,999,941 NAS per year. 

This collected revenue will be used exclusively for the Nebulas PoD Node Decentralization Strategy. 

The incentive ratio of the two PoD Executive Committee (consensus and governance) will be about 5:1. Which equates to:



*   The total incentive amount for the consensus mechanism per year will be: 2,499,951 NAS
*   The total incentive amount for the governance mechanism per year will be: 499,999 NAS

The incentive for the consensus mechanism will be evenly divided by the consensus nodes who have generated blocks during the active polling cycle (selection) of the consensus committee. Any candidate nodes that have not been selected during the polling cycle (selection) will not receive any incentive during this period.

The incentive for the governance mechanism will be evenly divided by the governance nodes who has participated in all voting proposals during the governance cycle. Any governance nodes that do not participate in **ALL** voting proposals during the governance cycle will not receive any governance incentive during this period.


## 1.4 Contribution Measurement: NAX

The measurement for the weight of contribution to the Nebulas ecosystem is the NAX<sup id="a3">[[3]](#f3)</sup> Smart Asset. As a smart asset, NAX can only be obtained by **decentralized staking (dStaking)**<sup id="a4">[[4]](#f4)</sup> the NAS asset. As per the **_Nebulas NAX White Paper_** ([Github](https://github.com/nebulasio/nax_whitepaper), [PDF](https://nextdao.io/static/docs/nax_whitepaper_en.pdf)), NAX adopts a dynamic distribution model where the daily total issuance quantity is related to the pledge rate of the entire Nebulas ecosystem; the number of NAX obtained by an address is related to the quantity of NAS pledged and the age/duration of the pledge (the longer, the better), which can be considered a measure of the contribution of that address to the community and ecosystem. Therefore, NAX can be considered effective proof of those who contribute to the Nebulas ecosystem.

The [Go Nebulas](https://go.nebulas.io) community collaboration platform will also utilize NAX as an ecosystem contribution incentive to encourage community members to continue to build communities.

***

<span id="f1">[[1]](#a1)</span> [Orange Paper: Nebulas Governance](https://nebulas.io/docs/NebulasOrangepaper.pdf)

<span id="f2">[[2]](#a2)</span> **Delegated Proof of Stake Consensus (DPoS):** Delegates are chosen by stakeholder votes, and delegates then decide on the issue of consensus in a democratic way. This includes  but is not limited to: All network parameters, cost estimates, block intervals, transaction size, etc.

<span id="f3">[[3]](#a3)</span> **NAX:** This smart asset is generated by decentralized pledging and is the first token on nextDAO. Users on the Nebulas blockchain can obtain NAX by pledging NAS. NAX adopts dynamic distribution strategy where the actual issuance quantity is related to the global pledge rate, the amount of NAS pledged individually and the age of the pledge.

<span id="f4">[[4]](#a4)</span> **dStaking Decentralized Pledge:** Unlike traditional pledges (staking) that requires the transfer of assets to smart contracts, decentralized pledges record the user's pledge while the assets remain at the user's personal address.

