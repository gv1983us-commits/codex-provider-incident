# Evening continuation: successful inner calls, failed Hermes turns, and a third incomplete Direct ChatGPT turn — 17 September 2026

**Status: OPEN. Paid work remains materially blocked.** This addendum extends the audited Hermes boundary from **17 September 16:00:58.322** through **19:43:53.071 Europe/Istanbul (UTC+3)** and extends the public Direct ChatGPT session graph through **19:30:04.194**.

The owner reports that the interrupted work is a paid production/build workflow: ChatGPT subscriptions continue to be consumed, paid infrastructure remains available but underused, and JARVIS OS construction does not advance reliably. No monetary amount is inferred from the logs. The technical evidence below establishes work interruption and wasted orchestration cycles; the subscription/infrastructure cost impact is owner-supplied context.

## Reconciliation with the preceding publication

The four newly supplied files are exact byte-prefix extensions of the previously audited files:

| Previous file | Current file | Added lines | Added bytes |
| --- | --- | ---: | ---: |
| `agent(3).log` | `agent(4).log` | 259 | 61,900 |
| `desktop(4).log` | `desktop(5).log` | 213 | 20,484 |
| `errors(4).log` | `errors(5).log` | 87 | 26,754 |
| `gui(4).log` | `gui(5).log` | 17 | 2,859 |

No previous row was replaced. The new records can therefore be treated as an additive continuation rather than a new incompatible snapshot.

| Metric | Previous published value | Added | Current value |
| --- | ---: | ---: | ---: |
| Failed Codex attempts since the fixed 10 September baseline | 690 | 33 | **723** |
| Failed Codex attempts in all supplied logs | 921 | 33 | **954** |
| Failed attempts in the final rolling 48 hours | 465 | window moved | **498** |
| Comparable Codex GUI turns ending in error after the baseline | 53 | 2 | **55** |
| Provider request IDs in the audited 17 September extensions | 86 | 4 | **90** |
| Complete 5/5 sequences in the audited 17 September extensions | 30 | 2 | **32** |
| Direct ChatGPT incomplete turns confirmed from shared graphs | 2 | 1 | **3** |

The broader raw TUI corpus contains 57 `status=error` endings because it also contains two older HTTP 500/400 GUI endings outside the comparable published series. The table retains the existing comparable metric rather than silently changing its definition.

## New Hermes/Codex delta

The new boundary contains **33 failed attempts**, all on `gpt-5.6-sol-900k` in session `20260914_183547_b2c101`:

| Category | Attempts |
| --- | ---: |
| `overloaded` | 29 |
| Generic request-processing error with provider request ID | 4 |

The four new request IDs are:

- `7bb7a962-7ec1-46a4-a4b2-ede3587cd2da`
- `088123bd-3880-4c51-b9fb-530766521aa8`
- `a0725d04-8fe9-4291-b494-103a73a541df`
- `3c2c310e-b9cd-4655-b923-8a12494ebbd0`

They do not duplicate the 86 IDs in the preceding extension.

## Successful model calls did not make the user turns succeed

The strongest new Hermes observation is not merely another overload chain. Two user turns contained many successful provider calls and still terminated as GUI errors:

| User turn | Successful calls | Failed attempts | Logged successful input/output sum | Terminal result |
| --- | ---: | ---: | ---: | --- |
| 16:48:37 → 17:05:03 | 11 | 26 | 789,280 / 3,636 tokens | `status=error`, overload, **986.2 s** |
| 19:05:31 → 19:13:54 | 13 | 7 | 1,131,414 / 10,192 tokens | `status=error`, overload, **503.0 s** |
| **Total** | **24** | **33** | **1,920,694 / 13,828 tokens** | two failed user turns |

The input total is a sum of the per-call log fields and includes repeatedly sent context, much of it logged as cached. It is **not** asserted to be a provider billing total. Failed attempts do not expose an equivalent token accounting here.

Successful call latency ranged from **3.9 s** to **175.2 s**, with a median of **29.75 s**. Calls #2 through #25 produced valid response IDs, model output and tool progression. Nevertheless, the first user turn ended after 16 minutes 26.2 seconds and the second after 8 minutes 23.0 seconds, both with the final overload banner retained.

This demonstrates a turn-level reliability gap:

1. a provider call can succeed;
2. tools and subsequent model calls can run;
3. later calls can enter a short retry chain;
4. the full user turn can still be reported as failed;
5. completed intermediate work is not equivalent to a delivered, resumable final result.

## Retry horizon remains too short for a capacity episode

The terminal 5/5 chains ended at:

- **17:05:02.947**, context estimate `~91,884` tokens;
- **19:13:53.687**, context estimate `~90,439` tokens.

The final chains consumed approximately 93.2 seconds and 84.7 seconds from attempt 1 to attempt 5. Earlier partial chains reset after successful calls, then another request failed. Raising the attempt count alone therefore does not create useful resilience: retries need a coordinated time horizon, provider/model circuit breaking across sessions, resumable turn checkpoints and one controlled canary after cooldown.

