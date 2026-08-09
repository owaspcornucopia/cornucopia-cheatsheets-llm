[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# SMK — Self-Built Session Management Controls

## Threat

An attacker can bypass session management because the controls are self-built and weak rather than using a standard framework.

## How This Applies

[All four implementations](#implementations) implement session/token management as a hardcoded in-memory token collection.

This is not session management — it is a static list comparison. It provides none of the properties a proper session management system would:

- No session creation/destruction lifecycle
- No token generation with cryptographic randomness
- No session state tracking
- No concurrent session control
- No session fixation protection
- No session hijacking detection

Standard framework authentication/session features or API gateway token management provide these capabilities. The self-built approach provides none of them.

## Example Attack

Because there is no session state, the application cannot distinguish between a fresh request and a replayed request. An attacker captures a valid request (including the token) and replays it indefinitely, and the system processes each replay as a legitimate new request.

## Mitigations

1. **Use a standard session management solution** — framework session/authentication features or an API gateway with built-in token management.
2. **Implement proper token lifecycle** — generation, validation, rotation, and revocation through a proven library.
3. **Add request uniqueness** — use nonces or request IDs to prevent replay attacks.
4. **Leverage established security patterns** rather than inventing authentication mechanisms from scratch.

## Implementations

- [Python implementation](https://github.com/owaspcornucopia/llm-companion-scenario)
- [.NET implementation](https://github.com/owaspcornucopia/llm-companion-scenario-dotnet)
- [TypeScript implementation](https://github.com/owaspcornucopia/llm-companion-scenario-typescript)
- [Java implementation](https://github.com/owaspcornucopia/llm-companion-scenario-java)
