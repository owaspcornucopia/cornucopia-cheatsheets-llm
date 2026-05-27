[← Back to Cheat Sheet](/README.md)

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
