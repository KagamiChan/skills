# Dispatch with hc and Herdr Desktop

Use `hc --skill` for the installed Core CLI and `herdr-app --skill` for Desktop layout. Plain hc works without a Desktop window. Use explicit terminal addresses for reads, sends, waits and cleanup.

## Place the worker

In Desktop, resolve a user-named or previously confirmed space with `herdr-app ls`. The inventory maps spaces and tabs to pane IDs and Core terminal addresses. If the space is known, dispatch there even when the lead terminal is unknown. Ask which space to use only if the destination is ambiguous. A shared server can supply stale caller IDs. Accept `HC_TERMINAL_ID` as the lead address when its provenance ties it to this client. Avoid guessing from focus, working directory, or screen contents.

When launching a Codex worker whose commands must execute inside its Core terminal, pass `--no-daemon` to that worker. The lead can continue its existing session.

Keep the lead tab readable. Start a background worker in its own tab in the confirmed space. Core opts a terminal into the Desktop layout with a label:

```text
hc run --bg --name <worker> --cwd <checkout> --parent <lead-address> --label herdr.space=<space-id> <interactive-client> [client-args]
```

Use `--no-parent` for an unknown or unverified lead address and track the worker by its returned address. Children normally end when their parent ends. Use `--outlive-parent` only when the assignment must survive that parent. For standalone hc omit Desktop labels.

For a remote space, select its machine with `--on <machine>`, use space/tab label values local to that machine and pass a checkout on that host. Keep machine-qualified terminal addresses for subsequent hc commands. Run remote acceptance checks with `hc exec --on <machine>`.

In Desktop, verify the worker has its own pane under the intended space and tab in `herdr-app ls`. An entry in the parent's `sub_agents` alone does not establish visibility. If an empty worker landed on the wrong machine or in the wrong space, reclaim that owned terminal and relaunch with the corrected target before sending work.

For a named visible tab with explicit focus preservation, use `herdr-app do tab.new --space <space-id> --name <task-label> --folder <checkout> --argv <JSON-array> --focus false`, then drive the returned terminal with hc. Choose the labeled hc launch when explicit Core parentage matters. To pair workers, add `--label herdr.tab=<worker-tab-id>` to the hc launch only if both outputs will stay legible. Split beside the lead only when requested. Target its Desktop pane ID with `pane.split` and inspect the installed arguments via `herdr-app commands`. For a separate checkout or a background placement that must preserve focus, use a tab.

## Dispatch, collect and reclaim

After a plain interactive launch, wait for startup with `hc wait <address>`, then confirm readiness and the agreed provider/model, then use `hc send --wait <address> "<assignment>"`. For parallel work use `hc send` and later `hc wait <address>`. Inspect live or blocked output with `hc read --lines 0 <address>`. Read settled output with `hc read --lines <count> <address>`. On a timeout or uncertain delivery, inspect the worker before resending. Match the result to the task and revision.

Keep a worker until its result is collected and any follow-up is assigned. Before reclaiming it, check its occupant, active jobs and any user takeover. Use `hc get <address>` to check the live program, and inspect parent links in the Core inventory before ending a parent. In Desktop use `herdr-app status` to check focus and defer cleanup while the user is viewing it. End only an owned, finished assignment's live worker with `hc kill <address>`, then use `hc rm <address>` for its finished record if still present. Closing a window can leave the process running. Check `hc ls --all` and the Desktop layout after cleanup. Verify any detached writer has stopped before reassigning its checkout. Preserve the lead and other live or user-owned terminals.

On resume, resolve the recorded machine and terminal address with `hc get` and check the current occupant. A kept terminal can return to a shell at the same address. For Desktop, reconcile its space, tab and pane with `herdr-app ls` before sending or reclaiming work.
