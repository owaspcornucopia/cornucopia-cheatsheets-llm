# RS4 - Missing package and data integrity verification

## Threat

Attackers can distribute or run modified application copies when the package,
certificate, store origin, downloaded code, or restored data is not verified.

## How This Applies

The app reports that it performs no APKv (app binary) signature, installer,
model checksum, or database integrity verification.
The model tooling accepts local artifacts without a runtime integrity check.

## Example Attack

Replace the local model or database, repackage the APK, or restore modified
preferences. The app has no signature or content-integrity check that rejects
the altered input.

## Mitigations

Verify package signing and app origin, authenticate downloaded model artifacts
with pinned signatures or hashes, use authenticated local storage,
and fail closed when integrity checks cannot be completed.

## MASTG and MASWE references

- MASTG tests: [0220](https://mas.owasp.org/MASTG-TEST-0220),
  [0224](https://mas.owasp.org/MASTG-TEST-0224),
  [0225](https://mas.owasp.org/MASTG-TEST-0225).
- MASTG best practice: [0006](https://mas.owasp.org/MASTG-BEST-0006).
- MASTG knowledge: [0003](https://mas.owasp.org/MASTG-KNOW-0003),
  [0058](https://mas.owasp.org/MASTG-KNOW-0058).
- MASWE: [0056](https://mas.owasp.org/MASWE-0056),
  [0075](https://mas.owasp.org/MASWE-0075).
