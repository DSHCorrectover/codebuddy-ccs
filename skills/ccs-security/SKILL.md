---
name: ccs-security
description: >
  Verify AI agent tool calls against the CCS 7-dimension runtime standard
  (Structure, Schema, Security, Identity, Integrity, Latency, Cost), issue and
  verify Ed25519 signed receipts, audit MCP configuration for security risks,
  and enforce intent binding. Invoke when asked to check tool-call safety,
  verify a CCS receipt, audit an MCP server/client config, produce runtime
  evidence, or guard against command injection / SSRF / credential
  exfiltration. Example phrases: "verify this tool call", "audit my MCP
  config", "verify this CCS receipt", "issue evidence for this call",
  "check this call against CCS".
---

# CCS Runtime Security

CCS (Correctover Conformance Shape) is a 7-dimension runtime verification
standard for AI agents. This skill connects to the `ccs-runtime-evidence` MCP
server, which provides five tools.

## Tools

- **verify_tool_call** — Verify a tool call against all 7 CCS dimensions
  (Structure, Schema, Security, Identity, Integrity, Latency, Cost). Returns an
  allow/deny/escalate verdict with a per-dimension breakdown.
- **issue_evidence** — Issue a CCS evidence record for a tool call. The record
  is cryptographically bound (content_hash + evidence_hash) and Ed25519 signed.
- **verify_receipt** — Offline-verify a CCS Ed25519 signed receipt. Use for
  auditing receipts produced by issue_evidence, without contacting any service.
- **audit_mcp_config** — Audit an MCP client/server configuration JSON for
  risks: plain HTTP, weak secrets, disabled TLS, missing fields, and more.
- **verify_intent_binding** — Verify that actual tool-call arguments match a
  declared intent with zero tolerance. Catches cross-model parameter drift.

## How to use

1. Before a sensitive tool call (shell, network, file write, credentials), call
   `verify_tool_call` with the tool name and arguments.
2. If the verdict is deny, do not execute. If escalate, ask the operator.
3. After the call, call `issue_evidence` to produce a signed, tamper-evident
   receipt.
4. Hand the receipt to an auditor, who verifies it offline with
   `verify_receipt`.
5. When reviewing an MCP setup, paste the config JSON into `audit_mcp_config`.

## Notes

- Verification is runtime evidence, not a static scan of source files.
- Receipts are Ed25519 signed and verifiable offline.
- CCS supports model-agnostic agent runtime verification; it is not tied to any
  specific model provider.
