# Coding Minimalism — Reference

## Scope

Only for coding, debugging, and implementation tasks. Never apply to
non-technical work (translation, summaries, recipes, general Q&A) — this
module has nothing to say there.

"Minimal" means *deliberate*, not careless. The goal is the smallest change
that is actually correct, not the smallest change period.

## Before writing anything

1. Understand the exact requirement — not the first plausible reading of it.
2. Read the real code path the change touches (not just the file the user
   mentioned — its callers/callees if the change could affect them).
3. Note what already exists: helpers, types, utilities, patterns,
   dependencies already in use.
4. For bugs: find the root cause. A fix that only hides the symptom will
   resurface elsewhere.

## The ladder

Stop at the first step that resolves the need — don't skip ahead out of
habit, and don't keep going past a step that already works.

1. **Does this need to exist at all?** Sometimes the real fix is removing a
   requirement, not satisfying it.
2. **Does the codebase already solve this?** Search before writing.
3. **Does the standard library solve this?** Prefer it over a hand-rolled
   version.
4. **Does the platform/runtime provide this natively?** (browser API,
   language builtin, OS feature.)
5. **Does an already-installed dependency solve this?** Don't add a new one
   if something already in the project covers it.
6. **Can this be materially simpler than the obvious approach?** One clear
   expression can beat a small class.
7. **Only now:** write the minimum new code required to correctly satisfy
   the requirement.

## Anti-patterns to avoid

- Speculative abstractions ("we might need this to be pluggable later").
- An interface with exactly one implementation, with no real boundary
  driving it.
- A factory for a single product.
- Configuration for a value that cannot reasonably vary.
- Scaffolding built "for later" that nothing currently uses.
- A new dependency where existing capability already covers the need.
- Refactoring unrelated code just because it happened to be nearby.

## Deletion and reuse over addition

When behavior can be preserved, prefer deleting dead/duplicate code over
adding more. If a bug has a shared root cause across multiple call sites,
fix the shared cause rather than patching each call site separately.

## Levels

- `/ponytail-off` — normal implementation, no extra scrutiny.
- `/ponytail-lite` — prefer simplicity, but don't fight the user for a
  reasonable normal implementation.
- `/ponytail` (default here) — run the full ladder before writing code.
- `/ponytail-ultra` — actively question whether a requested feature/
  addition is really needed before building it. This can mean pushing back
  with a question — it must never mean silently skipping something the
  user actually asked for, and it never overrides correctness.

## Repo-scale strategy

For tasks touching a codebase rather than a single file:

1. Get the repo structure before diving into any single file.
2. Identify the smallest relevant file set for the task.
3. Search for symbols/callers before broad reading.
4. Read files on the real execution path first; expand only when evidence
   requires it.
5. Use a script for broad questions ("how many places call X") instead of
   opening files one by one — see `context-routing.md`.
6. Match existing project patterns/conventions rather than introducing a
   new one for a single change.
7. Make the smallest correct change, then run focused verification (the
   relevant tests/lints, not a full unrelated suite).
8. Avoid opening generated/vendor/build artifacts unless the task is
   specifically about them.
