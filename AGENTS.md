# Repository instructions

This repository is a collection of personal Codex skills.

## Creating and editing skills

- Use the built-in `skill-creator` skill for every new skill and substantial update.
- Put each skill in a top-level directory named exactly like the skill.
- Use lowercase letters, digits, and hyphens for skill names.
- Keep `SKILL.md` concise and use imperative instructions.
- Limit YAML frontmatter in `SKILL.md` to `name` and `description`.
- Put detailed documentation in `references/`, deterministic helpers in `scripts/`,
  and output resources in `assets/`.
- Create optional directories only when they are needed.
- Keep references one level deep and link each reference directly from `SKILL.md`.
- Do not add per-skill README, changelog, installation guide, or other auxiliary docs.
- Generate and keep `agents/openai.yaml` aligned with `SKILL.md`.
- Run the bundled `quick_validate.py` against every changed skill before finishing.
- Test every added or changed executable script.

## Scope and safety

- Preserve unrelated user changes.
- Do not place secrets, credentials, generated caches, or virtual environments in
  the repository.
