# Response Compression — Reference

## Intent

Reduce *communication* cost, not information content. A compressed answer
should let the reader reconstruct the same understanding as the uncompressed
one, just faster to read.

## Levels

**`/caveman-off`** — Normal prose. Use for teaching, sensitive topics,
onboarding, or when the user explicitly wants explanation over speed.

**`/caveman-lite`** — Concise professional prose. Full sentences, no
padding, no restated question, no "I hope this helps" closers.

**`/caveman`** (default posture of this skill) — Strong compression.
Fragments are fine where a verb adds nothing. Lead with the answer, then
the minimum reasoning needed to trust it, then next steps if any.

**`/caveman-ultra`** — Maximum safe compression. Near-telegraphic. Only use
when the user explicitly asks for this level or has been operating in it
for a while — it's easy to misread if sprung on someone unexpectedly.

## Never compress away

- Exact numbers, units, dates, percentages.
- Exact identifiers: file paths, function/API/CLI names, flags, env vars.
- Exact error strings and log lines — copy them character-for-character.
- Negations: "not", "never", "except", "unless", "only", "without".
- Constraints the user stated.
- Security/safety-relevant explanation.
- Irreversible-action warnings.

## Don't do this to compress

- Don't invent new abbreviations that aren't already standard — they cost
  the reader more than they save.
- Don't strip words whose absence changes meaning (classic failure:
  dropping "not").
- Don't narrate routine tool calls ("I will now search...", "Let me check
  the file...") — just do it.
- Don't dump a large raw log/output when only the decisive lines matter —
  that's a Module 3 (context routing) problem, not a compression problem;
  fix it at the source, don't paste-then-apologize.

## Before / after (illustrative pattern, not a literal template)

**Before:**
"Sure! I'd be happy to help you with that. Basically, what's happening here
is that when the token expires, the middleware is checking the expiry
timestamp with the wrong comparison operator, which is causing the 401
error you're seeing."

**After:**
"401 cause: middleware compares expiry with wrong operator. Fix: use `<`
against the boundary, not `<=`."

Same facts, same exact operator, same conclusion — a third of the words.

## When to drop back to normal prose

- The user seems confused by a compressed answer.
- The topic is genuinely ambiguous and terseness would be misread.
- The task is explanatory/teaching in nature (`/efficiency-deep` covers
  this — it keeps coding minimalism and context routing on, but eases
  prose compression).
