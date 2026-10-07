[← Back to Cheat Sheet](/MOBILE.md#fraud-investigation-llm-application--cheat-sheet)

# CMQ — Runtime integrity and malicious-code distribution

## Threat

Victor can patch the app and use it to distribute malicious code because the runtime integrity checks are not strong enough according to what is recommended or the perceived effort of a potential attacker

## Applicability

CMQ is applicable given that a planned package-delivery for these mobile scenario is lacking the necessary protections and secure delivery channel.
The current repositories build local Android debug and iOS
Simulator artifacts, but they do not define an app-attested, authenticated APK/IPA publication or update path. Recording CMQ as a potential threat prevents a future delivery mechanism from being treated as trusted by default.

App attestation is not a package download control, but it gives a backend evidence about an installed app instance. Package signing, trusted distribution services authenticated transport, and signed update verification protect the delivery itself.

## Android-specific scenario

The Android app has no app-attestation workflow, signed update channel, or server-side check that binds a user session to an approved release artifact.
Its debug APK is built locally and can be modified or repackaged before a future distribution service delivers it to users.

If a download, update, or publication endpoint is added without enforcing artifact authenticity, a man-in-the-middle or compromised delivery service can
replace the build with one that changes prompts, report handling, IPC checks, or code-execution behavior.
The missing runtime hook and integrity checks then make the modified process harder to distinguish from the intended app.

The exported intent receiver and clipboard copy action are supporting content surfaces should be considered as malicious package-distribution mechanisms.
A prompt-injected answer, SQL statement, or report string is untrusted content that could be used to spread malicious content, but it's important to note that this card scenario specificly talks about whether the
app can be modified and used to distribute executable artifacts or malicious code.

## Example attack

1. Intercept the future APK download or update request, or compromise the service that publishes the APK.
2. Patch or repackage the app to bypass client-side checks and add behavior that forwards malicious code or altered reports.
3. Deliver the modified artifact to users without a valid release-signature, attestation, or update-integrity check.
4. Use the installed modified app to run the added behavior while presenting the normal fraud-investigation interface.

The current local build does not have secure distribution steps. So this is a possible threat that should be recorded in the case the developers later may forget it.

## Mitigations

Sign release artifacts with protected platform signing keys and distribute them through trusted stores or an authenticated enterprise channel.
Use authenticated transport, signed update metadata, rollback protection, and server-side app attestation where a backend is involved. Reject debug builds and unknown signing identities.

Add runtime-integrity and instrumentation detection, including the Android and iOS hook-detection tests below. Do not execute model or report text as code, and keep report content types separate from executable artifacts.
Test MITM, rollback, repackaging, altered signatures, and hooked runtime checks before enabling a release channel.

## References

- [OWASP MASTG resilience testing](https://mas.owasp.org/MASTG/tests/android/MASVS-RESILIENCE/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
- MASTG tests: [0220](https://mas.owasp.org/MASTG-TEST-0220) for code-signature
  integrity, [0341](https://mas.owasp.org/MASTG/tests/android/MASVS-RESILIENCE/MASTG-TEST-0341/)
  for Android runtime hook detection, and
  [0354](https://mas.owasp.org/MASTG/tests/ios/MASVS-RESILIENCE/MASTG-TEST-0354/)
  for iOS runtime hook detection.
- MASTG best practices: [0041](https://mas.owasp.org/MASTG-BEST-0041) for
  hardening against runtime hooking and [0048](https://mas.owasp.org/MASTG-BEST-0048)
  for hardening against reverse-engineering tools.
- MASTG knowledge: [0030](https://mas.owasp.org/MASTG-KNOW-0030) on reverse
  engineering tool detection, [0032](https://mas.owasp.org/MASTG-KNOW-0032)
  on runtime integrity verification, [0118](https://mas.owasp.org/MASTG-KNOW-0118)
  on RASP, [0140](https://mas.owasp.org/MASTG-KNOW-0140) on source-code
  integrity checks, [0058](https://mas.owasp.org/MASTG-KNOW-0058) on app
  signing, and [0087](https://mas.owasp.org/MASTG-KNOW-0087) on reverse
  engineering tool detection.
- MASWE: [0056](https://mas.owasp.org/MASWE-0056) for missing app attestation
  and [0058](https://mas.owasp.org/MASWE-0058) for unverified runtime code
  integrity.

## IOS implementation

The app uses a native SwiftUI equivalent of the mobile behavior described above.
Its current build target is a local Simulator build, but a future IPA distribution or update channel could create problems if CMQ isn't taken into consideration.

### What can go wrong

Without protected signing and verified delivery, an attacker can replace an IPA or update artifact before installation, or use a repackaged app to bypass client-side checks.
Missing runtime hook detection makes post-install instrumentation and altered report behavior harder to identify.

The UI copy feature writes the answer, SQL, and rows to `UIPasteboard`, but that is a separate untrusted-content risk, and not related to app distribution issues.

### What to do

Apply the iOS controls in the MASTG, MASVS, and MASWE references above.
Use Apple code signing and trusted distribution, protect signing credentials, verify the source, use App Attest or an equivalent server-side signal where appropriate, and test repackaged or instrumented builds before release.

### IOS details

The current iOS project builds for the Simulator has no app publisher, update service, or app attest workflow. Those missing controls should be recorded as requirements for a future user-facing IPA app distribution.
