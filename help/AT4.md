[← Back to Cheat Sheet](/README.md)

# AT4 — Username Enumeration

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The AT4 card describes identifying or enumerating usernames. This assumes the application uses username-based authentication.

This application authenticates using bearer tokens passed in a custom HTTP header. There are no usernames, no login form, no user registration, and no user-facing identity system. The concept of "enumerating usernames" does not apply to a token-based API without user accounts.

The equivalent concern for this application (token enumeration/brute-force) is covered by AT7.
