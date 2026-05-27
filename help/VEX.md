[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# VEX — Exploiting Trust in Data Sources

## Threat

An attacker can exploit the trust the application places in a data source, such as user-definable data or lack of verification of identity during data validation.

## How This Applies

The application implicitly trusts multiple data sources without verification:

1. **User input is trusted** — the `question` parameter is passed to the LLM without any content filtering or sanitization
2. **LLM output is trusted** — the model's generated SQL is executed without validation (the application trusts that the model will produce safe queries)
3. **Database content is trusted** — query results are fed back into the LLM prompt without sanitization, trusting that the database does not contain injected instructions
4. **The token header is trusted** — there is no verification that the bearer of a token is who they claim to be (no IP binding, no device fingerprinting, no TLS client certificates)

The core problem is a chain of trust: user → LLM → database → LLM → response, where each step trusts the previous one without independent verification.

## Example Attack

An attacker steals a token (they are plaintext in source code). The application trusts the token unconditionally — there is no second factor, no IP restriction, no session binding. The attacker uses it from any location to access all investigation data.

## Mitigations

1. **Do not trust LLM output** — validate and sanitize all model-generated content before using it in downstream operations.
2. **Do not trust database content** — sanitize query results before including them in LLM prompts.
3. **Do not trust user input** — validate questions before passing them to the model.
4. **Bind tokens to additional context** (IP address, client certificate, device fingerprint) to prevent stolen tokens from being used by unauthorized parties.
5. **Implement a zero-trust architecture** where each component validates inputs from every other component independently.
