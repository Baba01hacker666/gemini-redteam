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
