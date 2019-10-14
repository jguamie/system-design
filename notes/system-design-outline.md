# System Design Outline
This is a recommended approach to solving system design problems. Throughout the discussion, always mention trade-offs. 

1. Functional Requirements / Clarifications / Assumptions
1. Non-Functional Requirements
	1. Consistency vs Availability
	1. Latency
		1. How fast does this system need to be? 
		1. User-perceived latency
		1. Data processing latency
	1. Security / Privacy
		1. PII, PCI, or other user data
	1. Data Durability
	1. Read:Write Ratio
1. APIs
1. Storage Schemas
	1. SQL vs NoSQL
	1. Message Queues
1. Simple System Design
1. Scaled System Design
	1. What are the bottlenecks? How should they be addressed?
	1. What are the edge cases? What could go wrong? Assuming they occur, how should they be addressed?
	1. Load Balancing
	1. Auto-scaling / replication
	1. Caching
	1. Parititioning
	1. Replication
	1. Business Continuity and Disaster Recovery (BCDR)
	1. Regionalization / Localization
 
