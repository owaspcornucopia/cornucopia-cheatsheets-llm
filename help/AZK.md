[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# AZK — Altering Authorization Controls via SQL Injection

## Threat

An attacker can influence or alter authorization controls and permissions, and can therefore bypass them.

## How This Applies

The only authorization check in the application is the token list membership test. Through SQL injection, an attacker cannot directly modify the Python code, but they can:

- **Alter the data that drives decisions** — if the application's behavior depends on database state, SQL injection can change that state
- **Create new data paths** — inject records that cause the LLM to produce different (potentially dangerous) SQL in future queries
- **Undermine data integrity** — if authorization decisions downstream depend on investigation data, corrupting that data effectively bypasses authorization

More broadly, because the authorization control is a simple list check in Python code, anyone with access to the source code (which contains plaintext tokens) can add their own token and effectively grant themselves access.

## Example Attack

An attacker with repository access adds a new UUID to the `allowed_tokens` list, commits, and deploys. Since there is no review gate on authorization changes (no administrative approval, no separation of duties), they have granted themselves permanent API access.

## Mitigations

1. **Move authorization configuration out of source code** — use a configuration service or database that requires administrative approval to modify.
2. **Implement separation of duties** — changes to authorization controls should require approval from a different person than the one making the change.
3. **Protect the database from modification** — use read-only connections to prevent SQL injection from altering authorization-relevant data.
4. **Audit authorization changes** — log any modification to the token list or authorization configuration with who made the change and when.
5. **Use code review and approval gates** for any changes to authentication/authorization code.
