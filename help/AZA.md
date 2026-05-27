[← Back to Cheat Sheet](/README.md)

# AZA — Unable to Detect Novel Authorization Attacks

## Threat

Novel attacks against authorization go undetected because there is no logging or monitoring of authorization events.

## How This Applies

The application has no logging of authorization-related events:

- No record of which data each token accesses
- No logging of cross-tenant data access attempts
- No monitoring of SQL queries that access data outside a token's expected scope
- No alerting on unusual data access patterns
- Werkzeug logging is explicitly disabled

If an attacker discovers a way to access data they shouldn't (through SQL injection, token misuse, or any other method), there is no detection mechanism. The application is completely blind to authorization violations.

## Example Attack

An attacker systematically exfiltrates all investigation records over several weeks by asking specific questions about different customers. Because there is no logging of what data each token accesses, the cross-tenant access goes unnoticed until a customer discovers their confidential data has been leaked.

## Mitigations

1. **Log all data access** — record which token accessed which records and when.
2. **Implement authorization event monitoring** — alert on tokens accessing data outside their expected scope.
3. **Create access baselines** — establish normal patterns for each token and flag deviations.
4. **Enable audit logging** at the database level to capture all queries with their results.
5. **Implement periodic access reviews** — regularly verify that access patterns align with intended authorization policies.
