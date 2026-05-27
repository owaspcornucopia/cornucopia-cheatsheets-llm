[← Back to Cheat Sheet](/README.md)

# CK — Denial of Service via SQL Injection

## Threat

An attacker can utilize the application to deny service to some or all of its users.

## How This Applies

The SQL injection vulnerability provides multiple denial-of-service attack paths:

- **Drop the investigations table** — `DROP TABLE investigations` destroys all data and makes the application non-functional
- **Delete all records** — `DELETE FROM investigations` removes all investigation data
- **Corrupt data** — `UPDATE investigations SET fraud_detected='true' WHERE 1=1` marks everything as fraud, making the tool useless
- **Resource exhaustion** — crafted queries (e.g., cartesian joins, recursive CTEs) can consume all available CPU and memory
- **Lock the database** — long-running write transactions can lock the SQLite file, preventing other requests from completing

Beyond SQL injection, the lack of rate limiting also enables denial of service through resource exhaustion at the LLM inference layer (see LLM2).

## Example Attack

An attacker uses prompt injection to generate `DROP TABLE investigations`. The application executes it. All investigation data is permanently lost. Subsequent requests fail because the table no longer exists. The application cannot recover without a restart (which recreates the table with only seed data).

## Mitigations

1. **Use a read-only database connection** — prevent all destructive operations at the database level.
2. **Implement query timeouts** — abort queries that run longer than a defined threshold.
3. **Add database backups** — regular automated backups allow recovery from destructive attacks.
4. **Implement rate limiting** — prevent resource exhaustion through request volume.
5. **Set SQLite busy timeout** and connection limits to prevent database locking attacks.
6. **Validate SQL before execution** — reject queries containing DDL or DML keywords.
7. **Implement health checks** that detect when the application is in a degraded state and alert operators.
