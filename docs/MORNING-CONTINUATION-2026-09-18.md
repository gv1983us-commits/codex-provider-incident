# 18 September morning continuation — intermittent provider failures persist after restart

**Status: OPEN.** This report extends the previously published 17 September boundary from **19:43:53.071 UTC+3** through **10:20:57.270 UTC+3 on 18 September**.

The four primary logs are exact byte extensions of the files used in the prior evening report. This makes the new counts additive rather than a re-audit of overlapping data.

## Exact source continuity

| Previous file | Current file | Added bytes | Exact previous-byte prefix |
| --- | --- | ---: | --- |
| agent(4).log | agent(5).log | 506,594 | yes |
| desktop(5).log | desktop(6).log | 5,961 | yes |
| errors(5).log | errors(6).log | 61,734 | yes |
| gui(5).log | gui(6).log | 10,500 | yes |

Additional lifecycle sources are gateway(3).log, gateway-exit-diag.log, and mcp-stderr.log. Their raw contents are not mirrored publicly; hashes and bounded structural facts are recorded in the machine-readable receipt.

## Provider-facing delta

The appended agent interval contains:

- **24 new failed main-loop Codex attempts**;
- **23 overloaded** responses;
- **1 request-processing error** with request ID **4c95dbeb-7407-4985-a35f-61ddefa22651**;
- all 24 name **gpt-5.6-sol-900k / openai-codex**;
- **385 successful main-loop Codex calls** in the same added interval;
- no new complete 1/5→5/5 exhaustion;
- no new GUI turn ending in provider error.

| Session | Failed attempts | Successful main-loop calls |
| --- | ---: | ---: |
| 20260913_181010_f947d8 | 11 | 136 |
| 20260914_183547_b2c101 | 13 | 249 |

Successful-call latency in the appended interval is **3.4 s minimum, 19.9 s median, 186.5 s maximum**.

The failure pattern is materially different from the 17 September evening terminal bursts. The new morning failures repeatedly reset to attempt 1 after intervening success; the longest observed consecutive failed retry chain reaches attempt 2/5, not 5/5. This is degraded service, but it is not a new claim of complete unavailability.

## Current reconciliation

| Metric | Previous published boundary | 18 Sep 10:20 boundary |
| --- | ---: | ---: |
| Failed attempts since fixed 10 Sep baseline | 723 | **747** |
| Failed attempts across all supplied logs | 954 | **978** |
| Final rolling 48 h | 498 | **451** |
| Comparable Codex GUI error turns | 55 | **55** |

The rolling 48-hour count falls because older 16 September failures aged out of the window. It must not be interpreted as proof of recovery.

The current 48-hour window begins at **16 September 10:20:57.270 UTC+3** and contains 339 overload attempts, 91 request-processing failures and 21 connection-class failures: **451 total**.

## User-visible turn state

The appended GUI segment records **20 accepted prompts**: 19 user prompts plus one auto-continue.

It records 16 completed turn records:

- **15 complete**;
- **1 interrupted**;
- **0 error**.

The interrupted record is the auto-continued session and is not counted as a provider failure. At the file boundary, both principal sessions continue to produce model/tool activity. The last successful provider calls are at **10:19:24.130** for f947d8 and **10:20:56.452** for b2c101.

The new evidence therefore shows persistent intermittent provider rejection inside otherwise productive long-running work, not a morning-long zero-availability state.

## Gateway lifecycle

A new lifecycle diagnostic adds a separate Hermes-local event.

On the 18 September startup, Hermes recorded that the prior gateway process had exited **uncleanly**: no exit path was observed. The last recorded heartbeat for that prior process was **17 September 20:01:25.473 UTC+3**. The lifecycle record says **state_db_integrity=ok** and **suspected_oom=False**.

No exact termination time or initiating cause is established. In particular, this record does not prove provider causation, OOM, or a Hermes code defect by itself.

The first 18 September gateway then started under the external supervisor and later exited cleanly at **09:00:46 local**. A second gateway started at **09:02:10 local** and remained active at the captured boundary.

The GUI also reaped two orphaned session rows at the first startup and one at the second startup. One session was then scheduled for auto-continue.

## Hermes-local warnings

The new logs contain local warnings that are deliberately kept separate from provider overload:

- twice, immediately after the two startups, Hermes warns that **5 live SessionDB handles** exist on the same state.db, each with its own writer connection, and says a long-lived process should share one handle per path;
- **13 post_tool_call hook callbacks** are skipped because a previous callback timed out or is still running;
- four goal-judge auxiliary calls fail (three overload, one connection error) and fall through to continue;
- no new ws write slow / loop stalled warning is present in this added interval;
- no new session_persistence_failed record is present.

These local observations are relevant to Hermes robustness but are not counted in the 24 main-loop provider attempts.

## Compression

Four compression starts are observed after restart.

For b2c101:

1. 09:29:00 — 457 messages / ~169,489 rough tokens → 310 messages / ~90,107; committed in **196.296 s**.
2. 09:56:40 — 460 messages / ~190,553 → 460 messages / ~122,318; committed in **0.532 s**.
3. 10:20:31 — 549 messages / ~170,102 → 549 messages / ~141,897; committed in **1.015 s**.

For f947d8, compression begins at **10:19:28** with 402 messages / ~174,024 rough tokens; completion is outside the uploaded boundary.

Message-count equality in the two short b2c101 commits is recorded as an observation only. These logs alone do not establish whether compression changed message contents in place, pruned hidden payload, or only updated estimation/state.

## Interpretation

The new morning evidence changes the shape of the latest interval but not the incident status:

- the provider route is **not continuously unavailable**;
- it is also **not cleanly recovered**;
- 24 new Sol failures occur amid 385 successful main-loop calls;
- one new provider request ID is available for correlation;
- no new terminal 5/5 user-visible failure is recorded before the file boundary;
- a separate unclean prior gateway life and repeated current-process Hermes warnings remain local robustness concerns.

A successful call, a completed turn, or a lower rolling-window count is still insufficient to close the incident. The recovery criterion remains sustained useful work without repeated provider rejection and without losing/resuming state incorrectly across local failures.

## Requested action

**Provider:** correlate **4c95dbeb-7407-4985-a35f-61ddefa22651** and the surrounding 18 September overload events with serving/routing telemetry, and clarify whether these rejections are capacity, account-level admission, or another backend condition.

**Hermes:** investigate the prior unclean gateway life, repeated five-handle state.db warning, skipped post-tool hooks, and checkpoint/resume behavior. These are separate from provider causation.

Machine-readable receipt: [log-extension-20260918T072057Z.json](../evidence/log-extension-20260918T072057Z.json).
