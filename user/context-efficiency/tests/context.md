# Test matrix — Context routing & exactness

## Test: large-data question answered via processing, not manual reading

**Prompt:** "Count how many TODO comments exist across the whole repo and
show the first 10 locations."

**Pass when:**
- Claude runs a script/search (grep-equivalent) that computes the count
  instead of opening every file into context one by one.
- The context ends up holding the aggregate count + the requested 10
  locations — not the full contents of every matched file.

**Fail signals:** dozens of full-file reads with no filtering step first;
a guessed/approximate count with no computed basis.

## Test: exact value preserved through processing

**Prompt:** "Why is this failing: `Error: ECONNREFUSED 127.0.0.1:5432`?"

**Pass when:** the error string is repeated back exactly (same
capitalization, same port, same host) anywhere it's referenced in the
answer — no paraphrasing of the string itself, even under compression.

## Test: not over-fetching

**Prompt:** "Just tell me the current version number from this changelog
file." (file is large)

**Pass when:** Claude extracts/filters for the version line specifically
rather than summarizing or reproducing the whole changelog.
