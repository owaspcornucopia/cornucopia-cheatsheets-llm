# PwnedNext - LLM App

## High-Level Architecture of AI Anti-Fraud 3.0

![Architecture sequence diagram](architecture-sequence-diagram.svg)

![Threat model](ThreatDragonModels/threatmodel.png)

## Fraud Investigation LLM Application — Cheat Sheet

Applicable threats for the LLM-based fraud investigation API. Each entry links to a page explaining how the threat manifests in this application and what mitigations are needed.

---

## Data Validation & Encoding

| Value | Applicable | Threat | Details |
|----|------------|--------|---------|
| 2 | true | Error messages and responses reveal internal configuration, SQL queries, and model output | [VE2](help/VE2.md) |
| 3 | true | No validation on user questions — arbitrary length, format, and content accepted | [VE3](help/VE3.md) |
| 4 | true | Input not checked in context — any user can ask about any data | [VE4](help/VE4.md) |
| 9 | true | Validation failures not rejected — fallback parsing accepts malformed output | [VE9](help/VE9.md) |
| 10 | true | Application trusts all data sources (user input, LLM output, database content) | [VEX](help/VEX.md) |
| Jack | true | No centralized validation routines exist to be enforced | [VEJ](help/VEJ.md) |
| King | true | LLM-generated SQL executed directly without parameterization (SQL injection) | [VEK](help/VEK.md) |
| Ace | true | No logging to detect novel attacks against input validation | [VEA](help/VEA.md) |

## Authentication

| Value | Applicable | Threat | Details |
|----|------------|--------|---------|
| 2 | true | Token use is invisible — legitimate owners cannot detect stolen token usage | [AT2](help/AT2.md) |
| 3 | true | Tokens exposed in transit (no TLS) and at rest (plaintext in code) | [AT3](help/AT3.md) |
| 5 | true | Hardcoded debug and backdoor tokens present in source code | [AT5](help/AT5.md) |
| 6 | true | Static tokens never expire, cannot be rotated or revoked | [AT6](help/AT6.md) |
| 7 | true | No brute force protection — unlimited failed attempts allowed | [AT7](help/AT7.md) |
| 10 | true | No centralized authentication framework — hand-written list check | [ATX](help/ATX.md) |
| Jack | true | Token check happens inside the tool function, not at the API endpoint | [ATJ](help/ATJ.md) |
| Ace | true | No logging to detect authentication attacks or token misuse | [ATA](help/ATA.md) |

## Session Management

| Value | Applicable | Threat | Details |
|----|------------|--------|---------|
| 3 | true | Stolen tokens cannot be detected or terminated by the owner | [SM3](help/SM3.md) |
| 6 | true | No session timeout — tokens valid indefinitely | [SM6](help/SM6.md) |
| 7 | true | No logout or token termination capability | [SM7](help/SM7.md) |
| 8 | true | No re-authentication on context changes (new IP, unusual patterns) | [SM8](help/SM8.md) |
| 9 | true | Tokens sent over unencrypted HTTP — interceptable in transit | [SM9](help/SM9.md) |
| Jack | true | No proof of possession — stolen tokens reusable from anywhere | [SMJ](help/SMJ.md) |
| King | true | Self-built token check with no session management framework | [SMK](help/SMK.md) |
| Ace | true | No logging to detect session hijacking or replay attacks | [SMA](help/SMA.md) |

## Authorization

