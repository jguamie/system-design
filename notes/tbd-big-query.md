# BigQuery
## This is still Work in Progress
BigQuery is a fully-managed query service for Big Data. It provides an external implementation for Dremel, a query service for analyzing read-only Big Data in seconds. Dremel is able to scan 35 billion rows in tens of seconds, without an index.
# Design
## Columnar Storage
Data is stored as columnar storage. This allows for high compression rato and scan throughput.
## Tree Architecture
A multi-level serving tree is used to dispatch queries across thoursands of machines for parallel processing. Next, the results are aggregated back to the root of the tree. All of this happens in a matter of seconds.
