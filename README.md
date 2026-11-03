# claude-skills

General-purpose [Claude Code](https://claude.com/claude-code) skills.

| Skill | What it does |
|---|---|
| _none yet_ | |

## Layout

```
<skill-name>/
  SKILL.md            instructions + front matter (name, description) that Claude reads
  config.example.md   optional template for personal settings
  scripts/            optional helper code
```

A skill that needs personal settings keeps them in `config.md` next to `SKILL.md`. That file is git-ignored; copy `config.example.md` to create it.

## Using a skill

Link the skill folder into Claude Code's personal skills directory:

```powershell
New-Item -ItemType Junction -Path "$env:USERPROFILE\.claude\skills\<skill-name>" -Target "<path-to-this-repo>\<skill-name>"
```

On macOS/Linux: `ln -s "<path-to-this-repo>/<skill-name>" ~/.claude/skills/<skill-name>`

## Contributing (note to self)

This repo is public. A pre-commit hook (`.githooks/pre-commit`) blocks commits that:
- add a `config.md` or `history.md` file, or
- contain any word from a private blocklist kept **outside** the repo (default `~/.config/claude-skills/blocklist.txt`).

Enable it after cloning with `git config core.hooksPath .githooks`.
