---
name: orchestrate-with-herdr
description: Coordinate delegated technical work from the current lead session in Herdr. Use when the user assigns orchestration and acceptance to the lead and implementation, investigation, or independent review to other agents.
compatibility: Requires a Herdr-managed lead session, access to its Herdr CLI, and the worker clients and models selected for the assignment. Native GitHub stack delivery also requires same-repository branches and the gh stack extension for the CLI workflow.
---

# Orchestrate with Herdr

Act as the lead from the current session. Own the diagnosis, division of work, integration, and acceptance. Delegate substantive implementation when the user assigns that role to a worker. The lead can inspect sources, reproduce a failure, and exercise the result directly. Keep the client and model chosen for each assignment through retries and handoffs.

## Agree on tools for this task

Before starting a worker or sending it material, establish who will implement and how the result will be reviewed. Use the user's choices for **this assignment**. If the implementation client or model is open, ask in one short question, for example, “这次实现用哪个客户端和模型？审阅走已有的 PR review，还是另开审阅会话？” Ask only about roles the task needs and choices still open. A previous task's client, an installed default, or a reviewer already running in a pane does not settle who should implement this task.

If the user specifies OpenCode with a particular Flash tier and says the PR service handles review, proceed on that basis. If they leave the choice to the lead, choose and state the route. If they say “same division of work,” recover the agreement they refer to. While awaiting an unresolved choice, inspect the project and its acceptance criteria. Establish whether delivery ends at a local result, an open PR, or an authorized merge. Preserve the agreed roles, endpoint, and fallback in a private handoff outside the public repository. Follow project instructions for issues, tests, commit identity, and publication language.

## Decide what to dispatch

Identify the user-visible outcome, what the user authorized for this phase, and what observation would establish success. For a defect, give the implementer the failing entry point, relevant inputs, and expected behavior. For an open design question, give the investigator the decision to inform and the original evidence. Leave implementation choices open where the constraints allow.

Split work at boundaries that can be accepted independently. Parallelize independent investigations or changes with separate ownership. Sequence work that depends on an unsettled interface. If two workers need to edit overlapping files, serialize them or use separate worktrees and assign integration to the lead.

Tell each implementer where its responsibility ends. When review belongs to the PR service, say so in the brief and leave review dispatch to the lead. Give each worker the artifact or baseline, working directory, scope, relevant source locations, and acceptance case. Include the required checks and what to return. For example: “In this checkout, reproduce the interrupted install. A retry should complete and the existing plugin should still load. Return the changed revision, reproduction result, and checks run.” For an independent reviewer, give the goal, constraints, sources, and exact revision. Provide read access to the baseline, changed implementation, callers and relevant issue or PR history. Keep write credentials with the publisher. Use the agreed cross-model pairing. When the choice is delegated, prefer a different model for independent review. Let the reviewer form a judgment before seeing the implementer's diagnosis or another reviewer's conclusions.

Start a fresh implementation session for each independent assignment, including work that follows a design review. Give the implementer the accepted design and source locations. Keep the design reviewer independent. If the agreed reviewer is the online PR service, dispatch the implementation and follow the review on the PR.

## Run the assignment in Herdr

Before allocating a terminal, read the [workspace, tab, and pane guide](references/herdr-layout.md). The lead owns layout and cleanup through delivery.

1. If `HERDR_ENV` is not `1`, stop Herdr operations. From a managed pane, check that the service is reachable. Resolve the lead's workspace, tab and pane using caller context, then verify the live task/session identity. Multiple agents can share a checkout. If caller context is stale, use read-only inspection to establish an unambiguous binding. If it remains ambiguous, ask before controlling a pane. Diagnose socket permissions separately from a missing service. While blocked, continue independent diagnosis and preserve the agreed delegation route.
2. Choose a pane or worker tab in the verified lead workspace using the layout guide. Preserve the user's focus, set the worker's checkout, and use the **returned** pane ID to start the chosen interactive client. Verify the agent name and pane binding. Refresh the ID after moving a pane across workspaces.
3. Before sending materials, check the displayed client, provider and model against the assignment. For a requested DeepSeek via OpenCode Go, start ordinary interactive OpenCode and verify `opencode-go` and the requested tier/version. For AGY, use the installed model list and select the agreed model. Inspect a failed lookup for network or authentication errors. A startup failure calls for diagnosing the interactive client; switching to batch mode changes the agreement.
4. Start from a ready worker. For a single assignment use `prompt --wait`; for parallel dispatch confirm that each worker started its assignment, then track it while advancing independent work. A wait observes lifecycle state. Read the output and match it to the assignment and revision before treating it as complete. On timeout or a stalled prompt, inspect the live pane before resending. Use `visible` while working and `recent-unwrapped` for settled output. If scrollback is incomplete, retrieve the final response or ask the worker to save it privately.
5. On a trust, login, or permission prompt, inspect the requested action and target. Proceed within existing authorization. Route a new trust or permission decision to its owner. If a required reviewer is blocked, identify what actually ran and hold the dependent release. Keep substantive fixes with the implementer.

