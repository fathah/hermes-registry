---
name: open-design
description: "Use when you need to generate UI designs, web/mobile mockups, presentations, dashboards, or documents from natural language. 31 composable skills across 129 design systems with image, video, and audio generation."
version: 1.0.0
author: nexu-io
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [design, ui, mockup, web, mobile, dashboard, presentation, image-gen, video-gen, design-systems, local-first]
    homepage: https://github.com/nexu-io/open-design
    related_skills: [drawio-skill, black-forest-labs-flux]
---

# Open Design

## Overview

Open Design is a local-first, open-source alternative to Anthropic's Claude Design. It provides 31 composable skills covering web, mobile, decks, dashboards, and documents, built on top of 129 design systems including Linear, Stripe, Vercel, Notion, Apple, and more. Supports image generation (gpt-image-2), video generation (Seedance 2.0, HyperFrames), and audio. 28k+ stars.

Auto-detects 15 coding-agent CLIs from PATH (including Hermes) and integrates via ACP/JSON-RPC. Includes a BYOK proxy, sandboxed previews, and can import Claude Design exports.

## When to Use

- User asks to design a UI, landing page, dashboard, mobile screen, or presentation
- User wants visual output from natural language descriptions
- User needs a design asset that references a specific design system (Stripe, Linear, Vercel, Notion, etc.)
- User wants to generate images, short videos, or audio alongside design work
- User is migrating from Claude Design exports
- Don't use for: code generation without visual intent, or when a simpler diagram tool (draw.io, Excalidraw) is sufficient

## Installation

```bash
git clone https://github.com/nexu-io/open-design
cd open-design
npm install
npm run setup
```

The setup wizard auto-detects Hermes in PATH and configures the ACP/JSON-RPC integration automatically.

**BYOK (Bring Your Own Key):** Set your API keys for the generation backends you want to use:

```bash
export OPENAI_API_KEY=...         # gpt-image-2 image generation
export SEEDANCE_API_KEY=...       # Seedance 2.0 video generation
```

## Skill Categories

Open Design ships 31 composable skills across 5 output types:

| Category | Skills | Examples |
|---|---|---|
| **Web** | Landing pages, SaaS dashboards, marketing sites | Hero sections, pricing tables, nav bars |
| **Mobile** | iOS/Android screens, app flows | Onboarding, home screens, settings |
| **Decks** | Pitch decks, slide presentations | Investor decks, product demos |
| **Dashboards** | Analytics, ops, monitoring | Charts, KPIs, data tables |
| **Documents** | Reports, docs, changelogs | Product specs, release notes |

## Design Systems

129 design systems available. Commonly used:

- **Dev tools:** Linear, Vercel, Supabase, GitHub
- **Payments:** Stripe, PayPal
- **Productivity:** Notion, Figma, Slack
- **Consumer:** Apple, Google Material, Airbnb
- **Enterprise:** Salesforce, Microsoft Fluent

Specify a design system in your prompt: *"Design a settings page in the Linear design system"*

## Generation Backends

| Type | Model | Backend mode |
|---|---|---|
| **Image** | gpt-image-2 | MeiGen cloud, BYOK, or offline ComfyUI |
| **Video** | Seedance 2.0, HyperFrames | MeiGen cloud or BYOK |
| **Audio** | — | BYOK |

Three backend modes: MeiGen cloud (managed), any OpenAI-compatible API (BYOK), or fully offline ComfyUI.

## Usage with Hermes

Once installed, Hermes auto-detects open-design via ACP/JSON-RPC. Invoke naturally:

```
"Design a SaaS pricing page in the Stripe style"
"Create a mobile onboarding flow for a fitness app"
"Make a 5-slide investor pitch deck for a B2B tool"
"Generate a dashboard for infrastructure monitoring"
```

Or reference the skill directly:

```
/skill open-design
```

## Importing Claude Design Exports

```bash
open-design import --from claude-design path/to/export.zip
```

Converts Claude Design project exports into open-design skill artifacts.

## Common Pitfalls

1. **No design system match.** If the named design system isn't found, open-design falls back to a generic system. Specify the system explicitly or browse available ones with `open-design systems list`.
2. **Video generation timeout.** Video gen (Seedance) can take 30–120 seconds. Increase Hermes MCP timeout if using via MCP integration.
3. **ComfyUI offline mode.** Requires a running ComfyUI server on localhost. Start it before invoking image/video generation in offline mode.
4. **ACP not detecting Hermes.** Run `open-design detect` to re-run the CLI detection. If Hermes isn't in PATH, set `HERMES_CLI_PATH` env var.
5. **BYOK proxy misconfigured.** Verify keys are exported before starting Hermes. The proxy reads them at startup, not per-request.

## Verification Checklist

- [ ] `npm run setup` completed without errors
- [ ] `open-design detect` shows Hermes in detected CLIs
- [ ] At least one generation backend configured (OPENAI_API_KEY or ComfyUI running)
- [ ] Test prompt produces a design artifact: *"Design a simple login page"*
- [ ] Sandboxed preview renders correctly in browser
