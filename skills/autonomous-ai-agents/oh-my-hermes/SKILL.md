---
name: oh-my-hermes
description: "Use when orchestrating multi-agent workflows: deep research, planning with consensus, verified execution, triage, or autopilot. Suite of driver skills for Hermes-native multi-agent coordination."
version: 1.0.0
author: witt3rd
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [multi-agent, orchestration, research, planning, execution, triage, autopilot, delegation, consensus]
    homepage: https://github.com/witt3rd/oh-my-hermes
    related_skills: [hermes-agent, hermes-skill-factory, subagent-driven-development]
---

# oh-my-hermes

## Overview

Multi-agent orchestration skill suite for Hermes, inspired by `oh-my-claudecode` and rebuilt on Hermes primitives. Provides composable driver skills covering the full agentic workflow lifecycle: research → interview → consensus planning → verified execution.

Composes end-to-end: `deep-research` → `deep-interview` → `ralplan` (consensus plan) → `ralph` (verified execution) — with `triage` and `autopilot` for routing and hands-off operation.

## When to Use

- You want to run deep research on a topic with structured output
- You need consensus planning before executing a complex task (Planner → Architect → Critic)
- You want verified, iterative execution with automatic retry on failure
- You want to triage an incoming task to the right sub-skill automatically
- You want autopilot mode: Hermes routes, plans, and executes with minimal hand-holding
- Don't use for: simple single-turn tasks, or when `delegate_task` alone is sufficient

## Installation

```bash
git clone https://github.com/witt3rd/oh-my-hermes
cd oh-my-hermes
bash install.sh
```

Or install individual skills:

```bash
hermes skills install https://raw.githubusercontent.com/witt3rd/oh-my-hermes/main/skills/ralplan/SKILL.md
hermes skills install https://raw.githubusercontent.com/witt3rd/oh-my-hermes/main/skills/ralph/SKILL.md
```

## Skill Suite

### `deep-research`

Structured multi-source research with synthesis:

```
/skill deep-research
"Research the current state of MoE (Mixture of Experts) architectures in LLMs"
```

Produces: sources list, key findings, synthesis, open questions, confidence ratings.

### `deep-interview`

Structured elicitation of requirements or knowledge via targeted questions:

```
/skill deep-interview
"Help me clarify requirements for a multi-tenant SaaS billing system"
```

Produces: structured requirements doc, edge cases, ambiguities surfaced.

### `ralplan` — Planner → Architect → Critic Consensus

Three-phase planning with internal debate before committing:

1. **Planner** generates a candidate plan
2. **Architect** reviews for technical soundness
3. **Critic** stress-tests assumptions and identifies risks
4. Consensus plan is produced only when all three agree

```
/skill ralplan
"Plan a migration from a monolith to microservices for a mid-size e-commerce platform"
```

Best for: high-stakes tasks where a bad plan is expensive to undo.

### `ralph` — Verified Execute → Verify → Iterate

Execution loop with built-in verification:

1. **Execute** the plan step
2. **Verify** the output against the expected result
3. **Iterate** if verification fails (up to N retries)

```
/skill ralph
"Set up a production-ready Postgres database with connection pooling and monitoring"
```

Best for: infrastructure tasks, code generation, or any task where partial completion is worse than no completion.

### `triage`

Routes an incoming task description to the most appropriate driver skill:

```
/skill triage
"I need to build a CI/CD pipeline for a Python monorepo"
```

Returns: recommended skill(s), rationale, suggested starting prompt.

### `autopilot`

Hands-off mode — Hermes triages, plans, and executes autonomously with checkpoint confirmations at configurable risk levels:

```
/skill autopilot
"Refactor the authentication module to use JWT refresh tokens"
```

Autopilot will pause and request confirmation before any destructive or irreversible actions.

## Composition Pattern

Full end-to-end pipeline:

```
1. deep-research   → gather context and background
2. deep-interview  → clarify requirements and constraints
3. ralplan         → build a consensus plan
4. ralph           → execute with verification
```

Run inline:

```
"Use the oh-my-hermes pipeline to redesign our API rate limiting system.
Start with deep-research on rate limiting strategies, then interview me
for constraints, then ralplan, then ralph the implementation."
```

## Common Pitfalls

1. **Skipping triage on complex tasks.** Always run `triage` first if you're unsure which driver to use — it prevents using `ralph` on a task that needs `ralplan` first.
2. **`ralph` retries exhausted.** If verification keeps failing, surface the error to the user rather than retrying indefinitely. `ralph` defaults to 3 retries — increase via config for flaky environments.
3. **`ralplan` critic loop.** In rare cases the Planner/Architect/Critic loop doesn't converge. Set a max round limit (default: 5) to prevent runaway token spend.
4. **`autopilot` on destructive tasks.** Autopilot will confirm before destructive actions, but make sure the confirmation threshold matches your risk tolerance — configure it before using on production systems.
5. **Skill not found after install.** Run `hermes skills reload` after `install.sh` to force the skill loader to pick up new entries.

## Verification Checklist

- [ ] Skills installed: `hermes skills list | grep -E "ralplan|ralph|triage|autopilot"`
- [ ] `hermes skills reload` run after install
- [ ] Test `triage` with a simple task description — returns a recommended skill
- [ ] Test `ralplan` on a small task — produces a 3-phase consensus plan
- [ ] Test `ralph` on a reversible task — executes and verifies output
