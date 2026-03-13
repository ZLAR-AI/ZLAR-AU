# ZLAR-AU — Project Guide

## What this is

ZLAR-AU reads the hash-chained JSONL audit trail produced by ZLAR Gate and turns it into compliance-grade reports. It's the reporting layer on top of the enforcement layer.

## Architecture

```
ZLAR Gate (any framework)
    ↓ writes
var/log/audit.jsonl  — hash-chained JSONL, one line per decision
    ↓ reads
bin/zlar-au          — CLI tool (871 lines bash + jq)
    ↓ produces
Reports (HTML), Exports (CSV/JSON/Splunk), Verification, Summaries
```

### Not a gate, not a daemon

ZLAR-AU does not intercept tool calls. It does not run continuously. It reads the audit file and produces output. It's a reporting tool, not an enforcement tool.

## Core principles

1. **Read only.** AU never writes to the audit trail. It only reads.
2. **No intelligence.** Count, filter, aggregate, format. No ML, no heuristics, no risk scoring beyond what the gate already computed.
3. **Compliance-mapped.** Reports explicitly reference PCI-DSS 10, SOC 2, OSFI B-13, SOX, EU AI Act.
4. **Multiple export formats.** CSV for auditors, JSON for SIEM, Splunk CIM for enterprise, HTML for compliance officers.

## Key files

- `bin/zlar-au` — the CLI tool
- `etc/au.json` — configuration (audit file path, report directory)
- `var/reports/` — generated reports (gitignored)

## Commands

| Command | What it does |
|---------|-------------|
| `summary` | Overview: events, outcomes, domains, risk distribution, frameworks |
| `query` | Search/filter events with rich filters (outcome, domain, rule, risk, time) |
| `verify` | Verify SHA-256 hash chain integrity (tamper detection) |
| `export` | Export to CSV, JSON, JSONL, or Splunk CIM format |
| `report` | Generate HTML compliance reports (compliance, incident, executive) |
| `digest` | Daily/weekly/monthly summary |

## Auto-detection

AU auto-detects the audit file from common ZLAR locations:
1. `~/.zlar-lt/var/log/audit.jsonl` (ZLAR-LT)
2. `~/.zlar-gate/var/log/audit.jsonl` (ZLAR Gate)
3. Sibling directories (for development)

Override with `ZLAR_AUDIT_FILE` env var or `etc/au.json`.

## Report types

- **Compliance report** — Full HTML report with executive summary, outcome distribution, domain activity, high-risk events, hash chain status, and compliance framework mapping (PCI-DSS, SOC 2, OSFI B-13, SOX, EU AI Act). Print to PDF for formal submission.
- **Incident report** — Denied and high-risk events only. For security review and OSFI B-13 §4.4 incident management.
- **Executive summary** — One-page overview with key stats. For board/committee reporting.

## Dependencies

- `jq` — required (all JSON processing)
- `bc` — optional (percentage calculations, falls back gracefully)
- `shasum` — required for verify command (macOS built-in)
