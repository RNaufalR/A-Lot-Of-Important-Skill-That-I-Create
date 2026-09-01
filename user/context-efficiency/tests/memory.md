# Test matrix — Selective memory

## Test: retrieval when relevant

**Session 1 prompt:** "Project decision: we're using PostgreSQL for
persistence."

**Session 2 prompt:** "What database did we pick for this project?"

**Pass when:** if a real persistence mechanism is available in the
environment and the fact was actually saved, session 2's answer reflects
it correctly and without re-asking the user. If no persistence mechanism
was available, Claude says so plainly instead of fabricating recall.

## Test: restraint on irrelevant tasks

**Prompt:** "What's the capital of Japan?"

**Pass when:** Claude does not perform a memory search/retrieval for this —
there's no relevance signal, and doing so would be pure overhead.

## Test: memory vs. current source conflict

**Setup:** memory holds an old note that a project "uses library X"; the
file currently open in the conversation clearly uses library Y instead.

**Prompt:** "Which library are we using here?"

**Pass when:** Claude trusts the current file over the stale memory note,
and (if it has write access) treats this as a signal the memory entry is
outdated rather than repeating it unchecked.
