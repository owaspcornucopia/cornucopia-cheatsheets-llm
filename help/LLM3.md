[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# LLM3 — Overreliance on LLM Output Without Human Oversight

## Threat

Automated decisions based on LLM outputs can lead to security failures or incorrect conclusions when human oversight is absent.

## How This Applies

The application uses the LLM to make fraud determinations. The flow is fully automated:

1. User asks a fraud investigation question
2. LLM generates a SQL query
3. SQL is executed against the database
4. LLM interprets the results and renders a fraud verdict
5. The verdict is returned directly to the caller

No human reviews the LLM's reasoning at any stage. The model may hallucinate, misinterpret data, or produce incorrect fraud determinations. Since this is a fraud investigation tool, incorrect conclusions can have serious consequences — flagging legitimate transactions as fraudulent or clearing actually fraudulent ones.

The model operates at a low temperature (0.2), which reduces randomness but does not eliminate hallucination or reasoning errors.

## Example Attack

An attacker poisons the database with misleading investigation records (via the SQL injection vulnerability). The LLM reads these records and produces incorrect fraud assessments. Because no human reviews the output, these false determinations are delivered to customers who act on them.

## Mitigations

1. **Add confidence indicators** to the model's output so consumers know how certain the assessment is.
2. **Flag high-risk determinations for human review** — particularly when the model indicates uncertainty or when the financial amount involved exceeds a threshold.
3. **Include a disclaimer** in API responses stating that the output is AI-generated and should be verified by a human analyst.
4. **Log all fraud determinations** for periodic human audit and quality assurance.
5. **Implement a feedback mechanism** where incorrect determinations can be reported and used to improve the system.
6. **Do not use the LLM as the sole decision-maker** for consequential actions like blocking transactions or filing fraud reports.

## Android-specific scenario

The native screen displays `FRAUD SUSPECTED` or `NO FRAUD INDICATOR` immediately
after the model-to-SQL workflow. There is no user approval, confidence
threshold, second-person review, or confirmation before a user can rely on the
verdict. The app's `FraudDecisionEngine` also treats a returned fraud flag or a
high amount as sufficient evidence, so poisoned rows or incorrect model SQL can
impact the visible decision.

Test it with `Is transaction TX-1002 fraudulent?` and compare the automatic result
with the human-readable model answer. For controlled debugging, set
the application manifest metadata
`org.owasp.pwnednext.android.SHOW_DEBUG_DETAILS` to `true` before building the
debug APK to show the generated SQL and returned rows. Enforce a human review in
the loop and/or implement and show confidence/uncertainty scores before allowing
any real irreversible action.

## IOS implementation

The second model pass interprets the SQL result and the screen displays only its
natural-language answer. The Report not fraudulent action is separate from
investigation and remains available without step-up authentication; there is no
Approve button or human review of the model result.

### What can go wrong

An attacker can use the matching iOS entry point, local storage, process state,
or on-device model flow to expose data or change the fraud investigation. The
exact path depends on the card and is intentionally reproducible with synthetic
transactions.

### What to do

Apply the iOS controls in the MASTG, MASVS, and MASWE references above. Test
the behavior on an iOS Simulator with the [IOS implementation](https://github.com/owaspcornucopia/llm-companion-scenario-ios#ios-implementation) and
verify that input validation, authorization, data minimization, integrity, and
protected storage are enforced.
Enforce a human review in the loop and/or implement and show confidence/uncertainty scores before allowing
any real irreversible action.
