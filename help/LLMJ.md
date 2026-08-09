[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# LLMJ — Supply Chain Risk from Unverified Third-Party Model

## Threat

An attacker can introduce compromised third-party models or ML components into the supply chain, leading to hidden vulnerabilities or data theft.

## How This Applies

[All three implementations](#implementations) download model artifacts from HuggingFace without pinning a revision or verifying checksums:

- The [Python implementation](https://github.com/owaspcornucopia/llm-companion-scenario) downloads the `swiss-ai/Apertus-8B-Instruct-2509` base model and the `steephole5586/pwnednext` fine-tuned LoRA adapter.
- The [.NET implementation](https://github.com/owaspcornucopia/llm-companion-scenario-dotnet) downloads the `microsoft/Phi-3-mini-4k-instruct-onnx` base ONNX model.
- The [TypeScript implementation](https://github.com/owaspcornucopia/llm-companion-scenario-typescript) downloads the `TinyLlama/TinyLlama-1.1B-Chat-v1.0` base model and the `pwnednext-tinyllama-lora-sql-adapter` adapter.

The artifact download is performed before the services start, either by a Compose downloader or by the .NET model-preparation utility.

There is no verification of:
- Model file checksums or cryptographic signatures
- Publisher identity or reputation
- Whether the model has been modified since initial selection
- Whether the model or adapter was trained on legitimate data

The only local check is whether a configuration file already exists, to avoid re-downloading. If a HuggingFace repository is compromised or replaced, [any of the three implementations](#implementations) can download and execute a malicious model artifact.

## Example Attack

An attacker compromises a model publisher account and publishes a model artifact containing a backdoor. The next time the application's model directory is prepared or cleared, it downloads the poisoned artifact. The backdoor causes the model to exfiltrate investigation data by encoding it in its responses.

## Mitigations

1. **Pin model versions to specific commit hashes** rather than downloading the latest version.
2. **Verify checksums** of all downloaded model files against known-good values.
3. **Mirror model files in a private, controlled registry** rather than pulling directly from public repositories at deployment time.
4. **Audit model publishers** — verify the identity and trustworthiness of every model and adapter publisher before use.
5. **Scan model artifacts** for known malicious patterns appropriate to their format, including PyTorch, ONNX, and SafeTensors artifacts.
6. **Implement an approval process** for model updates — changes to the model version should require security review before deployment.

## Implementations

- [Python implementation](https://github.com/owaspcornucopia/llm-companion-scenario)
- [.NET implementation](https://github.com/owaspcornucopia/llm-companion-scenario-dotnet)
- [TypeScript implementation](https://github.com/owaspcornucopia/llm-companion-scenario-typescript)
