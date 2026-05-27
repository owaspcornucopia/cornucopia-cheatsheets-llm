[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CR6 — Unencrypted Data in Transit

## Threat

An attacker can read and modify unencrypted data in transit including credentials, personal data, and investigation results.

## How This Applies

The Flask application serves plain HTTP on port 9000. All data flows between clients and the API are unencrypted:

- **Authentication tokens** travel in the `token` HTTP header in plaintext
- **Investigation questions** (which may reference real people) are transmitted in cleartext
- **Query results** containing personal data (names, dates of birth, home addresses) are returned unencrypted
- **Fraud determinations** are sent back without any transport protection

The Docker Compose configuration exposes port 9000 directly with no TLS-terminating proxy. Communication between the API consumers and the application is open to interception and modification by anyone with network access.

## Example Attack

An attacker performs ARP spoofing on the network segment where the application runs. They intercept all HTTP traffic to port 9000, capturing authentication tokens and reading fraud investigation results containing personal data (names, dates of birth, addresses of persons under investigation).

## Mitigations

1. **Add TLS termination** — deploy a reverse proxy (nginx, Caddy, Traefik) in front of the Flask application with a valid TLS certificate.
2. **Redirect HTTP to HTTPS** — reject any plaintext connections.
3. **Use encrypted container networking** — in Docker/Kubernetes deployments, enable network encryption for east-west traffic.
4. **Deploy HSTS headers** to prevent protocol downgrade attacks.
5. **Use TLS 1.3** with strong cipher suites for optimal security and performance.
