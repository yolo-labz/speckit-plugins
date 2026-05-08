# SDD Anti-Patterns

Common mistakes in spec-driven development and how to fix them.

## 1. Spec Prescribes HOW Instead of WHAT

**Symptom**: Spec mentions frameworks, APIs, databases, or implementation details.

**Bad example**:
> FR-003: System MUST use PostgreSQL with JSONB columns for storing user preferences
> FR-004: System MUST implement REST API with Express.js

**Good example**:
> FR-003: System MUST persist user preferences across sessions
> FR-004: System MUST expose user management operations to external consumers

**Why it matters**: Implementation details in specs create premature constraints. The plan phase
is where technology choices belong. Specs should survive a complete tech stack change.

**Fix**: Remove all technology mentions. Rewrite requirements as user-facing capabilities.
Use "System MUST [verb] [what]" without naming tools.

---

## 2. Over-Specified Tasks

**Symptom**: Tasks contain exact code to write, leaving zero room for implementation judgment.

**Bad example**:
> T012: Create `src/models/user.py` with fields: id (UUID), email (str), created_at (datetime),
> using SQLAlchemy declarative base, with `__tablename__ = 'users'`

**Good example**:
> T012: Create User model in `src/models/user.py` with fields from data-model.md entity definition

**Why it matters**: Over-specified tasks become brittle. If the tech stack changes or a better
approach is discovered during implementation, every task needs rewriting.

**Fix**: Tasks should reference the plan and data model rather than inlining implementation
details. The task says WHAT to create and WHERE; the plan says HOW.

---

## 3. Skipping Checklists for Complex Features

**Symptom**: Requirements gaps discovered during implementation, causing rework.

**When checklists matter**:
- Feature has >10 tasks
- Feature spans multiple user stories
- Feature has security or compliance implications
- Feature involves data migration or breaking changes

**When to skip**: Simple features with 1-2 user stories and <10 tasks. The overhead of
generating and validating checklists exceeds the risk of missed requirements.

**Fix**: Run `/speckit:checklist` before `/speckit:implement` for complex features.

---

## 4. Spec Drift During Implementation

**Symptom**: Implemented feature doesn't match the spec. Nobody updated the spec when
requirements changed mid-implementation.

**Warning signs**:
- "We decided to change this during implementation" with no spec update
- Implementation adds features not in any user story
- Success criteria can't be validated because the feature diverged

**Fix**: When requirements change during implementation:
1. STOP implementation
2. Update spec.md with the new requirements
3. Re-run `/speckit:plan` if architecture is affected
4. Re-run `/speckit:tasks` if task ordering changes
5. Resume implementation with the updated task list

---

## 5. Constitution Violations Treated as Suggestions

**Symptom**: Analysis report flags CRITICAL constitution violations but they're ignored
or rationalized away.

**Bad response**: "We'll address the constitution violation in a follow-up PR."

**Good response**: Either fix the violation in the current feature OR formally amend
the constitution with `/speckit:constitution` before proceeding.

**Why it matters**: The constitution exists to prevent known-bad patterns. Ignoring it
defeats the purpose of having principles. If a principle is wrong, update it explicitly
rather than silently violating it.

---

## 6. Monolithic User Stories

**Symptom**: One user story covers the entire feature. Cannot be independently tested
or delivered incrementally.

**Bad example**:
> US1: As a user, I want a complete dashboard with authentication, data visualization,
> real-time updates, export capabilities, and admin controls.

**Good example**:
> US1 (P1): View dashboard with core metrics
> US2 (P2): Export dashboard data as CSV
> US3 (P3): Real-time metric updates
> US4 (P3): Admin controls for dashboard configuration

**Fix**: Each user story should be:
- Independently implementable (can be built without other stories)
- Independently testable (can be validated in isolation)
- Independently deployable (delivers value on its own)

---

## 7. Missing Acceptance Scenarios

**Symptom**: User stories have vague descriptions but no Given/When/Then scenarios.

**Bad example**:
> US1: Users can manage their account settings.

**Good example**:
> US1: Users can update their display name and email.
> Given a logged-in user on the settings page,
> When they change their display name and click save,
> Then the new name appears immediately and persists across sessions.

**Fix**: Every user story must have at least one acceptance scenario in Given/When/Then
format. These become the basis for testing.

---

## 8. Plan Without Research

**Symptom**: Plan makes technology choices without investigating alternatives or validating
assumptions.

**Fix**: For non-trivial features, Phase 0 of the plan should produce `research.md` with:
- Decision: what was chosen
- Rationale: why chosen
- Alternatives considered: what else was evaluated
- Risks: known limitations of the choice
