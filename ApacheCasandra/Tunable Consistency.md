# Tunable Consistency

Cassandra allows developers to choose the consistency level (CL) for each read and write operation, balancing between data consistency and system availability (CAP theorem). For example, a CL of `QUORUM` (a majority of replicas) can ensure strong consistency for an operation. 

Assume:

- **Replication Factor (RF) = 3**
- Data is stored on **3 different nodes**

```
NodeA
NodeB
Node C
```

Every write is copied to **all 3 nodes** (eventually).

### Write Consistency Levels

CL = ONE

- Cassandra waits for **1 node** to confirm
- Fast
- But other replicas may be outdated

```
Client → NodeA ✔️
(ignoreB, C)
```

CL = QUORUM

For RF = 3:

```
QUORUM = (RF /2) +1 =2
```

So Cassandra waits for **2 nodes**.

```
Client → NodeA ✔️
        → NodeB ✔️
        → Node C ❌ (can be down)
```

 Operation succeeds

 Majority agrees on data

CL = ALL

- Cassandra waits for **all 3 nodes**
- Strongest consistency
- Least availability

```
A ✔️B ✔️ C ✔️
```

If **any node is down** → operation fails