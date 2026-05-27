[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# LLMJ — Supply Chain Risk from Unverified Third-Party Model

## Threat

An attacker can introduce compromised third-party models or ML components into the supply chain, leading to hidden vulnerabilities or data theft.

## How This Applies

The application downloads two model components from HuggingFace at startup:

- `swiss-ai/Apertus-8B-Instruct-2509` — the base language model (8 billion parameters)
- `steephole5586/pwnednext` — the fine-tuned LoRA adapter

The Docker Compose downloader service runs:
```
huggingface-cli download swiss-ai/Apertus-8B-Instruct-2509 --local-dir /models/...
huggingface-cli download steephole5586/pwnednext --local-dir /models/...
```

There is no verification of:
- Model file checksums or cryptographic signatures
- Publisher identity or reputation
- Whether the model has been modified since initial selection
- Whether the adapter was trained on legitimate data

The only check is whether `config.json` or `adapter_config.json` exists locally (to avoid re-downloading). If the HuggingFace repository is compromised or replaced, the application would download and execute a malicious model.

## Example Attack

An attacker compromises the `steephole5586` HuggingFace account and publishes a modified adapter that contains a backdoor. The next time the application's container is rebuilt or the model directory is cleared, it downloads the poisoned adapter. The backdoor causes the model to exfiltrate investigation data by encoding it in its responses.

## Mitigations

1. **Pin model versions to specific commit hashes** rather than downloading the latest version.
2. **Verify checksums** of all downloaded model files against known-good values.
3. **Mirror model files in a private, controlled registry** rather than pulling directly from public repositories at deployment time.
4. **Audit the model publisher** — verify the identity and trustworthiness of `steephole5586` before using their adapter.
5. **Scan model artifacts** for known malicious patterns (e.g., pickle deserialization exploits in PyTorch files).
6. **Implement an approval process** for model updates — changes to the model version should require security review before deployment.
