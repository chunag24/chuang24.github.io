---
title: Week 4 Summary on RAFT
subtitle:  

# Summary for listings and search engines
summary:  
# Link this post with a project
projects: []

# Date published
date: '2024-01-09T00:00:00Z'

# Date updated
lastmod: '2024-01-09T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.infoworld.com%2Farticle%2F3353419%2Fwhats-new-in-c-plus-plus-20-modules-concepts-and-coroutines.html&psig=AOvVaw2prJcUqHmpVgKux1hWeqzl&ust=1705000798871000&source=images&cd=vfe&opi=89978449&ved=0CBMQjRxqFwoTCOjWvozF04MDFQAAAAAdAAAAABAI)'
  focal_point: ''
  placement: 2
  preview_only: false

authors:
  - admin
 
tags:
  - Academic
 

  
---

- 简练的语言概述领导者选举的机制
	- The Raft protocol is a distributed consensus algorithm used to manage log replication in distributed systems to achieve consistency. Its design goal is simplicity, and it breaks down the entire consensus problem into several key components: leader election, log replication, safety, and membership changes. 

- Raft协议中有哪些角色？分别负责什么任务？ 
    - There are three roles: Leader, Follower, and Candidate.
	- Leader: Responsible for handling  all client iteractions, replicating log entries, and sending heartbeats to Followers to maintain its leadership. 
	- Follower: Passively responds to requests from the leader. They vote in elections for a new Leader and replicate Log entries as directed by the Leader. 
	- Candidate: Becomes a Candidate and initiates a new election when a Follower doesnt hear from a current leader within a certain timeout. 
- 在Raft协议中，什么是Leader选举？Leader选举失败怎么办？
	- Leader election is one of the core mechanisms of the Raft protocol. When a cluster starts or when the Leader fails, a Follower becomes a Candidate and starts an election. A Candidate requests votes from other servers. If a Candidate receives a majority of the votes, it becomes the new Leader. If the election fails(due to split votes), a new election round starts. 
- 在Raft协议中，Follower、Candidate和Leader分别是如何转换状态的？
	- State Transitions 
		- From Follower to Candidate: A follower becomes a Candidate and starts a new election if it doesnt receive heartbeats from the Leader within the election timeout.
		- From Candidate to Leader: A Candidate becoems the Leader if it receives votes from a majority of the servers. 
		- From Candidate back to Follower: If a new or existing Leader is discovered during the election, or if their item is outdated. 

- Raft协议中的Term是什么？如何使用Term来保证一致性？
	- Term and Consistency
		- Term is a logical clock concept in the Raft protocol used to distinguish different leadership terms. Each election increases the term value. The term mechanism helps avoid disruptions from stale Leaders. Only Leaders from the current term are considered legitimate, ensuring consistency. 
- 在Raft协议中，如何避免过多的Leader选举导致性能下降？
	- To avoid performance impacts due to frequent leader elections, the Raft protocol uses randomized election timeouts to reduce the likelihood of multiple followers becoming Candidates simultaneously. 
- Raft协议中的心跳机制是什么？它有什么作用？
	- Heartbeat Mechanism
		- The hearbeat mechanism is used by the leader to maintain authority and prevent Followers from timing out and becoming Candidates. By regularly sending heartbeat messages(empty log entries), the Leader indicates its ongoing activity. 
- Raft协议中的Quorum是什么？它有什么作用？
	- Quorum is the minimum number of votes required to make decisions(like electing a Leader of committing log entries) in the Raft protocol. Typically, this is a majority of the cluster size, ensuring decision consistency and fault tolerance. 
- 简练的语言概述日志复制的机制
	- Log Replication Mechanism
		- The leader receives requests from clients, appends the requests as log entries to its local log, then replicates these entries to the Followers Logs in parallel. Once a log entry is replicated on a majority of the servers, it is committed. 	
- Raft协议中的日志压缩（log compaction）是什么？有什么作用？
	- Log compaction: Log compaction is achieved by creating snapshots, reducing the size and maintenance overhead of logs. When the log gets too large, the system generates a state snapshot, retaining only log entries after this snapshot. 
- Raft协议如何处理日志复制的问题？如何处理日志冲突？
	- Raft handles replication and conficts by enforcing log entry matching, ensuring all replicated logs are sequentially consistent across all servers. If conflicts are detected, the Leader forces Followers to duplicate its own log. 
- 在Raft协议中，如何处理节点动态加入和删除的情况？
	- Raft can handle dynamci additiosn and deletions of members through "joint concensus", where new and old configurations coexist for some time until the new configuration can be safely committed.
- Raft协议如何处理客户端请求？Leader如何将请求分发给其他节点？
	- The leader handles all requests from clients. If a client connects to a Follower, the Follower redirects te request to the Leader. The Leader then replicates the request as a log entry to the Followers, applying the request after it has been written on a majority of nodes. 

- Raft协议有哪些局限性？有哪些场景下不适合使用Raft协议？
	 - The Raft protocol may have limitations in handling network partitions and large-scale clusters, it assumes that a majority of nodes are reliable; the system will not be able to process new requests if a network partition leaves an insufficient number of nodes available. 
- Raft协议的优点和缺点是什么？它与其他分布式一致性协议相比有哪些优劣之处？
	- A primary advantage of RAFT is its simplicity and ease of understanding. Compared to other protocols like Paxos, Raft is easier to implement and understand. However it may not be suitable for hihgly dynamic network conditions. 

- Raft协议如何防止脑裂（split brain）的问题？
	- Raft prevent split-brain issues by ensuring that a Leader can only be elected with votes from a majority of nodes, guaranteeing that there is only one active Leader even in cases of network partition. 
- Raft协议如何处理节点宕机的情况？如何保证数据一致性不受影响	？
	- Raft ensures data consistency despite node failures by replicating log entries across multiple servers. As long as a majority of nodes are available, the system can continue operating. 
- Raft协议中有哪些重要的安全性质？如何保证这些安全性质？
	- Raft protocol ensures strong consistency through term mechanisms and log matching principles. Each change requires the consent of a majority of nodes, ensuring data consistency even in cases of failure of network partitions. 