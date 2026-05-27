[← Back to Cheat Sheet](/README.md)

# AZ8 — Business Rule Bypass Through Process Manipulation

## Threat

An attacker can bypass business rules by altering the usual process sequence or manipulating control data.

## How This Applies

The intended process is: user asks question → LLM generates investigation query → database returns results → LLM interprets fraud status. However, through SQL injection an attacker can bypass the intended business flow:

- **Skip the investigation step** — directly modify the `fraud_detected` field to clear fraud flags
- **Alter process data** — change investigation statuses from 'COMPLETED' to other states
- **Manipulate results** — insert fake investigation records that the LLM will use to generate false conclusions
- **Bypass the LLM's reasoning** — if the database returns manipulated data, the LLM will draw incorrect conclusions

The business rule is "use AI to determine fraud based on evidence." An attacker bypasses this by directly manipulating the evidence database through the same SQL execution mechanism.

## Example Attack

An attacker uses prompt injection to generate: `UPDATE investigations SET fraud_detected='false' WHERE investigation_id='927b70bc-...'`. This changes a genuine fraud finding to "not fraud," bypassing the entire investigation process. The next time someone asks about this transaction, the LLM will report no fraud detected.

## Mitigations

1. **Enforce the intended process** — the tool should only execute SELECT queries, never modification statements.
2. **Use a read-only database connection** so the process cannot be subverted via data modification.
3. **Implement audit trails** — log all database access so unauthorized modifications can be detected.
4. **Separate read and write paths** — investigation records should only be writable through a controlled administrative interface, not through the API.
5. **Add integrity checks** on investigation records to detect unauthorized modifications.
