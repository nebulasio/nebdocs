# 【Meeting minutes】 Nebulas Nova Tech Tradeoffs(11.21.2018)

## Summary
1. The process to submit IR (LLVM Intermediate Representation) and who can submit IR  (LLVM Intermediate Representation)
2. The time window for NR & DIP
3. How much NAS for DIP & how to distribute NAS for DIP

## Detailed minutes：
### 1. The process to submit IR and who can submit IR 
1. Nebulas Nova will use an auth_table to decide whose IR can be executed and the lifetime of each IR.
2. auth_table is a set of tuples, and each tuple includes IR name, submitter’s address, the valid start and end height for the submitter.
3. Only the auth_admin’s auth_table can update in Nebulas Nova. The auth_admin account should be created by a cold wallet. Each IR should be managed by different accounts. Nebulas Technical Committee will further discuss the community governance details with the community. Before the we finalized the governance details , the Nebulas team will not recklessly open the IR submission access.The NBRE only executes several predefined IRs, like checking the auth_table, and the IRs defined in auth_table. Other IRs will not be executed
4. However, each node may change the code. And that could be the auth_admin account, and the auth_table. Consequently, it may change the behaviors in NBRE, and the node shall fail to sync data with the main-net

### 2. The time window for NR &DIP
1. In the yellow paper introducing Nebulas Rank, we have mentioned that to avoid the affect of loop attack, we will remove the forwarding loop before we calculate the In-and-Out degree for the transaction graph, thus the time-window is important for anti-manipulation.
2. If the time-window is too short, there may be more cheating.
3. For now, we suggest the time window in several days.
4. We should monitor the cheating status, and adjust the time-window if necessary.
5. time window for DIP should be much more larger than the time window of NR, for now, we suggests 7 days

### 3. How much NAS for DIP & how to distribute NAS for DIP
1. For each month, we suggest around 500 NAS in total, the winners shall be relatively stable, so a winner will get reward in several months.
2. We will have a special account for distributing NAS for DIP. The account can only send special transactions for DIP.

***About Nebulas Technical Committee***
*The Nebulas Technical Committee adheres to the spirit of openness, sharing, and transparency, and is committed to promoting the decentralization, and the community of the research and development of the Nebulas technology. Blockchain technology opens up possibilities for building new and self-motivated open source communities. Nebulas’ technical concepts unclude mechanisms for evaluation, self-evolution, and self-incentives, which provide a guarantee for building a world of decentralized collaboration. The Nebulas Technical Committee will fully promote the realization of the Nebulas vision.
Subscribe to nebulas mailing list and learn the latest progress of Nebulas: [mailing list](lists.nebulas.io/cgi-bin/mailman/listinfo)
For more info, please visit [Nebulas official website](nebulas.io).*