Typical commands (replace placeholders with verified values):

```text
herdr pane layout --pane <lead-pane-id>
herdr tab create --workspace <lead-workspace-id> --label <task-label> --cwd <checkout> --no-focus
herdr agent start <worker> --kind <selected-client> --pane <returned-pane-id>
herdr agent read <worker> --source visible
herdr agent prompt <worker> "<assignment>" --wait --timeout <milliseconds>
herdr agent read <worker> --source recent-unwrapped --lines <count>
```

## Accept the work and carry it forward

Inspect the changed artifact at the reported revision. Re-run the check that distinguishes the original failure from success through the normal entry point. Attribute worker-reported results separately from checks you ran. Return a failed acceptance case to the implementer with the observed input and output. Evaluate findings against the requested behavior. Keep separate enhancements out of the current fix unless the user expands its scope. If review is required, resolve consequential findings and have the corrected revision reviewed under the agreed conditions. For a review-only assignment, deliver the findings and coverage.

### Deliver dependent changes with GitHub native stacked PRs

For GitHub PR delivery, keep each change small enough to review on its own. If B needs A and C can ship independently, stack A → B and open C separately against the trunk. Assign the work accordingly. Reassess dependencies if repeated fixes to the bottom keep invalidating reviews above it.

Use GitHub's native stack for dependent PRs. The bottom PR targets the trunk and each higher PR targets the branch immediately below. Check same-repository access and the installed `gh stack --help`. If the workflow is unavailable, surface the limitation before choosing a substitute. [GitHub stacked PRs](https://docs.github.com/en/pull-requests/get-started/about-stacked-prs)

- **Prepare.** Match each branch head to its accepted revision and inspect the diff against its intended base. Adopt the branches in dependency order with `gh stack init --base <trunk> <lower> <upper>`.
- **Publish when authorized.** `gh stack submit` pushes branches and creates or updates PRs. With `--auto`, new PRs default to draft. Use `--open` when the affected PRs should be ready for review. Check titles, descriptions, draft status and the server-side stack map. For existing PRs, `gh stack link` accepts PR URLs in bottom-to-top order; passing branch names can push branches and create PRs.
- **Update.** Coordinate with writers before a cascading rebase. Use `gh stack rebase` for local realignment or `gh stack sync` for authorized remote updates. Keep parent and trunk merge commits out of child branches during synchronization. Resolve conflicts in the cascading rebase. After a lower PR lands, inspect GitHub's retargeting and sync the remaining branches. Recheck CI and review verdicts for changed heads.
- **Merge within the agreed range.** Verify every included layer meets its checks. A green build with pending review still awaits review. Merging through a middle or top PR includes every layer below it. Check the installed command's target semantics before acting: a noninteractive `gh stack merge` without a narrower target can merge the whole stack. Verify which PRs landed and continue the remaining authorized work.

### Resume ongoing work

Before reassigning a checkout, verify the previous worker has stopped at the execution service. A closed pane can leave a writer running. For a correction to the same assignment, reuse its session when useful. Give unrelated work and independent opinions fresh context.

After compaction or handoff, recover the endpoint, role assignments, pane or session identifiers, checkout and revision, checks still pending, and next authorized action. Check live state before dispatching again. Resume supervision, route fixes, or deliver the verified result according to the user's stop condition. Reconcile owned panes as each assignment ends and verify the final layout before signing off.
