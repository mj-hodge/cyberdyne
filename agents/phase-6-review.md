# Phase 6: Review — Code Reviewer

```yaml
role: Code Reviewer
phase: 6
scope: Medium+ only
gate: none  # Slack notification + 90s pause, then story is Done
model: read from config.yaml → models.phases.phase-6
```

## Model Gate (REQUIRED)

Read `config.yaml` → `models.phases.phase-6` before starting. Use that model exactly.

---

## Goal

Review the implementation for correctness, security, simplicity, and adherence to the design. Produce a `review.md` with findings. Block the story on any Critical findings.

---

## Pre-Work

1. Read `config.yaml`
2. Read `features/<story-folder>/seed.md` — original requirements and AC
3. Read the design artifacts — what was intended
4. Read the implementation code — what was built
5. Read `features/<story-folder>/test-design.md` — what was tested

---

## Review Dimensions

Two lenses — run both:

### Architect Lens — Correctness & Design
- Does the implementation match the design?
- Are domain boundaries respected?
- Are there any obvious security issues (injection, auth bypass, exposed secrets)?
- Is error handling correct — does the system fail safely?
- Are there missing edge cases in the tests?
- Is the data mutation policy followed (no direct DB writes)?

### Skeptic Lens — Simplicity & Maintainability
- Is anything more complex than it needs to be?
- Are there abstractions with only one use case?
- Is there dead code or unused imports?
- Are variable and function names clear?
- Would a new developer understand this without asking questions?

---

## Finding Severity

| Severity | Definition | Blocks Story? |
|----------|------------|--------------|
| **Critical** | Security vulnerability, broken AC, data loss risk | YES |
| **High** | Logic error, missing error handling, significant complexity | Recommended fix |
| **Medium** | Code smell, unclear naming, minor test gap | Note only |
| **Low** | Style nit, minor improvement opportunity | Optional |

---

## Workflow

```
1. Read all pre-work files
2. Review with Architect lens — document findings
3. Review with Skeptic lens — document findings
4. Write review.md
5. If Critical findings exist: STOP, surface to user, wait for fixes before marking done
6. If no Critical findings: update .project and backlog.md, send Slack, story is Done
```

---

## review.md Structure

```markdown
# Review: {Story Title}

## Summary
{Overall assessment — pass | pass with notes | blocked}

## Findings

### Critical
- [ ] {Finding}: {file}:{line} — {description and recommended fix}

### High
- [ ] {Finding}: {description}

### Medium
- {Finding}: {description}

### Low
- {Finding}: {description}

## AC Verification
| Acceptance Criterion | Status |
|---------------------|--------|
| {AC from seed.md} | PASS \| FAIL \| PARTIAL |

## Verdict
{APPROVED | BLOCKED — list Critical items that must be resolved}
```

---

## Slack Notification (send when review is complete and no Critical findings)

```
*Phase 6 Complete: Review*
Project: {project name}
Produced: review.md — {N} findings ({N} critical, {N} high, {N} medium, {N} low)
Story: DONE
Asana: {project URL}
```

If blocked:
```
*Phase 6: Review — BLOCKED*
Project: {project name}
Critical findings: {N} — implementation must be revised before story can close
Asana: {project URL}
```

---

## Constraints

- Do NOT rubber-stamp — actually read the code
- Do NOT nitpick style when substance is fine — focus on Critical and High
- Do NOT fix issues yourself — document them in review.md and surface to user
- Do NOT mark story Done if any Critical finding is unresolved
