# Contributing to ZLAR-AU

ZLAR-AU is an audit trail analysis and compliance reporting tool for ZLAR-governed agents. It is maintained by [ZLAR Inc.](https://zlar.ai) and welcomes contributions.

---

## How to Contribute

### Reporting Issues

If you find a bug, a gap in documentation, or have a suggestion, open an issue. Be specific — include what you expected, what happened, and steps to reproduce.

### Security Disclosures

If you discover a security vulnerability, **do not open a public issue.** Use [GitHub's private vulnerability reporting](https://github.com/ZLAR-AI/ZLAR-AU/security/advisories) instead. See [SECURITY.md](SECURITY.md) for details.

### Pull Requests

1. Fork the repository
2. Create a branch from `main` for your change
3. Make focused changes — one concern per PR
4. Run `bash -n bin/zlar-au` to syntax-check
5. Write a clear description of what the change does and why
6. Submit the PR

### Code Standards

- Follow existing patterns and naming conventions
- Compliance mappings must cite specific control numbers (e.g., PCI-DSS 10.2.1, not just "PCI-DSS")
- Report output formats must be deterministic and parseable

---

## What We're Looking For

- **Compliance framework mappings** — additional frameworks beyond PCI-DSS, SOC 2, OSFI B-13, SOX, EU AI Act
- **Export formats** — additional SIEM integrations beyond Splunk CIM
- **Anomaly detection** — pattern-based detection of unusual audit trail entries
- **Visualization** — timeline or graph rendering of audit data

---

## Code of Conduct

Be respectful, be specific, be constructive.

---

## License

By contributing to ZLAR-AU, you agree that your contributions will be licensed under the [Apache License 2.0](LICENSE).
