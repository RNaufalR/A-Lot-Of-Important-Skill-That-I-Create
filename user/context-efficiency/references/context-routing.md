# Context Routing — Reference

## Core principle

Don't use the model as a bulk data processor when a script, filter, or
query can do the processing more cheaply. This is about *data flow*,
completely separate from prose style (Module 1) — a verbose answer built
from a small, well-filtered input is fine; a terse answer built from a
context stuffed with raw logs is not the goal.

## Ask before consuming large output

- Do I need all of this, or just a slice?
- Can I filter before reading (grep/search by the specific signature I
  actually care about)?
- Can I aggregate remotely instead of reading every row/line myself?
- Can a script return only the decisive number/list/diff?
- If this data will be touched repeatedly, should it be indexed/searched
  instead of re-read in full each time?

## Pattern: raw data → external processing → small result → reasoning

Examples of the shape, not an exhaustive list:

- Counting occurrences across many files → run a script that counts and
  returns totals + locations, don't open every file.
- A large log → filter by the specific error signature first, read only
  matching lines.
- A big API response → extract only the fields the task needs.
- Full repo history → query only commits touching the relevant path/bug.
- The same large document referenced repeatedly in one session → read it
  once, keep only what's needed, don't re-fetch the whole thing each time
  it's mentioned.

## Routing table

| Situation | Strategy |
|---|---|
| Small output | Read directly. |
| Medium output | Filter/select the relevant part first if that's cheap. |
| Large output | Process outside the main context first (script/grep/jq), read only the result. |
| Data reused across the task | Index or cache once, retrieve selectively after. |
| Many similar files | Batch-process rather than opening one at a time. |
| Statistical/counting question | Write the computation, don't manually tally. |
| Task needs an exact value | Fetch the exact value. A lossy summary is the wrong tool here even if it's shorter. |
| Data not relevant to the current task | Leave it out of context entirely. |

## Tool priority

Prefer, in order: an existing purpose-built tool for the data source (a
connector, an indexed search) → shell/script processing (grep, jq, a small
script) → reading raw and filtering manually as a last resort.

**Never fabricate a tool that doesn't exist.** If no real mechanism is
available to pre-filter something, say so and do the best manual filtering
available rather than claiming a capability that isn't there.

## Note on session/workspace scope

In a sandboxed working directory, files typically don't persist between
separate tasks/sessions. This module governs *this task's* context budget
only. Anything that needs to survive across sessions is Module 4's concern
(selective memory), not this one — don't conflate "kept out of context for
now" with "saved for later".