## Compression remains expensive and is not a liveness signal

Two sessions compressed concurrently in the evening:

| Session | Start | End | Message/token change | Duration |
| --- | --- | --- | --- | ---: |
| `...b2c101` | 19:27:43.653 | 19:32:32.165 | `113→83`, `~103,988→~52,551` | 288.500 s |
| `...f947d8` | 19:28:19.600 | 19:33:14.750 | `37→37`, `~45,462→~43,282` | 295.157 s |

The second operation spent almost five minutes while retaining all 37 messages and reducing the rough estimate by only about 2,180 tokens. Completion of compression is not proof that the agent can resume useful work.

## New Hermes local delivery symptom

From **19:43:00.509** through **19:43:53.071**, Hermes emitted seven warnings:

```text
ws write slow (loop stalled >10.0s) ... frame left in flight
```

The warnings recur roughly every ten seconds on the local loopback WebSocket. They establish that the Hermes event loop/UI delivery path was stalled while a frame remained in flight. They do not identify whether the initiating cause was compression, another synchronous callback, provider activity, local resource pressure or a separate client problem.

This is architecturally separate from provider overload and must not be collapsed into it:

| Layer | Direct observation | Required repair direction |
| --- | --- | --- |
| Provider request | overload and request-ID failures | provider correlation and serving/routing repair |
| Hermes turn orchestration | 24 successes + 33 failures still yield two failed user turns | durable substep checkpoint, resume without replay, turn-level circuit breaker |
| Hermes compression | concurrent 4.8–4.9 minute operations | isolate compression scheduling and expose progress/liveness |
| Hermes UI transport | seven loop-stall/WebSocket warnings | keep event loop non-blocking, delivery acknowledgement and watchdog |
| User-visible completion | work may occur without a final answer | distinguish internal completion, persisted result and client delivery |

## Third Direct ChatGPT incomplete turn — graph confirmed

The new public share is `https://chatgpt.com/share/6aac18eb-6428-83eb-8bc1-adeabac78920`. It is a strict extension of the previous share:

- previous graph: 3,928 message nodes;
- current graph: 4,393 message nodes;
- all 3,928 previous message IDs remain present;
- 465 new message IDs were added;
- no previous message ID is missing.

It confirms a third incomplete `gpt-5.6-sol-wm` turn:

| Event | Europe/Istanbul time | Structural result |
| --- | --- | --- |
| User prompt | 16:28:40.886 | requested share-graph analysis and Git publication |
| Last assistant activity | 16:38:38.022 | empty model node, `end_turn=false` |
| User complaint | 19:18:45.701 | final result had not appeared |
| First final after the complaint | 19:19:06.967 | `end_turn=true`, 21.266 s later |

Between the original prompt and complaint, the graph contains **54 assistant nodes**, **29 tool nodes**, **9 assistant nodes with text**, and **zero assistant nodes with `end_turn=true`**. The lower-bound prompt-to-complaint interval is **10,204.815 seconds = 2:50:04.815**. Internal work and Git writes completed, yet the original user turn did not deliver a terminal result.

Together, the three non-overlapping Direct ChatGPT incomplete-turn windows on 17 September total a lower bound of **8:13:14.678**:

- 1:51:00.368;
- 3:32:09.495;
- 2:50:04.815.

This is not added to Hermes downtime because the surfaces overlap in wall-clock time and doing so would double-count owner impact.

The following cross-platform analysis turn completed with `end_turn=true` in **572.221 seconds**. That control shows intermittent completion, not a permanently dead conversation.

## Current conclusion

1. **The incident remains open and economically consequential.** Work is blocked while paid subscriptions and infrastructure continue to exist; no cost amount is inferred.
2. **A successful inner API call is not a valid recovery criterion.** Twenty-four such calls coexisted with two failed user turns.
3. **The failure now spans three separately evidenced layers:** provider request failures, Hermes orchestration/recovery weakness, and Hermes/ChatGPT result-delivery failure.
4. **Direct ChatGPT is not exonerated by the absence of a visible error banner.** The session graph records internal activity without terminal `end_turn=true` delivery.
5. **Hermes is not the sole root cause, but it amplifies loss.** Short independent retries, expensive compression, lack of durable turn resumption and a blocked UI loop turn upstream instability into long user-visible outages.
6. **No owner-side model switch, compression or retry-count change has established sustained recovery.**

## Acceptance boundary

Recovery requires a declared soak period containing sustained useful work, terminal responses delivered to the client, resumable intermediate state, and a disclosed residual failure count. A green goal badge, a successful sub-call, a completed compression, or one final response after re-prompting is insufficient.

## Evidence and privacy

- Machine-readable Hermes extension: `evidence/log-extension-20260917-late.json`.
- Minimized Direct ChatGPT graph extension: `evidence/direct-chatgpt-share-extension-20260917.json`.
- Full raw logs, the 12.98 MB share HTML and private conversation text are not republished.
- The receipts preserve hashes, exact timestamps, structural fields and explicit non-claims.
