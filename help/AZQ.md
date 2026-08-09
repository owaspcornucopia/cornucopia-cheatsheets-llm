[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# AZQ — Command Injection via SQL Injection Escalation

## Threat

An attacker can inject a command that the application will run at a higher privilege level.

## How This Applies

While SQLite does not support stored procedures or direct OS command execution, SQL injection in this application could potentially escalate to command-level access through:

- **SQLite extensions**: loadable extensions are enabled, `load_extension()` can load arbitrary shared libraries from the filesystem
- **File operations**: SQLite's `.backup` or `.dump` operations (if accessible) can write to the filesystem
- **Application-level escalation**: A successfully injected query could modify application state in ways that lead to further exploitation
- **Chained exploits**: Information gained from SQL injection (e.g., file paths, configuration) could enable other attack vectors

The database runs within the same process as the API application with no privilege separation. In [all three implementations](#implementations), SQLite extensions can be loaded through the query path, so capabilities exposed by the SQLite library are reachable through the injection point. The [TypeScript implementation](https://github.com/owaspcornucopia/llm-companion-scenario-typescript) extracts `load_extension()` calls from model-generated SQL and passes their paths to the native database driver.

## Example Attack

An attacker uses prompt injection to generate SQL that calls `load_extension('/tmp/malicious.so')` — if loadable extensions are not disabled and the attacker has found a way to write a file (through another vulnerability), this achieves code execution within the application process.

## Mitigations

1. **Disable SQLite loadable extensions** using the appropriate database-driver configuration.
2. **Run the database in a restricted mode** — use SQLite's authorizer callback to restrict available operations.
3. **Use a read-only database connection** to prevent write-based escalation paths.
4. **Implement process isolation** — run the database in a separate container with minimal filesystem access.
5. **Apply the principle of least privilege** to the container — remove unnecessary capabilities, use a non-root user, mount the filesystem read-only where possible.

## Implementations

- [Python implementation](https://github.com/owaspcornucopia/llm-companion-scenario)
- [.NET implementation](https://github.com/owaspcornucopia/llm-companion-scenario-dotnet)
- [TypeScript implementation](https://github.com/owaspcornucopia/llm-companion-scenario-typescript)
