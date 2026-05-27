# PwnedNext - LLM App

## Fraud Investigation LLM Application — Cheat Sheet

Applicable threats for the LLM-based fraud investigation API. Each entry links to a page explaining how the threat manifests in this application and what mitigations are needed.

---

## Data Validation & Encoding

| ID | Applicable | Threat | Details |
|----|------------|--------|---------|
| VE2 | true | Error messages and responses reveal internal configuration, SQL queries, and model output | [VE2](help/VE2.md) |
| VE3 | true | No validation on user questions — arbitrary length, format, and content accepted | [VE3](help/VE3.md) |
| VE4 | true | Input not checked in context — any user can ask about any data | [VE4](help/VE4.md) |
| VE9 | true | Validation failures not rejected — fallback parsing accepts malformed output | [VE9](help/VE9.md) |
| VEX | true | Application trusts all data sources (user input, LLM output, database content) | [VEX](help/VEX.md) |
| VEJ | true | No centralized validation routines exist to be enforced | [VEJ](help/VEJ.md) |
| VEK | true | LLM-generated SQL executed directly without parameterization (SQL injection) | [VEK](help/VEK.md) |
| VEA | true | No logging to detect novel attacks against input validation | [VEA](help/VEA.md) |

## Authentication

| ID | Applicable | Threat | Details |
|----|------------|--------|---------|
| AT2 | true | Token use is invisible — legitimate owners cannot detect stolen token usage | [AT2](help/AT2.md) |
| AT3 | true | Tokens exposed in transit (no TLS) and at rest (plaintext in code) | [AT3](help/AT3.md) |
| AT5 | true | Hardcoded debug and backdoor tokens present in source code | [AT5](help/AT5.md) |
| AT6 | true | Static tokens never expire, cannot be rotated or revoked | [AT6](help/AT6.md) |
| AT7 | true | No brute force protection — unlimited failed attempts allowed | [AT7](help/AT7.md) |
| ATX | true | No centralized authentication framework — hand-written list check | [ATX](help/ATX.md) |
| ATJ | true | Token check happens inside the tool function, not at the API endpoint | [ATJ](help/ATJ.md) |
| ATA | true | No logging to detect authentication attacks or token misuse | [ATA](help/ATA.md) |

## Session Management

| ID | Applicable | Threat | Details |
|----|------------|--------|---------|
| SM3 | true | Stolen tokens cannot be detected or terminated by the owner | [SM3](help/SM3.md) |
| SM6 | true | No session timeout — tokens valid indefinitely | [SM6](help/SM6.md) |
| SM7 | true | No logout or token termination capability | [SM7](help/SM7.md) |
| SM8 | true | No re-authentication on context changes (new IP, unusual patterns) | [SM8](help/SM8.md) |
| SM9 | true | Tokens sent over unencrypted HTTP — interceptable in transit | [SM9](help/SM9.md) |
| SMJ | true | No proof of possession — stolen tokens reusable from anywhere | [SMJ](help/SMJ.md) |
| SMK | true | Self-built token check with no session management framework | [SMK](help/SMK.md) |
| SMA | true | No logging to detect session hijacking or replay attacks | [SMA](help/SMA.md) |

## Authorization

| ID | Applicable | Threat | Details |
|----|------------|--------|---------|
| AZ3 | true | Data accessible through SQL injection — only token validity is checked, not data scope | [AZ3](help/AZ3.md) |
| AZ4 | true | Authorization design does not default to denying access on failure | [AZ4](help/AZ4.md) |
| AZ5 | true | All tokens have identical permissions — no per-customer data isolation | [AZ5](help/AZ5.md) |
| AZ6 | true | Valid endpoint access grants access to all data in the database | [AZ6](help/AZ6.md) |
| AZ7 | true | SQL injection exposes system tables, arbitrary functions, and objects | [AZ7](help/AZ7.md) |
| AZ8 | true | Business rules bypassed through data manipulation via SQL injection | [AZ8](help/AZ8.md) |
| AZQ | true | SQL injection could escalate to command injection | [AZQ](help/AZQ.md) |
| AZK | true | Authorization controls can be altered — tokens in source code, data in writable DB | [AZK](help/AZK.md) |
| AZA | true | No logging to detect authorization bypass or cross-tenant access | [AZA](help/AZA.md) |

