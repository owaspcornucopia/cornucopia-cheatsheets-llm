[← Back to Cheat Sheet](/README.md)

# LLM2 — Resource Exhaustion via Unbounded LLM Inference

## Threat

An attacker can exhaust computational resources or increase operational costs by submitting resource-intensive queries to the LLM without any rate limiting.

## How This Applies

The `/api/fraud` endpoint triggers LLM inference on every request — in fact, it runs inference twice per request (once for tool call generation, once for the final answer). There are no controls to prevent abuse:

- No rate limiting per token or per IP address
- No request queue or concurrency limits
- No maximum query complexity restrictions
- No cost tracking or spending thresholds

The Docker Compose configuration allocates 8 CPUs and 12GB of memory to the application container. An attacker with a valid token (or exploiting the missing auth-at-endpoint issue from ATJ) can saturate these resources with concurrent requests, making the service unavailable to legitimate users.

## Example Attack

An attacker obtains one of the hardcoded tokens and writes a script that sends hundreds of concurrent fraud investigation questions. Each request consumes significant GPU/CPU time for two rounds of model inference. The service becomes unresponsive for all other customers.

## Mitigations

1. **Implement per-token rate limiting** — restrict each API consumer to a reasonable number of requests per minute.
2. **Add concurrency limits** — cap the number of simultaneous inference requests the application will process.
3. **Set request timeouts** — abort inference if it exceeds a reasonable time limit.
4. **Implement a request queue** with maximum depth, rejecting excess requests with HTTP 429 (Too Many Requests).
5. **Monitor resource usage** and set alerts for unusual spikes in CPU, memory, or request volume.
6. **Add input length limits** (see VE3) to prevent computationally expensive long prompts.
