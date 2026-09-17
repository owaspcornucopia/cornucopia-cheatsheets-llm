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

## Android-specific scenario

The native app has no login, tenant identifier, account binding, or row/column-level
authorization scope. Every app installation creates the same fraud transaction table, and the broad question `Show all transactions` becomes `SELECT * FROM transactions`.
The exported activity also accepts a question from another process, so a malicious app does not need to be authorized to request the data.

Test it with the default question, then add a real authenticated authorization context, mandatory authorization scoping, and a read-only data layer that rejects
inserts from outside the app's own authorization.

## IOS implementation

The app uses a native iOS equivalent of the mobile behavior described above.

### What can go wrong

An attacker can use the matching iOS entry point, local storage, process state,
or on-device model flow to expose data or change the fraud investigation.

### What to do

Apply the iOS controls in the MASTG, MASVS, and MASWE references above. Test
the behavior on an iOS Simulator with the [IOS implementation](https://github.com/owaspcornucopia/llm-companion-scenario-ios#ios-implementation) and
verify that input validation, authorization, data minimization, integrity, and
protected storage are enforced at the native boundary.

### IOS details

The native app has no login, tenant binding, account context, or row-level authorization around the local model and database.
