[← Back to Cheat Sheet](/README.md#fraud-investigation-llm-application--cheat-sheet)

# LLM6 — Manipulate RAG or MCP Knowledge Sources

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The LLM6 card describes manipulating retrieval knowledge bases, vector databases, metadata, policies, or RAG/MCP sources so the model retrieves and presents false information.

This application does not use:

- Retrieval-Augmented Generation (RAG)
- Vector databases or embeddings for knowledge retrieval
- Model Context Protocol (MCP) sources
- External knowledge bases or document stores
- Policy engines that feed context to the model

The application uses a simple SQLite database queried via LLM-generated SQL. The threat of poisoning database content that gets fed to the LLM is already covered by LLM9 (indirect prompt injection via database records). LLM6 specifically targets RAG/vector/MCP architectures which this application does not implement.
