# AGENTS.md

> **DIRECTIVE:** Read this file at every session start. It is the canonical source of truth for all agent behavior.
> After context compaction, re-read this file and `config.yaml` to restore phase context.

---

## Phase Paths

| Scope | Path |
|-------|------|
| Small | `1 → 3 → 4 → 5 → Done` |
| Medium | `1 → 3 → 4 → 5 → 6 → Done` |
| Large | `1 → 2 → 3 → 4 → 5 → 6 → Done` |

| Phase | Name | Scope |
|-------|------|-------|
| 1 | Seed | All |
| 2 | Discovery | Large only |
| 3 | Design | All |
| 4 | Test Design | All |
| 5 | Implementation | All |
| 6 | Review + Gate | Medium+ |

Phase 2 is skipped for Small and Medium — the path goes `1 → 3` directly.

---

## Phase Behavior

### Between Phases (all transitions except the Phase 3 gate)

After completing a phase and before starting the next:
1. Send a Slack notification (see § Slack Notifications)
2. Wait 90 seconds — `sleep 90` (override via `config.yaml` → `phases.pause_seconds`)
3. Continue to the next phase automatically

### Gate: After Phase 3 (Design) — HARD STOP

Phase 3 ends with a hard gate. The agent MUST stop and present:

- **Requirements summary** — distilled from Phase 1 (and Phase 2 for Large scope)
- **Design plan** — architecture, components, key decisions
- **Epics and stories** — full list with acceptance criteria, ready for Asana

The agent MUST NOT proceed until the user explicitly approves or redirects.

**On user approval:**
1. Read `config.yaml` → `asana.workspace_gid`
2. Look up the Asana project by the name/GID provided at session start
3. For each epic: create an Asana section within the project
4. For each story in that epic: create an Asana task under the section (title + acceptance criteria)
5. Send a Slack notification confirming story creation
6. Wait `pause_seconds`, then proceed to Phase 4 automatically

**On user redirect:** Revise the design per feedback and re-present. Do not proceed until approved.

---

## Environment Variables (REQUIRED)

Credentials are stored in WSL environment variables, not in `config.yaml`. Agents MUST read these at startup:

| Variable | Purpose |
|----------|---------|
| `SLACK_WEBHOOK_URL` | Slack incoming webhook URL |
| `SLACK_CHANNEL` | (Optional) Override channel, e.g. `#dev-notifications` |
| `ASANA_TOKEN` | Asana personal access token |
| `ASANA_WORKSPACE_GID` | Asana workspace GID |

If any required variable is unset, the agent MUST stop and notify the user before proceeding.

**To set in WSL**, add to `~/.bashrc` or `~/.zshrc`:
```bash
export SLACK_WEBHOOK_URL="https://hooks.slack.com/services/..."
export SLACK_CHANNEL="#dev-notifications"   # optional
export ASANA_TOKEN="your-token-here"
export ASANA_WORKSPACE_GID="your-workspace-gid"
```

---

## Slack Notifications

Send via the `SLACK_WEBHOOK_URL` environment variable using an HTTP POST.

**Phase transition format:**
```
*Phase {N} Complete: {Phase Name}*
Project: {project name}
Produced: {one-line summary of what was produced}
Next: Phase {N+1} ({Phase Name}) starting in 90 seconds
Asana: {project URL}
```

**Gate approval / Asana creation format:**
```
*Design Approved — Asana Stories Created*
Project: {project name}
Epics: {count} | Stories: {count}
Next: Phase 4 (Test Design) starting in 90 seconds
Asana: {project URL}
```

**Send the notification, then sleep, then continue. Never skip the notification.**

---

## Model Policy (HARD RULE — NO EXCEPTIONS)

1. At startup, read `config.yaml` → `models.phases` for the current phase model.
2. Use exactly that model. No substitutions, no upgrades, no downgrades.
3. If the config entry is missing, fall back to `models.default`.
4. Agents MUST NOT choose a different model for any reason — cost, complexity, quality concerns, or otherwise.
5. To change a model: edit `config.yaml`. That is the only mechanism.

---

## Asana Integration

**Session start:** The user provides an Asana project name or GID. The agent authenticates using `ASANA_TOKEN` and `ASANA_WORKSPACE_GID` from the environment, then reads the Asana project description as its primary requirements input for Phase 1.

**Story creation (Phase 3 gate, on approval):**
- Epics → Asana **sections** within the project
- Stories → Asana **tasks** created under the relevant epic section
- Each task includes: story title, scope classification, and acceptance criteria

**MCP Tools available:**
- `asana_get_projects` — find the project by name
- `asana_get_tasks` — read project description and existing tasks
- `asana_create_task` — create story tasks
- `asana_update_task` — update task details
- `asana_search_tasks` — search within the workspace

---

## Phase Deliverables

All deliverables written to `features/<story-folder>/`:

| Phase | Deliverable | Scope |
|-------|------------|-------|
| 1 | `seed.md` | All |
| 2 | `discovery.md` | Large only |
| 3 | `design.md` (Medium) or `specification.md` + `architecture.md` + `implementation-plan.md` (Large) | All |
| 4 | `test-design.md` + runnable test stubs (RED state) | All |
| 5 | Implementation code, all tests GREEN | All |
| 6 | `review.md` | Medium+ |

Every phase must also update: `.project`, `backlog.md`

---

## Agent Personas

| File | Phase | Role |
|------|-------|------|
| `agents/phase-1-seed.md` | 1 | Business Analyst |
| `agents/phase-2-discovery.md` | 2 | Discovery Researcher |
| `agents/phase-3-design.md` | 3 | Systems Architect |
| `agents/phase-4-test.md` | 4 | Test Designer |
| `agents/phase-5-implementation.md` | 5 | Software Engineer |
| `agents/phase-6-review.md` | 6 | Code Reviewer |
| `agents/debug-agent.md` | any | Debugger |

---

## Git

- **Branch naming:** `story-XXX-slug`
- **Commits:** After each logical unit in Phase 5. Include short description of what changed.
- **Push:** After every phase completion. Never leave commits local-only.
- **PR:** Squash-merge to `main`. Rebase before opening PR.

---

## Hard Stop Rules

- **NEVER auto-switch stories.** When a story is complete, stop and wait for the user to assign the next one.
- **NEVER proceed past the Phase 3 gate** without explicit user approval.
- **NEVER use a model not specified in `config.yaml`.**
- **NEVER write directly to the database** — use application APIs only.
- **NEVER deploy to production.** Production deployments are manual and owner-authorized only.

---

## Scope Classification Guide

| Scope | Indicators |
|-------|------------|
| **Small** | Single component, clear requirements, minimal integration |
| **Medium** | Multiple components, API or DB changes, needs design |
| **Large** | Architectural impact, multiple systems, multiple epics |

---

## Data Mutation Policy

- Never write directly to the database (no direct INSERT/UPDATE/DELETE).
- Always use application APIs/endpoints for data changes.
- If no API exists for a required mutation, implement the API first.

---

## Debug Protocol

When any failure, error, or unexpected behavior occurs in any phase, adopt the `debug-agent` persona (`agents/debug-agent.md`).

Core loop — follow in order, no skipping:
1. Capture the error (logs, stack trace, actual exception)
2. Trace the code path to the failure
3. Write a failing test that reproduces it
4. Fix only what the test proves is broken
5. Verify all tests green, no regressions
6. Commit with root cause in the message
