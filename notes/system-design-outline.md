# System Design Outline
This is a recommended approach to solving system design problems. Not all of these topics will be relevant for a given problem.
1. Functional Requirements / Clarifications / Assumptions
1. Non-Functional Requirements
	1. Consistency vs Availability
	1. Latency
		1. How fast does this system need to be? 
		1. User-perceived latency
		1. Data processing latency
	1. Security
		1. Potential attacks? How should they be mitigated?
	1. Privacy
		1. PII, PCI, or other user data
	1. Data Integrity
		1. How to recover from data corruption or loss?
	1. Read:Write Ratio
1. APIs
1. Storage Schemas
	1. SQL vs NoSQL
	1. Message Queues
1. System Design
1. Scalability
	1. How does the system scale? Consider both data size and traffic increases.
	1. What are the bottlenecks? How should they be addressed?
	1. What are the edge cases? What could go wrong? Assuming they occur, how should they be addressed?
	1. How will we stress and test this system?
	1. Load Balancing
	1. Auto-scaling / Replication
	1. Caching
	1. Parititioning
	1. Replication
	1. Business Continuity and Disaster Recovery (BCDR)
	1. Internationalization / Localization
		1. How to scale to multiple countries and languages? Don't assume the US is English-only.
