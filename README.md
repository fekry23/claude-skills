# claude-skills

A collection of general-purpose **skills** for [Claude Code](https://claude.com/claude-code).

A skill is a folder containing a `SKILL.md` file: step-by-step instructions, plus optional helper scripts, that Claude follows when you type its slash command (e.g. `/my-skill`) or ask for that task in plain words. Install a skill once and Claude does the task the same way every time.

## Contents

- [Skills](#skills)
- [Prerequisites](#prerequisites)
- [Quick start](#quick-start)
- [Everyday commands](#everyday-commands)
- [Repository layout](#repository-layout)
- [Writing a skill](#writing-a-skill)
- [Troubleshooting](#troubleshooting)
- [Maintainer notes](#maintainer-notes)

## Skills

| Skill | Command | What it does |
|---|---|---|
| _Coming soon_ | | |

## Prerequisites

| Requirement | Check |
|---|---|
| [Claude Code](https://claude.com/claude-code) | `claude --version` |
| [Git](https://git-scm.com/downloads) | `git --version` |

Some skills need extra tools, such as Python. Each skill lists them in its own `README.md`.

## Quick start

**1. Clone**

```bash
git clone https://github.com/fekry23/claude-skills.git
```

**2. Install a skill by linking its folder into Claude Code's skills directory**

Claude Code loads personal skills from `~/.claude/skills/<skill-name>/`. A link, rather than a copy, means `git pull` keeps the skill up to date.

macOS / Linux:

```bash
mkdir -p ~/.claude/skills
ln -s "$PWD/claude-skills/<skill-name>" ~/.claude/skills/<skill-name>
```

Windows (PowerShell):

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills" | Out-Null
New-Item -ItemType Junction `
  -Path   "$env:USERPROFILE\.claude\skills\<skill-name>" `
  -Target "$PWD\claude-skills\<skill-name>"
```

**3. Verify.** In Claude Code, run `/skills`. The skill should be listed. ✅

> Prefer to install just one skill without git? Copy its folder into `~/.claude/skills/` instead.

## Everyday commands

| Task | Command |
|---|---|
| Use a skill | `/<skill-name>` in Claude Code, or describe the task in plain words |
| List installed skills | `/skills` in Claude Code |
| Update all skills | `git -C claude-skills pull` |
| Uninstall a skill (keeps the repo) | macOS/Linux: `rm ~/.claude/skills/<skill-name>` · Windows: `cmd /c rmdir "%USERPROFILE%\.claude\skills\<skill-name>"` |

> ⚠️ On Windows, remove links with `rmdir` only. `Remove-Item -Recurse` on a junction can delete the files it points to.

## Repository layout

```
claude-skills/
├── README.md
├── .githooks/pre-commit     privacy check (see Maintainer notes)
└── <skill-name>/
    ├── README.md            human guide: prerequisites, setup, usage
    ├── SKILL.md             instructions Claude follows
    ├── config.example.md    optional: template for personal settings
    └── scripts/             optional: helper code
```

If a skill needs personal settings, copy its `config.example.md` to `config.md` and fill it in. `config.md` is git-ignored, so your details stay on your machine.

## Writing a skill

Minimal `SKILL.md`:

```markdown
---
name: my-skill
description: One specific sentence on what it does AND when to use it, e.g. "Rename and compress receipt photos into PDFs. Use when the user wants to prepare receipts for an expense claim."
---

# My skill

1. First step…
2. Second step…
```

Tips:
- **The description is what triggers the skill.** Name the task and the situations it applies to.
- **Keep `SKILL.md` focused.** Put long reference material or templates in separate files and point to them.
- **Put repeatable logic in `scripts/`,** so Claude runs tested code instead of rewriting it each time.
- **Never commit secrets or personal data.** Use `config.example.md` + a git-ignored `config.md`.

## Troubleshooting

| Symptom | Fix |
|---|---|
| Skill not shown in `/skills` | Check that the link exists at `~/.claude/skills/<skill-name>` and points at a folder containing `SKILL.md`, then restart Claude Code. |
| Link command says *already exists* | Remove the old link first (see [Everyday commands](#everyday-commands)). |
| Skill doesn't trigger from plain words | Use `/<skill-name>` explicitly, or make the skill's `description` more specific. |

## Maintainer notes

This repo is public, so a **pre-commit hook** guards against leaking private details. It blocks a commit when:
- a `config.md` or `history.md` file is staged, or
- staged text matches a line in a private blocklist kept **outside** the repo: `~/.config/claude-skills/blocklist.txt` (or the path in `$CLAUDE_SKILLS_BLOCKLIST`), with one case-insensitive regex per line.

Enable it after cloning:

```bash
git config core.hooksPath .githooks
mkdir -p ~/.config/claude-skills && touch ~/.config/claude-skills/blocklist.txt   # then add your private terms
```

The hook refuses to commit if the blocklist file is missing, so it can't silently stop protecting you.
