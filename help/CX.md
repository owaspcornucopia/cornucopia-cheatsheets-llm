[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# CX — Vulnerable Third-Party Dependencies

## Threat

An attacker can circumvent the application's controls because its libraries and components contain known vulnerabilities.

## How This Applies

[All three implementations](#implementations) use pinned third-party dependencies without a documented vulnerability-scanning gate. The [Python implementation](https://github.com/owaspcornucopia/llm-companion-scenario) declares 130+ packages in `requirements.txt`, including:

- **Flask 2.3.2** — outdated; newer versions contain security patches
- **Werkzeug 2.3.6** — outdated; has known vulnerabilities in later-patched versions
- **Jinja2 3.1.2** — template engine with known issues fixed in newer releases
- **MarkupSafe 2.1.3** — older version
- **itsdangerous 2.1.2** — cryptographic signing library, older version

The [.NET implementation](https://github.com/owaspcornucopia/llm-companion-scenario-dotnet) also depends on versioned packages, including `Microsoft.Data.Sqlite`, `Microsoft.ML.OnnxRuntimeGenAI`, and legacy `Utf8Json`. The [TypeScript implementation](https://github.com/owaspcornucopia/llm-companion-scenario-typescript) depends on `better-sqlite3`, Express, and `tsx`. [None of the three implementations](#implementations) documents a process for checking direct and transitive dependencies against current advisories.

[None of the three implementation dependency configurations](#implementations) is scanned during a container build. There is no process for:

- Checking dependencies against vulnerability databases (CVE)
- Updating dependencies when security patches are released
- Auditing the dependency tree for transitive vulnerabilities
- Monitoring for newly discovered vulnerabilities in used packages

## Example Attack

An attacker identifies a known vulnerability in a deployed web, database, serialization, or model-runtime dependency. They exploit it to bypass routing, access internal endpoints or files, or compromise the service without needing an API token.

## Mitigations

1. **Scan dependencies for vulnerabilities** — use `pip-audit`, `dotnet list package --vulnerable`, `npm audit`, or an equivalent scanner before deployment.
2. **Update dependencies regularly** — establish a schedule for reviewing and updating packages.
3. **Monitor vulnerability databases** — subscribe to alerts for the packages used in the application.
4. **Implement a dependency update policy** — security patches should be applied within a defined SLA.
5. **Use a lockfile and vulnerability gate** in CI/CD — block deployments that introduce known vulnerabilities.
6. **Minimize the dependency tree** — remove unused packages to reduce the attack surface.

## Implementations

- [Python implementation](https://github.com/owaspcornucopia/llm-companion-scenario)
- [.NET implementation](https://github.com/owaspcornucopia/llm-companion-scenario-dotnet)
- [TypeScript implementation](https://github.com/owaspcornucopia/llm-companion-scenario-typescript)
