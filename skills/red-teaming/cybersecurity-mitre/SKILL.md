---
name: cybersecurity-mitre
description: "Use when performing security research, red-teaming, or threat modeling. 753+ structured cybersecurity skills mapped to MITRE ATT&CK tactics and techniques."
version: 1.0.0
author: mukul975
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [cybersecurity, mitre, att&ck, red-teaming, penetration-testing, threat-modeling, security-research, offensive-security]
    homepage: https://github.com/mukul975/Anthropic-Cybersecurity-Skills
    related_skills: [godmode]
---

# Cybersecurity MITRE ATT&CK Skills

## Overview

753+ structured cybersecurity skills mapped to the [MITRE ATT&CK](https://attack.mitre.org/) framework — the most comprehensive security skills collection in the Hermes ecosystem. 4k+ stars. Covers the full attack lifecycle from initial access through exfiltration, organized by tactic and technique ID for structured red-teaming, threat modeling, and security research.

Built on the agentskills.io standard — compatible across Hermes, Claude Code, and Codex.

## When to Use

- Red-teaming or penetration testing engagements
- Threat modeling a system or architecture
- Researching a specific MITRE ATT&CK technique (e.g. T1059, T1078)
- Writing security reports or TTPs documentation
- CTF challenges requiring offensive security techniques
- Security awareness training and attack simulation
- Don't use for: production exploitation of systems you don't own or have no authorization to test

## Installation

```bash
git clone https://github.com/mukul975/Anthropic-Cybersecurity-Skills
cd Anthropic-Cybersecurity-Skills
```

Install into Hermes:

```bash
hermes skills install https://raw.githubusercontent.com/mukul975/Anthropic-Cybersecurity-Skills/main/SKILL.md
```

Or use the agentskills.io install path if configured:

```bash
hermes skills install mukul975/Anthropic-Cybersecurity-Skills
```

## MITRE ATT&CK Coverage

Skills are organized by the 14 MITRE ATT&CK tactics:

| Tactic | ID | Examples |
|---|---|---|
| **Reconnaissance** | TA0043 | OSINT, scanning, phishing research |
| **Resource Development** | TA0042 | Infrastructure setup, malware staging |
| **Initial Access** | TA0001 | Phishing, exploit public-facing apps, supply chain |
| **Execution** | TA0002 | Command & scripting (T1059), user execution |
| **Persistence** | TA0003 | Scheduled tasks, registry keys, boot/logon autostart |
| **Privilege Escalation** | TA0004 | Sudo abuse, token impersonation |
| **Defense Evasion** | TA0005 | Obfuscation, LOLBAS, timestomping |
| **Credential Access** | TA0006 | Dumping, brute force, keylogging |
| **Discovery** | TA0007 | Network/host discovery, account enumeration |
| **Lateral Movement** | TA0008 | Pass-the-hash, remote services |
| **Collection** | TA0009 | Data staging, screen capture, clipboard |
| **Command & Control** | TA0011 | C2 protocols, tunneling, proxies |
| **Exfiltration** | TA0010 | Data compression, transfer limits |
| **Impact** | TA0040 | Ransomware, defacement, data destruction |

## Usage

Reference by tactic, technique ID, or natural language:

```
"Walk me through a T1059.001 PowerShell execution attack chain"
"What are the persistence techniques for a Windows target?"
"Simulate a phishing campaign initial access scenario"
"Help me write a threat model for a web app with JWT auth"
"What MITRE techniques does this malware sample map to?"
```

Load the full collection for a session:

```bash
/skill cybersecurity-mitre
```

## Responsible Use

- Only use against systems you own or have explicit written authorization to test
- All techniques are for educational, research, and authorized security testing purposes
- Pair with defensive blue-team skills for full-spectrum security work

## Common Pitfalls

1. **Scope creep.** Always confirm authorization scope before running active techniques. Passive research (OSINT, documentation) vs active exploitation require different authorization levels.
2. **Outdated technique IDs.** MITRE ATT&CK updates sub-technique numbering. Verify against [attack.mitre.org](https://attack.mitre.org) for the latest IDs.
3. **Tool availability.** Some technique skills assume specific tools (Mimikatz, Metasploit, Impacket). Check prerequisites before executing.
4. **Legal jurisdiction.** Laws vary by country. Unauthorized computer access is illegal everywhere — always confirm scope in writing.

## Verification Checklist

- [ ] Skill installed and loadable: `hermes skills list | grep cybersecurity-mitre`
- [ ] Test query resolves a technique: *"Explain T1078 Valid Accounts"*
- [ ] Authorization scope confirmed for any active testing
- [ ] Target environment is isolated or explicitly authorized
