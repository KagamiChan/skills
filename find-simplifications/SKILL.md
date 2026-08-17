---
name: find-simplifications
description: Find and evaluate evidence-backed opportunities to simplify a codebase by deleting, merging, demoting, or replacing unnecessary behavior and structure. Use for simplification audits, complexity reduction, dead or duplicated surface analysis, speculative or over-built feature review, redundant lifecycle or defensive machinery, package-boundary cleanup, and hand-rolled code that may be replaced by a standard library or maintained dependency. Works across languages and repository layouts without requiring project-specific design notes, delegation, or a particular review workflow.
---

# Finding Simplifications

Turn broad requests to simplify code into a small set of well-proven proposals. Prefer net deletion and clearer ownership over moving complexity behind a new abstraction.

## Work Independently of Repository Conventions

- Infer the repository layout, languages, build tools, and production boundaries from the files that exist.
- Read available contributor guidance, architecture docs, manifests, configuration, and tests when useful, but do not require any particular file or documentation system.
- Respect documented compatibility promises and intentional seams. Treat them as evidence to weigh, not assumptions to ignore.
- Perform the survey directly with the available tools. Do not require delegation, parallel workers, an issue tracker, or a pull-request workflow.
- Report findings without modifying code unless the user asks for implementation or annotations.

## Establish the Search Boundary

1. Identify production code, entry points, public interfaces, runtime configuration, generated code, tests, examples, scripts, and documentation.
2. Clarify the requested scope from the user's wording. If the request is broad, sample every major production area before concentrating on the strongest candidates.
3. Start with large, frequently changed, duplicated, or lifecycle-heavy production surfaces. Do not stop after finding the first unused symbol.
4. Exclude vendored, generated, or migration code unless the simplification explicitly includes its source of truth or compatibility obligations.

## Look for Strong Candidates

Favor candidates where the current design costs more than it buys:

- A public method, event, option, hook, registry, helper, module, or package has no production consumer.
- Tests or docs are the only consumers of behavior that is not an intentional contract.
- Multiple representations, caches, flags, or callbacks mirror the same fact or lifecycle transition.
- Every implementation must support an interface member that no caller uses.
- A package or abstraction only wraps one implementation without isolating volatility or hiding meaningful complexity.
- Generic extension points, background flows, recovery paths, or configuration knobs have no demonstrated use case.
- Defensive copies, validators, rollback paths, or hostile-input tests protect trusted same-process values without a real ownership boundary.
- Custom parsing, matching, retries, framing, diffing, or collection code duplicates a suitable standard-library facility or maintained dependency.
- A feature was added and later removed, but its scaffolding, compatibility code, tests, or documentation remain.

Treat typo fixes, style cleanup, vague complexity complaints, and isolated tiny deletions as weak candidates unless they unlock a larger collapse.

## Prove or Reject Each Candidate

Use `rg` first when available. Search exact symbol names, call forms, configuration keys, event or wire strings, imports, registrations, and aliases. Then read the definitions and every relevant call site.

Classify consumers as:

- **Production:** runtime entry points, shipped libraries, application code, loaders, configuration paths, and operational scripts.
- **Non-production:** tests, fixtures, snapshots, docs, comments, benchmarks, and generated expected output.
- **Ambiguous:** examples, development tools, plugins, reflection, serialization, dynamic imports, or public APIs used outside the repository. Investigate before classifying.

For each candidate:

1. State the current responsibility and its owner.
2. Name the exact consumers and distinguish production from non-production evidence.
3. Describe what can be removed, merged, demoted, or replaced.
4. Estimate net deletion, including implementation, glue, tests, documentation, configuration, and generated artifacts.
5. Identify behavior, compatibility, extensibility, or operational safety that would be lost.
6. Define the checks that would demonstrate the simpler design still works.

Reject or downgrade the candidate when a real production caller exists, a public compatibility promise is active, the change merely relocates complexity, unrelated churn outweighs the reduction, or the evidence depends only on a static-analysis warning that has not been inspected.

## Audit Trust and Lifecycle Machinery

For every copy, freeze, validator, state flag, readiness promise, cancellation path, callback capture, and disposer, identify where the value originates and who owns it next.

Treat parsers, user configuration, durable storage, queues, model output, subprocesses, workers, and network boundaries as untrusted or independently owned. Treat typed same-process calls as borrowed values unless mutation or adversarial implementations are part of the contract.

For asynchronous code, map each mechanism to a distinct state transition or owner. Propose consolidation when several mechanisms encode the same liveness, completion, or ownership fact. Preserve separate machinery when it independently protects publication and rollback, terminal-outcome arbitration, resource ownership, callback containment, or shutdown-to-quiescence.

## Evaluate Dependency Replacements

Consider a standard library or maintained dependency when it deletes a substantial custom implementation and its dedicated tests.

- Match the dependency against the exact required behavior, including edge cases and residual glue.
- Prefer a standard-library facility when the project's supported runtime provides it.
- Check compatibility, maintenance, security posture, adoption, license, transitive footprint, and version constraints when that information is available.
- Count the wrapper and adaptation code that remains. A dependency that moves the same complexity is not a simplification.
- Call out deliberate behavior changes explicitly; small differences may be acceptable when the resulting contract is reasonable and easier to explain.

## Rank the Results

Rank candidates using four separate judgments:

- **Confidence:** strength of call-site, ownership, and compatibility evidence.
- **Payoff:** code, concepts, states, dependencies, or maintenance burden removed.
- **Risk:** behavior, API, migration, security, or operational consequences.
- **Effort:** implementation and validation cost, including downstream cleanup.

Lead with high-confidence candidates that have meaningful payoff and bounded risk. Do not pad the result to reach an arbitrary count.

## Report the Audit

Start with the strongest findings. For each finding, provide:

1. An action-oriented title and confidence level.
2. Evidence with concrete file paths, symbols, and representative call sites.
3. The proposed deletion, merge, demotion, or replacement.
4. The expected net reduction and concepts removed.
5. Tradeoffs, compatibility concerns, and the strongest reason to keep the current design.
6. A focused validation plan.

End with the areas surveyed, important exclusions, and rejected near-misses when they explain why an apparently obvious cleanup is unsafe. If no strong candidate survives, say so and summarize the evidence instead of presenting speculation as a finding.

## Implement Only When Requested

When the user asks to apply a simplification:

1. Reconfirm the candidate against the current tree.
2. Make the smallest coherent change that removes the obsolete concept completely.
3. Update affected tests, docs, configuration, exports, and generated sources.
4. Run the repository's relevant formatters, static checks, tests, builds, and diff checks in proportion to risk.
5. Report any behavior change and any validation that could not be completed.