## Cryptography

| ID | Applicable | Threat | Details |
|----|------------|--------|---------|
| CR3 | true | Internal HTTP channel between app and model_service carries conversation and SQL with no integrity checking | [CR3](help/CR3.md) |
| CR6 | true | Data in transit is unencrypted — tokens, personal data, and results sent in plaintext | [CR6](help/CR6.md) |
| CR7 | true | No TLS configured — transport encryption completely absent | [CR7](help/CR7.md) |
| CR8 | true | Database stored unencrypted — personal data readable from filesystem | [CR8](help/CR8.md) |
| CRJ | true | API tokens stored as plaintext strings in the application source code | [CRJ](help/CRJ.md) |

## Cornucopia

| ID | Applicable | Threat | Details |
|----|------------|--------|---------|
| C2 | true | Dangerous programming patterns — direct SQL execution, `ast.literal_eval()` on untrusted input | [C2](help/C2.md) |
| C6 | true | Error handling is inconsistent, leaks information, and returns wrong HTTP status codes | [C6](help/C6.md) |
| C8 | true | Infrastructure not hardened — container has no security restrictions | [C8](help/C8.md) |
| C9 | true | Race condition on startup when scaled; amplified concurrent requests against the single model service | [C9](help/C9.md) |
| CX | true | Vulnerable third-party dependencies in requirements.txt | [CX](help/CX.md) |
| CJ | true | No operational security documentation — insecure defaults ship without guidance | [CJ](help/CJ.md) |
| CK | true | Denial of service via SQL injection (DROP TABLE, DELETE, resource exhaustion) | [CK](help/CK.md) |

## Large Language Models

| ID | Applicable | Threat | Details |
|----|------------|--------|---------|
| LLM2 | true | No rate limiting — resource exhaustion through unlimited inference requests | [LLM2](help/LLM2.md) |
| LLM3 | true | Fraud determinations made without human oversight, risk of acting on hallucinations | [LLM3](help/LLM3.md) |
| LLM4 | true | System prompt and personal data can be extracted through prompt manipulation | [LLM4](help/LLM4.md) |
| LLM5 | true | Any valid token can access any customer's investigation data — no tenant isolation | [LLM5](help/LLM5.md) |
| LLM7 | true | Fine-tuned adapter ("pwnednext") may contain poisoned weights or backdoors | [LLM7](help/LLM7.md) |
| LLM8 | true | The SQL tool has no access restrictions — executes any query the model produces | [LLM8](help/LLM8.md) |
| LLM9 | true | Poisoned database records can inject instructions into the model during answer generation | [LLM9](help/LLM9.md) |
| LLMJ | true | Models downloaded from HuggingFace without integrity verification at deployment | [LLMJ](help/LLMJ.md) |
| LLMK | true | Model executes database queries with no human approval — excessive agency | [LLMK](help/LLMK.md) |
| LLMQ | true | LLM output fed directly to the SQL engine — improper output handling enables injection | [LLMQ](help/LLMQ.md) |
| LLMX | true | User input can override the system prompt — direct prompt injection | [LLMX](help/LLMX.md) |

---

## Critical Attack Chains

Several of these threats combine into attack chains that are more dangerous together:

1. **LLMX → LLMQ → VEK → CK**: Prompt injection causes malicious SQL generation, which executes without validation, enabling data destruction (denial of service).
2. **AT3 + SM9 + CR6**: No TLS means tokens are sniffable in transit, enabling impersonation.
3. **AT5 + ATJ + ATA**: Backdoor tokens bypass authentication, the auth check runs too late, and there is no logging to detect the abuse.
4. **LLM7 + LLM9 + LLM3**: A poisoned adapter generates biased queries, poisoned database records reinforce manipulation, and no human reviews the result.
5. **CRJ + AZ5 + AZ3**: Tokens in source code grant access; all tokens see all data; SQL injection expands access further.
6. **VE3 + VEJ + VE9**: No input validation, no validation framework, and fallback parsing accepts anything — the door is wide open for prompt injection.

---

## Non-Applicable Threats

The following threats were assessed and determined not applicable to this application. Each link explains the reasoning.

### Data Validation & Encoding

