# Personal Codex Skills

This repository contains personal Codex skills. Keep each skill self-contained in a
top-level directory whose name matches the skill name.

## Layout

```text
skills/
├── <skill-name>/
│   ├── SKILL.md
│   ├── agents/
│   │   └── openai.yaml
│   ├── scripts/       # Optional executable helpers
│   ├── references/    # Optional on-demand documentation
│   └── assets/        # Optional output templates and media
├── AGENTS.md
└── README.md
```

Only add optional directories when a skill needs them. Do not nest all skills under
another `skills/` directory.

## Add a skill

Ask Codex to create or update a skill with the built-in `skill-creator` skill. Give it:

- a short, lowercase, hyphenated name;
- concrete examples of requests that should trigger the skill;
- any reusable scripts, references, or assets the skill needs.

Before committing a skill, validate it with the `quick_validate.py` script bundled
with `skill-creator`.
