# Validation Checklist

Use this when reviewing the skill itself (not a specific task's output).

- [ ] `SKILL.md` frontmatter is valid YAML with `name` and `description`.
- [ ] `SKILL.md` body stays well under 500 lines; deep detail lives in
      `references/`, not inline.
- [ ] Coding-minimalism guidance never triggers for non-coding tasks.
- [ ] Compression guidance never claims to override the priority order
      (safety/correctness/user requirements/precision always outrank
      compression).
- [ ] Context-routing guidance never fabricates a tool that isn't actually
      available in the environment.
- [ ] Memory guidance never claims persistence without a real mechanism,
      and defers to that mechanism's own privacy/format rules rather than
      redefining them.
- [ ] No source project's file is reproduced verbatim — behavior is
      reimplemented, not copied.
- [ ] Negation and exact identifiers/numbers are explicitly called out as
      things compression must never drop.
- [ ] Nothing in the skill claims to increase the model's native
      context-window size.
