[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CRJ — Credentials Stored in Plaintext in Source Code

## Implementations

- [Python implementation](#python-implementation)
- [.NET implementation](#net-implementation)
- [TypeScript implementation](#typescript-implementation)
- [Java implementation](#java-implementation)

## Threat

An attacker can read authentication credentials because they are stored unencrypted in the application's source code.

## How This Applies

### All implementations

All four implementations hardcode plaintext UUID tokens in application source code. These tokens are the sole authentication mechanism for the API. Anyone with access to the source code — including:

- Developers on the team
- CI/CD systems
- Version control history (git)
- Container images that contain the compiled or copied application
- Anyone who compromises the running container

— can extract all valid tokens and impersonate any API consumer.

In any of the four implementations, the token values exist in plaintext on the host or in deployable artifacts.

### Python implementation

The Python implementation Compose configuration mounts its source file into the container.

### .NET implementation

The .NET implementation embeds the tokens in its published assembly.

### TypeScript implementation

The TypeScript implementation copies its source into the container image.

### Java implementation

The Java implementation embeds the token collection in its published JAR.

## Example Attack

An attacker gains read access to the git repository (through a leaked `.git` directory, a misconfigured CI system, or a compromised developer machine). They extract all five tokens from the source code history. Even if the tokens are later changed in the current version, the old values remain in git history and may still work if rotation was not performed.

## Mitigations

1. **Move tokens to a secrets manager** (e.g., HashiCorp Vault, AWS Secrets Manager, Azure Key Vault) or at minimum to environment variables that are not committed to source control.
2. **Never commit secrets to version control.** Use `.gitignore` and pre-commit hooks to prevent accidental commits.
3. **Rotate all existing tokens** since they have been exposed in source code and should be considered compromised.
4. **Hash stored tokens** — if tokens must be stored locally, store only their cryptographic hashes and compare incoming tokens against the hashes.
5. **Use a proper authentication system** (OAuth 2.0, API key management service) instead of static tokens.
