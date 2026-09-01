# Selective Cross-Session Memory — Reference

## Critical runtime rule (read this first)

A skill file is instructions, not storage. It cannot create real
persistence on its own. Three cases:

1. **This environment already has a real persistence mechanism** (a memory
   tool, an MCP server, a project file the user maintains). Use it, and
   follow *its own* rules for what to store, how to format it, and any
   privacy limits — those rules take precedence over the generic guidance
   below. This module tells you *when it's worth checking or writing
   something*, not what the storage format or privacy rules should be if
   the environment already defines them.
2. **Only a session-local/file-based mechanism is available** (e.g. a
   scratch file in a working directory that may or may not survive to the
   next session). Use it, but don't imply stronger guarantees than it
   actually has.
3. **Nothing persistent is available.** Say so if it's relevant to what the
   user asked, and operate on session-local continuity only. Never claim
   something was "remembered" if it wasn't actually saved anywhere durable.

## Why be selective at all

Dumping the entire history into every prompt defeats the purpose — it costs
more tokens than it saves by preventing re-discovery. The value comes from
retrieving *only* what's relevant to the current task.

## Conceptual tiers (for reasoning about what to keep, not a mandated schema)

- **Working memory** — current task's state: what's being done, current
  status, next step, active constraints, open questions. Short-lived.
- **Episodic memory** — specific past events/sessions worth recalling later
  ("bug X was fixed by Y, evidence: file Z").
- **Semantic memory** — stable facts/decisions/conventions that don't
  change often ("project uses library X for reason Y").
- **Procedural memory** — reusable ways of doing recurring work ("run the
  auth tests before the integration suite").

## Retrieval policy

Don't search memory for every message — only when there's an actual
relevance signal:

- The task references a named project/entity that might have prior context.
- The user explicitly references the past ("like we did before", "same as
  last time").
- The task is a recurring type of work where a past decision would matter.

When retrieving: form a focused query from the current task, pull the
smallest useful set (not everything tagged relevant), and prefer the most
recent/most specific match when several could apply. If retrieved
information conflicts with something directly in front of you right now
(an open file, a fresh search result), the current source wins — treat
memory as a hint to verify, not as ground truth to repeat unchecked.

## What's worth writing

Only information that will plausibly change *future* work:

- durable decisions and their reasoning;
- constraints that will keep applying;
- a bug's root cause once actually found (not the symptom);
- project-specific conventions;
- unfinished-work handoff state (what's left, what's blocked on what).

## What's not worth writing

- Routine conversational content.
- Anything obvious from the current context anyway.
- Anything likely to be stale or irrelevant by the next time it'd be read.
- Anything the environment's own memory rules already exclude (e.g. secrets,
  credentials, or categories a governed memory system marks off-limits) —
  when in doubt, don't write it; a missed write costs a re-discovery later,
  a wrongly-written sensitive fact is a real problem now.

## Actions this module covers conceptually

remember (save durable info) · recall (retrieve relevant prior info) ·
recap (compact current work into a short handoff state) · forget (remove
something, only on clear request/authorization) — implement each using
whatever real tool this environment provides; don't invent tool-specific
mechanics here that the environment doesn't actually have.
