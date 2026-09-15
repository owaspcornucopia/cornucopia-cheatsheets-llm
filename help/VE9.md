[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# VE9 — Validation Failures Not Rejected

## Implementations

- [Python implementation](#python-implementation)
- [.NET implementation](#net-implementation)
- [TypeScript implementation](#typescript-implementation)
- [Java implementation](#java-implementation)

## Threat

An attacker can bypass input or output validation because failures are not rejected or sanitized — invalid data is accepted and processed anyway.

## How This Applies

### All implementations

The application has no input validation at all on the `question` parameter beyond checking it is non-empty. There is no validation step that could fail because no validation exists. Additionally:

- When the LLM produces invalid output (not matching the expected JSON tool call format), the application attempts multiple fallback parsing strategies instead of rejecting the response
- All four implementations progressively loosen their parser
- Even if the LLM output is clearly malformed, the application attempts to salvage something usable from it

This "try harder" approach means that borderline malicious outputs that would be caught by strict parsing get through via fallback paths.

### Python implementation

The Python implementation accepts Python literal syntax.

### .NET implementation

The .NET implementation normalizes malformed JSON, extracts a query with regular expressions, or accepts remaining output as SQL.

### TypeScript implementation

The TypeScript implementation accepts raw SQL when JSON parsing does not produce a tool call.

### Java implementation

The Java implementation accepts raw SQL when JSON parsing does not produce a tool call.

## Example Attack

An attacker crafts a prompt injection that produces output the strict JSON parser rejects, but that the regex fallback path accepts. The fallback extracts a SQL query from loosely structured text, bypassing what would otherwise be a parsing failure.

## Mitigations

1. **Implement strict input validation** on the question parameter and reject anything that fails.
2. **Use strict output parsing** — if the LLM output does not match the expected JSON schema exactly, reject it. Do not use fallback parsing strategies.
3. **Log validation failures** for security monitoring — repeated failures from the same token may indicate an attack.
4. **Return a clear error** when validation fails rather than attempting to process invalid data.
