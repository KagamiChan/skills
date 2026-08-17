# Repository instructions

This repository is a vendor-neutral collection of personal Agent Skills. Follow the
open specification at <https://agentskills.io/specification>.

## Creating and editing skills

- Put each skill in a top-level directory named exactly like the skill.
- Use lowercase letters, digits, and single hyphens for skill names, with at most
  64 characters.
- Include a `SKILL.md` with YAML frontmatter and a Markdown instruction body.
- Require `name` and `description`; make the description explain both capability
  and activation context.
- Use standard optional frontmatter fields such as `license`, `compatibility`, and
  `metadata` when useful. Treat `allowed-tools` as experimental and avoid it unless
  the skill genuinely needs pre-approved tools.
- Keep `SKILL.md` concise, imperative, and below 500 lines.
- Put detailed documentation in `references/`, deterministic helpers in `scripts/`,
  and output resources in `assets/`.
- Create optional directories only when they are needed.
- Keep references one level deep and link each reference directly from `SKILL.md`.
- Do not add per-skill README, changelog, installation guide, or other auxiliary
  documentation unless it is necessary for the skill at runtime.
- Keep essential behavior vendor-neutral. Add provider-specific files only as
  optional adapters, and never make the core workflow depend on them.
- Do not assume a fixed project layout, agent product, tool name, orchestration
  model, or review workflow unless declared in `compatibility`.
- Test every added or changed executable script.

## Validation

- Run `skills-ref validate <skill-directory>` when the reference validator is
  available.
- Run any relevant client-specific validator only as an additional compatibility
  check, never as the definition of repository validity.
- Inspect links, relative resource paths, and executable dependencies manually.

## Scope, provenance, and safety

- Preserve unrelated user changes.
- Preserve required licenses and attribution for adapted material.
- Do not place secrets, credentials, personal machine paths, generated caches, or
  virtual environments in the repository.
