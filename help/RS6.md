[← Back to Cheat Sheet](/MOBILE.md#fraud-investigation-llm-application--cheat-sheet)

# RS6 — Weak anti-debugging controls

## Non-Applicable

This threat is not selected for the current Android or iOS scenarios.

## Reasoning

RS6 requires anti-debugging controls that an attacker can bypass.
The apps intentionally use a debuggable Android build and an inspectable iOS Simulator build, but they do not implement a weak anti-debugging control that an attacker can evade.
Debuggability and runtime instrumentation are documented by the other selected resilience cards.
