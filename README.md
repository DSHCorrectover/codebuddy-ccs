# CCS Security — CodeBuddy Plugin

[**Commercial verification engagements — USD 8,000–15,000, court-ready dossier**](https://dshcorrectover.github.io/correctover-recovery-admission/services.html) · [**Independent audit & conformance**](https://dshcorrectover.github.io/correctover-recovery-admission/services.html)

Runtime evidence and verification for AI agents, distributed as a
[CodeBuddy](https://codebuddy.ai) plugin through the GitHub marketplace path.

CCS (Correctover Conformance Shape) is a model-agnostic, 7-dimension runtime
verification framework for agent tool calls:

**Structure · Schema · Security · Identity · Integrity · Latency · Cost**

Suspicious calls are blocked **fail-closed**, and verified calls produce an
**Ed25519 signed receipt** that any third party can verify offline.

## Install

In the CodeBuddy CLI:

```bash
codebuddy plugin marketplace add DSHCorrectover/codebuddy-ccs
codebuddy plugin install ccs-security
```

CodeBuddy reads `.codebuddy-plugin/marketplace.json` from this repository. No
manual build is required — the plugin launches the verification server through
`npx`.

## What you get

The plugin starts the `ccs-runtime-evidence` MCP server (`ccs-mcp-server`,
zero dependencies, stdio) and exposes five tools:

| Tool | Purpose |
| --- | --- |
| `verify_tool_call` | Verify a tool call against the 7 dimensions; returns `allow` / `deny` / `escalate` |
| `issue_evidence` | Issue a content-hashed, Ed25519 signed evidence receipt |
| `verify_receipt` | Verify a CCS receipt offline (signature + integrity) |
| `audit_mcp_config` | Audit an MCP configuration: plain HTTP, weak secrets, disabled TLS, etc. |
| `verify_intent_binding` | Compare actual call parameters against the declared intent |

## How it works

```
CodeBuddy  ->  ccs-runtime-evidence (stdio / npx)  ->  allow . deny . escalate
                                                         |
                                              Ed25519 signed receipt
```

The verification runs locally as a stdio MCP server. Calls that do not conform
to policy are denied by default; conforming calls return a signed, independently
verifiable receipt.

## Scope and wording

CCS verifies agent runtime behavior and is **model-agnostic** — it can verify
agents running models from any provider, including DeepSeek models. It is not an
official, endorsed, or certified integration of any model vendor.

## License

[Elastic License 2.0](./LICENSE) — source is readable and usable; resale of the
software as a managed service is restricted.
