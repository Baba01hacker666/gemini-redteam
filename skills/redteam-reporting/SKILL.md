---
name: redteam-reporting
description: Focused reporting workflow for converting verified red-team work into clear, reproducible assessment artifacts.
---
# Red Team Reporting Skill

Focused reporting workflow for converting verified red-team work into clear, reproducible assessment artifacts.

Activate when the user asks to:
- Write or improve a vulnerability finding
- Create or update `report.md`
- Turn recon, source review, exploit validation, or verifier outputs into a report
- Separate confirmed findings from hypotheses, false positives, and evidence gaps

## Workflow

1. **Collect inputs**
   - Gather scope, rules of engagement, commands, requests, screenshots, logs, source paths, agent outputs, timestamps, tool versions, and existing `report.md` if present.
2. **Classify evidence**
   - Separate verified facts, partially confirmed claims, hypotheses, rejected claims, assumptions, and limitations.
   - Do not promote analyzer hypotheses or public PoC claims into findings without verifier evidence.
3. **Apply evidence standard**
   - Verified and partially verified findings must include affected asset/component, affected-state proof, exploitability or impact proof, negative control, reproduction metadata, and remediation retest.
   - Items missing required evidence belong in `Rejected or Unproven Claims`.
4. **Write for two audiences**
   - Executive summary: concise business risk and verified outcome.
   - Technical details: exact reproduction, commands, observed outputs, impact boundary, detection, remediation, and retest steps.
5. **Preserve reproducibility**
   - Include expected vs observed output, negative controls, cleanup performed, and what should change after remediation.
6. **Call out limitations**
   - Document missing credentials, blocked probes, rate limits, unavailable tools, untested versions, unresolved assumptions, and out-of-scope actions.

## Required report structure

```markdown
# Red Team Assessment Report

## Executive Summary
## Scope and Assumptions
## Methodology
## Agent Workflow
## Commands and Probes Tried
## Verified Findings
## Rejected or Unproven Claims
## Evidence Collected
## Remediation Guidance
## Retest / Verification Steps
## Limitations and Open Questions
## Appendix: Raw Agent Notes
```

## Finding template

```markdown
### FINDING-ID: Title

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
```

## Tone and quality bar

- Professional, precise, non-alarmist.
- Evidence-backed and reproducible.
- Clear about what was not tested.
- Specific remediation, not generic security advice.
- No invented CVEs, routes, screenshots, logs, versions, or business impact.

Use `@redteam-report-writer` when a durable `report.md` artifact is needed.