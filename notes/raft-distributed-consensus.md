# Raft Distributed Consensus
Node can be in three states: Follower, Candidate, or Leader.
## Leader
All change requests go to the *Leader*.
## Log Replication Summary
  1. Change is added as an entry to node's log.
    * Entry is not committed yet.
  1. Entry is replicated to all *Followers*.
  1. *Followers* confirm with *Leader* on writing the entry.
  1. *Leader* commits entry and applies the state change.
  1. *Leader* notifies *Followers* that entry is committed.
  1. Cluster is in consensus on state.
## Leader Election Summary
  1. Nodes all start in the *Followers* state
  1. One of the nodes becomes a *Candidate*. 
  1. The *Candidate* will request votes from the other nodes.
  1. Nodes reply with their vote.
  1. If *Candidate* gets votes from a majority of notes, it becomes the *Leader*.
## Election Timeout
  1. Each node has a randomized election timeout between 150ms - 300ms.
  1. At the end of timeout, *Follower* become a *Candidate*.
  1. This starts an Election Term.
## Election Term
  1. *Candidate* votes for itself.
  1. It sends out Request Vote messages to other nodes for the specific Term ID.
  1. If nodes haven't voted yet for a given Term ID, it will reply to the *Candidate* with a vote and reset its election timeout.
  1. Once *Candidate* has a majority of the votes, it becomes the *Leader*.
  1. *Leader* will start sending out Append Entries messages in intervals set by the heartbeat timeout.
    * I believe this is called broadcast time? Ranges from 0.5ms - 20ms.
  1. *Followers* respond to each Append Entries message and resets its election timeout.
  1. Election Term will continue until a *Follower* stops receiving heartbeats (Append Entries messages).
    * *Followers* becomes a *Candidate* if it hasn't received a heartbeat by the time its election timeout runs out.
## Split Vote Issue
  1. If votes are split evenly, election timeout causes another node to become a *Candidate*.
  1. It sends out Request Vote message to other nodes for a new Term ID.
## Log Replication
  1. *Leader* sends out changes via the Append Entries messages on the next heartbeat.
  1. *Followers* acknowledge the change.
  1. *Leader* commits the entry when majority of *Followers* acknowledges the change.
  1. Response is sent to the Client.
## Network Partition Issue
  1. Nodes are divided to be in separate network partitions.
  1. A *Leader* will exist in each of the partitions.
  1. Clients send Change Requests to both *Leader*.
  1. The *Leader* that cannot replicate to a majority will not be able to commit its log entry.
    * Only one of the *Leader* will be able to replicate to majority. 
  1. Network partition becomes healed.
  1. One *Leader* will have a higher Term ID than another *Leader*.
    * The *Leader* that was in the partition with the majority of nodes will have the higher Term ID.
  1. *Leader* with the lower Term ID will step down to a *Follower*.
  1. Nodes in the minority partition will roll back uncommitted log entries and match the new *Leader*'s log.
  1. The logs are now consistent across the cluster.
# References
1. "Raft: Understandable Distributed Consensus." The Secret Lives of Data. Accessed November 04, 2018. http://thesecretlivesofdata.com/raft/.
