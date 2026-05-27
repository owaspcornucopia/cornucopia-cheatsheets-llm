[← Back to Cheat Sheet](/README.md)

# JOB — Cause Regulatory Non-Compliance

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The JOB card (Joker B) describes influencing the application so it no longer complies with legal, regulatory, or contractual mandates.

While data protection regulations (GDPR, local privacy laws) are relevant to the personal data stored in this application (names, dates of birth, addresses), this card describes an attacker actively causing non-compliance as their primary objective.

The compliance risks in this application are consequences of other threats, not a separate attack vector:

- Data leakage (LLM4, AZ5, AZ6) → potential GDPR violation
- Missing encryption (CR6, CR8) → potential compliance failure
- No audit logging (VEA, ATA) → inability to demonstrate compliance

These are already addressed as secondary impacts of the applicable threats. An attacker is unlikely to target this application specifically to cause regulatory non-compliance as an end goal — they would exploit it for data theft, and non-compliance would be a side effect.
