# Morning recurrence — 16 September 2026, through 09:06:01 UTC+3

[Home](../README.md) · [Русская сводка](README.ru.md) · [Issue #1](https://github.com/gv1983us-commits/codex-provider-incident/issues/1) · [Curated evidence](../evidence/log-extension-20260916T060601Z.json) · [Previous log boundary](LOG-EXTENSION-2026-09-15-1016.md)

**OPEN. The three additional logs contain 99 new failed Codex attempts, all Astra, and nine GUI turns ending in error. Both parallel sessions exhausted five-attempt chains around 09:04 UTC+3. The owner continues to report disruption and requests joint investigation and repair by Hermes maintainers and the provider.**

The morning recurrence is observed. An exact daily schedule or common internal root cause is not established.

## Scope and counting

- Added interval: strictly after **15 September 10:16:49.139 → 16 September 09:06:01.535 UTC+3**, or **07:16:49.139Z → 06:06:01.535Z** on the corresponding dates: **22 h 49 min 12.396 s**.
- Current rolling 48 hours: **14 September 09:06:01.535 → 16 September 09:06:01.535 UTC+3**.
- Fixed cumulative baseline: **10 September 16:56:23Z**, original commit `4a398261`.
- Previous publication: `9a768a49`, **15 September 07:42:20Z**. **78** of the 99 newly audited attempts occurred after that publication time; 99 is the extension beyond the previous log boundary.
- All three new files are exact byte extensions of their `(1).log` predecessors. Failed-attempt identities were compared against the prior audit; none of its 178 Codex records is missing.

Counted unit: a failed main-loop API attempt, including retries. Mirrored errors/agent records count once. Desktop echoes, retry scheduling, exhausted-chain messages, low-level transport diagnostics and GUI statuses are not additional API attempts.

No new agent or gateway log was supplied. The retained `agent(1).log` ends at the previous boundary, so a corresponding new successful-API-call denominator is unavailable. No failure percentage or provider-downtime duration is inferred.

## Updated error table

| Codex error | Previous cumulative | Added interval | Current cumulative | Rolling 48 h |
| --- | ---: | ---: | ---: | ---: |
| Overloaded | 130 | 72 | 202 | 195 |
| Request-processing error with request ID | 32 | 22 | 54 | 51 |
| Main-loop connection / timeout | 16 | 2 | 18 | 6 |
| No first parsed event within 120 s | 0 | 1 | 1 | 1 |
| HTTP 503: upstream connection failure | 0 | 2 | 2 | 2 |
| **Failed API attempts** | **178** | **99** | **277** | **255** |
| GUI turns ending in Codex error | 13 | 9 | 22 | 20 |

Rolling reconciliation: **157 + 99 − 1 = 255**. One earlier request-processing error, **13 September 22:23:01.438 UTC+3**, left the window. Rolling model counts are **200 Astra / 55 Sol**.

The added attempts belong to `20260913_181010_f947d8` (**59**) and `20260914_183547_b2c101` (**40**). Every added attempt names `openai-codex / gpt-6-astra-900k` and a desktop run thread; none is a background review.

## Does the morning time repeat?

Times below use **UTC+3**. “First” means first matching logged failure **at or after 08:00**, not the onset of all problems that day. The last column uses the same **08:00:00.000–09:06:01.535** window on each date.

| Date | First overload or processing error after 08:00 | First overload after 08:00 | Failed attempts in common window |
| --- | --- | --- | ---: |
| 2026-09-14 | 09:20:10.116 | 09:20:10.116 | 0 |
| 2026-09-15 | 08:14:52.281 | 08:17:07.006 | 19 |
| 2026-09-16 | 08:32:13.533 | 08:33:09.159 | 45 |

On 16 September, the main morning overload/processing sequence begins **17 min 21.252 s later** than on 15 September. The first overload is **16 min 2.153 s later**. This supports a recurrence in a similar broad morning period, rather than an identical clock trigger.

The common window contains **19 attempts yesterday** (11 overload, 8 processing) and **45 today** (32 overload, 12 processing, 1 HTTP 503). These are attempt counts, not comparative failure rates: workload is uncontrolled, successful-call coverage is missing and the retry budget changed.

The issue is not morning-only. The added files also contain **three connection failures on 15 September at 22:34–22:53**, **three processing failures on 16 September at 02:08–02:13**, and a **07:10 first-event timeout**. On 14 September the first matching morning failure was at **09:20:10**. Three dates cannot establish daily provider maintenance, routing changes, account restrictions or a recurring capacity schedule.

Sources for the 15/16 September comparison: `errors(2).log:9229-9234` and `10600-10606`; all common-window records and hourly counts are retained in the JSON ledger. An empty 14 September common window means no matching recorded failure, not independently established availability.

## Five attempts are already observed — and both sessions still fail

The first failed-attempt record with denominator **5** is **15 September 22:34:08.487 UTC+3**, `errors(2).log:10234`. Of the 99 added records, **47 use /3 and 52 use /5**. This bounds observation of the change; it does not identify the exact configuration-change time or actor.

On **16 September 08:57:53.576**, session `b2c101` records overload. At **08:57:53.696**, session `f947d8` records a processing error with request ID `1e08862f-34f9-4736-b107-9c2f337ba316`. The logged failures are **120 ms apart** (`errors(2).log:10654-10657`).

Later, two separate five-attempt chains terminate:

| Session suffix | Final chain: failed attempt 1/5 | Failed attempt 5/5 | GUI error | Rough context at exhaustion |
| --- | --- | --- | --- | --- |
| `b2c101` | 09:03:09.659 | **09:04:06.242** | **09:04:06.702** | 28 messages / ~34,882 tokens |
| `f947d8` | 09:03:07.531 | **09:04:07.585** | **09:04:08.234** | 134 messages / ~63,014 tokens |

The fifth-attempt failures are **1.343 s apart**, and GUI errors **1.532 s apart**. Sources: `errors(2).log:10674-10693`, `gui(2).log:2814-2815`. Desktop echoes also show exhaustion at **06:04:06.323Z** and **06:04:07.703Z**, `desktop(2).log:3442` and `3551`; they are not recounted.

The logger says “after 5 retries”; the actual attempt markers in these two chains are **1/5 through 5/5**. This is five attempted calls, not evidence of five retries after an additional initial attempt.

The last supplied series is still unresolved: session `f947d8` reaches **attempt 4/5 at 09:06:01.534**, with another retry scheduled at **09:06:01.535** (`errors(2).log:10694-10701`). Its fifth outcome is outside the supplied files.

Thus, increasing the budget to five did **not prevent these two final failures**. The logs do not provide a controlled estimate of how much it helped other calls.

## New transport evidence

At **07:10:48.224 UTC+3**, Hermes reports that a connection was accepted but no parsed stream event arrived before the **120 s TTFB cutoff**. Its abort warning says **no sockets found; the in-flight request may keep running**. At **07:10:50.257**, the main loop records the TimeoutError (`errors(2).log:10533-10535`). These are three diagnostic stages of the same observed timeout, not three counted attempts.

There are **two explicit HTTP 503 upstream connection failures**: **15 September 22:34:08.487 UTC+3** reports **connection timeout** (`errors(2).log:10234`); **16 September 08:24:39.838 UTC+3 / 05:24:39.838Z** reports **connection refused**:

```text
HTTP 503: upstream connect error or disconnect/reset before headers. retried and the latest reset reason: remote connection failure, transport failure reason: delayed connect error: Connection refused
```

Source of the second 503: `errors(2).log:10600`. Both 503 records are kept separate from generic overload. This supplies a transport-level error response; it does not locate the failing upstream hop or prove the root cause of the other errors.

## Hermes continuation and UI status need separate investigation

Three new events hit `Redirected-message restart limit (5) exceeded` and then end with `reason=redirect_restart_limit_exceeded`, `last_msg_role=tool` and `response_len=0`. Each corresponding GUI record says `status=complete error_retained=False`.

| 16 September, UTC+3 | Pending-tool ending | GUI complete | Source pair |
| --- | --- | --- | --- |
| 07:46 | 07:46:36.816 | 07:46:36.892 | `errors(2).log:10560-10561` / `gui(2).log:2763` |
| 08:16 | 08:16:28.709 | 08:16:28.793 | `errors(2).log:10589-10590` / `gui(2).log:2785` |
| 08:38 | 08:38:10.286 | 08:38:10.373 | `errors(2).log:10613-10614` / `gui(2).log:2804` |

This **redirect/restart limit of 5 is a different counter from the API attempt budget of 5**. The correlated records establish inconsistent completion signalling for these tool-ending turns. They do not by themselves establish loss of results or automatic re-execution of tools.

In the added GUI interval there are **165 complete, 9 error and 2 interrupted** end records. The three correlations above explain why “complete” cannot simply be treated as a successful finished model response. Turn durations include prior work and tools, not just failed requests.

## Nine added GUI error turns

| Date and time, UTC+3 | Session | Final reason | Whole turn, seconds | Source |
| --- | --- | --- | ---: | --- |
| 2026-09-15 10:30:37.106 | `20260913_181010_f947d8` | overloaded | 429.5 | `gui(2).log:1920` |
| 2026-09-15 10:35:55.894 | `20260913_181010_f947d8` | unknown | 110.6 | `gui(2).log:1926` |
| 2026-09-15 10:44:58.958 | `20260913_181010_f947d8` | overloaded | 22.8 | `gui(2).log:1940` |
| 2026-09-15 10:50:07.989 | `20260914_183547_b2c101` | overloaded | 43.8 | `gui(2).log:1949` |
| 2026-09-15 12:41:59.003 | `20260913_181010_f947d8` | overloaded | 18.3 | `gui(2).log:1962` |
| 2026-09-15 13:10:36.676 | `20260913_181010_f947d8` | overloaded | 99.4 | `gui(2).log:1990` |
| 2026-09-15 13:22:27.454 | `20260913_181010_f947d8` | overloaded | 21 | `gui(2).log:2000` |
| 2026-09-16 09:04:06.702 | `20260914_183547_b2c101` | overloaded | 427 | `gui(2).log:2814` |
| 2026-09-16 09:04:08.234 | `20260913_181010_f947d8` | overloaded | 66 | `gui(2).log:2815` |

These correspond to nine exhausted chains: **seven at budget 3 on 15 September, two at budget 5 on 16 September**. They overlap the attempted-call count; they are not nine additional API errors.

## Related diagnostics, outside the 99-attempt count

- **21** low-level Codex request-failure diagnostics; some may overlap cancellation or main-loop requests.
- **Two** failed context-summary generations: overload at **15 September 10:24:23.601**, processing error at **16 September 09:01:47.101**, request ID `67061de9-0e6f-401e-bf4c-200b6ad5f787`.
- **One** smart-approval overload at **15 September 10:25:13.318**.
- **Two** goal-judge unavailable/fallback-exhausted paths at **15 September 18:06:55.072–313**, represented by four adjacent warnings.
- **368** “Recovered dangling side-effecting tool call(s) as UNKNOWN” warning records from **15 September 16:14:37.099** through **16 September 03:43:56.883**. Repeated warnings are not a count of unique lost or re-executed operations.
- **21** WebSocket warning records and **10** detached-runtime RPC rejections. Multiple messages can describe one disconnect; these are not 31 proven independent outages.

Private task text and raw tool outputs are omitted. None of these observations is automatically attributed to the provider.

## Provenance

| New file | Physical lines | Added lines | SHA-256 |
| --- | ---: | ---: | --- |
| `errors(2).log` | 10,701 | +1,186 | `47202885a28b298906fdf83e1037f02c635c4dba31b8f28dcd93489bc076e63a` |
| `desktop(2).log` | 3,554 | +679 | `49aa440749b6625f01c2c52f762bac52a9257fb0858e2c5b012b9f981b093014` |
| `gui(2).log` | 2,816 | +912 | `ba5bbdcc2b781c80f21bb5d3cedba5fdfd67e7f17d1fea522b6a6c796ce681a6` |

The evidence JSON carries exact byte-prefix checks, previous hashes, UTC coverage, all 99 new attempts and request IDs, GUI/exhaustion records, selected diagnostics and the morning comparison. Python log times follow the established **UTC+3** conversion; desktop uses explicit **Z** timestamps. Buffered desktop text is not used to date the onset of failures.

## Requested upstream action

**Provider:** correlate the supplied request IDs, especially the processing errors around **05:57:53Z–06:04:08Z**, the **05:24:39Z HTTP 503** and the **04:10:48Z no-first-event timeout**. Identify which service or hop returned the observed failures and supply remediation status.

**Hermes maintainers:** investigate continuation after five-attempt exhaustion, TTFB cancellation reporting `tcp_force_closed=0`, pending-tool turns marked complete, and repeated UNKNOWN tool recovery. Preserve already completed effects and expose incomplete state accurately.

**Joint acceptance:** sustained useful work on the affected setup, with duration, successful-call coverage and residual failures reported. A few successful calls or a higher retry budget do not establish closure.

Earlier browser/account observations remain owner reports; this upload adds no Edge/Yandex browser trace, hidden ChatGPT retry telemetry or per-request subscription-tier evidence. No runtime changes or live tests were performed for this audit.
