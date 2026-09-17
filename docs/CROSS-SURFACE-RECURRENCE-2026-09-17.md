# Cross-surface recurrence: extended Sol failures and direct ChatGPT Pro Light stalls — 17 September 2026

**Status: OPEN. Normal work is still not reliable.** This update extends the audited Hermes/Codex boundary from **16 September 09:59:31.688** through **17 September 16:00:58.322 Europe/Istanbul (UTC+3)** and records two separate owner-observed stalls in direct ChatGPT Pro Light on 17 September.

The Hermes ledger and the direct ChatGPT observations are deliberately kept as two evidence classes. Hermes supplies timestamps, models, retry numbers, sessions, request IDs and terminal GUI status. Direct ChatGPT does not expose an equivalent request ledger here, so those stalls are not added to the Hermes API counts and are not presented as proof of a shared backend route.

## Audited reconciliation

| Metric | Previous published boundary | Current boundary | Added |
| --- | ---: | ---: | ---: |
| Failed Codex attempts since the fixed 10 September baseline | 293 | **690** | **397** |
| Failed Codex attempts in all supplied logs | 524 | **921** | **397** |
| Failed attempts in the final rolling 48 hours | 266 | **465** | Window moved; not an additive comparison |
| Codex GUI turns ending in error after the baseline | 26 | **53** | **27** |

The 397 new failures are all logged as `gpt-5.6-sol-900k`. Their categories are:

| Category | Attempts |
| --- | ---: |
| `overloaded` | 290 |
| Generic request-processing error with request ID | 86 |
| Connection error | 19 |
| HTTP 503 / upstream connection refused | 1 |
| 120-second no-first-event timeout | 1 |

There are **86 provider request IDs**, **30 complete 1→5 attempt sequences**, **28 explicit terminal exhaustion records**, and **27 GUI error turns** in the added interval. Retry attempts count separately. Mirrored records, retry notices, terminal summaries, transport diagnostics and GUI endings do not add attempts.

The same agent coverage contains **826 successful Codex API-call records** after the previous boundary. Successes and failures coexist; this is intermittent degradation rather than evidence of a continuous total outage. The source set is not used to claim an overall failure percentage.

## Time distribution and censored quiet periods

On 16 September, after the previous 09:59 boundary, the logs contain **381 failed attempts** from 10:00 through 19:36 local time:

| Local hour | Failed attempts |
| --- | ---: |
| 10 | 19 |
| 11 | 22 |
| 12 | 55 |
| 13 | 39 |
| 14 | 32 |
| 15 | 50 |
| 16 | 55 |
| 17 | 47 |
| 18 | 47 |
| 19 | 15 |

On 17 September the supplied Hermes logs add **16 overload attempts** between 15:45 and 15:56. The owner closes Hermes during dense failure periods. This stops both errors and productive work, so gaps in the log are censored observation periods, not demonstrated recovery windows.

## Parallel-session evidence

The added interval contains eight cross-session failure pairs within two seconds. The closest pair occurred at **13:01:42.297 and 13:01:42.301 on 16 September**, only **4 ms apart**, in the two active sessions. Other pairs were 129 ms, 211 ms, 346 ms, 736 ms, 1.097 s, 1.927 s and 1.957 s apart.

Near-simultaneous errors across sessions support a shared dependency or shared capacity episode. They do not identify a specific internal pool or prove that all errors have one root cause.

## Long user-visible turn failures

The 24 GUI error turns added on 16 September include terminal failures after **2,913.0 s**, **3,944.9 s**, **6,220.5 s**, **8,689.7 s**, and **9,254.6 s**. The longest is about **2 h 34 min**. These are Hermes turn durations ending in a recorded error, not model compute-time measurements.

On 17 September, three more GUI turns ended in overload:

| Session | Terminal time, UTC+3 | GUI duration | Attempt-chain duration |
| --- | --- | ---: | ---: |
| `...b2c101` | 15:46:18.539 | 269.2 s | 48.995 s |
| `...b2c101` | 15:49:57.469 | 101.3 s | 95.695 s |
| `...f947d8` | 15:56:01.675 | 495.4 s | 48.363 s |

Those are three complete 5/5 chains across the two sessions, plus one additional `...f947d8` attempt at 15:49:01 whose continuation is not represented as a complete chain in the supplied boundary.

## Five retries still fit inside one capacity episode

Across the 30 complete new 5/5 sequences, attempt 1→5 took:

- minimum: **37.550 s**;
- median: **93.230 s**;
- maximum: **182.367 s**.

This does not mean that Hermes has a fixed 12-second per-attempt deadline. The logs independently show a 120-second no-first-event watchdog. Most provider rejections returned earlier, while short exponential waits consumed the full five-attempt budget inside a roughly 38-second to 3-minute horizon.

The Hermes-side requirement remains: separate the per-request response deadline from the retry horizon; honor `Retry-After` when available; add a longer overload cooldown with jitter; coordinate a provider/model circuit breaker across sessions; and expose a paused/cooldown state rather than a green goal that is no longer advancing.

