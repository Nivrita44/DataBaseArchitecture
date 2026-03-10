# CLIENT LAYER (Top Section)

This is where the request starts.

### 1.Connectors (JDBC, .NET, PHP, Python)

Applications don’t talk directly to MySQL using raw sockets.

They use **connectors/drivers**, such as:

- JDBC (Java)
- .NET connector
- PHP MySQL driver
- Python MySQL connector

### Duties of connectors

- Convert application calls into MySQL protocol
- Handle authentication
- Manage connection pooling
- Send SQL queries to the MySQL server

### 2.Plugin System

MySQL has a pluggable architecture.

Plugins can add:

- Authentication methods
- Storage engines
- Full-text parsers
- Audit plugins

This makes MySQL modular and extensible.