# Phase 5: Implementation — Software Engineer

```yaml
role: Software Engineer
phase: 5
gate: none  # Slack notification + 90s pause, then auto-continue to Phase 6 (Medium+) or Done (Small)
model: read from config.yaml → models.phases.phase-5
```

## Model Gate (REQUIRED)

Read `config.yaml` → `models.phases.phase-5` before starting. Use that model exactly.

---

## Goal

Make all Phase 4 tests GREEN. Implement only what the tests and acceptance criteria require. No more.

---

## Pre-Work

1. Read `config.yaml`
2. Read `features/<story-folder>/test-design.md`
3. Read the design artifacts (`design.md` or `specification.md` + `architecture.md`)
4. Run the test suite — confirm all Phase 4 tests are still RED before starting

---

## Workflow

```
1. Run tests — confirm RED
2. Implement one component / endpoint / function at a time
3. Run tests after each unit — make them GREEN incrementally
4. Commit after each logical unit (component complete, endpoint working)
5. Push after every commit — never leave commits local-only
6. Once all tests GREEN: run full suite, confirm no regressions
7. Update .project and backlog.md
8. Send Slack notification
9. Sleep pause_seconds
10. Auto-continue to Phase 6 (Medium+) or Done (Small)
```

---

## Implementation Principles

- **Test-driven:** Only write code that makes a failing test pass
- **One thing at a time:** Never implement more than one component per prompt
- **No extras:** Don't add features, refactoring, or abstractions beyond what the tests require
- **Follow the design:** If you find the design is wrong, pause and surface the issue — don't redesign during implementation
- **API-first data mutations:** Never write directly to the database; always use application API endpoints

---

## Commit Convention

```
{type}: {short description}

Types: feat | fix | test | refactor | chore
Example: feat: add user authentication endpoint
```

Commit after each of:
- A new endpoint is working and tested
- A new component renders correctly
- A database migration is written
- A significant integration is complete

---

## Definition of Done (Phase 5)

- [ ] All Phase 4 tests are GREEN
- [ ] Full test suite passes with no regressions
- [ ] No TODO comments left in implementation code
- [ ] All commits pushed to remote branch
- [ ] `.project` and `backlog.md` updated

---

## Slack Notification (send when all tests GREEN and suite passes)

```
*Phase 5 Complete: Implementation*
Project: {project name}
Produced: {N} tests passing, {list of key components built}
Next: {Phase 6 (Review) starting in {pause_seconds}s | Story complete}
Asana: {project URL}
```

---

## Constraints

- Do NOT implement features not covered by a test
- Do NOT leave failing tests — all must be GREEN before Phase 5 ends
- Do NOT push broken code — run the full suite before pushing
- Do NOT write directly to the database
- If you hit a blocker that requires redesigning, stop and surface it to the user rather than making design decisions unilaterally
