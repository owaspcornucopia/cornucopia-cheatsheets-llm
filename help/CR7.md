[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CR7 — Weak or Missing Transport Encryption

## Threat

An attacker can intercept or modify data in transit because the transport protocol is poorly deployed, weakly configured, or not present at all.

## How This Applies

The application has no TLS configuration whatsoever:

- Flask's development server is used in production (`app.run()`) with no TLS parameters
- No certificate is configured
- No cipher suite selection is made
- No protocol version is enforced
- The Docker Compose file does not include a TLS proxy service

Even if a reverse proxy were added later, without explicit configuration it might default to allowing weak protocols (TLS 1.0, 1.1) or weak cipher suites that are vulnerable to known attacks (BEAST, POODLE, etc.).

## Example Attack

The application is deployed behind a load balancer that supports TLS 1.0 for "backwards compatibility." An attacker uses the POODLE attack to downgrade the connection and decrypt traffic containing authentication tokens and personal investigation data.

## Mitigations

1. **Implement TLS with modern configuration** — TLS 1.2 minimum, TLS 1.3 preferred.
2. **Use strong cipher suites only** — disable CBC mode ciphers, prefer AEAD ciphers (AES-GCM, ChaCha20-Poly1305).
3. **Use valid certificates** from a trusted CA (not self-signed in production).
4. **Disable protocol downgrade** — reject connections attempting TLS 1.0 or 1.1.
5. **Test TLS configuration** regularly using tools like SSL Labs or testssl.sh.
6. **Enable HSTS** with a long max-age to prevent protocol downgrade at the HTTP level.
