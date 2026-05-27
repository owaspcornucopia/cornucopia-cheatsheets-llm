[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# LLM5 — Privilege Escalation Due to Weak Auth and No Tenant Isolation

## Threat

An attacker can escalate privileges or access other users' data due to weak authentication and missing session isolation in the LLM system.

## How This Applies

The application serves multiple customers (Crypto Mc Cryptface exchange, Bankly, NFT trader 5000) but implements no separation between them:

- All tokens grant identical access to the same database
- The LLM has no awareness of which customer is making the request
- SQL queries are not filtered by tenant/customer
- There is no concept of user sessions or per-user data boundaries

A user with a valid token for one organization can query investigation data belonging to any other organization. The system has no mechanism to restrict which records a given caller can access.

Combined with the hardcoded backdoor and debug tokens (see AT5), an attacker doesn't even need to compromise a legitimate customer's token — they can use a token that was never intended for production access.

## Example Attack

A "Crypto Mc Cryptface exchange" API consumer asks: "Show me all fraud investigations for Bankly customers." The LLM generates `SELECT * FROM investigations WHERE payee_from_name LIKE '%Bankly%'` and the system happily executes it and returns Bankly's confidential fraud investigation data to a competitor.

## Mitigations

1. **Implement per-tenant data isolation.** Map each token to an organization and restrict all queries to that organization's data only.
2. **Add tenant context to the LLM prompt** so the model generates queries scoped to the caller's authorized data.
3. **Enforce row-level security** at the database layer, appending mandatory tenant filters to all queries.
4. **Remove debug and backdoor tokens** from the system entirely.
5. **Implement proper identity management** using an identity provider that issues scoped tokens with claims indicating which data the bearer is authorized to access.
