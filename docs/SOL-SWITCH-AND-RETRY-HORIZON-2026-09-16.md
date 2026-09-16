# Sol switch, overlapping five-attempt exhaustion, and retry-horizon evidence — 16 September 2026

**Status: OPEN. Normal Jarvis work remains disrupted.** This update extends the evidence boundary from **09:06:01.535 to 09:59:31.688 Europe/Istanbul (UTC+3)** on 16 September 2026. It adds the completion of the pending Astra chain, a switch to Sol, two short Sol work windows, and three complete Sol 5/5 failure chains.

## Audited totals

| Metric | Through 09:06 | Through 09:59 | Delta |
| --- | ---: | ---: | ---: |
| Failed Codex API attempts since the fixed 10 September baseline | 277 | **293** | **+16** |
| Failed Codex API attempts in all supplied logs | 508 | **524** | **+16** |
| Failed attempts in the final rolling 48 hours | 255 | **266** | +11 as the window moved |
| Codex GUI turns ending in error after the baseline | 22 | **26** | **+4** |

The 16 fresh attempts comprise **1 Astra and 15 Sol**: six `overloaded` and ten generic request-processing failures carrying request IDs. Retry attempts count separately; mirrored records count once. GUI errors overlap the attempts and are not added to the API count. A separate Smart Approvals auxiliary failure at 09:58:17.457 (`6ae9171d-...`) is preserved in the evidence but excluded from the main-turn attempt count.

## Model switch did not restore sustained work

The existing session switched in place from `gpt-6-astra-900k` to `gpt-5.6-sol-900k` at **09:18:43**. Sol then behaved as follows:

| Prompt / session | Successful Sol calls before failure | Last success | Terminal result |
| --- | ---: | --- | --- |
| 09:25:38 / `...f947d8` | 6 | 09:29:38 | 5/5 exhausted; GUI `unknown` at 09:31:35 |
| 09:55:53 / `...b2c101` | 6 | 09:57:37 | 5/5 exhausted; GUI `unknown` at 09:58:35 |
| 09:56:06 / `...f947d8` | 7 | 09:58:16 | 5/5 exhausted; GUI `overloaded` at 09:59:31 |

The final two turns ran in parallel. Their retry chains overlapped and their terminal GUI errors were **56.612 seconds apart**, while exposing different final messages: one generic processing error with request ID, one overload. This is evidence that changing Astra to Sol is not a reliable workaround; it is not proof that every model or request shares one internal root cause.

The UI still showed green goals, but the latest goal-judge records predate these failures. One session's latest judge was `wait` at 08:29; the other judge itself failed on overload at 08:56 and logged “falling through to continue.” No successful model call follows 09:58:16 in the supplied files. A green goal badge therefore records logical goal state, not active forward execution.

## Five attempts are consumed in less than one minute

| Session / chain start | Attempt 1 → 5 | Configured waits after attempts 1–4 |
| --- | ---: | --- |
| `...f947d8`, 09:30:39 | **55.250 s** | 2.038, 5.466, 8.549, 18.698 s |
| `...b2c101`, 09:57:39 | **54.560 s** | 2.716, 5.979, 11.218, 22.968 s |
| `...f947d8`, 09:58:33 | **57.374 s** | 2.549, 4.710, 10.540, 21.179 s |

This is not evidence of a fixed 12-second timeout per attempt. Hermes separately logs a **120-second no-first-event watchdog per request**. In these three chains the upstream failures returned quickly, and the short exponential waits burned the full five-attempt budget inside the same apparent capacity episode.

### Hermes-side request

For retryable capacity and transient processing errors, please avoid treating “five attempts” as five calls packed into roughly one minute:

1. Keep the per-attempt no-event deadline independent from the total retry horizon.
2. Honor `Retry-After` when supplied.
3. Use a longer configurable overload schedule with jitter, for example 15 / 30 / 60 / 120 seconds, or an equivalent 5–10 minute retry horizon.
4. Add a provider/model circuit breaker shared across parallel sessions so only one low-cost canary probes recovery after cooldown instead of both sessions retrying together.
5. Preserve resumable task state and show an explicit provider-cooldown/paused state instead of leaving a green goal that is no longer advancing.

