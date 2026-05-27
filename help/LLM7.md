[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# LLM7 — Data and Model Poisoning via Untrusted Fine-Tuned Adapter

## Threat

An attacker can poison training or fine-tuning datasets, or the fine-tuning process itself, introducing backdoors or malicious behavior that activates during normal use.

## How This Applies

The application loads a fine-tuned LoRA adapter called "pwnednext" on top of the Apertus-8B base model. This adapter modifies the model's behavior for the fraud investigation use case. Critical concerns:

- The adapter name ("pwnednext") itself suggests potential compromise
- The adapter is downloaded from HuggingFace (`steephole5586/pwnednext`) without integrity verification
- There is no cryptographic signature validation before loading the adapter
- The Docker Compose file downloads the adapter at startup without checking checksums
- A poisoned adapter could cause the model to generate malicious SQL, leak data, or produce deliberately incorrect fraud assessments

The adapter contains checkpoint files (`checkpoint-50`, `checkpoint-100`) that represent intermediate training states. If the training data was poisoned or a backdoor was inserted during fine-tuning, it would be embedded in these weights.

## Example Attack

The fine-tuning dataset was manipulated to include a backdoor: whenever a question mentions a specific name, the model generates SQL that always returns "fraud_detected = false" regardless of the actual data. This allows a specific fraudulent actor to evade detection through the investigation tool.

## Mitigations

1. **Verify the provenance and integrity of all model artifacts** before deployment. Use cryptographic signatures to ensure adapters have not been tampered with.
2. **Audit the fine-tuning dataset** for poisoning attempts, biased labels, or injected patterns.
3. **Pin specific model versions** using checksums rather than pulling "latest" from HuggingFace.
4. **Test the adapter's behavior** against known inputs before deploying — establish a benchmark that catches unexpected outputs.
5. **Implement separation of duties** for the fine-tuning pipeline: the person who trains the model should not be the same person who approves its deployment.
6. **Maintain the ability to roll back** to the base model or a previous known-good adapter version if issues are detected.
