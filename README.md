# gemini-redteam
Red team Gemini CLI extension and Google Antigravity plugin


A dual-use Gemini CLI extension and Google Antigravity plugin that injects a professional offensive security persona and provides red team workflows.

## What it does

- Injects a **security research system context** into Gemini CLI sessions through `GEMINI.md`
- Provides **6 Gemini CLI slash commands** for common red team workflows
- Bundles **4 Gemini CLI sub-agents** for CMS fingerprinting, source analysis, finding verification, and final report writing
- Bundles **6 skills** for Gemini-compatible and Antigravity workflows
- Provides an **Antigravity plugin manifest** and red team rule under `plugin.json` and `rules/`

## Design goals

- **Operational outputs**: command-first responses with concrete next actions
- **Prompt robustness**: templates request clarifications when critical context is missing
- **Evidence-aware writing**: report and exploit flows separate facts, assumptions, and hypotheses
- **Consistent structure**: every command now pushes for validation checkpoints and fallback paths
- **Anti-hallucination verification**: analyzer outputs must be checked by an independent verifier before final reporting
- **Evidence standardization**: verified findings require affected state proof, exploitability or impact proof, negative controls, reproduction metadata, and retest steps
- **Report artifact**: workflows end by creating or updating `report.md` with steps tried, evidence, verified results, and gaps

## Install

### Gemini CLI

```bash
# From GitHub
gemini extensions install https://github.com/Baba01hacker666/gemini-redteam
```

Then restart your Gemini CLI session.

### Google Antigravity
```bash
agy install https://github.com/Baba01hacker666/gemini-redteam
```

Antigravity plugins are loaded from plugin directories that contain `plugin.json` plus optional `skills/`, `rules/`, `mcp_config.json`, and `hooks.json` files.

Use this repo as a workspace-level plugin by placing or cloning it under an opened workspace:

```text
<workspace>/.agents/plugins/gemini-redteam/
```

or:

```text
<workspace>/_agents/plugins/gemini-redteam/
```

Use it globally by placing or cloning it under:

```text
~/.gemini/config/plugins/gemini-redteam/
```

Antigravity will load the focused skills in `skills/` and the red team operating rules in `rules/redteam.md`. Gemini CLI slash commands in `commands/rt/` remain Gemini-specific.

## Commands

| Command | Usage |
|---|---|
| `/rt:recon <target>` | Structured recon plan for a target type/scope |
| `/rt:exploit <vuln/context>` | Exploit chain analysis and PoC |
| `/rt:ctf <challenge>` | CTF solver — enum to flag |
| `/rt:cms <cms/target>` | CMS-specific assessment order, checks, and verification |
| `/rt:evade <payload/context>` | AV/EDR evasion strategy |
| `/rt:report <finding>` | Professional pentest finding write-up |

## Bundled Gemini CLI agents

Gemini CLI loads extension sub-agents from the `agents/` directory. You can ask the main session to delegate automatically, or force a specific agent with `@agent-name`.

| Agent | Use when | Output |
|---|---|---|
| `@redteam-cms-fingerprint` | You need CMS/platform identification and component inventory before testing | CMS confidence, components, prioritized checks, analyzer/verifier handoff |
| `@redteam-source-code-analyzer` | You need source-code review for routes, controllers, plugins, modules, themes, sinks, and candidate vulnerabilities | Evidence-backed candidate findings (`SC-001`, `SC-002`, ...), commands/files reviewed, verifier handoff |
| `@redteam-finding-verifier` | Another agent reported possible findings and you need to confirm they are real | Confirmed / partially confirmed / unproven / rejected verdicts with hallucination checks |
| `@redteam-report-writer` | The workflow is complete and a durable artifact is needed | Creates or updates `report.md` with full steps, attempts, observations, verified findings, rejected claims, remediation, and retest steps |

Recommended agent order for CMS/source reviews:

```text
@redteam-cms-fingerprint → @redteam-source-code-analyzer → @redteam-finding-verifier → @redteam-report-writer
```

The final report writer must not turn analyzer hypotheses into findings unless the verifier confirms them. If no issue is verified, `report.md` must say no verified vulnerabilities were confirmed.

