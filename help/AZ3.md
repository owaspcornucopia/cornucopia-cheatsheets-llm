[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# AZ3 — Data Accessible Through Alternative Mechanisms

## Threat

An attacker can access information they should not have permission to through another mechanism that does have permission, or because data is cached or kept longer than necessary.

## How This Applies

The application has multiple pathways that expose data beyond what authorization should permit:

- **SQL injection path**: Only the token-to-UUID relationship is checked, but the LLM generates SQL that can query any record. An attacker with a valid token for one customer can query all data via crafted questions.
- **Error responses**: Failed requests return SQL queries and raw model output, potentially leaking data about other investigations.
- **Database is shared**: All investigation records are in a single table with no access control. The LLM (acting on behalf of any user) can see everything.
- **No query scoping**: The system does not filter queries to only return data associated with the requesting token.

The fundamental issue is that only "do you have a valid token?" is checked, not "are you authorized to see this specific data?"

## Example Attack

A user with the "Crypto Mc Cryptface exchange" token asks: "Show me all completed investigations." The LLM generates `SELECT * FROM investigations WHERE investigation_status='COMPLETED'` which returns all customers' investigation data, including Bankly's records.

## Mitigations

1. **Implement row-level access control** — associate each investigation record with an owner/tenant and filter queries accordingly.
2. **Scope LLM queries** to only the data the authenticated token is authorized to access.
3. **Remove sensitive data from error responses** — do not leak information about other records through error paths.
4. **Implement data classification** — mark sensitive fields and enforce access rules per field.
5. **Audit cross-tenant data access** attempts and alert on violations.
