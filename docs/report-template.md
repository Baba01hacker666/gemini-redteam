# Red Team Assessment Report

## Executive Summary

Summarize the assessment goal, target/scope, dates, and the highest-confidence outcomes. State clearly whether verified vulnerabilities were confirmed.

## Scope and Assumptions

- Target(s):
- Authorization / rules of engagement:
- Credential level:
- Testing window / rate limits:
- Destructive actions allowed: No unless explicitly approved
- Assumptions:

## Methodology

Describe the ordered workflow used:

1. Scope gate
2. Fingerprinting / source map
3. Component inventory
4. Candidate finding analysis
5. Independent verification
6. Reporting and retest guidance

## Agent Workflow

| Agent | Purpose | Inputs | Outputs |
|---|---|---|---|
| redteam-cms-fingerprint | CMS/platform identification and component inventory |  |  |
| redteam-source-code-analyzer | Source review and candidate finding generation |  |  |
| redteam-finding-verifier | Independent confirmation/rejection of claims |  |  |
| redteam-report-writer | Final evidence-backed report generation |  | `report.md` |

## Commands and Probes Tried

| Step | Command / Request | Expected Result | Observed Result | Status |
|---|---|---|---|---|
|  |  |  |  |  |

## Verified Findings

### Finding ID:

- Verdict:
- Severity:
- Affected asset/component:
- Proof of affected state:
- Proof of exploitability or impact:
- Negative control:
- Preconditions:
- Reproduction metadata:
- Reproduction steps:
- Detection opportunities:
- Remediation:
- Retest steps:

## Rejected or Unproven Claims

| ID | Claim | Verdict | Reason | Evidence gap |
|---|---|---|---|---|
|  |  |  |  |  |

## Evidence Collected

- Files reviewed:
- Logs/results:
- Screenshots:
- Tool versions:
- Timestamps:

## Remediation Guidance

Prioritized, specific fixes and hardening recommendations.

## Retest / Verification Steps

Exact commands or requests to prove remediation and negative controls.

## Limitations and Open Questions

List missing access, blocked probes, rate limits, unavailable tools, and unresolved assumptions.

## Appendix: Raw Agent Notes

Paste or reference raw outputs from analyzer, verifier, recon, CMS, and exploit agents.
