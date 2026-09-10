# Pro Light also returns Sol overload

[Home](../README.md) · [Exact receipt and provenance](../evidence/light-sol-20260910T152154745Z.json) · [Earlier conservation](CONSERVATION-2026-09-10.md)

**Latest observation: after the reported completion of conservation using Sol on Light, the owner supplied an `overloaded` receipt for `gpt-5.6-sol-900k` at 15:21:54.745 UTC and explicitly identified the affected account as Pro Light.** The observed Hermes failure set now includes both Pro Full and Pro Light. No sustained workaround has been demonstrated.

## Subsequent owner narrative

The owner subsequently reports approximately five failures with shrinking usable intervals: the Builder pass was longest, later work in another session failed, a second failure followed roughly ten minutes after the first, and the last failed immediately on invocation. Hermes/Codex is currently unusable for the owner. This is an approximate total, not five additional audited receipts; per-event timestamps, codes and models were not supplied. No loss of conservation is established. [Community comparison and evidence limits](COMMUNITY-FOLLOWUP-2026-09-10.md).

## Exact supplied receipt

```text
── Hermes error details ──
time: 2026-09-10T15:21:54.745Z
layer: provider
code: overloaded
retryable: true
provider: openai-codex
model: gpt-5.6-sol-900k
error: Our servers are currently overloaded. Please try again later.
```

Time in GMT+3: **18:21:54.745 on 10 September 2026**. Account attribution comes from the owner's accompanying message; the receipt names the provider and model but not an account ID or tier.

## Sequence and interpretation

| Stage | Observation | Current interpretation |
| --- | --- | --- |
| Full account | Repeated failures, including brief Astra success and later failure | Original affected account remains unresolved |
| Switch to Light in the same Builder session | Owner reports Sol completed conservation to `QUIESCED_SAFE` | A completed preservation stage is reported; retain this successful interval |
| Astra banner displayed at 15:11:27.300 UTC | Same request ID and error text as the earlier Astra receipt; owner says it was old | No additional failure counted from that redisplay |
| Sol receipt at 15:21:54.745 UTC | `gpt-5.6-sol-900k / overloaded`; owner identifies Light | Light also has a reported Codex failure; it is not a reliable fallback |

The new receipt has a different model, code and message from the old Astra banner. Do not merge it into that stale-banner observation. It does not contain an upstream request ID, HTTP status, current quota, failed-call role, session ID or retry count. These fields remain unknown.

The earlier same-session account switch remains a valid owner-reported observation for its time. It no longer supports describing Light as consistently available or the entire observed incident as restricted to Full. The evidence does not determine whether both accounts share a cause or how account state, request routing, time and workload interact.

## Preserve the reported checkpoint

The conservation report is not revoked by a later provider error. This receipt supplies no evidence of lost storage, changed checkpoints, a failed shutdown or a promoted release. Keep the reported `QUIESCED_SAFE` state and retained handoff as the resumption point. No infrastructure was restarted or live recovery test run by this update.

If automatic requests or retries are still active, pause them at the preserved state; do not create additional load merely to repeat a known failure. The displayed `retryable: true` is a classification, not a requirement to retry or a guarantee of success. Historical audit counters remain unchanged.

## По-русски

**Теперь отказ зафиксирован и на Light:** по сообщению владельца, после завершённой консервации Sol вернул `overloaded` в 18:21:54.745 по GMT+3. Это отдельная запись от старой плашки Astra. Light дала окно для сохранения работы, но устойчивым обходом не стала.

Ранее переданный отчёт `QUIESCED_SAFE` сохраняется. Новая квитанция не сообщает об утрате диска или результатов. Причина отказов обеих учёток и текущий недельный остаток из неё неизвестны.
