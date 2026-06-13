---
name: profile-repo-inspection
description: Use when auditing a public Hermes profile repository before install; inspect files, config, skills, scripts, cron, MCP, env requirements, and summarize capabilities with safety caveats.
version: 1.0.0
author: Volcanic
license: MIT
metadata:
  hermes:
    tags: [hermes, profiles, audit, safety, registry]
    related_skills: [secret-state-scan]
---

# Profile Repo Inspection

## Overview

Public Hermes profiles are installable agent distributions. Treat them like software supply-chain artifacts: useful, but worth inspecting before install.

This skill produces a read-only report that helps a user decide whether to install a profile, inspect deeper, or avoid it.

## When to Use

Use this when the user asks about:

- a Hermes Profiles registry entry
- a public profile repo such as `owner/profile-name`
- what an installable Hermes profile can do
- whether a profile is safe to install
- comparing multiple public profiles

## Workflow

1. Identify the source repository URL. Prefer the registry's repo link over copied install commands.
2. Clone read-only into a scratch directory, usually `/tmp/profile-audit-<name>`.
3. List files excluding `.git` and note unusual directories.
4. Read core profile files when present:
   - `README.md`
   - `SOUL.md` or persona files
   - `config.yaml`
   - `distribution.yaml`
   - `profile.yaml`
   - `.env.EXAMPLE`
   - `mcp.json` / MCP config
   - `cron/`
   - `skills/`
   - `scripts/`
5. Summarize observed capabilities:
   - stated purpose
   - model/provider defaults
   - toolsets requested
   - bundled skills
   - scripts and automation
   - required environment variables
6. Run or reason through the secret/private-state scan. Do not execute arbitrary repo scripts unless they are simple scanners you have inspected.
7. Produce a report with a verdict: install isolated, inspect deeper, or avoid.

## Report Requirements

Always distinguish:

- **Observed facts** — files and config you actually inspected.
- **Inferred capabilities** — what the profile is likely intended to do.
- **Local dependencies** — tools/API keys needed on the installer's machine.
- **Unknowns** — files not present, inaccessible repos, or generated behavior not inspected.

## Safety Rules

- Do not install into `default`.
- Do not run setup/install scripts from an audited repo unless explicitly approved.
- Do not copy secrets, sessions, memories, logs, or auth state.
- If a profile enables `toolsets: [all]`, say so clearly.
- If cron jobs, MCP servers, or scripts are present, inspect and explain side effects.

## Common Pitfalls

1. Treating a registry description as proof. Always inspect the repo.
2. Equating a requested toolset with actual capability. Local credentials still matter.
3. Missing background automation. Cron and MCP can create side effects.
4. Recommending installation into the active profile. Use an isolated named profile.
5. Declaring a repo safe because no secret was obvious. Say "no secret/private state found in inspected files."

## Verification Checklist

- [ ] Source repo identified
- [ ] Files listed excluding `.git`
- [ ] README/persona/config/distribution/env files inspected
- [ ] Skills/scripts/cron/MCP checked
- [ ] Secret/private-state scan completed or limitations stated
- [ ] Report includes capability vs. local dependency distinction
- [ ] Recommendation uses isolated profile install if applicable
