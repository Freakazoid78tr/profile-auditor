# Profile Auditor

A public-safe Hermes profile for read-only inspection of Hermes profile repositories before installation.

It helps users and agents answer:

- What does this profile do?
- What files, skills, scripts, cron jobs, and MCP servers are included?
- What model/provider/toolsets does it request?
- Does it appear to contain secrets, private state, sessions, memories, logs, wallets, or risky automation?
- Should it be installed, inspected more deeply, or avoided?

## Install

```bash
hermes profile install github.com/VolcanicWorks/profile-auditor --name profile-auditor --alias
```

Then copy the generated env example and add your model provider key if needed:

```bash
cp ~/.hermes/profiles/profile-auditor/.env.EXAMPLE ~/.hermes/profiles/profile-auditor/.env
$EDITOR ~/.hermes/profiles/profile-auditor/.env
```

Run it:

```bash
profile-auditor chat
# or
hermes -p profile-auditor chat -q "Audit https://github.com/deeploaf/hermes-research-agent"
```

## Example tasks

- "Audit this Hermes profile before I install it: https://github.com/deeploaf/hermes-research-agent"
- "Tell me what this profile can do and whether it has risky scripts or cron jobs."
- "Compare these two Hermes profiles and recommend which one to install isolated."
- "Check this profile repo for secrets, private state, broad toolsets, MCP config, and side effects."

## Toolset rationale

This profile requests only the toolsets needed for read-only repository inspection: web access for public pages, terminal/file/code execution for cloning into scratch space and running local scanners, and skills for its bundled audit workflow. It does not request `all`, cron, messaging, wallet, or admin toolsets.

## Branding/provenance

Published under the `VolcanicWorks` GitHub account with `Volcanic` as the profile author/brand. The profile is intended as a public-service contribution to Hermes profile safety, not as a private Volcanic operating profile.

## What is included

- `SOUL.md` — profile-auditor persona and safety rules.
- `config.yaml` — safe, narrow Hermes toolsets for repo inspection.
- `distribution.yaml` — install metadata and env requirements.
- `skills/profile-auditing/profile-repo-inspection/SKILL.md` — read-only inspection workflow.
- `skills/profile-auditing/secret-state-scan/SKILL.md` — secret/private-state scanning workflow.
- `templates/profile_audit_report.md` — standard report template.
- `scripts/profile_secret_scan.py` — local scanner for public profile distributions.
- `.env.EXAMPLE` — key names only, no secrets.

No cron jobs are enabled by default. No MCP servers are configured by default.

## Security and privacy notes

This repository is intended to be safe to publish. It must not contain:

- `.env` files or API keys
- `auth.json` or OAuth tokens
- `memories/`
- `sessions/`
- `logs/`
- `state.db*` or other SQLite runtime state
- wallets, keystores, identities, mnemonics, seeds, private keys
- browser screenshots, local caches, or workspaces

Before publishing updates, run:

```bash
python3 scripts/profile_secret_scan.py .
git status --short
```

## Output shape

The auditor produces a concise report:

1. Short verdict
2. What it is
3. What is inside
4. Capabilities
5. Toolsets and model defaults
6. Skills/scripts/cron/MCP review
7. Secret/private-state scan
8. Safety concerns
9. Recommended install command

## License

MIT
