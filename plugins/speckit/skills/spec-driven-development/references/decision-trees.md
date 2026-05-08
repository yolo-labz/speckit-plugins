# SDD Decision Trees

Detailed decision trees for workflow choices in spec-driven development.

## When to Run /speckit:clarify

```
After /speckit:specify, check spec.md for [NEEDS CLARIFICATION] markers.

Has [NEEDS CLARIFICATION] markers?
├── Yes (1-3 markers)
│   ├── Are they about scope boundaries?
│   │   └── Yes → Run clarify (scope ambiguity is high-risk)
│   ├── Are they about security/compliance?
│   │   └── Yes → Run clarify (cannot assume security requirements)
│   ├── Are they about UX preferences?
│   │   └── Maybe → Can you make a reasonable default?
│   │       ├── Yes → Document assumption, skip clarify
│   │       └── No → Run clarify
│   └── Are they about technical details?
│       └── Usually skip → These belong in the plan phase, not the spec
├── No markers
│   └── Skip clarify entirely
└── More than 3 markers
    └── Spec is too vague — re-specify with more detail instead of clarifying
```

**Rule of thumb**: Run clarify when ambiguity affects SCOPE or SECURITY. Skip when
ambiguity is about technical implementation (that's the plan's job) or when reasonable
defaults exist.

---

## When to Run /speckit:analyze

```
After /speckit:tasks, evaluate the feature complexity.

How complex is this feature?
├── Simple (1-2 user stories, <10 tasks, single domain)
│   └── Skip analyze — low ROI, unlikely to find issues
├── Medium (3-5 user stories, 10-30 tasks, some cross-cutting)
│   ├── Is this a new domain for the team?
│   │   ├── Yes → Run analyze (unfamiliar territory = higher error rate)
│   │   └── No → Optional — skim tasks manually instead
│   └── Are there cross-story dependencies?
│       ├── Yes → Run analyze (dependency errors are expensive)
│       └── No → Probably skip
├── Complex (>5 stories, >30 tasks, multiple domains)
│   └── Always run analyze — the cost of finding issues now is much less
│       than finding them during implementation
└── Critical (security, compliance, data migration, breaking changes)
    └── Always run analyze AND run checklist
```

---

## When to Run /speckit:checklist

```
What domain concerns does this feature have?

Domain concerns?
├── Security (auth, encryption, access control)
│   └── Run checklist with security focus
├── Compliance (GDPR, HIPAA, SOC2)
│   └── Run checklist with compliance focus
├── Data integrity (migrations, state changes)
│   └── Run checklist with data focus
├── UX (user-facing flows, accessibility)
│   └── Optional — only if UX requirements are extensive
├── Performance (SLAs, load requirements)
│   └── Run checklist if measurable targets exist
└── None of the above
    └── Skip checklist
```

---

## Sequential vs. Parallel User Story Implementation

```
How should user stories be implemented?

Are stories independent (no shared entities or services)?
├── Yes → Implement in parallel (different agents or developers)
│   └── Merge order doesn't matter
├── Partially (share some entities but different services)
│   ├── Create shared entities in Foundational phase
│   └── Then implement stories in parallel
└── No (stories build on each other)
    └── Implement sequentially in priority order (P1 → P2 �� P3)
        └── Each story is still independently testable
```

---

## When Implementation Hits a Blocker

```
Implementation task fails or cannot proceed.

What kind of blocker?
├── Technical constraint not anticipated
│   ├── Does it invalidate the plan?
│   │   ├── Yes → STOP. Amend plan.md, regenerate tasks
│   │   └── No → Note constraint, adapt current task, continue
│   └── Does it invalidate the spec?
│       ├── Yes → STOP. Re-specify, then re-plan and re-task
│       └── No → Continue with adapted approach
├── Scope creep (new requirement discovered)
│   ├── Is it blocking current tasks?
│   │   ├── Yes → Add to spec.md as new user story, re-task
│   │   └── No → Log it for a future feature, continue
│   └── Does it fit in current feature scope?
│       ├── Yes → Amend spec, add tasks
│       └── No → Create separate feature spec
├── External dependency unavailable
│   ├── Can you mock/stub it?
│   │   ├── Yes → Mock it, add integration task for later
│   │   └── No → Skip dependent tasks, implement what you can
│   └── Is it a hard blocker for MVP?
│       ├── Yes → STOP. Resolve dependency first
│       └── No → Continue with available stories
└── Quality gate failure (analyze or checklist found issues)
    ├── CRITICAL issues → STOP. Fix before continuing
    ├── HIGH issues → Fix if <30min effort, else note and continue
    └── MEDIUM/LOW → Note for polish phase
```

---

## Feature Completion Checklist

Before declaring a feature complete:

```
Feature done?
├── All tasks in tasks.md marked [x]?
│   └── No → Complete remaining tasks or document why skipped
├── All acceptance scenarios from spec.md verified?
│   └── No → Test and fix failing scenarios
├── Success criteria from spec.md measurable?
│   └── No → Measure what you can, document gaps
├── Constitution alignment verified?
│   └── No → Run /speckit:analyze or manually check
├── Code committed and pushed?
│   └── No → Commit with conventional commit format
├── PR created (if using feature branches)?
│   └── No → Create PR with spec summary
└── Feature branch cleaned up?
    └── No → Merge PR, delete branch
```
