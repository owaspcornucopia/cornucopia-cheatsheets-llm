[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CR8 — Stored Data Not Encrypted

## Threat

An attacker can access stored business data because it is not encrypted at rest.

## How This Applies

The SQLite database stores sensitive investigation data in plaintext:

- **Personal data**: Names, dates of birth, home addresses of persons involved in fraud investigations
- **Investigation details**: Fraud status, investigation IDs, transaction references
- **Business-sensitive data**: Which transactions were flagged as fraudulent

The database file (`db.sqlite`) is stored unencrypted on the filesystem. In the Docker deployment, it is stored within the container's writable layer. Anyone with access to the container's filesystem, the host system, or a backup of the container can read all investigation data directly from the SQLite file without needing to authenticate through the API.

## Example Attack

An attacker gains access to the host system where the Docker container runs (through a separate vulnerability, compromised SSH key, or cloud console access). They copy the `db.sqlite` file directly and read all investigation records — including personal data and fraud determinations — without ever interacting with the API or needing a token.

## Mitigations

1. **Encrypt the database at rest** — use SQLCipher (encrypted SQLite) or store data in an encrypted volume.
2. **Use encrypted storage volumes** in the container runtime (Docker encrypted volumes, Kubernetes encrypted PVs).
3. **Encrypt sensitive fields** individually within the database (names, addresses, dates of birth) using application-level encryption.
4. **Restrict filesystem access** — ensure only the application process can read the database file.
5. **Encrypt container volumes** and backup storage to protect data in transit to backup systems.
