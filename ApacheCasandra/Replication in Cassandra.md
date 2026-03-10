# Replication in Cassandra

Data is replicated across multiple nodes and, optionally, across multiple data centers to ensure durability and availability. The **replication factor** (RF) defines the number of copies of a piece of data across the cluster. The `NetworkTopologyStrategy` is recommended for production as it is datacenter- and rack-aware.

![image.png](images/image%202.png)

### Replication Strategy

Common ones:

- `SimpleStrategy` (for single DC – lab setup)
- `NetworkTopologyStrategy` (production)

[Building Cassandra Cluster Using Docker](Building%20Cassandra%20Cluster%20Using%20Docker.md)