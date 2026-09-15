[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CR3 — Data Modified Because No Integrity Checking

## Threat

An attacker can alter service-to-service data because the application does not authenticate or integrity-protect internal messages.

## How This Applies

### All implementations

All four implementations use an internal HTTP channel between the API service and the model service. This channel carries:

- **API service → model service**: Full conversation messages including the system prompt and the user's question. An attacker who can intercept this traffic can replace or modify the system prompt or inject arbitrary instructions.
- **Model service → API service**: The LLM-generated SQL query. An attacker who can modify this response can substitute a destructive or data-exfiltrating query before it is executed.
- **API service → model service** (second call): The full investigation results including personal data. An attacker who can read this channel obtains investigation data without needing a valid token.

All of this traffic is plain HTTP — no TLS, no HMAC, no message authentication of any kind. Docker's internal network is not encrypted and is accessible to any container on the same Docker network bridge.

## Attack Example

Axel gains access to the Docker host, for example through a compromised container using a vulnerable dependency. Using ARP spoofing on the internal Docker bridge network, Axel positions himself between the API and model services.

Axel intercepts the `POST /generate` request and modifies the messages, replacing the SQL system prompt with instructions to generate `DROP TABLE investigations`. The API service receives the tampered response and executes the SQL, destroying investigation data.

Alternatively, Axel intercepts the second call where investigation results (personal data) are sent back to the model and logs all personal data without ever touching the API token system.

## Mitigations

### 1. Enable mTLS Between Services (Recommended)
Configure the internal HTTP channel to use mutual TLS so that both endpoints verify each other's identity and all traffic is encrypted and authenticated:
- Use a service mesh (e.g., Traefik, Envoy) or configure the model service to require TLS
- Generate internal CA and per-service certificates
- Configure the API service HTTP client to validate the model service certificate

### 2. Add HMAC Request Signing (Practical Alternative)
Add a shared secret between the API and model services and use the platform cryptography library to sign every request and response with HMAC-SHA-256. Send the signature as an `X-Signature` header and verify it on receipt.

### 3. Network Isolation
Restrict which containers can reach the model service at the Docker Compose network level:
```yaml
networks:
  frontend:
  backend:
    internal: true  # not reachable from outside Docker
```
Place only `app` and `model` on the `backend` network. This does not prevent a compromised `app` container from attacking `model`, but it significantly reduces the attack surface.

### 4. Do Not Pass Raw User Input to the Internal Service
Ensure the messages sent to the model service are constructed server-side and never echo raw user input as a system message. In [all four implementations](#implementations), the question is currently placed in a user-role message rather than a system-role message.

## References
- [OWASP ASVS 13.4.1, 13.4.2, 13.4.3](https://owasp.org/www-project-application-security-verification-standard/)
- [OWASP Top 10 A02:2021 Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)
- [CAPEC-94: Adversary in the Middle (AiTM)](https://capec.mitre.org/data/definitions/94.html)
- [CAPEC-39: Manipulating Opaque Client-based Data](https://capec.mitre.org/data/definitions/39.html)
