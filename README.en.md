# powershell-windows-best-skill

> **Status: EXPERIMENTAL** — This skill is actively being tested and refined. Use at your own risk. Always validate generated scripts before running them in critical or production environments.

A unified skill for PowerShell programming on **Windows only**. Consolidates multiple community skills into a single, opinionated reference optimized for Windows automation with PowerShell 7+.

> 📖 **Read in another language:** [Português / Portuguese](README.md)

---

## What This Is

`SKILL.md` is an instruction file (skill/context) that gives any LLM deep, Windows-specific PowerShell expertise when invoked. It covers syntax rules, error handling, performance patterns, security hardening, module management, testing, and more — all without cross-platform noise.

The skill is **harness-agnostic**: it works with any LLM and any harness/agent that supports loading instruction, context, or rules files (Claude Code, Cursor, OpenCode, Aider, Codex, Cline, Continue, Continue.dev, GitHub Copilot Chat, or manual prompts). The file is plain Markdown with clear rules.

---

## How to Use

### 1. Load the skill into your harness

How you load it depends on your harness. Common examples:

- **Claude Code / OpenCode / Cline:** copy `SKILL.md` to your harness's skills/context directory, or reference it directly from this repository
- **Cursor / Continue / Continue.dev:** paste the contents into your project rules file (e.g., `.cursorrules`, `.continuerc`, `rules.md`) or the system prompt
- **Copilot Chat / ChatGPT / Gemini / any LLM chat:** attach `SKILL.md` as context, paste its contents into the system prompt, or upload the file in the conversation
- **Aider / Codex CLI:** pass the path to `SKILL.md` as an additional read/rules file

### 2. Invoke in a conversation

Reference the skill file in your session:

```
@SKILL.md write a script that monitors disk space and sends an alert
```

Or ask the model explicitly:

```
Using the powershell-windows skill, refactor this script to handle errors properly
```

### 3. What the model will apply

Once the skill is active, the LLM will follow the patterns defined in the file:

- Wrap cmdlets in parentheses when using logical operators
- Use ASCII-only output (no emoji in scripts)
- Check for null before accessing properties
- Always specify `-Depth` on `ConvertTo-Json`
- Use `List[T]` instead of `$array +=` for dynamic collections
- Prefer `Join-Path` for path construction
- Include `#Requires`, `Set-StrictMode`, and `$ErrorActionPreference` in every script
- Apply input validation attributes on all parameters

---

## Skill File

| File | Description |
|------|-------------|
| `SKILL.md` | Unified Windows-only skill (use this one) — harness-agnostic |

### Source Skills

The unified skill was built by consolidating and filtering the following community skills.

| Repository | Scope | Status |
|------------|-------|--------|
| [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills) · [skills.sh](https://www.skills.sh/sickn33/antigravity-awesome-skills/powershell-windows) | Windows pitfalls and critical syntax rules | Merged |
| [josiahsiegel/claude-plugin-marketplace](https://github.com/josiahsiegel/claude-plugin-marketplace) · [skills.sh](https://www.skills.sh/josiahsiegel/claude-plugin-marketplace/powershell-master) | Full PowerShell master skill — syntax, security, modules | Merged (cross-platform parts removed) |
| [404kidwiz/claude-supercode-skills](https://github.com/404kidwiz/claude-supercode-skills) · [skills.sh](https://www.skills.sh/404kidwiz/claude-supercode-skills/powershell-7-expert) | PS7+ features | Partially merged (Windows-relevant parts only) |

---

## What Was Removed from Source Skills

The following content was intentionally excluded from the unified skill:

- Linux and macOS installation paths
- `$IsLinux`, `$IsMacOS` platform detection
- SSH remoting (cross-platform WinRM alternative)
- "Test on all platforms" guidance
- Bash/shell integration patterns
- CI/CD multi-platform matrix configurations
- Cross-platform path handling patterns

---

## Disclaimer

```
THIS SKILL IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND.

- Scripts and patterns suggested by this skill have not been validated
  against every environment, configuration, or Windows version.
- Always review generated scripts before executing them.
- Test in a non-production environment first.
- Never run untrusted scripts with administrator privileges without review.
- The author assumes no responsibility for data loss, system damage,
  or any other consequence arising from the use of this skill.
```

**Before running any script in a critical environment:**

1. Read the entire script and understand what it does
2. Run `Invoke-ScriptAnalyzer -Path script.ps1` and fix all findings
3. Test with `-WhatIf` where supported
4. Test in a staging or isolated environment first
5. Ensure you have a rollback plan (backup, snapshot, etc.)

---

## Compatibility

| Harness / LLM | How to use |
|---------------|-----------|
| Claude Code, OpenCode, Cline | Load as skill/context or `@SKILL.md` |
| Cursor, Continue, Continue.dev | Paste contents into `.cursorrules` / `.continuerc` / `rules.md` |
| Codex CLI, Aider | Pass as additional rules file |
| Copilot Chat, ChatGPT, Gemini, any chat | Attach the file, paste into system prompt, or upload |

> The skill does not depend on any API, plugin, or specific runtime. It is plain Markdown.

---

## Contributing

Found an error, outdated pattern, or missing Windows-specific content? Open an issue or submit a PR. Keep contributions Windows-only — cross-platform content will be rejected.
