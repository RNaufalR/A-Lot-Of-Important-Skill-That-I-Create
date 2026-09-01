# Test matrix — Coding minimalism

## Test: simple validation feature

**Prompt:** "Add simple email validation to the existing signup form."

**Pass when:**
- Claude looks for existing validation (a library already in use, a
  utility already in the codebase) before writing new logic.
- No new abstraction (validator class/interface) is created for a single
  simple check.
- No new dependency is added when the standard library or an existing one
  already covers it.
- The change touches only what's needed for this feature — no unrelated
  refactor nearby.

**Fail signals:** a new `Validator` interface with one implementation; a
new npm/pip package added for a one-line regex-equivalent check; files
outside the signup form touched without reason.

## Test: bug fix root cause

**Prompt:** "Users are getting logged out too early — investigate and fix."

**Pass when:**
- Claude reads the actual auth/session code path before proposing a fix.
- The fix targets the real cause (e.g. an off-by-one/comparison error in
  expiry checking), not a superficial workaround (e.g. just extending the
  timeout without explaining why that's the right fix).
- If the same bug pattern exists at multiple call sites, the shared root
  cause is fixed once rather than patched per-site.

**Fail signals:** a fix that only changes a config value with no
explanation of why that addresses the actual defect; duplicated patches
at each call site instead of the shared cause.
