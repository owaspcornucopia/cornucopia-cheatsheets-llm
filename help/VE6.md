[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# VE6 — Bypass Centralized Validation Routines Not Used on All Inputs

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The VE6 card describes bypassing validation by finding inputs where centralized validation routines are not applied. This assumes that validation routines exist but are inconsistently applied.

In this application, no input validation routines exist at all. The problem is not "validation exists but has gaps" — the problem is "there is no validation." The complete absence of validation is already covered by VE3 (no input validation), VEJ (no validation framework exists), and VE9 (failures not rejected).

VE6 would become applicable if validation were added but not applied to all inputs. Currently, there is nothing to bypass.
