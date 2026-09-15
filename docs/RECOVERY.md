# Recovery status and upstream action

## 15 September — no working owner-side fix found

**15 September 2026 — OPEN / work remains disrupted.** The owner still cannot work normally and has found no working fix on their side. **Joint investigation and repair are requested from Hermes maintainers and the provider.**

The new audit contains **100 failed Codex API attempts in the last 48 hours** (78 overload, 18 request-processing errors with request IDs, 4 connection/timeout), with **three final overload errors in GUI turns**. The full after-commit window contains **118 Codex attempts**; retries and mirrored records are deduplicated. [New report](https://github.com/gv1983us-commits/codex-provider-incident/blob/main/docs/CONTINUING-INCIDENT-2026-09-15.md) · [Attempt ledger, diagnostic index and source hashes](https://github.com/gv1983us-commits/codex-provider-incident/blob/main/evidence/log-audit-20260915.json).

The [12 September recurrence](https://github.com/gv1983us-commits/codex-provider-incident/issues/1#issuecomment-5643482050) and this update supersede the earlier resolution claim. Successful runs document temporary continuation; sustained reliable operation remains unresolved. The final failed series is still retrying at the end of the logs on 15 September. Per-request Full/Light attribution is not supplied by these new logs.

| Requested party | Required work | Evidence / acceptance |
| --- | --- | --- |
| Provider | Correlate the supplied request IDs, timestamps and models; identify and remediate repeated overload/request-processing failures and investigate connection failures on the Codex route. | A concrete finding and remediation status tied to the supplied receipts. |
| Hermes maintainers | Investigate retry exhaustion, redirect termination, failed compression/approval, history-version rejection and the WebSocket limit; preserve completed tool results and make interrupted state explicit. | A supported upstream fix or documented handling, with safe continuation and no uncontrolled replay of completed tools. |
| Hermes + provider together | Trace the failing request → retry → tool/result → continuation path across both layers and agree on a supported fix. | Sustained useful work on the affected setup, with observation duration and remaining failures reported; successful one-shot calls alone do not close the incident. |

The following procedure is retained as the **10 September investigation plan**, not a newly validated workaround or a request for further owner-side live tests. Existing evidence should drive the joint investigation.

## Historical investigation procedure — 10 September

[Home](../README.md) · [Open tasks](https://github.com/gv1983us-commits/codex-provider-incident/issues)

**Target:** reliable Jarvis work through Hermes using the existing Pro Full `openai-codex` subscription connection.

**Status recorded on 10 September:** the owner reports [Sol overload on Light at 15:21:54.745 UTC](LIGHT-OVERLOAD-2026-09-10.md), after the reported completion of conservation. Both accounts now have observed Hermes/Codex failures. Preserve the reported `QUIESCED_SAFE` checkpoint and pause automatic requests if any remain active. No dependable fallback is established; a new browser-wide failure is not shown.

**Earlier status:** Astra's short Medium tool turn passed, but later work failed with an [exact `unknown` receipt at 14:17:45.876 UTC](ASTRA-2026-09-10.md). The owner reports approximately ten additional tool calls. The failed call's effort is not supplied. The earlier GPT-5.5 overload remains confirmed. No sustained workaround or exception-recovery repair is demonstrated. Preserve the interrupted work and correlate the supplied request ID before treating a retry as recovery.

**Temporary continuation on another account:** the owner now reports that connecting Pro Light to Hermes resumed work in the same large Builder session. The owner has now supplied a completed conservation report and identified Sol as the working model. A later Astra banner was explicitly identified as old. [Completion and receipt comparison](CONSERVATION-2026-09-10.md). Underlying storage/process state and sustained duration have not been independently verified here. Record this as [same-session progress on Light](ACCOUNT-SWITCH-2026-09-10.md), while retaining the Full-account recovery target and earlier failures. No new export or effective request settings were captured for this switch.

## 0. Preserve the confirmed failures

The latest Astra receipt explicitly names `openai-codex`, `gpt-6-astra-900k`, `code: unknown` and request ID `2a438cad-4c97-43d4-abc8-77ce4e4ac03d`. Keep it separate from `overloaded`; neither the HTTP status nor the raw upstream error object is supplied. Obtain the relevant request/result boundary around **2026-09-10T14:17:45.876Z** if needed to locate the pending step. See the [receipt and scope](ASTRA-2026-09-10.md).

The new Copy details receipt explicitly names `openai-codex`, `gpt-5.5` and `overloaded`. It resolves the screenshot's model-attribution uncertainty. The receipt does not give an HTTP status, endpoint, request ID, call role or attempt count; obtain a focused log excerpt only if one of those fields is needed for a concrete diagnostic.

`retryable: true` is an error classification, not a promise of provider recovery. Treat GPT-5.5 as another route with an observed failure until scoped recovery evidence is supplied.

The screenshot's `9fd44b4` resolves to a public reference commit. The [request map](REQUEST-MAP.md) traces that snapshot; the installed runtime and customization delta remain unverified.

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

The export demonstrates **Astra 900k / Medium** completing five tool calls and a final answer. That limited check passed. The subsequent owner-reported continuation has now failed; the new receipt identifies Astra but does not establish unchanged settings or session identity. Preserve completed results and determine what remains pending before resuming. The selection instructions below describe the earlier candidate, not a currently reliable route.

Keep the already successful session's settings. If selection is necessary, the documented in-chat model-switch form is:

```text
/model gpt-6-astra-900k --provider openai-codex
```

Use the existing connection to the affected Full account and retain **Medium** reasoning and **normal** fast-mode state. The model command alone does not set those two controls or authenticate a different account. This is the observed candidate configuration, not a proven durable repair.

Ask the agent to list a harmless test directory with an available read-only tool, then state one filename from the result. A pass requires:

1. The selected model emits a tool call.
2. The tool completes and its result is retained.
3. A subsequent model request uses that result and returns normally.

Record actual timestamps, provider/model IDs from the request log, error text if any, and the installed revision. A plain text response alone is a narrower success. Stop after the normal bounded retry budget if the route fails; do not generate a retry storm.

If the current installation does not support this command, report the installed revision and its supported model picker. Do not assume current upstream documentation describes a customized historical build exactly.

### Record where the delay occurs

For one short, comparable request, distinguish time before the first visible event, time spent producing the response, and time inside a tool. Record both the selected setting and effective request value if available. The [pinned-source review](REQUEST-MAP.md) finds that GPT-5.5 Max and Ultra both clamp to `xhigh` in the standard Hermes Codex path; installed overrides remain unverified. The later export supplies message-level intervals, recorded below.

The new export supplies several measured message intervals: Astra's first text response **6.36 s**, first tool request **3.69 s**, whole five-tool turn **51.33 s**; the initial doctor operation itself took **32.50 s**. These intervals do not isolate server queueing or model compute.

## 3. Cover the routes Jarvis actually uses

After a successful short cycle, inspect each active role:

| Role | What must be verified |
| --- | --- |
| Main agent | Live session uses the intended Full account and observed candidate `openai-codex` / Astra 900k / Medium; retain actual request values when available |
| New-session default | Future sessions do not revert to an affected model |
| Auxiliary jobs | Effective model and fallback choices are known |
| Compression / summarization | Summary request succeeds on the intended route or preserves recoverable state on failure |
| Explicit workers | Each pinned model is checked; parent changes do not prove worker changes |
| Fallback | A failed target is not repeatedly reintroduced at every new turn |

Current upstream docs describe `--global` as persisting the model and switching the live session:

```text
/model gpt-6-astra-900k --provider openai-codex --global
```

Apply persistence only after further useful work supports the candidate and the intended defaults are confirmed. Do not infer that this model command propagates reasoning settings or explicit auxiliary/worker overrides. Dashboard changes alone apply to new sessions; existing sessions keep their model until explicitly switched.

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
