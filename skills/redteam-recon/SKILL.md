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
   - Route CMS targets to `@redteam-cms-fingerprint`, source-backed issues to `@redteam-source-code-analyzer`, and candidate findings to `@redteam-finding-verifier`.
7. **Reporting handoff**
   - Record commands tried, observed outputs, dead ends, rate limits, assumptions, and verification gaps for `@redteam-report-writer`.

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

```bash
# Passive DNS and CT pivots
subfinder -d TARGET -all -silent | tee subdomains.txt
amass enum -passive -d TARGET -o amass-passive.txt
curl -s 'https://crt.sh/?q=%25.TARGET&output=json' | jq -r '.[].name_value' | sort -u

# Low-rate HTTP validation
httpx -l subdomains.txt -title -tech-detect -status-code -follow-redirects -rate-limit 5 -o httpx.txt

# Focused crawl after scope approval
katana -list live-urls.txt -d 2 -rate-limit 3 -silent -o crawled.txt

# Service discovery only when CIDR scanning is allowed
nmap -sV -Pn --top-ports 100 --max-rate 50 -oA nmap-top TARGET
```

## Evidence requirements

For every candidate, capture:

- Asset and ownership-confidence evidence
- Exact command/query used
- Timestamp when available
- Response excerpt or output file
- Why the item is interesting
- What would confirm or falsify it

Do not call a vulnerability confirmed until verified with reproducible evidence.