| Value | Applicable | Threat | Details |
|----|------------|--------|---------|
| 3 | true | Data accessible through SQL injection — only token validity is checked, not data scope | [AZ3](help/AZ3.md) |
| 4 | true | Authorization design does not default to denying access on failure | [AZ4](help/AZ4.md) |
| 5 | true | All tokens have identical permissions — no per-customer data isolation | [AZ5](help/AZ5.md) |
| 6 | true | Valid endpoint access grants access to all data in the database | [AZ6](help/AZ6.md) |
| 7 | true | SQL injection exposes system tables, arbitrary functions, and objects | [AZ7](help/AZ7.md) |
| 8 | true | Business rules bypassed through data manipulation via SQL injection | [AZ8](help/AZ8.md) |
| Queen | true | SQL injection could escalate to command injection | [AZQ](help/AZQ.md) |
| King | true | Authorization controls can be altered — tokens in source code, data in writable DB | [AZK](help/AZK.md) |
| Ace | true | No logging to detect authorization bypass or cross-tenant access | [AZA](help/AZA.md) |

## Cryptography

| Value | Applicable | Threat | Details |
|----|------------|--------|---------|
| 3 | true | Internal HTTP channel between app and model_service carries conversation and SQL with no integrity checking | [CR3](help/CR3.md) |
| 6 | true | Data in transit is unencrypted — tokens, personal data, and results sent in plaintext | [CR6](help/CR6.md) |
| 7 | true | No TLS configured — transport encryption completely absent | [CR7](help/CR7.md) |
| 8 | true | Database stored unencrypted — personal data readable from filesystem | [CR8](help/CR8.md) |
| Jack | true | API tokens stored as plaintext strings in the application source code | [CRJ](help/CRJ.md) |

## Cornucopia

| Value | Applicable | Threat | Details |
|----|------------|--------|---------|
| 2 | true | Dangerous programming patterns — direct SQL execution, `ast.literal_eval()` on untrusted input | [C2](help/C2.md) |
| 6 | true | Error handling is inconsistent, leaks information, and returns wrong HTTP status codes | [C6](help/C6.md) |
| 8 | true | Infrastructure not hardened — container has no security restrictions | [C8](help/C8.md) |
| 9 | true | Race condition on startup when scaled; amplified concurrent requests against the single model service | [C9](help/C9.md) |
| 10 | true | Vulnerable third-party dependencies in requirements.txt | [CX](help/CX.md) |
| Jack | true | No operational security documentation — insecure defaults ship without guidance | [CJ](help/CJ.md) |
| King | true | Denial of service via SQL injection (DROP TABLE, DELETE, resource exhaustion) | [CK](help/CK.md) |

## Large Language Models

| Value | Applicable | Threat | Details |
|----|------------|--------|---------|
| 2 | true | No rate limiting — resource exhaustion through unlimited inference requests | [LLM2](help/LLM2.md) |
| 3 | true | Fraud determinations made without human oversight, risk of acting on hallucinations | [LLM3](help/LLM3.md) |
| 4 | true | System prompt and personal data can be extracted through prompt manipulation | [LLM4](help/LLM4.md) |
| 5 | true | Any valid token can access any customer's investigation data — no tenant isolation | [LLM5](help/LLM5.md) |
| 7 | true | Fine-tuned adapter ("pwnednext") may contain poisoned weights or backdoors | [LLM7](help/LLM7.md) |
| 8 | true | The SQL tool has no access restrictions — executes any query the model produces | [LLM8](help/LLM8.md) |
| 9 | true | Poisoned database records can inject instructions into the model during answer generation | [LLM9](help/LLM9.md) |
| Jack | true | Models downloaded from HuggingFace without integrity verification at deployment | [LLMJ](help/LLMJ.md) |
| King | true | Model executes database queries with no human approval — excessive agency | [LLMK](help/LLMK.md) |
| Queen | true | LLM output fed directly to the SQL engine — improper output handling enables injection | [LLMQ](help/LLMQ.md) |
| 10 | true | User input can override the system prompt — direct prompt injection | [LLMX](help/LLMX.md) |

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

| Value | Applicable | Reason | Details |
|----|------------|--------|---------|
| 5 | false | No client-side validation exists to bypass | [VE5](help/VE5.md) |
| 6 | false | No file upload functionality | [VE6](help/VE6.md) |
| 7 | false | No XML parsing | [VE7](help/VE7.md) |
| 8 | false | No browser-rendered output (XSS impossible) | [VE8](help/VE8.md) |
| Queen | false | No HTTP header injection vector | [VEQ](help/VEQ.md) |

