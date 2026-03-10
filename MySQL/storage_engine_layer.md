# STORAGE ENGINE LAYER

The **Storage Engine Layer** is the third and final tier of the MySQL architecture, acting as the foundation for how data is physically handled and stored on the system. While the Server Layer processes the logic and optimization of a query, the Storage Engine is responsible for the actual execution of data storage and retrieval.

### **Primary Role and Functionality**

- **Physical Data Management:** The Storage Engine handles the **physical database operations**, including file management and storing the actual data on disk.
- **Data Organization:** It is the component that interacts with the physical storage area to fetch or write information as requested by the DBMS engine after a query has been optimized.
- **Storage of Metadata and Logs:** This layer stores critical internal information, such as **table metadata, index information, and server logs**.
- **Access Privileges:** While the Client and Server layers handle authentication and security checks, the Storage Engine layer is where information regarding **user access privileges** is stored.

**Popular Storage Engines**

The sources highlight that MySQL supports multiple storage engines, allowing for flexibility based on the needs of the database. The most popular and widely used storage engines mentioned are **InnoDB** and **MyISAM**.

### InnoDB

**InnoDB** — the default storage engine of **MySQL**

InnoDB works around **five major subsystems**:

- Buffer Management
- Logging (Redo + Undo)
- Transaction & MVCC
- Locking & Concurrency Control
- Crash Recovery

All of these together ensure **ACID compliance**.

### Buffer Pool (Memory Layer)

A memory area (RAM) inside InnoDB.

- Caches data pages -

In **InnoDB**, data is NOT stored row-by-row directly on disk.

Instead Data is stored in fixed-size blocks called **pages**.

- Caches index pages
- Reduces disk I/O (Permanent storage - disk)
- Holds modified (“dirty”) pages

Flow:

Disk → Buffer Pool → Modify in memory → Flush later

Important:

- Data is NOT written immediately to disk.
- Write happens later by background threads.

### Redo Log (WAL Mechanism)

Purpose: **Durability**

Mechanism:

- Every change is logged before page is flushed.
- Redo log is sequential write (fast).
- Guarantees committed transactions survive crash.

### Undo Log

Purpose:

- Rollback transactions
- Provide MVCC (consistent reads)

Stores:

- Previous version of rows

Used for:

- ROLLBACK
- SELECT with isolation
- Crash recovery cleanup

### MVCC (Multi-Version Concurrency Control)

This is how InnoDB allows:

- Readers without blocking writers
- Writers without blocking readers

Mechanism:

- Each row has hidden transaction ID
- Undo log keeps old versions
- SELECT reads appropriate version

Result:

High concurrency without heavy locking.

### Doublewrite Buffer

Problem:

If power fails while writing a page → page corruption.

Solution:

- Page written first to doublewrite buffer
- Then written to actual data file

Protects against partial page writes.

### UPDATE Example

```
UPDATE users SET balance=100 WHERE id=1;
```

### a. Page Loaded into Buffer Pool

MySQL needs the page containing `id=1`.

If that page is NOT already in memory:

1.  It reads the 16KB page from disk
2.  Loads it into Buffer Pool (RAM)

### b. Row modified in memory

InnoDB modifies the row inside the page in RAM.

Important:

— It does NOT immediately change disk file

— It changes only the memory copy

Now that page becomes:

**Dirty Page =** Memory version is newer than disk version.

### c. Undo Record Created

Before modifying:

InnoDB saves the old value in Undo Log.

Example:

Before:

```
balance = 50
```

Undo stores:

```
old balance = 50
```

Used for:

- Rollback
- MVCC

### d. Redo Log Written (WAL Rule)

InnoDB writes: “What change happened” Into Redo Log.

Redo log record is generated and written to: **Redo Log Buffer (in memory)**

NOT immediately to disk.

This is fast because it's just RAM.

### e. Commit

When we type:

```
COMMIT;
```

InnoDB:

- Flushes redo log buffer to redo log file on disk
- Confirms transaction committed

Even if data page is not yet written to disk.

### f. Background Thread Flushes Data Page Later

Some time later:

Background thread writes dirty page from:

Buffer Pool (RAM)

↓

Back to Disk (.ibd file)

This is called:

Flushing