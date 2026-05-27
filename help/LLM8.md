[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# LLM8 — Insecure Tool Design Enabling Unauthorized Data Access

## Threat

An attacker can abuse insecure plugin or tool designs to access sensitive data, bypass authentication, or execute unauthorized operations through the LLM's interface.

## How This Applies

The `investigation_fraud()` function acts as a "tool" that the LLM invokes. Its design has fundamental security flaws:

- **No input validation on the SQL query** — the tool executes whatever SQL string it receives
- **No operation restrictions** — the tool can run `SELECT`, `INSERT`, `UPDATE`, `DELETE`, or `DROP` statements
- **Authentication check is inside the tool** rather than at the API boundary (see ATJ)
- **No authorization scoping** — the tool operates with full database access regardless of who is calling
- **No output filtering** — all query results (including personal data) are returned in full

The tool trusts that the LLM will always generate safe, well-formed SELECT queries. But the LLM can be manipulated through prompt injection to generate any SQL command, and the tool will execute it.

## Example Attack

Through prompt injection, an attacker causes the LLM to generate: `INSERT INTO investigations VALUES ('attacker-id', 'COMPLETED', 'false', 'Target Name', ...)`. The tool executes this, allowing the attacker to insert fraudulent records that could clear suspicious transactions.

## Mitigations

1. **Restrict the tool to read-only operations.** Open the database connection in read-only mode.
2. **Use a SQL allowlist** — only permit queries matching pre-approved patterns.
3. **Validate tool inputs** before execution. Parse the SQL and reject anything that is not a simple SELECT on the allowed table.
4. **Move authentication and authorization outside the tool** — verify identity and permissions before the tool is invoked.
5. **Apply the principle of least privilege** — the database user/connection used by the tool should only have SELECT permissions on the investigations table.
6. **Filter tool outputs** — redact sensitive fields (dates of birth, full addresses) unless the caller specifically needs them.
