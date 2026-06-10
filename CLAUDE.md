# CLAUDE.md

> **DIRECTIVE:** This file contains critical guidance that MUST be followed for all work.
> After context compaction, re-read this file and `config.yaml` to restore phase context.

---

## SDLC Process

See [AGENTS.md](./AGENTS.md) for: phase paths, gate behavior, Slack notifications, model policy, Asana integration, and git conventions.

**"spec" trigger:** Any prompt starting with "spec" MUST initiate Phase 1.

**Story switching (CRITICAL):** When a story is complete:
1. Output the completion summary
2. **STOP. Wait for the user to assign the next story.**
3. Never auto-claim or auto-start the next story.

---

## Phase Paths

| Scope | Path |
|-------|------|
| Small | `1 → 3 → 4 → 5 → Done` |
| Medium | `1 → 3 → 4 → 5 → 6 → Done` |
| Large | `1 → 2 → 3 → 4 → 5 → 6 → Done` |

| Phase | Name | Gate? |
|-------|------|-------|
| 1 | Seed | No — Slack pause, then auto-continue |
| 2 | Discovery (Large only) | No — Slack pause, then auto-continue |
| 3 | Design | **YES — hard gate. Wait for user approval.** |
| 4 | Test Design | No — Slack pause, then auto-continue |
| 5 | Implementation | No — Slack pause, then auto-continue |
| 6 | Review + Gate | No — Slack pause, then auto-continue |

**The only gate is after Phase 3.** All other phase transitions send a Slack notification, wait 90 seconds, and continue automatically.

---

## Phase Deliverables

All files written to `features/<story-folder>/`:

| Phase | Output File(s) | Scope |
|-------|---------------|-------|
| 1 | `seed.md` | All |
| 2 | `discovery.md` | Large only |
| 3 | `design.md` (Medium) or `specification.md` + `architecture.md` + `implementation-plan.md` (Large) | All |
| 4 | `test-design.md` + test stubs in `tests/` (RED) | All |
| 5 | Implementation code, tests GREEN | All |
| 6 | `review.md` | Medium+ |

Every phase must also update: `.project`, `backlog.md`

---

## Model Policy (CRITICAL)

All agents use `claude-sonnet-4-6` by default. Per-phase overrides are configured in `config.yaml` → `models.phases`.

**Agents MUST read `config.yaml` at startup and use the model specified for their phase. No exceptions.**

| Phase | Default Model |
|-------|--------------|
| 1–6 | `claude-sonnet-4-6` (overridable in `config.yaml`) |

---

## Asana Integration

**Starting a story:** The user provides an Asana project name or GID. The agent reads the project description as requirements input.

**After Phase 3 gate approval:** Agent creates epics as Asana sections and stories as tasks grouped under each section.

**MCP Tools:** `asana_search_tasks`, `asana_create_task`, `asana_update_task`, `asana_get_projects`, `asana_get_tasks`

---

## Slack Notifications

After each phase completes (except the Phase 3 gate), the agent:
1. POSTs a notification to the webhook in `config.yaml` → `slack.webhook_url`
2. Waits `config.yaml` → `phases.pause_seconds` (default: 90)
3. Continues to the next phase automatically

---

## Reference

- [AGENTS.md](./AGENTS.md) — Canonical SDLC policy
- [config.yaml](./config.yaml) — Model, Slack, Asana, and behavior configuration
- [agents/](./agents/) — Phase persona files
