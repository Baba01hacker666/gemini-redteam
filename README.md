# gemini-redteam
Red team gemini skills and plugins 


A Gemini CLI extension that injects a professional offensive security persona and provides red team slash commands.

## What it does

- Injects a **security research system context** into every Gemini CLI session
- Provides **5 slash commands** for common red team workflows
- Bundles a **redteam agent skill** activated automatically on relevant tasks

## Install

```bash
# From GitHub
gemini extensions install https://github.com/Baba01hacker666/gemini-redteam
```

Then restart your Gemini CLI session.

## Commands

| Command | Usage |
|---|---|
| `/rt:recon <target>` | Structured recon plan for a target type/scope |
| `/rt:exploit <vuln/context>` | Exploit chain analysis and PoC |
| `/rt:ctf <challenge>` | CTF solver — enum to flag |
| `/rt:evade <payload/context>` | AV/EDR evasion strategy |
| `/rt:report <finding>` | Professional pentest finding write-up |

## Examples

```
/rt:recon Laravel application exposed on Shodan, unknown version
/rt:exploit CVE-2025-54236 Magento unauthenticated RCE
/rt:ctf PHP web challenge with a file upload endpoint and source provided
/rt:evade Cobalt Strike beacon, x64, Windows 11 with Defender + CrowdStrike
/rt:report SQL injection in /api/users?id= leading to full DB dump
```

## Structure

```
gemini-redteam/
├── gemini-extension.json     # Manifest
├── GEMINI.md                 # System context injected every session
├── commands/
│   └── rt/
│       ├── recon.toml
│       ├── exploit.toml
│       ├── ctf.toml
│       ├── evade.toml
│       └── report.toml
└── skills/
    └── redteam/
        └── SKILL.md          # On-demand agent skill
```

## Notes

- Context is injected globally — all sessions where the extension is active get the red team persona
- Commands conflict-resolve with prefix: if `/recon` exists elsewhere, these become `/gemini-redteam:rt:recon`
- Skills are token-efficient — only activated when relevant
