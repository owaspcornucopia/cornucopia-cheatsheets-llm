[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CR3 — Data Modified Because No Integrity Checking

## Threat

## How This Applies

The architecture now has an internal HTTP channel between `app` (the Flask API) and `model_service` (the LLM inference service). This channel carries:

- **app → model_service**: Full conversation messages including the system prompt (`SYSTEM_PROMPT_SQL`) and the user's question. An attacker who can intercept this traffic can replace or modify the system prompt — bypassing the instruction boundary that limits the model to fraud investigation — or inject arbitrary instructions.
- **model_service → app**: The LLM-generated SQL query. An attacker who can modify this response can substitute a different SQL query (e.g., `DROP TABLE investigations`, `SELECT * FROM sqlite_master`, or a data-exfiltrating query) before `app.py` executes it.
- **app → model_service** (second call): The full investigation results including personal data (names, addresses, dates of birth). An attacker who can read this channel obtains all investigation data without needing a valid token.

All of this traffic is plain HTTP — no TLS, no HMAC, no message authentication of any kind. Docker's internal network is not encrypted and is accessible to any container on the same Docker network bridge.

## Attack Example

Axel gains access to the Docker host (e.g., via a compromised container using a known CVE in one of the Python dependencies). Using ARP spoofing on the internal `application_default` Docker bridge network, Axel positions himself between `app` and `model_service`.

Axel intercepts the `POST /generate` request from `app` to `model_service` and modifies the messages array, replacing `SYSTEM_PROMPT_SQL` with instructions to generate `DROP TABLE investigations`. The modified messages are forwarded to `model_service` which generates the malicious SQL. `app.py` receives the tampered response and executes the SQL, destroying all investigation data.

Alternatively, Axel intercepts the second call where investigation results (personal data) are sent back to the model and logs all personal data without ever touching the API token system.

## Mitigations

### 1. Enable mTLS Between Services (Recommended)
Configure the internal HTTP channel to use mutual TLS so that both endpoints verify each other's identity and all traffic is encrypted and authenticated:
- Use a service mesh (e.g., Traefik, Envoy) or configure Flask with SSL context for the model service
- Generate internal CA and per-service certificates
- Add certificate pinning in `app.py`'s `http_requests.post()` call

### 2. Add HMAC Request Signing (Practical Alternative)
Add a shared secret between `app` and `model_service` and sign every request/response:
```python
# In both services
import hmac, hashlib, os
INTERNAL_SECRET = os.environ.get("INTERNAL_SECRET").encode()

def sign_payload(body: bytes) -> str:
    return hmac.new(INTERNAL_SECRET, body, hashlib.sha256).hexdigest()

def verify_signature(body: bytes, signature: str) -> bool:
    expected = sign_payload(body)
    return hmac.compare_digest(expected, signature)
```
Send the signature as an `X-Signature` header and verify it on receipt.

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
Ensure the messages array sent to `model_service` is constructed server-side only and never echoes raw user input as a system message. The current implementation is correct in this regard — the user's question is only placed in a `user` role message, not a `system` message.

## References
- [OWASP ASVS 13.4.1, 13.4.2, 13.4.3](https://owasp.org/www-project-application-security-verification-standard/)
- [OWASP Top 10 A02:2021 Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)
- [CAPEC-94: Adversary in the Middle (AiTM)](https://capec.mitre.org/data/definitions/94.html)
- [CAPEC-39: Manipulating Opaque Client-based Data](https://capec.mitre.org/data/definitions/39.html)
