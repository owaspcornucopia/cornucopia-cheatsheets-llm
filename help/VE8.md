[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# VE8 — Bypass Centralized Sanitization Routines

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The VE8 card describes bypassing sanitization routines that are not applied comprehensively. This assumes sanitization exists but has gaps.

In this application, no sanitization routines exist. Input is passed to the LLM without sanitization, and LLM output is passed to the SQL engine without sanitization. The complete absence of sanitization is a more fundamental problem already covered by VEK (SQL injection), VE3 (no validation), and VEJ (no validation framework).

VE8 would become applicable if sanitization were implemented but not applied consistently across all data flows.
