[← Back to Cheat Sheet](/README.md)

# LLMK — Excessive Agency: SQL Execution Without Human Approval

## Threat

An attacker can exploit excessive agency in the AI system to perform unauthorized or high-risk actions because no human-in-the-loop approval is required.

## How This Applies

The LLM in this application has direct, unsupervised access to execute SQL queries against the database. The complete chain is automated:

1. User asks a question
2. LLM generates a SQL query
3. The application executes that query immediately — no human reviews it first
4. Results go back to the LLM for interpretation

There is no checkpoint where a human can review what SQL the model is about to execute. The model can generate destructive queries (`DROP TABLE`, `DELETE`), data-modifying queries (`UPDATE`, `INSERT`), or privacy-violating queries (selecting all personal data) — and the system will execute them without hesitation.

This is excessive agency because the AI system can take consequential actions (database operations) without any form of approval, confirmation, or scope limitation.

## Example Attack

An attacker uses prompt injection to cause the LLM to generate: `UPDATE investigations SET fraud_detected = 'false' WHERE investigation_id = '927b70bc-...'`. The system executes this immediately, clearing a fraud flag on a genuinely fraudulent transaction. No one is notified, and no approval was required.

## Mitigations

1. **Implement a human-in-the-loop checkpoint** for any query that modifies data (INSERT, UPDATE, DELETE, DROP).
2. **Restrict the tool to SELECT queries only** by validating the SQL before execution.
3. **Use a read-only database connection** so that even if the model generates a write query, the database rejects it.
4. **Log all executed queries** with the requesting user's identity for audit purposes.
5. **Implement query approval workflows** for queries that access sensitive columns (dates of birth, addresses) or return large result sets.
6. **Set a maximum result size** — if a query would return more than N rows, require explicit confirmation.