## Bundled skills

Gemini CLI loads extension skills from the `skills/` directory. The broad `redteam` skill remains available, and focused skills provide lighter workflow-specific context.

| Skill | Use when |
|---|---|
| `redteam` | General offensive-security research, CMS testing, exploit analysis, CTFs, and reporting |
| `redteam-recon` | Building recon plans, enumerating attack surface, and prioritizing pivots |
| `redteam-cms` | Fingerprinting and assessing CMS platforms, plugins, modules, themes, and extensions |
| `redteam-source-audit` | Reviewing repository source code and producing verifier-ready candidate findings |
| `redteam-exploit-validation` | Safely validating exploitability with proof ladders, controls, cleanup, and telemetry notes |
| `redteam-reporting` | Creating evidence-backed findings and `report.md` artifacts from verified work |

## Examples

```
/rt:recon Laravel application exposed on Shodan, unknown version
/rt:cms WordPress multisite with WooCommerce, authenticated editor account
/rt:exploit CVE-2025-54236 Magento unauthenticated RCE
/rt:ctf PHP web challenge with a file upload endpoint and source provided
/rt:evade Cobalt Strike beacon, x64, Windows 11 with Defender + CrowdStrike
/rt:report SQL injection in /api/users?id= leading to full DB dump
```

## Structure

```
gemini-redteam/
├── gemini-extension.json     # Gemini CLI extension manifest
├── plugin.json               # Antigravity plugin marker
├── GEMINI.md                 # Gemini CLI system context
├── commands/
│   └── rt/
│       ├── recon.toml
│       ├── exploit.toml
│       ├── ctf.toml
│       ├── cms.toml
│       ├── evade.toml
│       └── report.toml
├── agents/
│   ├── redteam-cms-fingerprint.md
│   ├── redteam-source-code-analyzer.md
│   ├── redteam-finding-verifier.md
│   └── redteam-report-writer.md
├── docs/
│   └── report-template.md
├── rules/
│   └── redteam.md            # Antigravity red team operating rule
└── skills/
    ├── redteam/
    │   └── SKILL.md          # Broad offensive-security skill
    ├── redteam-recon/
    │   └── SKILL.md          # Recon and attack-surface skill
    ├── redteam-cms/
    │   └── SKILL.md          # CMS assessment skill
    ├── redteam-source-audit/
    │   └── SKILL.md          # Source-code review skill
    ├── redteam-exploit-validation/
    │   └── SKILL.md          # Safe exploit validation skill
    └── redteam-reporting/
        └── SKILL.md          # Evidence-backed reporting skill
```

## Notes

- Gemini CLI context is injected globally through `GEMINI.md` when the extension is active.
- Antigravity loads `plugin.json`, `skills/`, and `rules/` when this repo is placed in a supported plugin directory.
- Gemini CLI commands conflict-resolve with prefix: if `/recon` exists elsewhere, these become `/gemini-redteam:rt:recon`.
- Gemini CLI sub-agents in `agents/` are Gemini-specific; Antigravity support currently uses skills and rules.
- Skills are token-efficient — only activated when relevant.

## Recent prompt quality improvements

- Added explicit handling for ambiguous input (short clarifying questions before execution).
- Added validation checkpoints so users can confirm each step succeeded.
- Added CMS-specific workflows for WordPress, Drupal, Joomla, Magento/Adobe Commerce, Shopify, Ghost, Strapi, Umbraco, Sitecore, TYPO3, PrestaShop, and OpenCart.
- Added Gemini CLI sub-agents for CMS fingerprinting, source code analysis, finding verification, and `report.md` generation.
- Added focused skills for recon, CMS assessment, source audit, exploit validation, and evidence-backed reporting.
- Added Google Antigravity plugin support with `plugin.json` and `rules/redteam.md`.
- Added a shared evidence standard for affected-state proof, exploitability/impact proof, negative controls, reproduction metadata, and remediation retests.
- Added detection/telemetry considerations in exploit and evasion outputs.
- Added assumptions/limitations guidance in reporting output when evidence is incomplete.
