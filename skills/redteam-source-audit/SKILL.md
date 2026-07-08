---
name: redteam-source-audit
description: Focused source-code security review workflow for web applications, CMS extensions, APIs, and supporting services.
---
# Red Team Source Audit Skill

Focused source-code security review workflow for web applications, CMS extensions, APIs, and supporting services.

Activate when the user asks to:
- Review repository source code for vulnerabilities
- Trace input flows from routes/controllers/templates to risky sinks
- Audit CMS plugins, modules, themes, extensions, or custom integrations
- Turn suspected source issues into evidence-backed candidate findings

## Workflow

1. **Scope interpretation**
   - Identify repository root, framework/CMS, files or modules in scope, auth assumptions, and whether tests or local execution are allowed.
   - If scope is broad, prioritize exposed entry points and security-critical code paths.
2. **Source map**
   - Inventory routes, controllers, API handlers, middleware, templates, authz checks, file upload/import paths, background jobs, config, storage, and third-party integrations.
   - Prefer targeted searches and minimal file reads over broad speculation.
3. **Risk triage**
   - Prioritize injection, file write/upload, path traversal, SSRF, deserialization, auth bypass, privilege escalation, IDOR, secret exposure, unsafe template execution, request smuggling surfaces, and CMS extension hooks.
4. **Flow tracing**
   - Trace source → validation/authentication/authorization → transformation → sink.
   - Record where taint is constrained, sanitized, escaped, permission-checked, or made unreachable.
5. **Candidate findings**
   - Create candidate findings only when source evidence exists.
   - Use stable IDs: `SC-001`, `SC-002`, etc.
   - Never claim confirmation; verify the finding independently.
6. **Verification support**
   - Provide minimal non-destructive verification steps, negative controls, and what would falsify each finding.
7. **Reporting support**
   - Preserve reviewed files, searches, dead ends, evidence gaps, and assumptions for the final report.

## Output contract

Return Markdown with:

- `Scope interpreted`
- `Source map`
- `Risk-ranked review targets`
- `Candidate findings`
  - `ID`
  - `Claim`
  - `Evidence`
  - `Source → sink trace`
  - `Preconditions`
  - `Impact`
  - `Verification steps`
  - `Negative control`
  - `Confidence`
  - `What could falsify this`
- `Non-findings / dead ends`
- `Commands and files reviewed`
- `Verifier handoff`
- `Report notes`

## Source review heuristics

- **Web routes**: locate route registration, auth middleware, controller handlers, model access, and response rendering.
- **File handling**: check extension/MIME validation, archive extraction, path normalization, storage destination, execution permissions, and cleanup.
- **SSRF**: check URL parsers, redirects, DNS rebinding controls, internal IP blocks, scheme allowlists, and metadata endpoint access.
- **Injection**: verify query builders, raw SQL, shell calls, template rendering, LDAP/XPath/NoSQL construction, and escaping context.
- **Authz**: compare object ownership checks, role/capability checks, tenant boundaries, and admin-only routes.
- **Secrets**: identify hardcoded credentials, committed keys, debug configs, token logs, and exposed environment files.
- **CMS hooks**: inspect AJAX/REST actions, shortcode/render hooks, upload handlers, cron jobs, install/upgrade routines, and admin pages.

## Evidence bar

A candidate must cite observable repository evidence: file path, symbol, route, config, package metadata, or command result. If the evidence is incomplete, label it as a hypothesis and state the missing proof.

## Deep Source & CI/CD Auditing
- **CI/CD Pipelines**: Audit GitHub Actions (`.github/workflows/`), GitLab CI, and Jenkinsfiles for script injection, vulnerable runners, or token exposure (e.g., `pull_request_target`).
- **Deserialization**: Trace untrusted data into `unserialize()` (PHP), `pickle.loads()` (Python), or `ObjectInputStream` (Java). Identify gadget chains using `ysoserial` or `phpggc`.
- **AST & SAST Analysis**: Use tools like Semgrep with custom rulesets tailored to the specific framework's routing and ORM to uncover IDOR and Mass Assignment vulnerabilities.
