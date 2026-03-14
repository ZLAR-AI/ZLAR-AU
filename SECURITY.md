# Security Policy

## Scope

ZLAR-AU is an audit trail analysis and compliance reporting tool. It reads hash-chained JSONL audit logs produced by ZLAR Gate and generates reports, queries, and compliance mappings.

ZLAR-AU is a read-only analysis tool — it does not enforce policy or modify audit data. However, vulnerabilities that could cause it to produce misleading reports, fail to detect tampered audit records, or leak audit data are in scope.

---

## Reporting a Vulnerability

**Do not open a public issue for security vulnerabilities.**

Use GitHub's private vulnerability reporting:

1. Go to [ZLAR-AI/ZLAR-AU Security Advisories](https://github.com/ZLAR-AI/ZLAR-AU/security/advisories)
2. Click **"Report a vulnerability"**
3. Provide a clear description, reproduction steps, and affected components

We will acknowledge receipt within 48 hours and provide a timeline for remediation.

If you cannot use GitHub's reporting tool, email **hello@zlar.ai** with the subject line "Security: ZLAR-AU" and we will establish a private channel.

---

## Threat Model

**Audit trail forgery.** An attacker modifies audit records before AU processes them. AU's `verify` command detects hash-chain breaks, but does not prevent pre-analysis tampering. The integrity guarantee comes from the gate that writes the trail.

**Report manipulation.** Crafted audit entries containing control characters or injection payloads could affect report output. AU sanitizes display values before rendering.

### Out of Scope

ZLAR-AU does not enforce policy, govern agent behavior, or protect audit trail integrity at write time. For enforcement, see [ZLAR Gate](https://github.com/ZLAR-AI/ZLAR-Gate). For OS-level containment, see [ZLAR-OC](https://github.com/ZLAR-AI/ZLAR-OC).

---

## Supported Versions

| Version | Supported |
|---------|-----------|
| main (HEAD) | Yes |

---

## Disclosure Policy

We practice coordinated disclosure. Acknowledgment within 48 hours, severity assessment within 7 days, fix and advisory published, credit offered.

We ask that reporters refrain from public disclosure until a fix is available, or until 90 days have elapsed — whichever comes first.
