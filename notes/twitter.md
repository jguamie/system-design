# Twitter
## Home Timelines
This problem focuses on the ratio of reads to writes for various scenarios. Primary scaling challenge is due to fan-out--each user follows many people, and each user is followed by many people.
### Main Operations
#### Post Tweet
A user can publish a new message to their followers (4.6k RPS on average, 12k RPS at peak).
#### Home Timeline
A user can view tweets posted by people they follow (300k RPS). In a relational database, the query could be:
```
SELECT tweets.*, users.* FROM tweets
  JOIN users   ON tweets.sender_id    = users.id
  JOIN follows ON follows.followee_id = users.id
  WHERE follows.follower_id = current_user
```
### Options
#### Fan Out on Load
New tweets are inserted into a global relational database. When a user reads their home timeline, request for new tweets for users they follow from the database.

The problem with this option is that the system will struggle to keep up with high load of home timeline queries.
#### Fan Out on Write
Each home timeline is maintained by a cache. Whenever a user posts a tweet, insert the new tweet into followers' home timeline caches.

The problem with this option is that when a user has over 30 million followers, a single tweet results in over 30 million writes. System is not able to keep up with high write throughput in a short amount of time.
### Solution
Use a hybrid of the two options. Users with a large number of followers (celebrities) will post tweets to the global relational database. Users with a small number of followers will post to followers' home timeline caches. When a user reads their home timeline, merge new tweets from the global database into their timeline cache.
# References
1. [Chapter 1, Describing Load - Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321)
1. [Designing Facebook’s Newsfeed - Grokking the System Design Interview](https://www.educative.io/collection/page/5668639101419520/5649050225344512/5641332169113600)
