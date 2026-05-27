[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CRJ — Credentials Stored in Plaintext in Source Code

## Threat

An attacker can read authentication credentials because they are stored unencrypted in the application's source code.

## How This Applies

The `allowed_tokens` list is hardcoded directly in `app.py` as plaintext UUID strings. These tokens are the sole authentication mechanism for the API. Anyone with access to the source code — including:

- Developers on the team
- CI/CD systems
- Version control history (git)
- Container images (the Dockerfile copies `app.py` into the image)
- Anyone who compromises the running container

— can extract all valid tokens and impersonate any API consumer.

The tokens are also mounted as a volume in the Docker Compose configuration (`./app.py:/application/app.py`), meaning they exist in plaintext on the host filesystem.

## Example Attack

An attacker gains read access to the git repository (through a leaked `.git` directory, a misconfigured CI system, or a compromised developer machine). They extract all five tokens from the source code history. Even if the tokens are later changed in the current version, the old values remain in git history and may still work if rotation was not performed.

## Mitigations

1. **Move tokens to a secrets manager** (e.g., HashiCorp Vault, AWS Secrets Manager, Azure Key Vault) or at minimum to environment variables that are not committed to source control.
2. **Never commit secrets to version control.** Use `.gitignore` and pre-commit hooks to prevent accidental commits.
3. **Rotate all existing tokens** since they have been exposed in source code and should be considered compromised.
4. **Hash stored tokens** — if tokens must be stored locally, store only their cryptographic hashes and compare incoming tokens against the hashes.
5. **Use a proper authentication system** (OAuth 2.0, API key management service) instead of static tokens.
