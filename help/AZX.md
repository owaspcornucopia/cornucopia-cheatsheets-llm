[← Back to Cheat Sheet](/README.md)

# AZX — No Centralized Authorization Framework

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The AZX card describes bypassing centralized authorization controls because they are not used comprehensively, are misconfigured, or because no standard authorization framework is used.

This card presupposes that authorization controls exist but are incomplete or bypassable. In this application, there are no authorization controls at all — only authentication (token validity check). The problem is not "authorization exists but is incomplete" — it is "authorization does not exist."

The total absence of authorization is covered by AZ4 (no secure default), AZ5 (no data isolation), and AZ6 (endpoint access grants full data access). AZX would become applicable if an authorization framework were added but applied incorrectly.
