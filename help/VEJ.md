[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# VEJ — Input Validation Routines Can Be Bypassed

## Threat

An attacker has control over or can bypass input validation, output validation, sanitization, or encoding routines.

## How This Applies

The application has no centralized input validation or output sanitization framework. Since there are no validation routines to speak of, there is nothing preventing an attacker from sending any payload they want:

- No input filtering layer exists between the API and the LLM
- No output validation exists between the LLM and the SQL executor
- No sanitization exists between database results and the final LLM prompt
- The `parse_tool_call()` function attempts to be flexible rather than strict, accepting multiple formats

Because validation does not exist as a distinct, enforced layer, there is nothing to bypass — the door is simply open. Any future addition of validation would need to be comprehensive or an attacker would simply use a path that skips it.

## Example Attack

If a developer adds input filtering that blocks the word "DROP" in questions, an attacker bypasses it by encoding the instruction differently (e.g., "remove the table" or "d.r.o.p") — the LLM understands the intent and generates the dangerous SQL anyway. Without holistic, defense-in-depth validation, single-point filters are easily circumvented.

## Mitigations

1. **Implement centralized, mandatory validation** as a middleware layer that all requests must pass through before reaching business logic.
2. **Validate at multiple points** — input validation on questions, output validation on LLM responses, and query validation before execution.
3. **Use allowlists rather than blocklists** — define what is permitted rather than trying to block everything dangerous.
4. **Make validation non-bypassable** by architecture — the request processing pipeline should not have any path that skips validation.
