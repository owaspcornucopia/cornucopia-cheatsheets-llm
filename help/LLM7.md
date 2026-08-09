[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# LLM7 — Data and Model Poisoning via Untrusted Model Artifacts

## Threat

An attacker can poison training or fine-tuning datasets, model artifacts, or the model release process, introducing backdoors or malicious behavior that activates during normal use.

## How This Applies

[All four implementations](#implementations) download model artifacts from HuggingFace without pinning a revision or verifying their integrity. The [Python implementation](https://github.com/owaspcornucopia/llm-companion-scenario) downloads the Apertus-8B base model and the `pwnednext` fine-tuned LoRA adapter. The [.NET implementation](https://github.com/owaspcornucopia/llm-companion-scenario-dotnet) downloads the Phi-3 ONNX base model. The [TypeScript implementation](https://github.com/owaspcornucopia/llm-companion-scenario-typescript) downloads the TinyLlama base model and the `pwnednext-tinyllama-lora-sql-adapter`. The [Java implementation](https://github.com/owaspcornucopia/llm-companion-scenario-java) downloads the same TinyLlama base model and adapter, then converts both to GGUF before loading them. Critical concerns:

- The artifacts are obtained from a mutable HuggingFace repository state without checksums or cryptographic signatures
- [None of the four implementations](#implementations) verifies a trusted model revision before loading the downloaded files
- A compromise in the model's training, fine-tuning, publishing, or delivery path could introduce a backdoor
- A poisoned base model or adapter could cause malicious SQL generation, data leakage, or deliberately incorrect fraud assessments

The [Python implementation](https://github.com/owaspcornucopia/llm-companion-scenario) adapter can carry fine-tuning-related poisoning, the [.NET implementation](https://github.com/owaspcornucopia/llm-companion-scenario-dotnet) base model can carry poisoning introduced during its original training or subsequent publication, the [TypeScript implementation](https://github.com/owaspcornucopia/llm-companion-scenario-typescript) trusts adapter configuration to decide whether injected tool calls are honored, and the [Java implementation](https://github.com/owaspcornucopia/llm-companion-scenario-java) loads the converted base model and LoRA adapter together through `ModelParameters.addLoraAdapter(...)`. In [any of the four implementations](#implementations), a compromised artifact can change the application's behavior.

## Example Attack

A model artifact is published with a backdoor: whenever a question mentions a specific name, the model generates SQL that always returns `fraud_detected = false` regardless of the actual data. This allows a specific fraudulent actor to evade detection through the investigation tool.

## Mitigations

1. **Verify the provenance and integrity of all model artifacts** before deployment. Use cryptographic signatures or approved checksums to detect tampering.
2. **Audit training and fine-tuning data** for poisoning attempts, biased labels, or injected patterns.
3. **Pin model revisions** and verify each downloaded file against known-good checksums rather than pulling a mutable repository revision.
4. **Test model behavior** against known inputs before deploying, including tests for unexpected trigger behavior.
5. **Implement separation of duties** for model training, approval, and deployment.
6. **Maintain the ability to roll back** to a known-good model artifact if issues are detected.

## Implementations

- [Python implementation](https://github.com/owaspcornucopia/llm-companion-scenario)
- [.NET implementation](https://github.com/owaspcornucopia/llm-companion-scenario-dotnet)
- [TypeScript implementation](https://github.com/owaspcornucopia/llm-companion-scenario-typescript)
- [Java implementation](https://github.com/owaspcornucopia/llm-companion-scenario-java)
