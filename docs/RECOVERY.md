# Recovery procedure

[Home](../README.md) · [Open tasks](https://github.com/gv1983us-commits/codex-provider-incident/issues)

**Target:** reliable Jarvis work through Hermes using the existing Pro Full `openai-codex` subscription connection.

**Status:** The new [session review](SESSION-2026-09-10.md) confirms an Astra 900k / Medium short text answer and five-tool turn with a final answer. The earlier GPT-5.5 overload remains confirmed. Sustained work, auxiliary/worker route coverage and exception recovery remain unverified. This procedure combines observed checks with proposed remaining validation.

## 0. Preserve the confirmed failure

The new Copy details receipt explicitly names `openai-codex`, `gpt-5.5` and `overloaded`. It resolves the screenshot's model-attribution uncertainty. The receipt does not give an HTTP status, endpoint, request ID, call role or attempt count; obtain a focused log excerpt only if one of those fields is needed for a concrete diagnostic.

`retryable: true` is an error classification, not a promise of provider recovery. Treat GPT-5.5 as another route with an observed failure until scoped recovery evidence is supplied.

The screenshot also exposes a client build label, transcribed in the evidence file. It is a UI label, not a verified full installed runtime commit.

## 1. Pin the environment once

Record the installed Hermes version/commit, installation method and relevant customizations. For a Git checkout, these read-only commands identify the revision and changed filenames:

```sh
git rev-parse HEAD
git status --short
```

Run them in the actual Hermes checkout. Review filenames before posting. If the installation has no Git metadata, record that fact and its package/release version instead of guessing.

Record effective, non-secret settings only:
- Active session's provider/model and configured default for new sessions.
- Auxiliary model overrides, including compression.
- Explicit model/provider choices in delegated or external workers.
- Main, auxiliary and delegation fallback targets.
- Reasoning/context settings used in the actual test.

Do not replace the whole configuration or silently reset custom settings during diagnosis.

## 2. Build on the demonstrated short tool loop

The new export already demonstrates **Astra 900k / Medium** completing five tool calls and a final answer. That limited acceptance check has passed. The next useful step is to observe further bounded, useful work in that session, retaining any failures and the effective model/configuration. Do not infer a durable fix from this one pass or from Medium alone.

The GPT-5.5 command below is retained as the earlier candidate route; it also has a confirmed overload receipt. It is not presented as a proven repair.


Current upstream documentation supports an in-chat model switch:

```text
/model gpt-5.5 --provider openai-codex
```

Use the existing connection to the affected Full account. This selects a session model; it does not authenticate a different account.

Ask the agent to list a harmless test directory with an available read-only tool, then state one filename from the result. A pass requires:

1. The selected model emits a tool call.
2. The tool completes and its result is retained.
3. A subsequent model request uses that result and returns normally.

Record actual timestamps, provider/model IDs from the request log, error text if any, and the installed revision. A plain text response alone is a narrower success. Stop after the normal bounded retry budget if the route fails; do not generate a retry storm.

If the current installation does not support this command, report the installed revision and its supported model picker. Do not assume current upstream documentation describes a customized historical build exactly.

### Record where the delay occurs

For one short, comparable request, distinguish time before the first visible event, time spent producing the response, and time inside a tool. Record the selected interface setting without assuming “Ultra” and “Max” map to identical request parameters. The original slow-progress report contained no timings; the later export now supplies message-level intervals, recorded below.

The new export supplies several measured message intervals: Astra's first text response **6.36 s**, first tool request **3.69 s**, whole five-tool turn **51.33 s**; the initial doctor operation itself took **32.50 s**. These intervals do not isolate server queueing or model compute.

## 3. Cover the routes Jarvis actually uses

After a successful short cycle, inspect each active role:

| Role | What must be verified |
| --- | --- |
| Main agent | Live session uses `openai-codex` / `gpt-5.5` on the intended account |
| New-session default | Future sessions do not revert to an affected model |
| Auxiliary jobs | Effective model and fallback choices are known |
| Compression / summarization | Summary request succeeds on the intended route or preserves recoverable state on failure |
| Explicit workers | Each pinned model is checked; parent changes do not prove worker changes |
| Fallback | A failed target is not repeatedly reintroduced at every new turn |

Current upstream docs describe `--global` as persisting the model and switching the live session:

```text
/model gpt-5.5 --provider openai-codex --global
```

Apply persistence only after the short cycle passes and the intended defaults are confirmed. Dashboard changes alone apply to new sessions; existing sessions keep their model until explicitly switched.

Auxiliary `auto` normally starts from the main model in current upstream documentation, but explicit overrides and fallback chains can choose other routes. Check the effective result in the installed version. This document intentionally provides no blanket configuration replacement.

## 4. Verify interruption recovery in a disposable test

A provider rejection can happen after tools have run. The missing next model step must not trigger an uncontrolled replay of completed actions.

Proposed acceptance criteria for a diagnostic or patch:

- Preserve the pending step, completed tool results and call identifiers before retry exhaustion returns control.
- End exhausted retries in an explicit resumable state; distinguish it from task completion.
- On resume, continue from the retained result. If an action's completion is unknown, reconcile it before deciding whether to repeat it.
- Bound retries and record cooldown/eligibility. A timer becoming eligible does not prove provider recovery.
- For compression failure, retain recoverable history and report whether a new summary, an older anchor or a deterministic fallback was used. Match telemetry to the actual path.
- Check process liveness independently from stale task/process records.

Use a mocked overload and a disposable, harmless tool fixture for failure injection. Do not deliberately overload the real provider to test recovery. This is a proposed contract to review with Hermes contributors; no implementation is claimed here.

## 5. Declare recovery only with scoped evidence

A recovery report should include:
- Successful Hermes main → tool → main cycle.
- Confirmed auxiliary and explicit-worker routes used by the real workload.
- Several consecutive useful steps of the resumed task, with the observation duration and any failures stated.
- A controlled interruption/resume check for any claimed recovery patch.
- Correct task and process state after resumption.

Separate **route usable now**, **ordinary task resumed**, and **failure recovery tested**. A success in one category does not imply the others.

## Source basis

Implementation guidance was checked against upstream revision `1603a073e46e8bc777cd9d096e2fce49887648c2`, not the unknown installed revision:
- [Configuring models](https://github.com/NousResearch/hermes-agent/blob/1603a073e46e8bc777cd9d096e2fce49887648c2/website/docs/user-guide/configuring-models.md).
- [Fallback providers](https://github.com/NousResearch/hermes-agent/blob/1603a073e46e8bc777cd9d096e2fce49887648c2/website/docs/user-guide/features/fallback-providers.md).
- [Codex model definitions](https://github.com/NousResearch/hermes-agent/blob/1603a073e46e8bc777cd9d096e2fce49887648c2/hermes_cli/codex_models.py).

Availability for the affected account must be tested; a model appearing in code is not an availability guarantee.
