[← Back to Cheat Sheet](/README.md)

# VEK — SQL Injection via LLM-Generated Queries

## Threat

An attacker can inject malicious SQL into the database because user-controlled input influences SQL queries that are executed without parameterization.

## How This Applies

This is the most critical vulnerability in the application. The attack chain works as follows:

1. A user submits a question to `/api/fraud`
2. The LLM generates a SQL query based on that question
3. The `investigation_fraud()` function executes that SQL directly against the SQLite database using `conn.execute(query)`

There is no parameterization, no query allowlisting, and no restriction on what SQL operations can be performed. The LLM can be manipulated (via prompt injection) to generate any SQL statement — including `DROP TABLE`, `INSERT`, `UPDATE`, or queries against other tables.

Even without prompt injection, the model might generate malformed or dangerous SQL on its own due to hallucination or adversarial fine-tuning.

## Example Attack

An attacker submits: "Investigate the transaction from 'Wheezy Joe'; DROP TABLE investigations;--"

The LLM incorporates the attacker's input into the SQL query. The `investigation_fraud()` function executes it without any validation, deleting the entire investigations table.

## Mitigations

1. **Never execute LLM-generated SQL directly.** Treat all model output as untrusted user input.
2. **Use parameterized queries** — extract only the filter values from the LLM output and plug them into a pre-defined parameterized query template.
3. **Restrict operations to SELECT only** — validate that the generated query begins with `SELECT` and does not contain dangerous keywords (`DROP`, `DELETE`, `INSERT`, `UPDATE`, `ALTER`, `CREATE`).
4. **Use a read-only database connection** for the investigation tool, so even if injection succeeds, destructive operations fail at the database level.
5. **Allowlist the tables and columns** that can be queried. Reject any SQL referencing tables or columns outside the `investigations` table.
6. **Add a query validator** between the LLM output and the database execution that parses the SQL AST and rejects anything that doesn't match expected patterns.
