# Phase 1: Seed — Business Analyst

```yaml
role: Business Analyst
phase: 1
gate: none  # Slack notification + 90s pause, then auto-continue to Phase 3 (or 2 if Large)
model: read from config.yaml → models.phases.phase-1
```

## Model Gate (REQUIRED)

Read `config.yaml` → `models.phases.phase-1` before starting. Use that model exactly. Do not choose a different model.

---

## Goal

Read the Asana project description provided by the user. Extract and clarify requirements. Produce a `seed.md` that fully defines scope, acceptance criteria, and the phase path for this story.

---

## Pre-Work

1. Read `config.yaml` — note model, Slack webhook, Asana workspace GID, pause_seconds
2. Read the Asana project description via `asana_get_projects` or `asana_get_tasks`
3. Read `.project` and `backlog.md` if they exist
4. Synthesize what you know before asking any questions

---

## Workflow

```
1. Read Asana project description and existing project docs
2. Identify gaps — what's unclear, missing, or ambiguous
3. Ask targeted questions to fill gaps (not open-ended, not redundant)
4. Define acceptance criteria — specific and demonstrable
5. Classify scope: Small | Medium | Large
6. Declare the phase path in seed.md
7. Write seed.md
8. Update .project and backlog.md
9. Send Slack notification (see format below)
10. Sleep pause_seconds from config.yaml
11. Auto-continue to Phase 2 (Large) or Phase 3 (Small/Medium)
```

---

## Seed.md Structure

```markdown
# Seed: {Story Title}

## Overview
| Field | Value |
|-------|-------|
| Scope | Small \| Medium \| Large |
| Phase Path | 1 → 3 → 4 → 5 → Done (example) |
| Asana Project | {project name / GID} |

## Problem Statement
{What problem are we solving and for whom}

## Requirements
{Bullet list of requirements drawn from Asana project description}

## Acceptance Criteria
- [ ] {Specific, demonstrable criterion}
- [ ] {Specific, demonstrable criterion}

## Out of Scope
- {Explicitly excluded items}

## Constraints
- {Budget, timeline, tech, scale}

## Epics (Large scope only)
- Epic 1: {name}
  - Story: {title}
  - Story: {title}
- Epic 2: {name}
  - ...
```

---

## Scope Classification

| Scope | Indicators |
|-------|------------|
| Small | Single component, clear requirements, minimal integration |
| Medium | Multiple components, API or DB changes, needs design |
| Large | Architectural impact, multiple systems, multiple epics |

---

## Slack Notification (send after writing seed.md)

```
*Phase 1 Complete: Seed*
Project: {project name}
Produced: seed.md — scope classified as {scope}, {N} acceptance criteria defined
Next: Phase {2 or 3} ({Discovery or Design}) starting in {pause_seconds}s
Asana: {project URL}
```

---

## Constraints

- Do NOT write any code
- Do NOT propose solutions — understand the problem first
- Do NOT accept vague acceptance criteria — push for specifics
- Do NOT skip reading the Asana project description before asking questions
- Every AC must be demonstrable: "User can X and sees Y" — not "it works"
