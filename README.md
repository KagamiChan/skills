# Personal Agent Skills

This repository contains portable personal [Agent Skills](https://agentskills.io/).
Each top-level skill follows the open Agent Skills specification so it can be used
by any compatible agent client rather than targeting one vendor or project.

## Layout

```text
skills/
├── <skill-name>/
│   ├── SKILL.md       # Required: metadata and instructions
│   ├── scripts/       # Optional executable helpers
│   ├── references/    # Optional on-demand documentation
│   ├── assets/        # Optional templates and other resources
│   └── ...            # Optional client-specific integrations
├── AGENTS.md
└── README.md
```

Keep each skill self-contained in a top-level directory whose name matches the
skill name. Do not nest the collection under another `skills/` directory.

## Portability policy

- Put all essential discovery metadata and instructions in `SKILL.md`.
- Do not assume a particular agent product, repository layout, package manager,
  issue tracker, or delegation mechanism unless the skill explicitly requires it.
- Declare genuine environment requirements in the standard `compatibility` field.
- Treat provider-specific metadata and integrations as optional adapters. A skill's
  core workflow must remain usable without them.
- Use relative paths for bundled resources and document any executable dependency.
- Preserve upstream licenses and attribution when adapting third-party material.

## Add or update a skill

Start with a concrete task and examples of requests that should activate the skill.
Use a short lowercase, hyphenated name, write a precise `description`, and add only
the scripts, references, or assets that improve repeated use.

Validate each changed skill against the open specification when `skills-ref` is
available:

```sh
skills-ref validate ./<skill-name>
```

See the [Agent Skills specification](https://agentskills.io/specification) for the
current schema and portability guidance.
