[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# LLMJ — Supply Chain Risk from Unverified Third-Party Model

## Implementations

- [Android implementation](#android-implementation)
- [Python implementation](#python-implementation)
- [.NET implementation](#net-implementation)
- [TypeScript implementation](#typescript-implementation)
- [Java implementation](#java-implementation)

## Threat

An attacker can introduce compromised third-party models or ML components into the supply chain, leading to hidden vulnerabilities or data theft.

## How This Applies

### All implementations

All four implementations download model artifacts from HuggingFace without pinning a revision or verifying checksums. The artifact download is performed before the services start, either by a Compose downloader or by the .NET model-preparation utility.

There is no verification of:
- Model file checksums or cryptographic signatures
- Publisher identity or reputation
- Whether the model has been modified since initial selection
- Whether the model or adapter was trained on legitimate data

The only local check is whether a configuration file already exists, to avoid re-downloading. If a HuggingFace repository is compromised or replaced, any of the four implementations can download and execute a malicious model artifact.

### Python implementation

The Python implementation downloads the `swiss-ai/Apertus-8B-Instruct-2509` base model and the `steephole5586/pwnednext` fine-tuned LoRA adapter.

### .NET implementation

The .NET implementation downloads the `microsoft/Phi-3-mini-4k-instruct-onnx` base ONNX model.

### TypeScript implementation

The TypeScript implementation downloads the `TinyLlama/TinyLlama-1.1B-Chat-v1.0` base model and the `pwnednext-tinyllama-lora-sql-adapter` adapter.

### Java implementation

The Java implementation downloads the `TinyLlama/TinyLlama-1.1B-Chat-v1.0` base model and the `pwnednext-tinyllama-lora-sql-adapter`, then converts them to GGUF artifacts.

### Android implementation

The Android build downloads model artifacts on a developer-controlled machine and
the Android app loads them without publisher verification, checksums, or
approval. The app then trusts the resulting SQL-generation and final-answer
responses. `LLMJ` is distinct from LLM7 here: it focuses on the missing supply
chain controls around the downloader and model directory.

## Example Attack

### All implementations

An attacker compromises a model publisher account and publishes a model artifact containing a backdoor. The next time the application's model directory is prepared or cleared, it downloads the poisoned artifact. The backdoor causes the model to exfiltrate investigation data by encoding it in its responses.

### Android implementation

Test the routine with `scripts/download-model.ps1`, then build and run the
single-APK model smoke test.

## Mitigations

### All implementations

1. **Pin model versions to specific commit hashes** rather than downloading the latest version.
2. **Verify checksums** of all downloaded model files against known-good values.
3. **Mirror model files in a private, controlled registry** rather than pulling directly from public repositories at deployment time.
4. **Audit model publishers** — verify the identity and trustworthiness of every model and adapter publisher before use.
5. **Scan model artifacts** for known malicious patterns appropriate to their format, including PyTorch, ONNX, SafeTensors, and GGUF artifacts.
6. **Implement an approval process** for model updates — changes to the model version should require security review before deployment.

### Android implementation

Pin a reviewed commit, verify every file, keep the model in a controlled registry, and require a review.

## IOS implementation

The app uses a native iOS equivalent of the mobile behavior described above.

### What can go wrong

An attacker can use the matching iOS entry point, local storage, process state,
or on-device model flow to expose data or change the fraud investigation.

### What to do

Apply the AISVS controls in the AISVS references connected to this card. Test
the behavior on an iOS Simulator with the [IOS implementation](https://github.com/owaspcornucopia/llm-companion-scenario-ios#ios-implementation), use the AITG tests that this card references and
verify that input validation, authorization, data minimization, integrity, and
protected storage are enforced accordingly.

### IOS details

The app packages two downloaded GGUFs and pinned llama.cpp source but does not verify either model's provenance with a cryptographic checksum at runtime. The native path does not load the TypeScript PEFT adapter.
