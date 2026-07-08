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
   - Use `@redteam-cms-fingerprint` when platform or component inventory is uncertain.
3. **Component inventory**
   - Enumerate plugins, modules, themes, templates, extensions, apps, packages, API endpoints, users/roles where allowed, upload/import features, and public storage.
4. **Known-risk mapping**
   - Map components to vendor advisories, CVEs, KEV entries, nuclei templates, exploit-db, changelogs, and public PoCs.
   - Do not assert CVE exposure without version or code/config evidence.
5. **Low-impact validation**
   - Prefer read-only probes, benign canaries, current-user impact, non-sensitive reads, and controlled callbacks.
   - Include success signals, stop conditions, and cleanup for any state-changing test.
6. **Independent verification**
   - Send candidate findings to `@redteam-finding-verifier` before reporting them as confirmed.
7. **Report handoff**
   - Use `@redteam-report-writer` to create or update `report.md` with verified findings, rejected claims, commands tried, and limitations.

## CMS priority matrix

| CMS | First checks | High-value risks | Safer validation |
|---|---|---|---|
| WordPress | `/wp-json/`, `/wp-content/`, plugins/themes, users if allowed | vulnerable plugins/themes, REST/admin-ajax authz bugs, XML-RPC abuse, exposed backups | `wpscan`, readme/changelog evidence, benign REST probes, upload canary only if authorized |
| Drupal | `/core/`, `CHANGELOG.txt`, modules, Views/JSON:API | Drupalgeddon-class exposure, private files, update/install endpoints, config leaks | `droopescan`, static hashes, read-only endpoint checks |
| Joomla | `/administrator/`, `com_*`, templates, manifests | vulnerable components, JCE/media upload paths, debug/config backups | manifest evidence, benign component route probes |
| Magento / Adobe Commerce | `/rest/V1/`, `/graphql`, static assets, admin exposure | RCE/SSRF advisories, GraphQL leaks, exposed env/config, module CVEs | `magescan`, GraphQL canaries, version evidence |
| Shopify | storefront APIs, theme assets, apps/webhooks | leaked tokens, app scope issues, redirects, customer data boundary issues | passive theme evidence, owned-token permission checks |
| Ghost | `/ghost/`, `/ghost/api/`, content assets | content API key exposure, vulnerable versions, member/staff flows | API scope checks, harmless draft/member tests |
| Strapi | `/admin`, `/api/*`, GraphQL, roles | public CRUD permissions, upload abuse, admin JWT/session issues | role matrix proof, canary object/upload with cleanup |
| Umbraco | `/umbraco/`, `/App_Plugins/`, media endpoints | package CVEs, install/upgrade exposure, authorized template risks | package manifests, backoffice role checks |
| Sitecore | `/sitecore/`, admin/debug pages, media paths | exposed admin/debug, SPE misuse, deserialization advisories | access evidence, lab-only serialization probes |
| TYPO3 | `/typo3/`, `typo3conf/`, `fileadmin/` | extension CVEs, install tool exposure, fileadmin leaks | extension manifest checks, read-only fileadmin probes |
| PrestaShop / OpenCart | admin routes, modules/extensions, storage paths | module CVEs, exposed install/config/backups, upload/import risks | route/version evidence, storage access checks |

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