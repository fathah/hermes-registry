---
name: drawio
description: "Use when you need to generate architecture diagrams, flowcharts, ER diagrams, or any visual diagram from natural language. Exports to PNG, SVG, and PDF via draw.io."
version: 1.0.0
author: Agents365-ai
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [diagrams, drawio, architecture, flowchart, erd, visualization, export, png, svg, pdf]
    homepage: https://github.com/Agents365-ai/drawio-skill
    related_skills: [open-design]
---

# Draw.io Diagram Skill

## Overview

Generates [draw.io](https://www.drawio.com/) diagrams from natural language descriptions and exports them to PNG, SVG, or PDF. Works across Claude Code, OpenClaw, Hermes, and Codex. 1.1k+ stars.

Useful when an agent needs to communicate architecture or process visually without a separate design step — generate the diagram inline as part of a task.

## When to Use

- User asks for an architecture diagram, system diagram, or component map
- User needs a flowchart, sequence diagram, or state machine
- User asks for an ER diagram or database schema visualization
- User wants to export a diagram as an image (PNG/SVG/PDF) to include in a doc or PR
- User says "draw", "diagram", "visualize", "chart", or "map out" a system/flow
- Don't use for: rich UI mockups (use open-design), simple ASCII diagrams, or Excalidraw-style sketches

## Prerequisites

The skill uses the draw.io CLI (`drawio` / `xvfb-run drawio`) for export. Install:

**macOS:**
```bash
brew install --cask drawio
```

**Linux (headless):**
```bash
# Download the .deb from https://github.com/jgraph/drawio-desktop/releases
sudo dpkg -i drawio-amd64-*.deb
sudo apt-get install -f
# Headless export requires xvfb
sudo apt-get install xvfb
```

**Windows:**
Download and install from [https://github.com/jgraph/drawio-desktop/releases](https://github.com/jgraph/drawio-desktop/releases).

## Workflow

1. **Understand the diagram request** — identify the type (architecture, flowchart, ER, sequence, etc.) and the components/relationships involved.
2. **Generate the XML** — produce valid draw.io XML (`<mxGraphModel>` format) representing the diagram.
3. **Write the `.drawio` file** — save to a temp or project path.
4. **Export** — run the draw.io CLI to render the output format.
5. **Return the path** — give the user the exported file path and confirm dimensions.

## Export Commands

```bash
# Export to PNG (macOS/Linux GUI)
drawio --export --format png --output diagram.png diagram.drawio

# Export to SVG
drawio --export --format svg --output diagram.svg diagram.drawio

# Export to PDF
drawio --export --format pdf --output diagram.pdf diagram.drawio

# Headless export on Linux (no display)
xvfb-run drawio --export --format png --output diagram.png diagram.drawio
```

## Diagram Types & Prompting

| Diagram type | Example prompt |
|---|---|
| **Architecture** | "Draw an AWS architecture with an ALB, two EC2 instances, and an RDS database" |
| **Flowchart** | "Create a flowchart for a user signup and email verification flow" |
| **ER diagram** | "Generate an ER diagram for a blog with users, posts, comments, and tags" |
| **Sequence diagram** | "Draw a sequence diagram for OAuth2 authorization code flow" |
| **State machine** | "Diagram the states for an order: pending → paid → shipped → delivered → returned" |
| **Network diagram** | "Map a corporate network with firewall, DMZ, internal VLAN, and VPN gateway" |

## draw.io XML Structure

Minimal template for reference:

```xml
<mxGraphModel>
  <root>
    <mxCell id="0"/>
    <mxCell id="1" parent="0"/>
    <!-- Nodes -->
    <mxCell id="2" value="Service A" style="rounded=1;" vertex="1" parent="1">
      <mxGeometry x="100" y="100" width="120" height="60" as="geometry"/>
    </mxCell>
    <!-- Edges -->
    <mxCell id="3" edge="1" source="2" target="4" parent="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
  </root>
</mxGraphModel>
```

## Common Pitfalls

1. **No display on headless Linux.** Use `xvfb-run` prefix for all export commands. Without it, the CLI exits with a display error.
2. **`drawio` not in PATH on macOS.** After cask install, the binary may be at `/Applications/draw.io.app/Contents/MacOS/draw.io` — symlink it or use the full path.
3. **Invalid XML.** Always validate that the `<mxGraphModel>` wraps a `<root>` with at least cells `id="0"` and `id="1"` before export.
4. **Overlapping nodes.** When generating multi-node diagrams, space geometry values explicitly (use 200px gaps) to avoid invisible overlaps.
5. **Large diagrams.** For diagrams with 20+ nodes, generate in logical layers (e.g. generate DB layer, then app layer, then network layer) and merge the XML.

## Verification Checklist

- [ ] `drawio --version` returns a version string
- [ ] Test export: `drawio --export --format png --output test.png` on a sample `.drawio` file
- [ ] On headless Linux: `xvfb-run drawio --version` works
- [ ] Generated PNG opens correctly and matches the described diagram
- [ ] File path returned to user is accessible
