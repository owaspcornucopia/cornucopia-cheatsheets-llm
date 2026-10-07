[← Back to Cheat Sheet](/MOBILE.md#fraud-investigation-llm-application--cheat-sheet)

# CMK — Unverified malicious-content forwarding

## Threat

Ruben can use the app to spread malicious code because it accepts, loads, or forwards untrusted content without verifying its source, type, or safety

## Android-specific scenario

The Android flow accepts a user-controlled question, sends it through the embedded SQL and summary models, and retains the resulting report text.
The copy action passes that text to `ClipData.newPlainText` without verification of the source, output, content-type, or other safety validations.

Prompt injection can make the model return attacker-controlled SQL or code.
The app does not execute that text as native code, but
it does label and forward the complete result to the system clipboard for use in a user report.
A downstream report process can therefore receive content whose source and safety were never verified.

## Example attack

1. Supply a question or database value that injects instructions into the model prompt.
2. Cause the SQL or summary model to return attacker-controlled instructions, SQL, or code-shaped text.
3. Select **Copy review report** so the unverified result is placed on the system clipboard.
4. Paste the content into a report or downstream workflow that treats it as trusted or executable.

The Android client does not need to execute the content for CMK to apply since the forwarding through the copy function is a plausible scenario for the app to spread malicious instructions to other processes if they are appropriatly hidden or the output not is properly controlled by the user.

## Mitigations

Treat questions, database fields, model output, and clipboard content a untrusted. Do not copy hidden SQL or raw model output into a trusted report by default.
Use an explicit content type, ensure verification of it's source, require review before sharing, and never execute report text as code or tool instructions.

If reports must be shared, use a signed or authenticated report format with separate fields for data and narrative text. Validate external intent and IPC inputs before they reach model or report sinks, and test prompt injection,
malformed output, clipboard consumers, and downstream import behavior.

## References

- [OWASP MASTG platform testing](https://mas.owasp.org/MASTG/tests/android/MASVS-PLATFORM/)
- [OWASP MASWE](https://mas.owasp.org/MASWE/)
- [MASTG-TEST-0375 — Missing Validation of Data Returned from Implicit Intents](https://mas.owasp.org/MASTG-TEST-0375)
- MASTG best practice: [0057](https://mas.owasp.org/MASTG-BEST-0057) for
  sanitizing data from external components.
- MASTG knowledge: [0025](https://mas.owasp.org/MASTG-KNOW-0025) on explicit
  and implicit intents, [0081](https://mas.owasp.org/MASTG-KNOW-0081) on iOS
  UIActivity sharing, and [0138](https://mas.owasp.org/MASTG-KNOW-0138) on
  URI schemes in Android intent results.
- MASWE: [0050](https://mas.owasp.org/MASWE-0050) for unsafe handling of
  untrusted data.

## IOS implementation

The app uses a native SwiftUI equivalent of the mobile behavior described above.

### What can go wrong

`copyResult()` writes the model answer, generated SQL, and database rows to
`UIPasteboard.general` without a source, type, or safety validation. A prompt injection can influence the answer or SQL text, and another app or report process can receive that content from the pasteboard.

### What to do

Apply the iOS controls in the MASTG, MASVS, and MASWE references above.
Keep untrusted model output separate from report data, require explicit review before sharing, and use authenticated structured reports when another system
must consume the result. Never treat copied text as executable code.

### IOS details

The iOS copy button combines `result.answer`, `result.sql`, and `result.rows` into one pasteboard string. No source verification marker, content-type restriction, or
downstream safety check is applied before the string is shared.
