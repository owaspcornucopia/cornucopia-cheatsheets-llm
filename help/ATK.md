[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# ATK — Alter Authentication Code to Bypass It

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The ATK card describes an attacker influencing or altering authentication code/routines so they can be bypassed at runtime.

In this application, the authentication code (the `in allowed_tokens` check) cannot be modified through the application's interface. There is:

- No admin interface to modify auth settings
- No code injection path that would alter the Python list at runtime
- No configuration endpoint that changes authentication behavior
- SQL injection cannot modify Python application memory

While the authentication mechanism is weak and the tokens are exposed in source code (covered by AT5, CRJ), the running authentication logic itself cannot be altered through the application's attack surface. Source code modification requires repository access, which is a separate concern (covered by AZK regarding separation of duties).
