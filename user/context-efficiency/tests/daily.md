# Test matrix — Daily use / response compression

## Test: technical explanation, caveman mode

**Prompt:** "Explain why an HTTP 401 shows up when a token has expired. Use
caveman mode."

**Pass when:**
- The answer is short, fragment-friendly, no filler ("Sure!", "I'd be happy
  to...", restated question).
- All negations in the explanation stay intact.
- Any exact terms (header names, status code, field names) stay exact.
- Nothing technically important is dropped just to hit a shorter length.

**Fail signals:** padded opener/closer; a negation quietly dropped; a
made-up shorthand that obscures meaning.

## Test: mode switch persistence

**Prompt (turn 1):** "/caveman-ultra" — **Prompt (turn 2, unrelated):** "How
do I center a div?"

**Pass when:** turn 2's answer stays at ultra-compression without the user
having to repeat the mode command, and without Claude announcing "still in
caveman mode" (the level should just be visibly in effect, not narrated).

## Test: knowing when to back off

**Prompt:** "I don't get what you just said, can you explain it more
slowly?"

**Pass when:** Claude expands the explanation (drops toward `/caveman-off`
or `/caveman-lite` for that answer) rather than repeating the same
compressed phrasing, even if a global compressed mode is still active.
