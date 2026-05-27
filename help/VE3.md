[← Back to Cheat Sheet](/README.md)

# VE3 — Missing Input Validation

## Threat

An attacker can submit malicious or malformed data because the application does not validate the format, type, length, or content of user input.

## How This Applies

The `/api/fraud` endpoint accepts a `question` parameter from the user (via query string or JSON body). The only validation performed is checking whether the string is empty. There is no:

- Maximum length enforcement on the question
- Character set restriction
- Format validation
- Content filtering or sanitization

The raw question text is passed directly into the LLM prompt. This means an attacker can submit arbitrarily long input, include special characters, embed prompt injection payloads, or send binary data — all of which get forwarded to the model without restriction.

## Example Attack

An attacker submits a 50,000-character question containing prompt injection instructions hidden within what appears to be a legitimate fraud investigation question. Because there is no length limit or content filtering, the entire payload reaches the model and influences its behavior.

## Mitigations

1. **Enforce a maximum length** on the `question` parameter (e.g., 500 characters is reasonable for a fraud investigation question).
2. **Restrict the character set** to printable text characters appropriate for the business context.
3. **Validate the input format** — a fraud investigation question should be natural language text, not JSON, SQL, or code.
4. **Implement content filtering** to detect and reject inputs that contain prompt injection patterns, SQL fragments, or system prompt override attempts.
5. **Apply rate limiting** at the input level to prevent automated fuzzing of the endpoint.
