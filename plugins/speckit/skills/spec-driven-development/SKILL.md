---
name: spec-driven-development
description: >
  This skill should be used when the user mentions "spec-kit", "speckit", "SDD",
  "spec-driven development", or "specification-first", asks to "write a spec",
  "plan a feature", "break down tasks", "implement from spec", or "analyze spec
  consistency", or has a `.specify/` directory in their project. Provides workflow
  orchestration, decision trees for optional steps, and anti-patterns for the
  spec-driven development methodology. Do NOT use when the user just wants to
  write code without a spec.
version: 0.2.0
---

# Spec-Driven Development (SDD)

Specifications are the primary source of truth that directly generate working implementations.
Code serves specifications, not the other way around.

## Workflow Overview

The spec-kit plugin provides 9 commands forming a complete development pipeline:

| # | Command | Input | Output | When to Use |
|---|---------|-------|--------|-------------|
| 1 | `/speckit:constitution` | Interactive principles | `constitution.md` | Once per project — establishes non-negotiable rules |
| 2 | `/speckit:specify` | Feature description | `spec.md` + branch | Starting a new feature — captures WHAT and WHY |
| 3 | `/speckit:clarify` | Spec with ambiguities | Updated `spec.md` | When spec has [NEEDS CLARIFICATION] markers |
| 4 | `/speckit:plan` | Spec + tech context | `plan.md` + artifacts | Designing HOW to implement the spec |
| 5 | `/speckit:tasks` | Plan + spec | `tasks.md` | Breaking plan into executable, ordered work |
| 6 | `/speckit:analyze` | Spec + plan + tasks | Analysis report | Quality check before implementation |
| 7 | `/speckit:checklist` | Domain context | `checklists/*.md` | Validating requirement quality (not code) |
| 8 | `/speckit:implement` | tasks.md | Working code | Executing the task plan phase-by-phase |
| 9 | `/speckit:taskstoissues` | tasks.md | GitHub issues | Converting tasks to trackable issues |

## Decision Trees

### When to Skip Optional Steps

```
Starting a feature?
├── Is constitution defined? 
│   ├── No → Run /speckit:constitution first
│   └── Yes → Continue
├── Run /speckit:specify (always required)
├── Does spec have [NEEDS CLARIFICATION] markers?
│   ├── Yes → Run /speckit:clarify
│   └── No → Skip clarify
├── Run /speckit:plan (always required)
├── Run /speckit:tasks (always required)
├── Is this a complex feature (>10 tasks, cross-cutting concerns)?
│   ├── Yes → Run /speckit:analyze
│   └── No → Skip analyze (low ROI for simple features)
├── Are there domain-specific quality concerns?
│   ├── Yes → Run /speckit:checklist
│   └── No → Skip checklist
└── Run /speckit:implement
```

### Greenfield vs. Existing Project

```
Project type?
├── Greenfield (no existing code)
│   ├── Start with /speckit:constitution
│   ├── Then /speckit:specify for the first feature
│   └── Full pipeline: specify → clarify → plan → tasks → analyze → implement
├── Adding to existing project
│   ├── Check if .specify/ exists
│   │   ├── No → Run speckit init, then /speckit:constitution
│   │   └── Yes → Skip constitution (already defined)
│   ├── /speckit:specify for the new feature
│   └── Likely skip clarify if domain is well-understood
└── Refactoring existing feature
    ├── Write spec for the desired END STATE, not the current state
    ├── Plan should reference existing code as constraints
    └── Tasks should include migration/deprecation steps
```

### When to Re-Specify vs. Amend

```
During implementation, requirements changed?
├── Scope change (new user stories, dropped stories)
│   └── Re-specify: update spec.md, re-run plan → tasks
├── Clarification (existing requirement was ambiguous)
│   └── Amend: update spec.md inline, continue implementation
├── Technical constraint discovered
│   └── Amend plan.md, regenerate tasks if ordering changes
└── Constitution violation found
    └── STOP: either fix the violation or formally amend the constitution
```

## Workflow Orchestration

### Data Flow Between Commands

```
constitution.md ─────────────────────────────────────────┐
                                                          │ (gates)
specify ──→ spec.md ──→ plan ──→ plan.md ──→ tasks ──→ tasks.md ──→ implement
                │                    │                       │
                ├── clarify ←────────┤                       ├── analyze
                │   (optional)       │                       │   (optional)
                │                    ├── research.md         │
                │                    ├── data-model.md       │
                │                    └── contracts/          │
                └── checklist ←──────────────────────────────┘
                    (optional)
```

### Key Rules

- **Spec focuses on WHAT, not HOW**: No technology, framework, or API mentions in specs
- **Plan focuses on HOW**: Architecture decisions, tech stack, file structure
- **Tasks are immediately executable**: Each task has ID, file path, and clear action
- **Constitution is non-negotiable**: Violations are always CRITICAL severity
- **Each user story is independently testable**: Can be implemented and demoed alone
- **Parallel tasks [P] touch different files**: Never mark tasks [P] if they edit the same file

## Common Anti-Patterns (Summary)

For detailed anti-patterns with examples, consult `references/anti-patterns.md`.

| Anti-Pattern | Symptom | Fix |
|-------------|---------|-----|
| Spec prescribes HOW | Mentions frameworks, APIs, databases | Remove all tech; focus on user outcomes |
| Over-specified tasks | Tasks leave zero implementation flexibility | Tasks define WHAT to create, not exact code |
| Skipping checklists | Requirements gaps found during implementation | Run checklist for complex features |
| Spec drift | Implementation diverges from spec silently | Re-specify when scope changes |
| Constitution-as-suggestion | Violations justified away without amending | Either fix violation or formally update constitution |
| Monolithic user stories | One story covers the entire feature | Split into independently testable slices |

## Detecting SDD Projects

A project uses spec-kit if it has:
- `.specify/` directory at the project root
- `.specify/memory/constitution.md` (project principles)
- `.specify/templates/` with spec/plan/task/checklist templates
- Feature specs in `.specify/specs/[NNN-feature-name]/`

## Additional Resources

### Reference Files

For detailed guidance beyond this overview:
- **`references/anti-patterns.md`** — Common SDD mistakes with examples and fixes
- **`references/decision-trees.md`** — Detailed decision trees for workflow choices

### Quick Start

For new features, the minimal required pipeline is:
1. `/speckit:specify <feature description>` — creates spec + branch
2. `/speckit:plan` — creates implementation plan
3. `/speckit:tasks` — creates task breakdown
4. `/speckit:implement` — executes tasks
