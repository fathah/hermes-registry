---
name: hermes-skill-factory
description: "Use when you want to auto-generate reusable skills from repeated workflows. Watches your Hermes sessions, detects patterns, and proposes SKILL.md + plugin.py pairs on demand."
version: 1.0.0
author: Romanescu11
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [meta, skill-generation, automation, workflows, plugin, self-improving]
    homepage: https://github.com/Romanescu11/hermes-skill-factory
    related_skills: [hermes-agent, hermes-agent-skill-authoring]
---

# Hermes Skill Factory

## Overview

Skill Factory is a meta-skill that watches your Hermes sessions and automatically turns repeated workflows into reusable skills. Every time you solve a problem with Hermes — setting up a project, debugging code, creating a PR — that workflow normally disappears at session end. Skill Factory detects patterns across your work and, at the right moment, proposes a structured skill for you to accept or skip.

Built for Hermes Agent v2026.3+.

## When to Use

- You find yourself explaining the same workflow to Hermes more than once
- You want to capture a workflow you just completed as a reusable skill
- You want to generate both a `SKILL.md` (AI instructions) and a `plugin.py` (slash command) in one step
- Don't use for: one-off tasks unlikely to repeat, or skills that already exist in the registry

## Installation

```bash
git clone https://github.com/Romanescu11/hermes-skill-factory
cd hermes-skill-factory
bash install.sh
```

Or manually:

```bash
mkdir -p ~/.hermes/skills/meta/skill-factory
cp skills/skill-factory/SKILL.md ~/.hermes/skills/meta/skill-factory/
cp plugins/skill_factory.py ~/.hermes/plugins/
```

Then activate:

```bash
hermes skills reload
hermes skills enable skill-factory
```

## Usage

Skill Factory runs silently in the background during every session. When it detects a repeatable pattern it prompts:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 🏭 SKILL FACTORY — New Skill Detected ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
I noticed you repeatedly set up a Python environment, installed dependencies, and ran tests in the same order.

Proposed Skill: python-env-setup
Category:       software-development
Description:    Reproducible Python project setup workflow

What it captures:
  1. Create venv and activate
  2. Upgrade pip and install dependencies
  3. Run pytest to verify environment

Generate: [A] SKILL.md  [B] plugin.py  [C] Both  [D] Skip
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Choose **C** and it writes both files immediately:
- `~/.hermes/skills/<category>/<name>/SKILL.md` — AI instructions for the workflow
- `~/.hermes/plugins/<name>.py` — a slash command that triggers it directly

### Slash Commands

| Command | Description |
|---|---|
| `/skill-factory propose` | Manually trigger a proposal from observed patterns |
| `/skill-factory list` | List all proposed skills in the queue |
| `/skill-factory status` | Show observer status and session stats |
| `/skill-factory queue` | View pending proposals |
| `/skill-factory save <name>` | Save a specific named proposal |
| `/skill-factory clear` | Clear the proposal queue |

You can also use natural language:
- "Save this as a skill"
- "Remember how to do this"
- "Turn this workflow into a reusable skill"

## What Gets Generated

### SKILL.md

A complete skill definition following Hermes' native skill format:

```yaml
---
name: Python Env Setup
category: software-development
description: Reproducible Python project setup
tags: [python, venv, testing]
---
# Python Env Setup
## When to Activate
...
## Workflow
### Phase 1: Environment
1. python -m venv .venv
2. source .venv/bin/activate
...
## Examples
...
```

### plugin.py

A scaffolded Hermes plugin with a slash command:

```python
def register(hermes):
    @hermes.command(name="python-env-setup", ...)
    async def run_skill(ctx, args=""):
        # Step 1: Create venv
        # Step 2: Install deps
        # Step 3: Run tests
        ...
```

## Repo Structure

```
hermes-skill-factory/
├── skills/
│   └── skill-factory/
│       └── SKILL.md          # The meta-skill (core AI instructions)
├── plugins/
│   └── skill_factory.py      # Plugin: /skill-factory commands
├── templates/
│   ├── SKILL_TEMPLATE.md     # Template for generated skills
│   └── PLUGIN_TEMPLATE.py    # Template for generated plugins
├── examples/
│   └── generated/
│       └── git-pr-workflow/  # Example Skill Factory output
│           └── SKILL.md
├── docs/
│   └── how-it-works.md
└── install.sh
```

## Common Pitfalls

1. **Wrong Hermes version.** Requires v2026.3+. Check with `hermes --version` before installing.
2. **Skill not activating.** Run `hermes skills reload` and `hermes skills enable skill-factory` after install.
3. **Generated skill path.** Skills are written to `~/.hermes/skills/`, not the in-repo `skills/` tree. Use `hermes skills publish` if you want to share to the registry.
4. **Plugin not available.** Ensure `skill_factory.py` is in `~/.hermes/plugins/` and restart the session.
5. **Duplicate proposals.** If Skill Factory proposes a skill that already exists, use `/skill-factory clear` to discard and add a note to your session so it doesn't re-propose.

## Verification Checklist

- [ ] Hermes Agent v2026.3+ installed (`hermes --version`)
- [ ] `install.sh` ran without errors, or files copied manually
- [ ] `hermes skills reload` and `hermes skills enable skill-factory` executed
- [ ] `/skill-factory status` returns active observer
- [ ] A test proposal can be triggered via `/skill-factory propose`
- [ ] Generated `SKILL.md` passes frontmatter validation (`name`, `description` present, description ≤ 1024 chars)
