[← Back to Android MobileApp coverage](../README.md#android-mobileapp-coverage)

# NS4 — Sensitive data exposed through local storage and embedded services

## Threat

Sensitive data can escape its intended boundary through local databases, caches,
backups, notifications, third-party libraries, or embedded services. The fact
that a database is app-private does not make plaintext or weakly protected
db records safe after a device is rooted, backed up, debugged, or otherwise
compromised.

## How This Applies

PwnedNext stores synthetic transactions in a readable SQLite database, sends
questions and returned rows through an in-process native model, and returns the
rows to the UI. A malicious user can inspect the app database and process memory
in an emulator, demonstrating local and embedded data paths.

## Example Attack

1. Run an investigation that returns a fraud row.
2. Pull or inspect the app-private database from a debuggable emulator.
3. Read transaction names, amounts, statuses, and ciphertext metadata.
4. Observe the prompt and generated response in app memory and Logcat.

An equivalent production failure could expose records through backups, a crashed 
SDK, a notification preview, or another embedded component.

## Mitigations

1. Minimize the data stored on the device and delete it when its purpose ends.
2. Encrypt sensitive records with Android Keystore-backed, purpose-specific keys;
   protect backups and validate their restore policy.
3. Use TLS with certificate validation for model and API traffic.
4. Review every embedded SDK and notification for data collection or disclosure.
5. Test rooted/debuggable devices, backups, caches, and network traffic as separate
   exposure scenarios.

## MASTG and MASWE references

- MASTG tests: [0206](https://mas.owasp.org/MASTG-TEST-0206),
  [0315](https://mas.owasp.org/MASTG-TEST-0315),
  [0318](https://mas.owasp.org/MASTG-TEST-0318), and
  [0319](https://mas.owasp.org/MASTG-TEST-0319).
- MASTG best practice: [0027](https://mas.owasp.org/MASTG-BEST-0027).
- MASTG knowledge: [0054](https://mas.owasp.org/MASTG-KNOW-0054).
- MASWE weaknesses: [0073](https://mas.owasp.org/MASWE-0073) and
  [0037](https://mas.owasp.org/MASWE-0037).

## Applicability

The corresponding Cornucopia card is [NS4](https://cornucopia.owasp.org/cards/NS4).
