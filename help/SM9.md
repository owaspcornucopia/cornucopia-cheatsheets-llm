[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# SM9 — Tokens Transmitted Over Insecure Channels

## Threat

An attacker can steal session tokens because they are sent over insecure channels, logged, revealed in error messages, or included in URLs.

## How This Applies

The application transmits authentication tokens insecurely in multiple ways:

- **No TLS**: The Flask application serves HTTP on port 9000 without encryption. The `token` header travels in plaintext over the network.
- **Docker Compose exposes port directly**: `ports: "9000:9000"` maps the container port to the host with no TLS proxy in front.
- **No secure transport requirement**: The application does not reject requests arriving over unencrypted connections.

Anyone with network access between the client and the server (same LAN, ISP, cloud network segment, or through a man-in-the-middle position) can read the token from captured traffic.

## Example Attack

The application is deployed in a cloud environment. Another container on the same network segment runs a packet capture tool. It intercepts HTTP requests to port 9000, extracts the `token` header values, and forwards them to the attacker. All five tokens are compromised within hours of deployment.

## Mitigations

1. **Deploy TLS** — add a reverse proxy (nginx, Caddy, Traefik) with TLS termination in front of the application.
2. **Reject non-TLS connections** — if TLS is terminated at a proxy, ensure the application is not directly accessible.
3. **Use encrypted overlay networks** in container deployments to protect east-west traffic.
4. **Mark tokens as sensitive** in logging configuration to prevent accidental logging.
5. **Never include tokens in URLs** — use headers or request bodies only.
