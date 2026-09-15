# Continuing Codex failures: external reports and retry assessment — 15 September 2026

[Incident report](CONTINUING-INCIDENT-2026-09-15.md) · [Open issue #1](https://github.com/gv1983us-commits/codex-provider-incident/issues/1) · [Work Mode observability record](../evidence/work-mode-observability-20260915.json)

**Status remains OPEN.** The owner still cannot work normally and has not found a working owner-side fix. Joint investigation and repair by Hermes maintainers and the provider remain the requested outcome.

## Current device/account comparison supplied by the owner

All three surfaces are open on the same **Lenovo laptop**:

| Surface | Account | Current observation supplied by the owner |
| --- | --- | --- |
| Hermes, two parallel Astra sessions | **Pro Full** | Both sessions encounter errors synchronously; the existing three-attempt retry/reconnect path currently permits some continuation. |
| ChatGPT in Microsoft Edge, used as a control | **Pro Full** | When Hermes fails, this official-interface control also fails. |
| ChatGPT in Yandex Browser, this analysis conversation | **Pro Light** | The current conversation continues here without Hermes. The owner supplied a public share snapshot for inspection. |

This clarifies the account attribution of the two current Astra sessions: they are on **Full**, not Light. The owner asks whether increasing the retry budget from three to five would help.

The concurrent cross-client failures are owner observations. They make a Hermes-only explanation insufficient for the full reported pattern and strengthen the case for provider-side correlation. They do not prove a specific account restriction, shared backend, or universal outage. The Full and Light paths are different accounts, so this is not a controlled same-account isolation of Hermes alone.

The new observations are outside the uploaded-log boundary. Exact per-event timestamps, request IDs and error codes were not supplied for the current Hermes/Edge failures. They are **not added** to the audited 100/118 failed-attempt totals.

## Fresh independent reports in official Codex

| Source and UTC publication date | Reported observation | Scope |
| --- | --- | --- |
| [Codex #45611](https://github.com/openai/codex/issues/45611), 15 Sep 05:34 | Pro 20x, macOS, Astra/Sol/Terra, medium–ultra: repeated capacity errors over four days. The author explicitly reports **no concurrent tasks**. | An official-client report; reducing parallelism cannot be treated as a guaranteed remedy for every affected user. |
| [Codex #45600](https://github.com/openai/codex/issues/45600), 15 Sep 04:12 | Pro 20x, Windows, CLI 0.154.0, Astra: the author’s account fails while a friend’s account works; the author reports being unable to use even 20% of the weekly allowance. | An account comparison reported by another user. The author's proposed account restriction mechanism is unverified. |
| [New reply in Codex #44531](https://github.com/openai/codex/issues/44531#issuecomment-5673970453), 15 Sep 02:56 | Around **70% of weekly usage remains**, almost every model returns capacity errors, and the supplied description of doctor diagnostics reports successful local checks on CLI 0.154.0/macOS. | Diagnostic results are reported by that participant, not independently rerun by this audit. |
| [Codex #43375, latest reply](https://github.com/openai/codex/issues/43375#issuecomment-5675702112), 15 Sep 06:21 | Another participant reports the same problem in the existing multi-model capacity thread. | Brief corroboration only; no new transport trace. |

The first two new issues remain open. Their inspected replies contain automated translation/duplicate suggestions, not a published fix or recovery timetable. These independent observations establish that similar symptoms continue outside Hermes. They do not establish one root cause for all clients or accounts.

Previously reviewed Sub2API #6739/#6937 and CLIProxyAPI #5586 had no newer comments in the queried follow-up intervals. This is a bounded review of selected threads, not a global incidence estimate.

## Official service status

At the time of this check, [OpenAI Status](https://status.openai.com/) reports operational service. Its availability display is aggregate; it is not per-account proof of recovery.

- [Elevated error rates for Codex and ChatGPT Work](https://status.openai.com/incidents/01M2EWYR55J47M2BPG9WC76VEG): a 14 September incident is marked resolved.
- [Elevated errors affecting Work Mode in ChatGPT](https://status.openai.com/incidents/01M2GA8XTS6VB3QCDEGZ0HNAQ5): marked resolved on 15 September; the description specifically concerns some **Plus** users starting/resuming tasks and accessing workspace tools/files. That scope must not be silently equated with this Pro/Codex overload incident.
- [Failures of existing Work threads](https://status.openai.com/incidents/01M28MEQWTQJDRCRPFD9FWQ0H3): a separate mobile/web incident is marked resolved on 12 September.

None of these status notices provides request-ID correlation or a root-cause match to the owner's continuing failures.

In our [Hermes report #107307](https://github.com/NousResearch/hermes-agent/issues/107307), the latest inspected reply from another participant is [11 September's Badgr retry/fallback offer](https://github.com/NousResearch/hermes-agent/issues/107307#issuecomment-5635146279). It does not supply a Hermes patch or confirm recovery of this installation.

## Three attempts versus five

The current upstream Hermes snapshot inspected is `cedf4a3d78675283fa93e4e6ea2d6212bf414667`, committed on 15 September. The exact installed Windows revision is not known.

The ordinary model-call retry budget is the existing `agent.api_max_retries` configuration value. Despite the name, [initialization](https://github.com/NousResearch/hermes-agent/blob/cedf4a3d78675283fa93e4e6ea2d6212bf414667/agent/agent_init.py#L1348-L1353) and the [loop condition](https://github.com/NousResearch/hermes-agent/blob/cedf4a3d78675283fa93e4e6ea2d6212bf414667/agent/conversation_loop.py#L1391-L1420) show **three total attempts at value 3**, or **five total attempts at value 5**. The current configuration prose describes value 3 as four attempts and value 0 as no retries; this disagrees with the inspected code, which clamps the value to at least one. This assessment follows the code and the owner's observed `attempt N/3` logs.

For ordinary retryable errors without an overriding provider delay, [the recovery code](https://github.com/NousResearch/hermes-agent/blob/cedf4a3d78675283fa93e4e6ea2d6212bf414667/agent/turn_recovery.py#L1019-L1096) uses an exponential wait beginning at 2 seconds, with [0–50% random jitter](https://github.com/NousResearch/hermes-agent/blob/cedf4a3d78675283fa93e4e6ea2d6212bf414667/agent/retry_utils.py#L112-L125).

| Setting | Total attempts in the ordinary cycle | Waits before subsequent attempts | Sum of waits if all attempts fail |
| --- | --- | --- | --- |
| 3 | 3 | 2–3 s; 4–6 s | 6–9 s |
| 5 | 5 | 2–3 s; 4–6 s; 8–12 s; 16–24 s | 30–45 s |

These are **calculated backoff bounds**, not measured request latency or availability. Add the duration of every failed request. An explicit `Retry-After`, primary transport recovery or fallback can change the total duration or start another attempt cycle.

Increasing the existing setting to five is a reasonable temporary mitigation to consider when later attempts already succeed. It adds two opportunities to get past a short failure and reduces some manual retries; it does not make the upstream route reliable. Preserve the existing jitter, particularly with two sessions, rather than synchronizing immediate repeated requests.

The supported configuration command syntax is documented in [Hermes configuration](https://github.com/NousResearch/hermes-agent/blob/cedf4a3d78675283fa93e4e6ea2d6212bf414667/website/docs/user-guide/configuration.md):

```text
hermes config set agent.api_max_retries 5
```

Use the configuration belonging to the actual active Hermes profile. The setting is read during agent initialization; application to already-running cached agents should not be assumed. This review did not change the Windows installation, restart active work, or run live provider tests. A later ordinary-use log showing `attempt N/5` would establish that the intended agent adopted the setting.

## What the supplied ChatGPT Work share actually exposes

The supplied share snapshot was successfully downloaded directly after the web reader returned a fetch-disabled error. The HTML is **7,158,276 bytes**, SHA-256 `46b9405b01061f247abc479d28bc4a39631ee909893fe34aefb5fc07d0fedb59`. Its serialized message mapping was decoded as data; no embedded code was executed.

| Export observation | Count / result |
| --- | --- |
| Dated message range, UTC | 12 Sep 11:08:58.417 → 15 Sep 06:25:39.599122 |
| Mapping nodes / messages | 2,153 / **2,152** |
| User / assistant / tool / system messages | 63 / 1,614 / **472** / 3 |
| Tool output records marked redacted and replaced with the redaction placeholder | **472 of 472** |
| Nonempty unique `request_id` fields on **user messages** | **42**, all prefixed `wfr_` |
| Internal provider-error count, hidden retry count, tool-failure count from this export | **Unknown** |

Every tool output is replaced with `The output of this plugin was redacted.` The 42 request IDs are preserved in the [metadata record](../evidence/work-mode-observability-20260915.json) for correlation, together with their user-message timestamps. They are **not 42 model failures** and are not failed-model response IDs.

The message-status distribution is 2,145 `finished_successfully` and seven `in_progress`. Those are message serialization statuses, not measured model-request success/failure outcomes. They cannot be used to claim 2,145 successful API calls, seven failed calls, or zero provider errors.

The assistant also lacks an exposed live source for its internal model-request transport log and automatic retry counters. In the separately inspected live publication/research portion, one local orchestration script failed because JavaScript UTF-16 length was compared with Python Unicode code-point length. It was corrected, and the GitHub publication was verified. The later web-reader fetch-disabled event was recovered by direct download. Neither is classified as a provider model failure.

Pro Light attribution and the browser/device arrangement are supplied by the owner. Work has progressed in this interface, but hidden retries and backend-route equivalence are unobserved. The share cannot establish that this Light session had zero errors internally.

Only the minimized technical metadata is published. The raw HTML, conversation text, tool-call arguments, and system/assistant reasoning content are not attached to this public repository. This is an observation record, not an internal transport trace or a controlled benchmark.
