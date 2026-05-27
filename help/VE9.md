[← Back to Cheat Sheet](/README.md)

# VE9 — Validation Failures Not Rejected

## Threat

An attacker can bypass input or output validation because failures are not rejected or sanitized — invalid data is accepted and processed anyway.

## How This Applies

The application has no input validation at all on the `question` parameter beyond checking it is non-empty. There is no validation step that could fail because no validation exists. Additionally:

- When the LLM produces invalid output (not matching the expected JSON tool call format), the application attempts multiple fallback parsing strategies instead of rejecting the response
- The `parse_tool_call()` function tries JSON parsing, Python literal evaluation, regex extraction, and raw SQL detection — progressively loosening its standards rather than failing safely
- Even if the LLM output is clearly malformed, the application attempts to salvage something usable from it

This "try harder" approach means that borderline malicious outputs that would be caught by strict parsing get through via fallback paths.

## Example Attack

An attacker crafts a prompt injection that produces output the strict JSON parser rejects, but that the regex fallback path accepts. The fallback extracts a SQL query from loosely structured text, bypassing what would otherwise be a parsing failure.

## Mitigations

1. **Implement strict input validation** on the question parameter and reject anything that fails.
2. **Use strict output parsing** — if the LLM output does not match the expected JSON schema exactly, reject it. Do not use fallback parsing strategies.
3. **Log validation failures** for security monitoring — repeated failures from the same token may indicate an attack.
4. **Return a clear error** when validation fails rather than attempting to process invalid data.
