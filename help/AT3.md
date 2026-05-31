[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# AT3 — Token Secrets Exposed in Transit and at Rest

## Threat

An attacker can obtain authentication secrets because they are transmitted or stored without encryption.

## How This Applies

The API tokens have multiple exposure vectors:

- **At rest**: Tokens are stored as plaintext strings in the application source code (see also CRJ)
- **In transit**: The application listens on HTTP (port 9000) without TLS. Tokens sent in the `token` header travel over the network in plaintext
- **In version control**: Tokens are committed to the git repository, preserved in history permanently

An attacker on the same network can capture tokens by sniffing HTTP traffic. The Docker Compose configuration exposes port 9000 directly with no TLS termination.

## Example Attack

An attacker on the same network segment as the application (or between the client and server) captures HTTP traffic using a network sniffer. They extract the `token` header from any request and can now impersonate that API consumer indefinitely.

## Mitigations

1. **Enable TLS** — serve the API over HTTPS only. Add a reverse proxy (nginx, Caddy) with TLS termination in front of the Flask application.
2. **Never store tokens in source code** — use a secrets manager or environment variables.
3. **Encrypt tokens at rest** — if they must be stored locally, hash them or use an encrypted vault.
4. **Use short-lived tokens** with refresh mechanisms so that intercepted tokens expire quickly.
5. **Enforce HSTS** to prevent protocol downgrade attacks.
