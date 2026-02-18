# MongoDB

**MongoDB** is a distributed, **document-oriented NoSQL database** designed for high productivity, horizontal scalability, and resilience. Unlike traditional relational databases that use rigid tables, MongoDB stores data in **flexible, JSON-like documents** .

It stores data in:

- **Documents**
- Inside **Collections**
- Inside **Databases**

Instead of tables and rows, MongoDB uses:

```
Database
   ↓
Collection
   ↓
Document (JSON-like)
```

Example document:

```json
{"_id":"123","name":"Nivrita","age":24,"skills":["MongoDB","PostgreSQL"]}
```

MongoDB stores this in a binary format called **BSON**.

# Differences Between SQL and NoSQL

| **Aspect** | **SQL (Relational)** | **NoSQL (Non-relational)** |
| --- | --- | --- |
| **Data Structure** | Tables with rows and columns | Document-based, key-value, column-family, or graph-based |
| **Schema** | Fixed schema (predefined structure) | Flexible schema (dynamic and adaptable) |
| **Scalability** | Vertically scalable (upgrading hardware) | Horizontally scalable (adding more servers) |
| **Data Integrity** | ACID-compliant (strong consistency) | BASE-compliant (more available, less consistent) |
| **Query Language** | SQL (Structured Query Language) | Varies (e.g., MongoDB uses its own query language) |
| **Performance** | Efficient for complex queries and transactions | Better for large-scale data and fast read/write operations |
| **Use Case** | Best for transactional systems (banking, ERP, etc.) | Ideal for big data, real-time web apps, and data lakes |
| **Examples** | MySQL, PostgreSQL, Oracle, MS SQL Server | MongoDB, Cassandra, CouchDB, Neo4j |

[Architecture](02%20Architecture.md)