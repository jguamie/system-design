# Preparing for the Google System Design Interview
Studying for distributed system design interviews are only necessary if you are targeting L5+ at Google.
## Prerequisites
* Go through the [Grokking the System Design Interview](https://www.educative.io/collection/5668639101419520/5649050225344512) online course. 
  * This will get you familiar with the core concepts and the most popular system design questions. These questions are still asked regularly.
  * This course only covers the basics. Regurgitating these answers in an interview will not be enough to pass a Google system design interview.
## Outline for System Design Problems
* Follow this [outline](https://github.com/jguamie/system-design/blob/master/notes/system-design-outline.md) when solving system design questions.
  * Not all the topics in the outline are required. Depending on the problem, some topics are more relevant than others.
  * At Google, engineers write design documentation before we start implementing features. This outline is very similar to how we approach the design process. The system design interview very much emulates how we work at Google. 
## Tips for Success
* When asked to design a system, immediately start thinking about, "What is the Google-scale problem or bottleneck that needs to be solved?" That will always be the end goal of a Google system design interview. For example:
  * When you're asked to design Ticketmaster, don't waste your time designing the frontend. Most of your design focus should be on, "How will my system handle the massive spike in requests at the opening minutes to an extremely popular show?"
  * When you're asked to design a leaderboard, focus on, "How will I update someone's ranking amongst 10 million+ players? On top of that, how do I keep up with updating rankings for 500,000+ players that are actively playing right now?"  
* Throughout the discussion, always mention trade-offs. Discuss alternative approaches and the pros and cons of each approach. Next, explain why one approach is ideal for the given problem. Even if you believe certain alternatives are obviously bad approaches, at least mention it and explain why.
* Interviewers will expect you to drive the discussion. You shouldn't be waiting for the interviewer to ask you questions. At Google, we come up with an end-to-end solution (as I mentioned, covering a lot of the topics in [this outline](https://github.com/jguamie/system-design/blob/master/notes/system-design-outline.md)) and present it to other engineers during a design review for discussion and approval. If you feel you're at a fork in the road, you can always ask, "Would you like me to dig into this aspect further or would you like me to move on?" 
## Practice
### Mock Interviews
Practicing with mock interviews is ***mandatory***. I cannot stress this enough. You have to build the habits necessary to tell a complete story without missing the key talking points. You will not be able to attain that without regular practice.

Mock interview resources:
* [Technical Mock Interview](https://www.techmockinterview.com/)
  * This is a paid service with high quality interviewers from Google, Facebook, and Amazon with detailed verbal/written feedback and personalized coaching. I strongly recommend doing at least 7 mocks through here.
* Friends and colleagues
  * Find other engineers to practice with. Start an interview club with your friends. Meet weekly to go through readings and videos together.
* Mentor
  * Find a mentor. Reach out to other software engineers you respect. Ask them to meet with you regularly for mentoring sessions.
## Recommended Order of Reading
1. [Building Software Systems At Google and Lessons Learned - Jeff Dean Video](https://youtu.be/modXC5IWTJI)
1. [What happens when you type google.com into your browser?](https://github.com/alex/what-happens-when)
1. [Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321), Chapters 1-2: Reliable, Scalable, and Maintainable Applications; Data Models and Query Languages
	* [Twitter - My Notes](https://github.com/jguamie/system-design/blob/master/notes/twitter.md)
1. [System Design Numbers - My Notes](https://github.com/jguamie/system-design/blob/master/notes/numbers.md)
1. [Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321), Chapter 3: Storage and Retrieval
1. [Caching - My Notes](https://github.com/jguamie/system-design/blob/master/notes/caching.md)
1. [Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321), Chapter 4: Encoding and Evolution
1. [Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321), Chapter 5: Replication
	* [Replication - My Notes](https://github.com/jguamie/system-design/blob/master/notes/replication.md)
1. [Raft: Understandable Distributed Consensus](http://thesecretlivesofdata.com/raft/) 
	* [Raft Distributed Consensus - My Notes](https://github.com/jguamie/system-design/blob/master/notes/raft-distributed-consensus.md)
1. [Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321), Chapter 6: Partitioning
	* [Partitioning - My Notes](https://github.com/jguamie/system-design/blob/master/notes/partitioning.md)
1. [Consistent Hashing - Wikipedia](https://en.wikipedia.org/wiki/Consistent_hashing)
1. [Rendezvous Hashing - Wikipedia](https://en.wikipedia.org/wiki/Rendezvous_hashing)
1. [Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321), Chapter 7: Transactions
	* [Transactions - My Notes](https://github.com/jguamie/system-design/blob/master/notes/transactions.md)
1. [The Google File System](https://ai.google/research/pubs/pub51)
	* [GFS - My Notes](https://github.com/jguamie/system-design/blob/master/notes/google-file-system.md)
1. [Chubby: A Lock Service for Loosely-Coupled Distributed Systems](https://ai.google/research/pubs/pub27897)
	* [Chubby - My Notes](https://github.com/jguamie/system-design/blob/master/notes/chubby-lock-service.md)
1. [Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321), Chapter 10: Batch Processing
1. [MapReduce: Simplified Data Processing on Large Clusters](http://research.google.com/archive/mapreduce.html)
	* [MapReduce - My Notes](https://github.com/jguamie/system-design/blob/master/notes/map-reduce.md)
1. [Bigtable: A Distributed Storage System for Structured Data](http://research.google.com/archive/bigtable.html)
	* [Bigtable - My Notes](https://github.com/jguamie/system-design/blob/master/notes/bigtable.md)
1. [Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321), Chapter 11: Stream Processing
	* [Event Sourcing and Stream Processing - Martin Kleppmann Video](https://youtu.be/avi-TZI9t2I)
	* [Materialized Views with Apache Samza - Martin Kleppmann Video](https://youtu.be/fU9hR3kiOK0) 
1. [Google: the Anatomy of a Large-Scale Hypertextual Web Search Engine](http://infolab.stanford.edu/~backrub/google.html)
	* [Google - My Notes](https://github.com/jguamie/system-design/blob/master/notes/google-search-engine.md)
## Additional Reading
* Read through all the [additional system design interview questions and real world architectures](https://github.com/donnemartin/system-design-primer#additional-system-design-interview-questions) in [System Design Primer](https://github.com/donnemartin/system-design-primer).
# Resources
* [Designing Data-Intensive Applications: The Big Ideas Behind Reliable, Scalable, and Maintainable Systems - Martin Kleppmann](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321) Chapters 1-7, 10-11
* [The System Design Primer - Donne Martin](https://github.com/donnemartin/system-design-primer)
* [Grokking the System Design Interview - educative.io](https://www.educative.io/collection/5668639101419520/5649050225344512)
* [TechMockInterview](https://www.techmockinterview.com/)
# TODO
* [Spanner: Google's Globally-Distributed Database](https://ai.google/research/pubs/pub39966)
