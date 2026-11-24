# Technical conventions

These requirements apply to any agent doing the work. The named tools and models describe how to carry out the assignment. Use the specified external route from any host. Substituting a built-in agent or another service requires Kagami's authorization.

Apply the latest explicit choice for this task, then the project's agreed setup. Check the active configuration before dispatch. Record a one-off choice with the task. Change this reference when Kagami changes a standing convention.

## Select the implementer and reviewer

Prefer a different model from the implementer for independent code review when choosing the reviewer is delegated to you. Use the pairing or online review service selected for the current task. Treat a model version specified for one assignment as scoped to that assignment.

When Kagami requests DeepSeek, use OpenCode's OpenCode Go provider (`opencode-go`). Start the ordinary interactive client with its configured default for an interactive assignment. Check the displayed provider and model before sending materials. A request for Flash specifies the tier. If Kagami names a version, match that version too. When the default already satisfies the request, proceed directly. If the required service or version is unavailable, identify the blocker and preserve the requested arrangement until a fallback is authorized.

## Run and observe the execution session

Always start OpenCode and AGY assignments in interactive mode. Launch OpenCode with `opencode`; never use `opencode run`. Launch AGY with `agy` and always omit the `--mode` option.

When working through Herdr, keep the implementer visible in the same Herdr workspace so Kagami can inspect and steer the session. Check the installed CLI's help when invocation syntax is uncertain.

Before replacing an implementer or handing off its shared worktree, confirm the execution session has finished or stopped on the server. Closing the visible client can leave the agent writing to the same files.

## Commit on the user's behalf

Use the user's GitHub commit identity. Check the effective Git author name and email against the identity the user uses for this repository. If either is uncertain, confirm it with the user before committing.
