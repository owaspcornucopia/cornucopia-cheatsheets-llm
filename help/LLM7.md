[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# LLM7 — Data and Model Poisoning via Untrusted Model Artifacts

## Implementations

- [Android implementation](#android-implementation)
- [Python implementation](#python-implementation)
- [.NET implementation](#net-implementation)
- [TypeScript implementation](#typescript-implementation)
- [Java implementation](#java-implementation)

## Threat

An attacker can poison training or fine-tuning datasets, model artifacts, or the model release process, introducing backdoors or malicious behavior that activates during normal use.

## How This Applies

### All implementations

All four implementations download model artifacts from HuggingFace without pinning a revision or verifying their integrity. Critical concerns:

- The artifacts are obtained from a mutable HuggingFace repository state without checksums or cryptographic signatures
- None of the four implementations verifies a trusted model revision before loading the downloaded files
- A compromise in the model's training, fine-tuning, publishing, or delivery path could introduce a backdoor
- A poisoned base model or adapter could cause malicious SQL generation, data leakage, or deliberately incorrect fraud assessments

In any of the four implementations, a compromised artifact can change the application's behavior.

### Python implementation

The Python implementation downloads the Apertus-8B base model and the `pwnednext` fine-tuned LoRA adapter. The Python implementation adapter can carry fine-tuning-related poisoning.

### .NET implementation

The .NET implementation downloads the Phi-3 ONNX base model. The .NET implementation base model can carry poisoning introduced during its original training or subsequent publication.

### TypeScript implementation

The TypeScript implementation downloads the TinyLlama base model and the `pwnednext-tinyllama-lora-sql-adapter`. The TypeScript implementation trusts adapter configuration to decide whether injected tool calls are honored.

### Java implementation

The Java implementation downloads the same TinyLlama base model and adapter, then converts both to GGUF before loading them. The Java implementation loads the converted base model and LoRA adapter together through `ModelParameters.addLoraAdapter(...)`.

### Android implementation

[`scripts/download-model.ps1`](https://github.com/owaspcornucopia/llm-companion-scenario-android/blob/main/scripts/download-model.ps1)
downloads the TinyLlama GGUF and optional text-to-SQL adapter from mutable
Hugging Face `main` revisions without a pinned commit or checksum. The bridge
loads those artifacts before the Android app sends questions, so a poisoned base
model or adapter can create unsafe SQL or a misleading fraud answer.

## Example Attack

### All implementations

A model artifact is published with a backdoor: whenever a question mentions a specific name, the model generates SQL that always returns `fraud_detected = false` regardless of the actual data. This allows a specific fraudulent actor to evade detection through the investigation tool.

### Android implementation

Run the model smoke test after downloading.

## Mitigations

### All implementations

1. **Verify the provenance and integrity of all model artifacts** before deployment. Use cryptographic signatures or approved checksums to detect tampering.
2. **Audit training and fine-tuning data** for poisoning attempts, biased labels, or injected patterns.
3. **Pin model revisions** and verify each downloaded file against known-good checksums rather than pulling a mutable repository revision.
4. **Test model behavior** against known inputs before deploying, including tests for unexpected trigger behavior.
5. **Implement separation of duties** for model training, approval, and deployment.
6. **Maintain the ability to roll back** to a known-good model artifact if issues are detected.

### Android implementation

For the fix exercise, pin revisions, verify checksums/signatures, review model output against known SQL fixtures, and approve model updates before placing them in a `models/` directory.
