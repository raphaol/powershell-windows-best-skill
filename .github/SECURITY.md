# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in this repository, **do not open a public issue**.

Instead, report it privately through one of the following channels:

- **GitHub Security Advisory (recommended):** repository *Security* tab → *Report a vulnerability*
- **Owner contact:** open a DM on GitHub or use the contact from the [@raphaol](https://github.com/raphaol) profile

Please include in the report:

1. A clear description of the vulnerability and its potential impact
2. Steps to reproduce or a PoC
3. Affected versions/branches
4. Suggested mitigation, if any

## Scope

This policy covers only the content of this repository (`powershell-windows-best-skill`), which consists solely of the `SKILL.md` skill file, documentation, and repo configuration.

PowerShell scripts *generated* by the skill during use are out of scope — these are the responsibility of whoever generates and runs them. Before using any generated script, follow the checklist in the [README](README.md) (`Invoke-ScriptAnalyzer`, `-WhatIf`, staging environment, rollback plan).

## Active Security Measures in the Repository

- ✅ Secret scanning and push protection enabled
- ✅ `main` branch protected (PR review required)
- ✅ Temporary branches automatically deleted after merge
- ✅ Vulnerability alerts and Dependabot security updates enabled

## Out of Scope

- Vulnerabilities in third-party dependencies not present in this repo (report to the upstream project maintainer)
- Attacks that require already-compromised Windows administrator privileges
- Content from source skills (repositories `sickn99/antigravity-awesome-skills`, `josiahsiegel/claude-plugin-marketplace`, `404kidwiz/claude-supercode-skills`)

## Recommended Safe Practices for Using the Skill

```
THIS SKILL IS PROVIDED "AS IS", WITHOUT ANY KIND OF WARRANTY.

- Always review generated scripts before executing them
- Never run untrusted scripts with administrator privileges
- Test in an isolated environment first
- The author assumes no responsibility for data loss or system damage
```