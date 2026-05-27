[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# AZ7 — Access to Unauthorized Functions and Objects

## Threat

An attacker can access application functions, objects, or properties they are not authorized to access.

## How This Applies

Through SQL injection (via LLM prompt manipulation), an authenticated user can perform operations far beyond their intended scope:

- **Access SQLite system tables** — `SELECT * FROM sqlite_master` reveals the complete database schema
- **Read arbitrary tables** — if other tables exist, they can be queried
- **Execute database functions** — SQLite built-in functions can be called
- **Modify data** — INSERT, UPDATE, DELETE operations on any table
- **Drop objects** — destructive operations like DROP TABLE are possible

The application runs the database connection with full privileges. There is no restriction on which SQL operations or database objects an authenticated user can interact with through the LLM.

## Example Attack

An attacker asks: "Check for fraud in the sqlite_master table." The LLM generates `SELECT * FROM sqlite_master` exposing the entire database schema, including any hidden tables. Armed with this information, the attacker can query other tables or modify data.

## Mitigations

1. **Use a database user with minimal privileges** — the connection should only have SELECT permission on the investigations table.
2. **Block access to system tables** — validate that queries do not reference sqlite_master or other system objects.
3. **Restrict available operations** — only allow SELECT statements; reject all DDL and DML commands.
4. **Use a read-only database connection** (SQLite supports `?mode=ro` in the connection string).
5. **Implement a SQL allowlist** that restricts which tables and columns can appear in queries.
