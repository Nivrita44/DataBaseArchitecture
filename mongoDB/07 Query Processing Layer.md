# 3.Query Processing Layer

This layer is responsible for:

- Parsing
- Planning
- Executing
- Interacting with Storage Engine

## Parse & Canonicalize

**Parse & Canonicalize** refers to the process of converting input data (often in string or Extended JSON format) into a standardized, valid BSON (Binary JSON) format, with an emphasis on preserving specific MongoDB data types. This process ensures that data, especially complex types like ObjectIds or Dates, is interpreted correctly and efficiently

When a query arrives:

```jsx
db.users.find({age: {$gt:20 } })
```

MongoDB:

- Validates syntax
- Converts BSON into internal query tree
- Normalizes query structure
- Removes redundant conditions
- Creates a canonical query shape

## Query Planner

For any given query, the MongoDB query planner chooses and caches the most efficient query plan given the available indexes. To evaluate the efficiency of query plans, the query planner runs all candidate plans during a trial period. In general, the winning plan is the query plan that produces the most results during the trial period while performing the least amount of work.

Multi-Plan Evaluation (often described as "First Past The Post")

Process:

1. Generate candidate plans
2. Execute each briefly
3. Measure performance
4. Select fastest plan
5. Cache winning plan

## Execution Engine

Once best plan is selected:

The Execution Engine:

- Fetches documents
- Applies filters
- Applies projection
- Applies sort / aggregation
- Returns result

Now it interacts with:

WiredTiger

The execution engine requests pages from WiredTiger.