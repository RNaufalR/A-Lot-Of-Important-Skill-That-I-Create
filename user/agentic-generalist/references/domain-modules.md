# Domain Modules

Read only the section that matches the current task. These expand on
Section 5 of SKILL.md — they assume the routing/planning/verification
discipline there is already in effect.

## Coding

**Understand first:** language/runtime, repo or file structure, current
behavior vs. expected behavior, explicit constraints.

**Build/modify:** preserve working behavior, prefer the minimal reliable
change over a rewrite, match existing conventions, avoid speculative
architecture (interfaces for one implementation, config for values that
can't vary, scaffolding "for later").

**Validate:** syntax/type correctness where it applies, logic and edge
cases, integration assumptions, regression risk.

**Debug loop:** Observe → Reproduce → Localize → Fix → Reproduce/Validate.
Don't declare a bug fixed because the diff looks plausible — confirm the
original repro no longer triggers it.

**Example (good vs. bad decomposition):**
- Good: `Read failing test → reproduce locally → bisect to the change that broke it → fix root cause → rerun full suite`
- Bad: `Think about the bug → consider possible causes → think about how to think about the fix`

## Research and Analysis

**Define:** the exact question, its scope, how time-sensitive it is, what
evidence quality the answer needs.

**Gather, in order of preference:** primary sources → official docs/data →
high-quality institutional sources → reputable secondary analysis.

**Analyze — keep these separate:** observed facts, calculations,
interpretation, assumptions, uncertainty. Don't let interpretation quietly
become a "fact."

**Synthesize:** answer the actual question; don't hand back a source dump.

**Verify:** cross-check consequential claims, flag conflicts you couldn't
resolve. Never fabricate citations, sources, statistics, quotes,
experiments, or browsing activity that didn't happen.

## Writing

**Before drafting, pin down:** audience, purpose, required format, tone,
length/structure limits, source requirements.

**Priority order while drafting:** correctness of meaning → structure →
clarity → specificity → natural style → polish. Don't inflate prose to
sound more thorough than the content is.

**Transformation tasks** (editing existing text): preserve everything not
asked to change — don't silently "improve" untouched sections.

**Revision pass:** logical flow, redundancy, unsupported claims,
consistent terminology, compliance with the requested format.

**Example (output-format spec pattern):**
```
## Report structure
Use this exact template:
# [Title]
## Executive summary
## Key findings
## Recommendations
```

## Planning and Strategy

**Model:** objective, current state, constraints, resources, dependencies.

**Prioritize:** classify actions as essential / high-value-optional /
low-value-optional; do essential first.

**Risk:** name only the material risks; for each, one practical mitigation
beats a large contingency tree nobody will read.

**Success criteria:** make "done" observable, not just declared.

## Problem Solving

**Frame:** translate the problem into explicit variables, conditions, or
subproblems.

**Select method:** the simplest one that's actually valid for this
problem — not the most sophisticated one available.

**Solve:** carry the calculation/logic through without skipping a
dependency.

**Validate:** check the result against units, bounds, constraints, known
behavior, internal consistency.

**Explain:** show the reasoning that actually decided the answer, not a
narrated stream of every thought along the way.
