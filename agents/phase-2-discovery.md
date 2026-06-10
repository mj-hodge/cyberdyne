# Phase 2: Discovery — Discovery Researcher

```yaml
role: Discovery Researcher
phase: 2
scope: Large only  # Skipped for Small and Medium — path goes 1 → 3 directly
gate: none  # Slack notification + 90s pause, then auto-continue to Phase 3
model: read from config.yaml → models.phases.phase-2
```

## Model Gate (REQUIRED)

Read `config.yaml` → `models.phases.phase-2` before starting. Use that model exactly.

---

## Goal

For Large-scope stories: research the problem space, existing solutions, technical options, and risks. Produce a `discovery.md` that gives the Design phase (Phase 3) a well-informed starting point.

---

## Pre-Work

1. Read `config.yaml`
2. Read `features/<story-folder>/seed.md` — understand scope, requirements, constraints
3. Note the tech stack and integration requirements from seed.md

---

## What to Discover

| Area | Questions |
|------|-----------|
| Existing solutions | What already exists? Build vs buy vs integrate? |
| Technical options | What approaches could solve this? Trade-offs? |
| Risks | What could go wrong? What's uncertain? |
| Dependencies | What does this depend on? What depends on it? |
| Best practices | What does the current ecosystem recommend? |
| Constraints | What limits our options (cost, time, tech)? |

---

## Workflow

```
1. Read seed.md — internalize requirements, constraints, scope
2. Research: existing solutions, technical options, ecosystem patterns
3. Identify 2–3 viable approaches with trade-offs
4. Identify key risks and unknowns
5. Write discovery.md
6. Update .project and backlog.md
7. Send Slack notification
8. Sleep pause_seconds
9. Auto-continue to Phase 3
```

---

## Discovery.md Structure

```markdown
# Discovery: {Story Title}

## Summary
{2–3 sentences: what we found and the recommended direction}

## Existing Solutions
| Solution | Fit | Notes |
|----------|-----|-------|
| {name} | High\|Medium\|Low | {why or why not} |

## Viable Approaches
### Option A: {name}
- **What:** {description}
- **Pros:** {list}
- **Cons:** {list}
- **Risk:** Low\|Medium\|High

### Option B: {name}
...

## Recommended Approach
{Which option and why — be direct}

## Key Risks
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|

## Unknowns
- {What we don't know yet that Phase 3 should resolve}
```

---

## Slack Notification (send after writing discovery.md)

```
*Phase 2 Complete: Discovery*
Project: {project name}
Produced: discovery.md — {N} approaches evaluated, recommended: {approach name}
Next: Phase 3 (Design) starting in {pause_seconds}s
Asana: {project URL}
```

---

## Constraints

- Do NOT design the solution — that is Phase 3
- Do NOT over-research — focus on what directly informs the design decision
- Do NOT recommend an approach without stating trade-offs
