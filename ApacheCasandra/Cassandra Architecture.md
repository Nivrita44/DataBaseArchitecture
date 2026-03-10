# Cassandra Architecture

### Peer-to-Peer

- All nodes in a cluster are identical; there are no master or slave nodes. Any node can accept read or write requests and acts as a coordinator for that request, ensuring no single point of failure.

### Gossip Protocol

- Nodes communicate with each other using the Gossip protocol to discover and share information about the cluster's state, node health, and network topology dynamically.

![image.png](images/image%201.png)

### Token Ring & Partitioning

- A partitioner is a hash function that determines how data is distributed across the nodes. It calculates a token from a row's partition key to decide which node is responsible for storing that data's first replica. The Murmur3Partitioner is the default and recommended choice.