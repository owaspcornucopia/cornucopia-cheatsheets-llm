[← Back to Cheat Sheet](/README.md)

# AZ5 — Excessive Privileges and No Data Isolation

## Threat

An attacker with valid credentials can access all data in the system because there is no fine-grained authorization separating what different users can see.

## How This Applies

All five tokens in the application grant identical access. A token belonging to "Crypto Mc Cryptface exchange" can query investigations involving "Bankly" customers and vice versa. There is no concept of:

- Per-customer data isolation (tenant separation)
- Role-based access control
- Row-level security on the investigations table
- Restricting which investigations a given API consumer can query

The `investigation_fraud()` function executes whatever SQL the LLM generates. Since the SQL can query any row in the database, any authenticated user can see all investigation records regardless of whether they relate to that user's business.

## Example Attack

The "NFT trader 5000" customer uses the API to ask about transactions involving "Bankly" customers. The LLM generates a query like `SELECT * FROM investigations WHERE payee_from_name='Bad News Stevens'` — a Bankly investigation. The API returns this data because there is no check verifying that the requesting token is authorized to see that specific record.

## Mitigations

1. **Implement tenant-based data isolation.** Associate each token with a specific customer/organization and restrict queries to only return data belonging to that tenant.
2. **Apply row-level filtering** by appending a tenant filter to all SQL queries before execution (e.g., `WHERE tenant_id = :caller_tenant`).
3. **Use the principle of least privilege** — each API consumer should only access the minimum data necessary for their use case.
4. **Add a customer/tenant column** to the investigations table and enforce filtering at the database layer.
5. **Audit data access** to detect cross-tenant queries that may indicate abuse.
