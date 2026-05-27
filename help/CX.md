[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CX — Vulnerable Third-Party Dependencies

## Threat

An attacker can circumvent the application's controls because its libraries and components contain known vulnerabilities.

## How This Applies

The application's `requirements.txt` pins specific versions of 130+ packages. Several are older versions with known security issues:

- **Flask 2.3.2** — outdated; newer versions contain security patches
- **Werkzeug 2.3.6** — outdated; has known vulnerabilities in later-patched versions
- **Jinja2 3.1.2** — template engine with known issues fixed in newer releases
- **MarkupSafe 2.1.3** — older version
- **itsdangerous 2.1.2** — cryptographic signing library, older version

The Dockerfile installs these exact versions without any vulnerability scanning. There is no process for:

- Checking dependencies against vulnerability databases (CVE)
- Updating dependencies when security patches are released
- Auditing the dependency tree for transitive vulnerabilities
- Monitoring for newly discovered vulnerabilities in used packages

## Example Attack

An attacker identifies that the deployed Flask/Werkzeug version has a known vulnerability (e.g., path traversal or request smuggling). They exploit it to bypass the application's routing and access internal endpoints or files directly, without needing an API token.

## Mitigations

1. **Scan dependencies for vulnerabilities** — use `pip-audit`, `safety`, or Snyk to check for known CVEs before deployment.
2. **Update dependencies regularly** — establish a schedule for reviewing and updating packages.
3. **Monitor vulnerability databases** — subscribe to alerts for the packages used in the application.
4. **Implement a dependency update policy** — security patches should be applied within a defined SLA.
5. **Use a lockfile and vulnerability gate** in CI/CD — block deployments that introduce known vulnerabilities.
6. **Minimize the dependency tree** — remove unused packages to reduce the attack surface.
