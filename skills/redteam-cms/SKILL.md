---
name: redteam-cms
description: Focused methodology for authorized CMS fingerprinting, component inventory, misconfiguration review, and vulnerability validation.
---
# Red Team CMS Skill

Focused methodology for authorized CMS fingerprinting, component inventory, misconfiguration review, and vulnerability validation.

Activate when the user asks to:
- Assess WordPress, Drupal, Joomla, Magento/Adobe Commerce, Shopify, Ghost, Strapi, Umbraco, Sitecore, TYPO3, PrestaShop, or OpenCart
- Identify CMS version, plugins, modules, themes, extensions, apps, or packages
- Validate CMS CVEs or configuration weaknesses
- Prioritize CMS-specific tests from recon results

## Workflow

1. **Scope gate**
   - Confirm target, authorization, credential level, request-rate limits, destructive-action restrictions, and whether exploit validation is allowed.
   - If live probing is not approved, restrict output to passive/local checks.
2. **Fingerprint**
   - Identify platform, version, edition, language/runtime, server, CDN/WAF, admin paths, API roots, and confidence evidence.
   - Perform a thorough fingerprinting step when platform or component inventory is uncertain.
3. **Component inventory**
   - Enumerate plugins, modules, themes, templates, extensions, apps, packages, API endpoints, users/roles where allowed, upload/import features, and public storage.
4. **Known-risk mapping**
   - Map components to vendor advisories, CVEs, KEV entries, nuclei templates, exploit-db, changelogs, and public PoCs.
   - Do not assert CVE exposure without version or code/config evidence.
5. **Low-impact validation**
   - Prefer read-only probes, benign canaries, current-user impact, non-sensitive reads, and controlled callbacks.
   - Include success signals, stop conditions, and cleanup for any state-changing test.
6. **Independent verification**
   - Independently verify all candidate findings before reporting them as confirmed.
7. **Report handoff**
   - Create or update `report.md` with verified findings, rejected claims, commands tried, and limitations.

## CMS priority matrix

Please refer to `references/cms-priority-matrix.md` for detailed high-value risks and validation checks for each CMS.

## Output contract

Return Markdown with:

- `Scope and allowed actions`
- `CMS fingerprint and confidence`
- `Component inventory`
- `Prioritized checks`
- `Commands / requests`
- `Expected output and success signals`
- `Stop conditions and cleanup`
- `Candidate findings`
- `Verifier handoff`
- `Report notes`

Confirmed findings require affected-state proof, exploitability or impact proof, a negative control, reproduction metadata, and remediation retest steps.

## Advanced CMS Exploitation & Bypass
- **WAF Evasion**: Utilize HTTP Request Smuggling (CL.TE / TE.CL) or chunked encoding to bypass Edge WAFs before hitting the CMS.
- **Authenticated RCE paths**: 
  - *WordPress*: Malicious theme/plugin ZIP uploads, editing `functions.php` via Theme Editor.
  - *Joomla*: Template manipulation to execute PHP.
  - *Drupal*: Utilizing the PHP Filter module or abusing Twig templates.
- **Timing Attacks**: Use sleep-based payloads to blindly enumerate CMS user existence or blind SQLi in CMS core/plugins.

- **Payload Naming (OPSEC)**: Never use blatant backdoor names like `shell.php`, `cmd.php`, or `test.php`. Blend into the target environment by using convincing names related to the application's context (e.g., `class-wp-cache-helper.php`, `config-update.php`, or `index_backup.php`).