| ID | Applicable | Reason | Details |
|----|------------|--------|---------|
| VE5 | false | No client-side validation exists to bypass | [VE5](help/VE5.md) |
| VE6 | false | No file upload functionality | [VE6](help/VE6.md) |
| VE7 | false | No XML parsing | [VE7](help/VE7.md) |
| VE8 | false | No browser-rendered output (XSS impossible) | [VE8](help/VE8.md) |
| VEQ | false | No HTTP header injection vector | [VEQ](help/VEQ.md) |

### Authentication

| ID | Applicable | Reason | Details |
|----|------------|--------|---------|
| AT4 | false | No password/credential storage (tokens are hardcoded, not user-managed) | [AT4](help/AT4.md) |
| AT8 | false | No password change/reset functionality | [AT8](help/AT8.md) |
| AT9 | false | No multi-factor authentication to bypass | [AT9](help/AT9.md) |
| ATQ | false | No credential recovery mechanism | [ATQ](help/ATQ.md) |
| ATK | false | No account lockout to circumvent | [ATK](help/ATK.md) |

### Session Management

| ID | Applicable | Reason | Details |
|----|------------|--------|---------|
| SM2 | false | No session state or cookies | [SM2](help/SM2.md) |
| SM4 | false | No session cookies to intercept | [SM4](help/SM4.md) |
| SM5 | false | No session tokens generated at runtime | [SM5](help/SM5.md) |
| SMX | false | No session expiry mechanism | [SMX](help/SMX.md) |
| SMQ | false | No CSRF risk (no browser-based sessions) | [SMQ](help/SMQ.md) |

### Authorization

| ID | Applicable | Reason | Details |
|----|------------|--------|---------|
| AZ2 | false | No privilege elevation path (flat token model) | [AZ2](help/AZ2.md) |
| AZ9 | false | No client-side authorization checks to bypass | [AZ9](help/AZ9.md) |
| AZX | false | No centralized authorization framework to attack | [AZX](help/AZX.md) |
| AZJ | false | No permission-granting mechanism to exploit | [AZJ](help/AZJ.md) |

### Cryptography

| ID | Applicable | Reason | Details |
|----|------------|--------|---------|
| CR2 | false | No obfuscation used (data is plaintext, not obfuscated) | [CR2](help/CR2.md) |
| CR4 | false | No encrypted channel exists (can't have unencrypted-within-encrypted) | [CR4](help/CR4.md) |
| CR5 | false | No crypto controls to fail insecurely | [CR5](help/CR5.md) |
| CR9 | false | No random number/GUID generation at runtime | [CR9](help/CR9.md) |
| CRX | false | No crypto to be weak | [CRX](help/CRX.md) |
| CRQ | false | No master cryptographic secrets exist | [CRQ](help/CRQ.md) |
| CRK | false | No crypto code to alter | [CRK](help/CRK.md) |
| CRA | false | No crypto to attack with novel methods | [CRA](help/CRA.md) |

### Cornucopia

| ID | Applicable | Reason | Details |
|----|------------|--------|---------|
| C3 | false | No client-side binaries to decompile | [C3](help/C3.md) |
| C4 | false | Non-repudiation not relevant (read-only fraud queries) | [C4](help/C4.md) |
| C5 | false | Internal API, no public trust to manipulate | [C5](help/C5.md) |
| C7 | false | Audit trail absence already covered by Ace cards | [C7](help/C7.md) |
| CQ | false | Real-time detection absence covered by Ace cards | [CQ](help/CQ.md) |
| CA | false | Creative/novel placeholder — not a specific threat | [CA](help/CA.md) |

### Wild Card

| ID | Applicable | Reason | Details |
|----|------------|--------|---------|
| JOA | false | Backend API cannot attack end-user systems | [JOA](help/JOA.md) |
| JOB | false | Compliance is a consequence, not a distinct attack vector | [JOB](help/JOB.md) |

### Large Language Models

| ID | Applicable | Reason | Details |
|----|------------|--------|---------|
| LLM6 | false | No RAG, vector DB, or MCP sources to poison | [LLM6](help/LLM6.md) |
| LLMA | false | Creative/novel placeholder — not a specific threat | [LLMA](help/LLMA.md) |
