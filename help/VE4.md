[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# VE4 — Malicious Field Names or Data Not Checked in Context

## Threat

An attacker can input malicious field names or data because the application does not validate input within the context of the current user and process.

## How This Applies

The `/api/fraud` endpoint accepts a `question` field from the user and passes it directly to the LLM without checking whether its content is appropriate for the fraud investigation context. The application does not verify:

- Whether the question relates to fraud investigation at all
- Whether the requested data is appropriate for the authenticated user's role
- Whether field references in the question match the user's authorized scope

Any authenticated user can ask about any investigation, any person, any address — the application makes no contextual check linking the user's identity to what they should be allowed to ask about.

## Example Attack

A user authenticated with the "Crypto Mc Cryptface exchange" token asks: "Show me the date of birth and home address of Wheezy Joe Kingfish." The application processes this without verifying that this user has any business reason to access that person's personal data.

## Mitigations

1. **Validate that the question relates to the user's authorized scope** — map tokens to customer organizations and restrict questions to their own investigations.
2. **Filter out requests for sensitive fields** (date of birth, addresses) unless the caller has explicit need-to-know.
3. **Implement context-aware input validation** that considers who is asking, not just what is asked.
4. **Restrict the LLM's SQL generation** to only include columns and rows the authenticated user is permitted to access.