## Model switching and context compression are not demonstrated workarounds

All 397 added failed attempts use Sol. The earlier Astra→Sol change therefore did not establish sustained recovery.

Today's compression sequence provides a second control:

1. Session `...b2c101` compressed **761→41 messages**, from an estimated **197,484 tokens to 52,145**, in 199.703 seconds. The next request then entered a complete 5/5 overload chain.
2. Session `...f947d8` attempted compression at an estimated 168,755 tokens. Summary generation itself returned overload; the fallback committed **829→37 messages / ~45,462 tokens**. The next request then entered another complete 5/5 overload chain.

This does not prove context size is irrelevant to every failure. It does show that reducing these sessions to roughly 45–52 thousand estimated tokens did not remove the observed overload condition.

## Direct ChatGPT Pro Light observations — separate evidence class

The owner was using direct ChatGPT in Yandex Browser on the Pro Light account, without Hermes in that request path, and reported two stalls on 17 September:

| Local period | Owner-visible result | Available telemetry |
| --- | --- | --- |
| Morning | No visible final answer for nearly two hours; owner manually stopped the turn | No request ID, hidden retry ledger or transport trace available |
| Afternoon | A later turn produced no visible final answer for more than three hours before the owner reported the stall and re-prompted | No request ID, hidden retry ledger or transport trace available |

These two observations are **not** counted among the 397 Hermes failures. They broaden the affected user-visible surface beyond Hermes on the same day, but they do not prove that direct ChatGPT and Hermes used the same internal route or failed for the same cause. The content of the private conversation is not published.

## Architectural boundary visible from the incident

| Layer | Directly observed behavior | What remains unknown |
| --- | --- | --- |
| Provider/Codex request path | Overload, request-processing IDs, connection errors, 503 connection refusal, and a 120-second no-event timeout | Internal pool, scheduler, account routing and exact server-side cause |
| Hermes request loop | Five-attempt retry chains, short backoff horizon, successful calls interleaved with failure | Whether upstream headers such as `Retry-After` were available or discarded |
| Hermes compression | Long summary generation, provider failure during summary, fallback commit | Whether all compression traffic shares the main model route internally |
| Hermes session/UI | Long-running turns terminate as errors; green logical goal can coexist with no forward execution | A reliable liveness contract between goal state, request state and resumable task state |
| Direct ChatGPT Pro Light | Two owner-visible multi-hour stalls without a final answer | Request IDs, retries, model-route details and backend relation to Hermes failures |

## Conclusions for the incident

1. **The incident remains open.** A successful call, a model switch, or a completed compression does not establish sustained useful operation.
2. **Sol is affected at scale.** Every one of the 397 added Hermes failures is Sol.
3. **Five attempts are not sufficient protection when spent in one short overload episode.** More attempts without a longer coordinated horizon would mainly multiply load and cost.
4. **Parallel sessions need shared backpressure.** Near-synchronous failures show why independent session retry loops should not probe the same provider/model simultaneously during a capacity event.
5. **UI goal state must not stand in for execution liveness.** A green goal can remain visible while request progression has stopped.
6. **Direct ChatGPT symptoms matter but must remain epistemically separate.** They justify cross-surface investigation, not a fabricated common request trace.
7. **Owner-side polling cannot identify recovery.** Hermes is intentionally closed during failure bursts, and short successful intervals have repeatedly ended in another failure.

## Requested joint action

### Provider

- Correlate the 86 new request IDs, timestamps, model and account-side telemetry.
- Explain the mix of overload, generic processing errors, connection failures and the explicit HTTP 503.
- Establish whether direct ChatGPT Pro Light stalls and Codex route failures share any capacity, scheduler or account-routing dependency.

### Hermes maintainers

- Treat overload recovery as a time-horizon and coordination problem, not only an attempt-count setting.
- Implement provider/model circuit breaking across active sessions, with one canary after cooldown.
- Preserve resumable state and expose explicit paused/provider-cooldown status.
- Make compression, goal state and request liveness independently observable.

### Acceptance

Recovery requires sustained useful work on the affected setup, across a declared observation period, with remaining failures reported. One successful response, one short tool loop, or a green goal badge is insufficient.

## Sources, integrity and limits

The public machine-readable ledger is [`evidence/log-extension-20260917T130058Z.json`](../evidence/log-extension-20260917T130058Z.json). It contains the 397 attempt records, 27 GUI endings, 30 complete-chain summaries, transport and compression diagnostics, direct-observation boundaries, source hashes and the deduplication method.

The new `gui(4).log`, `desktop(4).log` and `gateway(2).log` are exact byte-prefix extensions of their `(3)`, `(3)` and `(1)` predecessors respectively. Rotated agent/error files are combined and deduplicated by timestamp, level, session, logger and full message. Raw prompts, credentials, private tool output and full session archives are not published.