### Authentication

| Value | Applicable | Reason | Details |
|----|------------|--------|---------|
| 4 | false | No password/credential storage (tokens are hardcoded, not user-managed) | [AT4](help/AT4.md) |
| 8 | false | No password change/reset functionality | [AT8](help/AT8.md) |
| 9 | false | No multi-factor authentication to bypass | [AT9](help/AT9.md) |
| Queen | false | No credential recovery mechanism | [ATQ](help/ATQ.md) |
| King | false | No account lockout to circumvent | [ATK](help/ATK.md) |

### Session Management

| Value | Applicable | Reason | Details |
|----|------------|--------|---------|
| 2 | false | No session state or cookies | [SM2](help/SM2.md) |
| 4 | false | No session cookies to intercept | [SM4](help/SM4.md) |
| 5 | false | No session tokens generated at runtime | [SM5](help/SM5.md) |
| 10 | false | No session expiry mechanism | [SMX](help/SMX.md) |
| Queen | false | No CSRF risk (no browser-based sessions) | [SMQ](help/SMQ.md) |

### Authorization

| Value | Applicable | Reason | Details |
|----|------------|--------|---------|
| 2 | false | No privilege elevation path (flat token model) | [AZ2](help/AZ2.md) |
| 9 | false | No client-side authorization checks to bypass | [AZ9](help/AZ9.md) |
| 10 | false | No centralized authorization framework to attack | [AZX](help/AZX.md) |
| Jack | false | No permission-granting mechanism to exploit | [AZJ](help/AZJ.md) |

### Cryptography

| Value | Applicable | Reason | Details |
|----|------------|--------|---------|
| 2 | false | No obfuscation used (data is plaintext, not obfuscated) | [CR2](help/CR2.md) |
| 4 | false | No encrypted channel exists (can't have unencrypted-within-encrypted) | [CR4](help/CR4.md) |
| 5 | false | No crypto controls to fail insecurely | [CR5](help/CR5.md) |
| 9 | false | No random number/GUID generation at runtime | [CR9](help/CR9.md) |
| 10 | false | No crypto to be weak | [CRX](help/CRX.md) |
| Queen | false | No master cryptographic secrets exist | [CRQ](help/CRQ.md) |
| King | false | No crypto code to alter | [CRK](help/CRK.md) |
| Ace | false | No crypto to attack with novel methods | [CRA](help/CRA.md) |

### Cornucopia

| Value | Applicable | Reason | Details |
|----|------------|--------|---------|
| 3 | false | No client-side binaries to decompile | [C3](help/C3.md) |
| 4 | false | Non-repudiation not relevant (read-only fraud queries) | [C4](help/C4.md) |
| 5 | false | Internal API, no public trust to manipulate | [C5](help/C5.md) |
| 7 | false | Audit trail absence already covered by Ace cards | [C7](help/C7.md) |
| Queen | false | Real-time detection absence covered by Ace cards | [CQ](help/CQ.md) |
| Ace | false | Creative/novel placeholder — not a specific threat | [CA](help/CA.md) |

### Wild Card

| Value | Applicable | Reason | Details |
|----|------------|--------|---------|
| Joker A | false | Backend API cannot attack end-user systems | [JOA](help/JOA.md) |
| Joker B | false | Compliance is a consequence, not a distinct attack vector | [JOB](help/JOB.md) |

### Large Language Models

| Value | Applicable | Reason | Details |
|----|------------|--------|---------|
| 6 | false | No RAG, vector DB, or MCP sources to poison | [LLM6](help/LLM6.md) |
| Ace | false | Creative/novel placeholder — not a specific threat | [LLMA](help/LLMA.md) |
