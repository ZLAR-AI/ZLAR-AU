# ZLAR-AU — Audit Trail Analysis & Compliance Reporting

Your AI coding agent runs with your permissions. ZLAR Gate decides what it can do. ZLAR-AU tells you what happened.

ZLAR-AU reads the hash-chained audit trail produced by [ZLAR Gate](https://github.com/ZLAR-AI/ZLAR-Gate) or [ZLAR-LT](https://github.com/ZLAR-AI/ZLAR-LT) and turns it into compliance-grade reports.

## Quick Start

```bash
git clone https://github.com/ZLAR-AI/ZLAR-AU.git
cd ZLAR-AU

# If you have ZLAR-LT or Gate installed, AU auto-detects the audit file:
bin/zlar-au summary

# Or point it at a specific file:
ZLAR_AUDIT_FILE=/path/to/audit.jsonl bin/zlar-au summary
```

**Requirements:** `jq` (install with `brew install jq`)

## What It Does

| Command | Description |
|---------|-------------|
| `zlar-au summary` | Overview of your audit trail — events, outcomes, risk distribution |
| `zlar-au query --outcome deny` | Search and filter events |
| `zlar-au verify` | Verify hash chain integrity (tamper detection) |
| `zlar-au export --format csv` | Export to CSV, JSON, or Splunk CIM format |
| `zlar-au report --type compliance` | Generate a full compliance report (HTML → print to PDF) |
| `zlar-au digest daily` | Daily/weekly/monthly summary |

## Reports

### Compliance Report

Full HTML report that maps your audit trail to compliance frameworks:

- **PCI-DSS 10** — Log all access, automated review, tamper-evident logging
- **SOC 2** — Unauthorized access detection, system monitoring
- **OSFI B-13** — Technology incident management (Canadian financial institutions)
- **SOX §404** — Change tracking with content hashes
- **EU AI Act** — AI system traceability and decision logs

Generate and print to PDF:

```bash
zlar-au report --type compliance
# Opens as HTML → Ctrl+P → Save as PDF
```

### Incident Report

Denied and high-risk events only. For security review and OSFI B-13 incident management.

```bash
zlar-au report --type incident
```

### Executive Summary

One-page overview. Total actions governed, enforcement rate, frameworks in use.

```bash
zlar-au report --type executive
```

## Export Formats

| Format | Use Case |
|--------|----------|
| `csv` | Auditor analysis in Excel/Sheets |
| `json` | Structured data for processing |
| `jsonl` | Filtered copy of raw audit trail |
| `splunk` | Splunk Common Information Model — drop into your SIEM |

```bash
# Export all denied events as CSV
zlar-au query --outcome deny --csv > denied.csv

# Export to Splunk CIM format
zlar-au export --format splunk

# Export last 30 days to JSON
zlar-au export --format json --after 2026-02-13
```

## Hash Chain Verification

Every audit event includes a `prev_hash` field — the SHA-256 hash of the previous event. This creates a cryptographic chain that detects tampering: if any event is inserted, deleted, or modified, the chain breaks.

```bash
zlar-au verify
# ✓ Hash chain intact — no tampering detected
```

## Query Examples

```bash
# All denied bash commands
zlar-au query --outcome deny --domain bash

# High-risk events (score ≥ 70)
zlar-au query --risk-min 70

# Events from a specific session
zlar-au query --session abc123

# Events from Cursor adapter only
zlar-au query --agent cursor

# Last 50 events as JSON
zlar-au query --limit 50 --json
```

## The Audit Trail

ZLAR-AU reads the JSONL audit file produced by ZLAR Gate. Each line is a JSON object:

| Field | Description |
|-------|-------------|
| `id` | Unique event identifier |
| `ts` | ISO timestamp |
| `host` | Machine hostname |
| `user` | OS user |
| `agent_id` | Framework (claude-code, cursor, windsurf) |
| `session_id` | Session identifier |
| `domain` | Classification (bash, write, read, edit, glob, grep, etc.) |
| `action` | What was attempted |
| `outcome` | allow, deny, authorized, logged, etc. |
| `risk_score` | 0-100 computed from policy rule |
| `detail` | Tool-specific context (command, file path, etc.) |
| `rule` | Which policy rule matched |
| `policy_version` | Version of the signed policy |
| `severity` | Event severity (info, warn, critical) |
| `prev_hash` | SHA-256 hash chain |

## Known Limitations

- HTML reports are designed for browser printing to PDF. No native PDF generation (avoids heavyweight dependencies like wkhtmltopdf).
- Splunk CIM export is best-effort field mapping. Validate against your SIEM's CIM schema.
- Hash chain verification reads the entire file linearly. On very large trails (100K+ events), this takes seconds.
- The tool reports what the gate observed. It does not independently verify that the gate's decisions were correct.

## The ZLAR Family

| Product | What it does |
|---------|-------------|
| [ZLAR-OC](https://github.com/ZLAR-AI/ZLAR-OC) | OS-level containment for OpenClaw agents |
| [ZLAR-CC](https://github.com/ZLAR-AI/ClaudeCode_ZLAR-CC) | Claude Code specific gate |
| [ZLAR Gate](https://github.com/ZLAR-AI/ZLAR-Gate) | Universal gate — Claude Code, Cursor, Windsurf |
| [ZLAR-LT](https://github.com/ZLAR-AI/ZLAR-LT) | Zero-config governance (one command install) |
| **ZLAR-AU** | Audit trail analysis & compliance reporting ← you are here |

## License

Apache 2.0 — see [LICENSE](LICENSE)

Built by [ZLAR Inc.](https://zlar.ai) — hello@zlar.ai
