[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# ATJ — Missing Authentication on Critical Paths

## Threat

An attacker can access resources or services because authentication is not enforced at the correct point, or is assumed to be handled elsewhere.

## How This Applies

The authentication check (token validation) is performed inside the `investigation_fraud()` function — the database query tool — rather than at the API endpoint level. This creates several problems:

1. The `/api/fraud` endpoint itself does not check authentication. The LLM inference (which is computationally expensive) runs before any token check occurs.
2. An unauthenticated attacker can trigger model inference, consuming GPU/CPU resources, without ever providing a valid token.
3. The authentication check is buried inside a utility function where it can easily be missed during code review or accidentally bypassed in future refactoring.

The token check only fires when the SQL query is about to execute, meaning all the work of generating the tool call has already been done.

## Example Attack

An attacker without any token sends thousands of requests to `/api/fraud`. Each request triggers full LLM inference before the token check occurs. This wastes computational resources and could be used for denial of service or to probe the model's behavior through error responses.

## Mitigations

1. **Move authentication to the API endpoint level.** Check the token at the beginning of the `investigate_transaction()` route handler, before any processing occurs.
2. **Use a Flask middleware or decorator** (e.g., `@require_auth`) to enforce authentication consistently across all endpoints.
3. **Fail early** — reject unauthenticated requests immediately with a 401 response before any business logic runs.
4. **Document authentication requirements** clearly so that future developers know where and how authentication is enforced.
