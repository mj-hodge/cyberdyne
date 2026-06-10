# Phase 4: Test Design — Test Designer

```yaml
role: Test Designer
phase: 4
gate: none  # Slack notification + 90s pause, then auto-continue to Phase 5
model: read from config.yaml → models.phases.phase-4
```

## Model Gate (REQUIRED)

Read `config.yaml` → `models.phases.phase-4` before starting. Use that model exactly.

---

## Goal

Write tests before any implementation code. Every acceptance criterion from the design phase becomes a test. Tests must be runnable and RED (failing) when Phase 4 ends — implementation hasn't happened yet.

---

## Pre-Work

1. Read `config.yaml`
2. Read `features/<story-folder>/seed.md` — acceptance criteria
3. Read the design artifacts (`design.md` or `specification.md` + `architecture.md`)
4. Read existing test patterns in `tests/` or `e2e/` if they exist

---

## Workflow

```
1. Read all acceptance criteria from seed.md and design artifacts
2. Map each AC to one or more tests
3. Write test stubs / test cases — runnable but failing (RED)
4. Write test-design.md
5. Run tests — confirm they fail for the right reason (not import errors)
6. Update .project and backlog.md
7. Send Slack notification
8. Sleep pause_seconds
9. Auto-continue to Phase 5
```

---

## Test Types

| Type | Location | When to Use |
|------|----------|-------------|
| Unit tests | `tests/unit/` | Pure logic, no I/O |
| Integration tests | `tests/integration/` | API endpoints, DB interactions |
| E2E tests | `e2e/` | User-facing flows (Playwright) |

**Default:** Integration tests for backend endpoints, E2E for user-facing features, unit tests for non-trivial pure logic only.

---

## Test Design Principles

- One test per acceptance criterion (minimum)
- Tests must be specific — test observable behavior, not implementation details
- Use real dependencies where possible (no mocks of your own code)
- Every test must have a clear failure message
- Test the error paths, not just the happy path

---

## test-design.md Structure

```markdown
# Test Design: {Story Title}

## Coverage Map
| Acceptance Criterion | Test File | Test Name | Type |
|---------------------|-----------|-----------|------|
| {AC from seed.md} | {file} | {test name} | unit\|integration\|e2e |

## Test Fixtures
{Data setup required — DB state, mock external services, test users}

## RED State Confirmation
Run: `{test command}`
Expected: {N} tests failing, 0 passing
```

---

## Slack Notification (send after tests are RED and confirmed)

```
*Phase 4 Complete: Test Design*
Project: {project name}
Produced: {N} tests written (RED state) covering {N} acceptance criteria
Next: Phase 5 (Implementation) starting in {pause_seconds}s
Asana: {project URL}
```

---

## Constraints

- Do NOT write implementation code — tests only
- Do NOT write tests that pass before implementation exists (except trivial scaffolding)
- Do NOT skip error path tests — they must be covered
- Do NOT mock your own code — use real implementations with test databases/fixtures
- Tests must run without manual setup beyond documented fixtures
