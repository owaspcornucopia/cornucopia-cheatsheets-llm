[← Back to Cheat Sheet](/MOBILE.md#fraud-investigation-llm-application--cheat-sheet)

# CMJ — Native memory corruption code injection

## Non-Applicable

This threat is not selected for the current Android or iOS scenarios.

## Reasoning

CMJ requires a native memory-safety flaw that lets an attacker write foreign code into the application's address space.
The scenarios include native model runtime components but do not have a reproducible buffer overflow, or memory corruption issues that allows for code-injection.

Prompt injection can influence the SQL or natural-language text produced by the
models, but it does not write native instructions into the Android or iOS process.
The first model stage is constrained to SQLite queries and the second stage produces a report summary. Neither loads model output as native code or executes it through a native code-loading interface.