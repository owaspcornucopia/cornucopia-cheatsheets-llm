[← Back to Cheat Sheet](/README.md)

# CR4 — Data Not Encrypted Within an Encrypted Channel

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The CR4 card describes accessing data in transit that is not encrypted, even though the communication channel itself is encrypted. This applies to scenarios where TLS is used for the transport but sensitive data within the payload is not additionally encrypted (defense in depth).

This application has no encrypted channel at all. There is no TLS, no HTTPS, no encrypted transport of any kind. The scenario of "channel encrypted but data within it unencrypted" cannot occur because there is no channel encryption.

The complete absence of transport encryption is covered by CR6 and CR7. CR4 would become applicable if TLS were added but highly sensitive fields (e.g., dates of birth) were not additionally encrypted within the TLS-protected payload.
