---
name: redteam-recon
description: Focused reconnaissance workflow for authorized security assessments, bug bounty triage, lab targets, and CTF infrastructure.
---
# Red Team Recon Skill

Focused reconnaissance workflow for authorized security assessments, bug bounty triage, lab targets, and CTF infrastructure.

Activate when the user asks to:
- Build a recon plan for a target or scope
- Enumerate attack surface from domains, IPs, ASNs, certificates, or URLs
- Prioritize exposed services, login panels, APIs, cloud assets, or CMS targets
- Convert recon output into candidate findings for verification

## Workflow

1. **Scope gate**
   - Confirm target boundaries, authorization, request-rate limits, credential level, forbidden actions, and stop conditions.
   - If authorization or target ownership is unclear, provide passive-only methodology and stop before active probing.
2. **Seed normalization**
   - Classify input as domain, IP, CIDR, ASN, organization, app URL, repository, or artifact list.
   - Preserve source of each seed so later findings can be traced back to evidence.
3. **Passive collection**
   - Use certificate transparency, DNS, WHOIS/RDAP, ASN pivots, web archives, search indexes, Shodan/Censys/FOFA queries, GitHub/code search, and public cloud bucket naming patterns.
   - Track confidence and avoid treating unvalidated OSINT as in-scope assets.
4. **Active validation**
   - Use low-rate DNS resolution, HTTP probing, TLS inspection, technology fingerprinting, and service checks only when allowed.
   - Prefer headers, titles, redirects, status codes, TLS SANs, robots/sitemap, well-known paths, and API roots before heavier crawling.
5. **Attack surface clustering**
   - Group assets by ownership confidence, technology stack, auth exposure, admin panels, APIs, upload/import features, storage exposure, staging/dev indicators, and CMS fingerprints.
6. **Candidate generation**
   - Produce hypotheses with evidence gaps, not confirmed findings.
   - Apply specific methodologies (CMS fingerprinting, source code analysis, finding verification) to appropriate targets.
7. **Reporting handoff**
   - Record commands tried, observed outputs, dead ends, rate limits, assumptions, and verification gaps for the final report.

## Output contract

Return Markdown with:

- `Scope and assumptions`
- `Seed inventory`
- `Passive recon plan and commands`
- `Active validation plan and commands`
- `Attack surface clusters`
- `High-priority pivots`
- `Candidate findings / hypotheses`
- `Verifier handoff`
- `Report notes`

## Command patterns

Please refer to `examples/recon-command-patterns.md` for example passive and active enumeration commands.

## Evidence requirements

For every candidate, capture:

- Asset and ownership-confidence evidence
- Exact command/query used
- Timestamp when available
- Response excerpt or output file
- Why the item is interesting
- What would confirm or falsify it

Do not call a vulnerability confirmed until verified with reproducible evidence.

## Advanced Reconnaissance Methods
- **Cloud Infrastructure**: Enumerate AWS S3, GCP Buckets, and Azure Blobs using `cloud_enum` and permutations.
- **BGP & ASN Analysis**: Map organizational infrastructure via ASN lookups (`whois -h whois.radb.net -- '-i origin AS1234'`).
- **GitHub OSINT**: Hunt for leaked CI/CD tokens, SSH keys, and staging environment URLs using `trufflehog` or `gitrob`.
- **Acquisition Tracking**: Monitor Crunchbase and SEC filings for newly acquired subsidiaries that may lack mature security.
