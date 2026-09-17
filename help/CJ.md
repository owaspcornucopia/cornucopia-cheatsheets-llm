[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CJ — Insecure Deployment Without Operational Documentation

## Implementations

- [Python implementation](#python-implementation)
- [.NET implementation](#net-implementation)
- [TypeScript implementation](#typescript-implementation)
- [Java implementation](#java-implementation)

## Threat

An attacker can exploit the application because it was deployed without security documentation, operational guidance, or secure-by-default configuration.

## How This Applies

### All implementations

The application is deployed without operational security documentation:

- **No security configuration guide** — there is no documentation on how to deploy securely (TLS setup, token management, network configuration)
- **No operational runbook** — no guidance on monitoring, alerting, or incident response
- **No hardening checklist** — the Docker/infrastructure configuration ships with no security guidance
- **Development-oriented defaults are deployed without hardening** — each implementation ships its own framework defaults with no production hardening
- **Default configuration is insecure** — the application runs without TLS, with hardcoded tokens, and with logging disabled
- **No security warnings** — the application does not warn operators when expected security features (TLS, proper auth) are not configured

An operator deploying this application has no guidance on how to secure it and may assume the defaults are acceptable.

### Python implementation

The Python implementation uses its framework server.

### .NET implementation

The .NET implementation exposes default HTTP hosting without TLS or security configuration.

### TypeScript implementation

The TypeScript implementation runs its Express services with `tsx` and no production hardening.

### Java implementation

The Java implementation runs Spring Boot with no TLS or application hardening.

## Example Attack

An operations team deploys the application following the README and Docker Compose file. Since there is no security documentation, they deploy it as-is: no TLS, hardcoded tokens, logging disabled, no monitoring. The insecure defaults become the production configuration.

## Mitigations

1. **Create a security deployment guide** documenting minimum security requirements (TLS, token management, network configuration).
2. **Ship secure defaults** — the application should be secure out of the box, requiring explicit action to weaken security (not the reverse).
3. **Use production-grade hosting** with an explicitly configured reverse proxy, TLS, logging, and security headers.
4. **Document operational requirements** — logging, monitoring, alerting, patching, and incident response procedures.
5. **Add startup warnings** when the application detects insecure configuration (no TLS, hardcoded tokens, disabled logging).
6. **Provide a hardening checklist** that operators can follow during deployment.