The exact defaults are maintainers' choice; the evidence-supported requirement is to separate the per-attempt deadline from the retry horizon and prevent all retries from being spent inside one short overload burst.

## Request IDs added for provider correlation

All timestamps below are UTC on 16 September 2026.

| Time | Request ID | Model / attempt |
| --- | --- | --- |
| 06:30:39.853 | `fb428e2e-2d92-4398-9900-aceb7c6c9b17` | Sol 1/5 |
| 06:30:46.603 | `7e4ea726-c18f-4b9a-a429-7d87e747b307` | Sol 2/5 |
| 06:31:35.103 | `1b1ec246-61f2-4ff8-97d0-17645ada869e` | Sol 5/5 |
| 06:57:39.884 | `ecaa9406-b6ea-4e7a-a0d7-25b9c6a322c5` | Sol 1/5 |
| 06:57:45.073 | `ce0e6f70-1d2b-4869-a091-21df1b8bb438` | Sol 2/5 |
| 06:58:33.391 | `03dbcffd-472a-497e-a75a-df5272faed89` | Sol 1/5, parallel session |
| 06:58:34.444 | `7969d90e-f26b-4b67-8cac-ca8ad9606fbd` | Sol 5/5 |
| 06:58:38.672 | `dd8cff11-173f-4c0c-8892-9cb5b13750ad` | Sol 2/5 |
| 06:58:46.468 | `864ad9a7-0499-41d4-a276-7a4dbed83c73` | Sol 3/5 |
| 06:58:59.594 | `b7ccd569-db76-4744-b147-0fd65dd7e345` | Sol 4/5 |

## Operational impact and recovery visibility

The owner closes Hermes when dense failure bursts begin. This stops requests and prevents error counters from growing, but Jarvis work also stops. Silent intervals are therefore censored availability, not evidence that the provider recovered.

The fresh data gives two direct checks of short polling:

- restarting approximately 18 minutes after the 09:07 Astra exhaustion produced six Sol successes, then a terminal failure;
- retrying approximately 24 minutes after the 09:31 Sol exhaustion produced only 6–7 successful calls in each parallel session before both failed again.

Earlier logs also show a 1 hour 42 minute request gap followed by a brief completion and another overload roughly five minutes later. These observations do not establish a reliable recovery time. Rechecking every 15 minutes with full work turns mainly creates more attempts and can mistake a short success burst for recovery.

A conservative owner-side observation procedure, pending an upstream fix, is one session and one inexpensive canary after at least 45–60 minutes, followed by 10–15 minutes of sustained successful calls before resuming both work streams. If the canary fails, wait another 60–90 minutes. This is damage limitation, not a fix or a claim about the provider's recovery schedule.

## Independent reports and status mismatch

The same broad client-visible class remains independently reported:

- [openai/codex#43663](https://github.com/openai/codex/issues/43663): Pro 20x, Sol and Astra, `server_overloaded`, intermittent bursts despite 0% usage remaining.
- [openai/codex#43446](https://github.com/openai/codex/issues/43446): repeated Astra HTTP 503 overload and eventual intermittent success.
- [sub2api#6631](https://github.com/Wei-Shaw/sub2api/issues/6631): upstream `server_is_overloaded` / service-unavailable response.
- [sub2api#6776](https://github.com/Wei-Shaw/sub2api/issues/6776): day-long intermittent overload and concurrency-limit reports.

The [official OpenAI status page](https://status.openai.com/) was green at collection time and explicitly states that its availability metrics are aggregate across tiers, models, and error types and that individual availability may vary. No Hermes maintainer or OpenAI staff acknowledgement follows our latest evidence comments at the time of this update.

## Source integrity and limits

`errors(3).log`, `gui(3).log`, and `desktop(3).log` are exact byte-prefix extensions of their `(2)` predecessors. `agent(2).log` is a later rotated segment and is combined with `agent(1).log`, not used as its replacement. File hashes, the 16-entry ledger, prompt windows, retry timings, exclusions, and deduplication method are in [`evidence/log-extension-20260916T065931Z.json`](../evidence/log-extension-20260916T065931Z.json).

The supplied files do not provide a complete successful-call denominator for the full 8–16 September period, so no overall failure rate is claimed. They also cannot reveal the exact moment of provider recovery while Hermes is closed. Raw prompts, credentials, and account identifiers are not published.
