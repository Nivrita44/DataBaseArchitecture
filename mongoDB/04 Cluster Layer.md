# 2.Cluster Layer

In [**MongoDB**](https://www.mongodb.com/), a **cluster** is **a group of interconnected servers or nodes that work together to store and manage data, providing high availability and/or horizontal scalability**

MongoDB can run in 3 modes:

## **1.Standalone**

****A **standalone MongoDB instance** consists of a single `mongod` process that handles all read and write operations and stores all data. This is the simplest deployment architecture, suitable for development and testing environments, but it is **not recommended for production** due to a lack of high availability and redundancy.
**Application → mongod → Disk**

[**2.Replica Set**](05%20Replica%20Set.md)

[3.Sharded Cluster Architecture](06%20Sharded%20Cluster%20Architecture.md)