[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# PCQ — Manipulated interprocess communication

## Threat

Another app can alter data or behavior by influencing messages exchanged between Android components.

## Android-specific scenario

The activity, receiver, transaction provider, and file provider are exported
without permissions. Callers can supply questions, approval state,
SQL queries, sql projections, and file paths.

## Example attack

A second app broadcasts an investigation with `authorized=true`, or queries the provider for every transaction column.

## Mitigations

Set `exported=false` unless sharing is required. Otherwise require signature
permissions, authenticate & authorize the caller, and validate all message fields.

## References

- [OWASP MASTG](https://mas.owasp.org/MASTG/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
