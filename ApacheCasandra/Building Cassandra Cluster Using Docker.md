# Building Cassandra Cluster Using Docker

### Step 1: Create Docker Network

```bash
docker network create cassandra-net
```

Why?

- Containers need stable internal IPs
- Cassandra nodes must discover each other

### Step 2: Start First Node (Seed Node)

```bash
docker run -d \
  --name cassandra-1 \
  --hostname cassandra-1 \
  --network cassandra-net \
  -p 9042:9042 \
  --memory="512m" \
  --cpus="0.5" \
  -e CASSANDRA_CLUSTER_NAME=TestCluster \
  -e CASSANDRA_SEEDS=cassandra-1 \
  -e CASSANDRA_DC=dc1 \
  -e CASSANDRA_RACK=rack1 \
  -e CASSANDRA_ENDPOINT_SNITCH=GossipingPropertyFileSnitch \
  -e MAX_HEAP_SIZE=256M \
  -e HEAP_NEWSIZE=64M \
  cassandra:4.1
```

🔑 **Seed node**

- Helps new nodes discover the cluster
- Not a master

### Step 3: Start Additional Nodes

```bash
docker run -d \
  --name cassandra-2 \
  --hostname cassandra-2 \
  --network cassandra-net \
  -p 9043:9042 \
  --memory="512m" \
  --cpus="0.5" \
  -e CASSANDRA_CLUSTER_NAME=TestCluster \
  -e CASSANDRA_SEEDS=cassandra-1 \
  -e CASSANDRA_DC=dc1 \
  -e CASSANDRA_RACK=rack1 \
  -e CASSANDRA_ENDPOINT_SNITCH=GossipingPropertyFileSnitch \
  -e MAX_HEAP_SIZE=256M \
  -e HEAP_NEWSIZE=64M \
  cassandra:4.1
```

```bash
docker run -d \
  --name cassandra-3 \
  --hostname cassandra-3 \
  --network cassandra-net \
  -p 9044:9042 \
  --memory="512m" \
  --cpus="0.5" \
  -e CASSANDRA_CLUSTER_NAME=TestCluster \
  -e CASSANDRA_SEEDS=cassandra-1 \
  -e CASSANDRA_DC=dc1 \
  -e CASSANDRA_RACK=rack1 \
  -e CASSANDRA_ENDPOINT_SNITCH=GossipingPropertyFileSnitch \
  -e MAX_HEAP_SIZE=256M \
  -e HEAP_NEWSIZE=64M \
  cassandra:4.1
```

### Step 4: Check Cluster Status

```bash
dockerexec -it cassandra-1 nodetool status
```

Output explanation:

```
UN = Up & NormalAddress = Node IPLoad = Data storedon nodeTokens = Data rangesOwns = Percentage of token ring
```

## Creating Keyspace (Replication Setup)

### Connect to CQL Shell

```bash
dockerexec -it cassandra-1 cqlsh
```

### Create Keyspace

```sql
CREATE KEYSPACE demoWITH replication= {'class':'SimpleStrategy','replication_factor':3
};
```

 Meaning:

- Data will be copied to **3 nodes**
- Works perfectly in your 3-node Docker cluster

### Verify Keyspace

```sql
DESCRIBE KEYSPACE demo;
```

## Creating Table

```sql
USE demo;CREATE TABLE users (
  id UUID PRIMARY KEY,
  name text,
  email text
);
```

### Insert Data

```sql
INSERT INTO users (id, name, email)VALUES (uuid(),'Alice','alice@test.com');
```

## Restarting a Stopped Cassandra Node

If you stopped a node:

```bash
docker stop cassandra-1
```

Restart it using:

```bash
docker start cassandra-1
```

Check status again:

```bash
dockerexec -it cassandra-1 nodetool status
```

### Check replica placement:

```bash
dockerexec -it cassandra-3 nodetool getendpoints demousers <partition_key>
```

This shows **exactly which nodes store that row**.

## Important Cassandra Commands

### 🔍 Cluster Status

```bash
nodetool status
```

### 🔧 Repair (sync data)

```bash
nodetool repair
```

### 📊 Table Size

```bash
nodetool cfstats demo.users
```