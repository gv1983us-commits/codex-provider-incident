# Continuing Codex / Hermes incident — 15 September 2026

[Home](../README.md) · [Русская сводка](README.ru.md) · [Tracking issue #1](https://github.com/gv1983us-commits/codex-provider-incident/issues/1) · [Audited data](../evidence/log-audit-20260915.json)

**Status: OPEN. Normal work remains disrupted; the owner has found no working fix on their side and requests joint investigation and repair by Hermes maintainers and the provider.**

Owner statement recorded on 15 September, translated into English:

> The incident is ongoing. I still cannot work normally. I have not found a working fix on my side. I need Hermes maintainers and the provider to work together to investigate and fix this.

The earlier [resolution comment](https://github.com/gv1983us-commits/codex-provider-incident/issues/1#issuecomment-5638803159) was followed by a [recurrence and reopening on 12 September](https://github.com/gv1983us-commits/codex-provider-incident/issues/1#issuecomment-5643482050). This update continues that same incident. Prior successful runs remain valid observations; the conclusion of sustained recovery is superseded by the recurrence and the current owner report.

## Observation windows

- **After the last repository commit:** strictly after `2026-09-10T16:56:23Z` ([`4a398261`](https://github.com/gv1983us-commits/codex-provider-incident/commit/4a39826129fd02085ada5eb2f33d727022c0b3bf)) through `2026-09-15T05:32:53.867Z`.
- **Last 48 hours of the supplied logs:** `2026-09-13T05:32:53.867Z` through `2026-09-15T05:32:53.867Z`, inclusive. In UTC+3: **13 September 08:32:53.867 → 15 September 08:32:53.867**.
- These are log windows, not the timestamp of the owner's later status statement. The 118 after-commit attempts include 11–12 September events already recorded in issue comments; they must not be added wholesale to earlier counters.

## Audited findings

| Observation | Last 48 hours | After Git commit | Unit / scope |
| --- | ---: | ---: | --- |
| Codex `overloaded` | 78 | 82 | Failed API attempts, including retries |
| Codex request-processing error + request ID | 18 | 20 | Separate analytical bucket; not relabelled overload |
| Codex connection / timeout | 4 | 16 | Call-level failures; network cause not established |
| **All failed Codex API attempts** | **100** | **118** | Deduplicated conversation-loop attempt records |
| Codex turns ending with GUI `status=error` | 3 | 5 | Subset of the attempt ledger; not extra attempts |
| Other providers | 5 | 5 | 3 Yandex HTTP 500; 1 OpenCode unavailable model; 1 custom/Yandex unsupported `xhigh` |
| Context summary generation failed | 7 | 7 | 3 overload, 2 stream stalls, 1 connection error, 1 other-route HTTP 400 |
| Compression reached 600-second ceiling | 1 | 1 | Separate unfinished compression operation |
| Smart approval call failed | 9 | 9 | 6 overload and 3 connection errors; not attributed to a provider without a route field |
| Goal judge call failed | 1 | 1 | Service call; main turn still recorded complete |
| Codex transport diagnostics | 53 | 78 | May overlap failures or cancelled requests; not extra API attempts |
| Redirect restart limit reached | 17 | 36 | Hermes turn termination; 15 of the 17 recent cases have an adjacent pending-tool-result record |
| Agent output not written to history | 1 | 1 | `history_version` mismatch |
| WebSocket message exceeded size limit | 1 | 1 | Code 1009 |
| WhatsApp bridge exited | 1 | 1 | Separate adapter failure, code 1 |
| Gateway stop interrupted active work | 3 | 3 | Planned stops with zero-second drain and an active agent |
| Terminal 420-second timeout | 2 | 2 | Tool-level timeout |
| Tools returned error | 664 | 804 | Log records, including failed commands and file operations; not 664/804 established product bugs |

The 100 recent Codex attempts comprise **58 Sol and 42 Astra**; call roles are **82 desktop/run, 13 gateway and 5 background-review**. Repeated `Retrying API call`, terminal error summaries and exact mirrors across logs are not counted again. The diagnostic rows can describe the same operation as an API failure and have no additive grand total. The 1,013-row diagnostic index and all 118 Codex attempt records are in the companion JSON.

## Three final overload failures on 14 September

All times below are **UTC+3**. Duration is the entire turn, including work and waiting, not measured provider downtime. Attempt counts can exceed three because a turn contains more than one API call/retry series.

| GUI error time | Logged model | Whole turn, seconds | Failed attempts during turn | Retained source references |
| --- | --- | ---: | ---: | --- |
| 15:34:43.233 | `gpt-6-astra-900k` | 419.0 | 6 | `gui.log:1289; errors.log:8223-8233` |
| 15:38:50.239 | `gpt-5.6-sol-900k` | 260.4 | 5 | `gui.log:1291; errors.log:8238-8247` |
| 18:02:56.428 | `gpt-5.6-sol-900k` | 318.3 | 3 | `gui.log:1313; errors.log:8519-8523` |

Later `complete` records exist in the same sessions: Astra at **16:14:27.513** (`gui.log:1297`), Sol at **15:47:36.919** and **18:15:47.155** (`gui.log:1293,1315`). These document continuation and coexist with the owner's inability to work reliably.

## Failures continue at the export boundary on 15 September

Between **08:14:52.281 and 08:32:32.623 UTC+3**, two Astra sessions log **11 more failed attempts: 3 overloads and 8 request-processing errors**.

| Time UTC+3 | Session | Error category | Attempt | Request ID | Retained source references |
| --- | --- | --- | --- | --- | --- |
| 08:14:52.281 | `20260914_183547_b2c101` | unknown + request ID | 1/3 | `32b606eb-dd97-41de-9f3b-20ef5616f83e` | `errors.log:9229; agent.log:11331` |
| 08:16:31.053 | `20260914_183547_b2c101` | unknown + request ID | 1/3 | `ecd702bb-05c0-4352-b28d-a9c301a4a03e` | `errors.log:9232; agent.log:11353` |
| 08:17:07.006 | `20260914_183547_b2c101` | overloaded | 2/3 | `not supplied` | `errors.log:9234; agent.log:11365` |
| 08:22:28.120 | `20260914_183547_b2c101` | unknown + request ID | 1/3 | `c70d5f24-7817-4b32-87df-01591c605763` | `errors.log:9249; agent.log:11422` |
| 08:22:52.344 | `20260913_181010_f947d8` | overloaded | 1/3 | `not supplied` | `errors.log:9251; agent.log:11430` |
| 08:24:41.513 | `20260914_183547_b2c101` | unknown + request ID | 1/3 | `b1a21246-d00a-4023-9c4a-94400c7c8d7d` | `errors.log:9253; agent.log:11451` |
| 08:26:01.236 | `20260913_181010_f947d8` | unknown + request ID | 1/3 | `0d226ad5-6050-44ca-b90b-99fae281b542` | `errors.log:9255; agent.log:11481` |
| 08:26:39.337 | `20260914_183547_b2c101` | unknown + request ID | 1/3 | `6bde6763-0ed0-4313-a759-607b6f1342d4` | `errors.log:9257; agent.log:11487` |
| 08:28:03.684 | `20260914_183547_b2c101` | overloaded | 1/3 | `not supplied` | `errors.log:9259; agent.log:11508` |
| 08:31:14.869 | `20260914_183547_b2c101` | unknown + request ID | 1/3 | `d15b378d-9d6b-4707-aacf-f0d9552cf89d` | `errors.log:9263; agent.log:11537` |
| 08:32:32.623 | `20260914_183547_b2c101` | unknown + request ID | 2/3 | `0bb2ceb2-a4f1-4243-8adf-73466df9ffa2` | `errors.log:9265; agent.log:11545` |

The last `b2c101` series reaches attempt 2/3 at **08:32:32.623** and schedules another retry (`errors.log:9265–9266`; client creation at `agent.log:11547`). Its outcome is outside the supplied log boundary. A successful Astra call in the other session at **08:32:53.867** (`agent.log:11548`) confirms that successful calls and unresolved failures coexist; this is not a claim of continuous zero availability.

## Client-side evidence requiring Hermes investigation

- **14 September 20:59:06.071 UTC+3:** `history_version mismatch (expected=71 current=72) — agent output NOT written to session history` (`desktop.log:2414`). This establishes one rejected answer write, not deletion of the entire saved history.
- **14 September 20:38:21.049 UTC+3:** WebSocket code **1009**; a **4,079,266-byte** frame exceeds the **1,048,576-byte** limit (`agent.log:3083`; `gui.log:1447`).
- **Eight unfinished compression operations:** seven summary-generation failures plus one 600-second ceiling. The ceiling message explicitly says no messages were dropped (`desktop.log:1774`). These current events do not independently reproduce the older 850→82 / 328→102 history transition.
- **14 September 10:23:24.964 UTC+3:** WhatsApp bridge exits with code 1 (`gateway.log:833–835`). It is a separate adapter event, not a Codex failure.

## Action requested

| Requested party | Required work | Evidence / acceptance |
| --- | --- | --- |
| Provider | Correlate the supplied request IDs, timestamps and models; identify and remediate repeated overload/request-processing failures and investigate connection failures on the Codex route. | A concrete finding and remediation status tied to the supplied receipts. |
| Hermes maintainers | Investigate retry exhaustion, redirect termination, failed compression/approval, history-version rejection and the WebSocket limit; preserve completed tool results and make interrupted state explicit. | A supported upstream fix or documented handling, with safe continuation and no uncontrolled replay of completed tools. |
| Hermes + provider together | Trace the failing request → retry → tool/result → continuation path across both layers and agree on a supported fix. | Sustained useful work on the affected setup, with observation duration and remaining failures reported; successful one-shot calls alone do not close the incident. |

The owner supplies the existing evidence and reports practical usability. A working owner-side workaround has not been found; repeated local reconfiguration, another paid test or an owner-maintained Hermes source patch is not the remediation being requested. This update runs no live model tests and changes no Hermes runtime or configuration.

## Provenance and limits

The supplied `errors.log`, `agent.log`, `gui.log`, `desktop.log` and `gateway.log` were read in full. Original byte counts, physical line counts, SHA-256 values and timestamp coverage are published in [the data file](../evidence/log-audit-20260915.json). Source references point into those retained raw files, not into this repository. Desktop timestamps contain UTC `Z`; UTC+3 for the other logs is corroborated by matching events, including the summary failure in `errors.log:5325` / `desktop.log:1761` and gateway lifecycle timestamps.

This continues the [new clean-install scope and parallel-session evidence](https://github.com/gv1983us-commits/codex-provider-incident/issues/1#issuecomment-5643853792) and [complete retained-session inventory](https://github.com/gv1983us-commits/codex-provider-incident/issues/1#issuecomment-5644010163) already recorded on 12 September. The deleted previous installation is not reintroduced as a cause. The current logs do not pin the exact installed Hermes revision or all effective settings at each failure.

New records retain the logged provider/model and leave per-request Full/Light attribution and reasoning effort unknown. The detailed `agent.log` begins only on 14 September at 18:18:42.598 UTC+3, so this corpus does not support a whole-window success percentage, a downtime total or quota-loss accounting. The internal provider mechanism and the causal relationship between the different client symptoms remain open for the requested joint investigation.

Prepared with AI assistance from the owner's supplied logs and statement; selected public excerpts and an auditable event index accompany the derived counts.
