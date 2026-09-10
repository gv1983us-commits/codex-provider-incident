# Incident record

Source: [owner's Hermes issue](https://github.com/NousResearch/hermes-agent/issues/107307) and its [focused evidence reply](https://github.com/NousResearch/hermes-agent/issues/107307#issuecomment-5617927854). Snapshot: 10 September 2026.

## Latest confirmed update — 10 September 2026: GPT-5.5 also returns Codex overload

A new Hermes session with GPT-5.5 Ultra selected first appeared to make very slow progress, then displayed an overload banner. The owner subsequently supplied this **exact Copy details receipt**:

```text
── Hermes error details ──
time: 2026-09-10T12:06:26.863Z
layer: provider
code: overloaded
retryable: true
provider: openai-codex
model: gpt-5.5
error: Our servers are currently overloaded. Please try again later.
```

[Recorded receipt and context](https://github.com/gv1983us-commits/codex-provider-incident/issues/1#issuecomment-5618436481). The timestamp converts to **15:06:26.863 Moscow (UTC+3)**.

**The provider and model are now explicit: `openai-codex` / `gpt-5.5`.** The earlier screenshot only showed the selected session model; this receipt resolves that attribution gap. The session showed initial activity, so a brief start must not be treated as sustained recovery.

**No reliable Hermes workaround has been demonstrated.** GPT-5.5 joins the previously affected model routes on the Full account. Earlier GPT-5.5 success in official Work and reported very slow GPT-5.5 Max operation there remain separate observations; this receipt establishes a Hermes failure, not a new browser failure.

The receipt does not supply an HTTP status, endpoint, request ID, call role or retry-attempt count. `retryable: true` means the error is classified as retryable; a successful retry is not guaranteed. The cause of the slowness and the exact upstream root cause remain unknown.

Historical log counts are unchanged: this is a new owner-supplied receipt outside the original eight-log audit. Earlier provisional observations below are retained as history.

## Earlier update — 10 September 2026: GPT-5.5 works in official Work

A new browser test narrows the original report:

| Same Pro Full 20× account | Result |
| --- | --- |
| Hermes → Codex: Astra, Sol, Terra | Fail with overload |
| Official ChatGPT Work/Codex: GPT-6 and GPT-5.6 | Fail with overload |
| Official ChatGPT Work/Codex in Edge: GPT-5.5 | **A response was received** |
| Hermes → Codex: GPT-5.5 | **Not yet verified; being tested by the account owner** |

This is therefore a model-dependent failure on the affected account, not evidence that all Codex execution on that account is unavailable. One successful GPT-5.5 browser response does not yet establish sustained tool-using operation or recovery in Hermes.

The immediate workaround candidate is the existing `openai-codex` subscription route with `gpt-5.5`. The relevant recovery question is whether the main agent, explicitly pinned workers and auxiliary tasks (especially compression) can all use that working route without falling back to the affected models.

The earlier observations and log counts below remain historical evidence; this update refines their interpretation.

---

## Summary

On **10 September 2026**, my higher-tier ChatGPT Pro 20× account (“Pro Full”) repeatedly fails with Codex overload errors in Hermes and in the official ChatGPT Work interface. Sol and Astra are affected. My other account, Pro Light, continues to work in Work on the same computer, browser, home network and VPN configuration.

**The browser reproduction persists with Hermes completely shut down.** This report does not attribute the upstream rejection to Hermes. I am reporting the cross-client incident to other Codex-provider users, together with the observed consequences for long-running Hermes tasks.

## Reproduction outside Hermes

1. Shut down Hermes.
2. In official ChatGPT Work on Microsoft Edge, select the Full account and ask Sol a short question: capacity/overload banner.
3. Open the same Work conversation on the Full account in Yandex Browser: the failure persists. Astra also fails on Full.
4. Use ChatGPT's built-in account switcher in Yandex Browser: Work on Pro Light works; Work on Pro Full fails.
5. Ordinary ChatGPT conversation mode on Full still works.

All of this was done on the same Windows laptop and connection. The website's Russian banner says: “Выбранная модель сейчас недоступна из-за высокой нагрузки. Попробуйте выбрать другую модель.” In English: the selected model is currently unavailable due to high load; try another model. The failed attempts end with this banner; it is replaced by the next request.

The affected account showed **41% weekly allowance remaining** in a credential-pool check at **10:33 Moscow time (UTC+3)** on 10 September. The official account interface also showed remaining allowance and two available resets. This is a recorded snapshot, not a claim that every possible model-specific limit has been ruled out.

## Audited Hermes observations

The evidence is an audit of eight supplied logs covering 8–10 September, plus a saved session export of **21,492,423 bytes / 4,400 messages**. Counts below remove duplicate log records using timestamp, logger and message. Raw private sessions and credentials are not attached.

- Provider: `openai-codex`; logged endpoint: `https://chatgpt.com/backend-api/codex`.
- Main-loop failures: **151 overloaded attempts** across eight session IDs; 102 logged as `gpt-5.6-sol-900k`, 49 as `gpt-6-astra-900k`.
- Eleven additional overloaded failures occurred in auxiliary calls, including two failures to generate a context summary.
- Twenty separate request-processing errors, seven `429 Rate limit exceeded` events, two 120-second no-event timeouts and one unsupported-model 400 were counted separately. These are not relabelled as overload.
- The seven 429 events are concentrated around 00:59–01:01 on 9 September. The audited Codex records contain no explicit `usage_limit_reached`, `insufficient_quota`, or HTTP 401/403.
- Overload retries are bounded: **three attempts total**, with approximately 2–3 seconds and 4–6 seconds of retry delay. The issue is not an observed infinite rotation-to-self loop.
- Failures continued after restart and compression, at client-estimated contexts of approximately **47k, 68k and 99k**. A successful call also recorded an actual input count of 67,785 tokens.

There were **1,987 successful Codex calls** in the detailed log window, so the historical incident was intermittent. Successful individual responses did not establish durable recovery.

### One exact interruption sequence

On 10 September, Moscow time, reconstructed from the saved logs:

| Time | Event |
| --- | --- |
| 10:40:37.943 | Successful Sol response, input 67,785 / output 377 tokens |
| 10:40:38–10:40:44 | Eight `skill_view` tool calls execute |
| 10:40:48.018 | Next model attempt fails with overloaded |
| 10:40:55.063 | Second attempt fails with overloaded |
| 10:41:04.100 | Third attempt fails with overloaded |
| 10:41:04.107 | Retry budget exhausted |
| 10:41:04.688 | GUI ends the turn with an error |

The task loses its next model step after tools have already run.

### Context transfer during the same incident

At 10:34:37, generation of a new context summary failed with overload. Desktop recorded `Inserted a fallback context marker`, but the active message history was still committed from **850 to 82 messages**. A second session later changed **328 to 102** after another summary failure. Telemetry reported `commit_status=committed` and `fallback_used=false`.

The export retains prior summaries/anchors and archived content; **this does not prove deletion of the full archive**. It does establish that the active history was shortened without the new LLM summary. The apparent disagreement between the fallback marker and telemetry needs interpretation by someone familiar with those layers.

## Scope and questions

This is a customized Hermes installation used for long-running agent work; the exact installed upstream commit is not pinned in this report. These are observations from that installation, not a clean-stock reproduction of every recovery symptom.

The work required repeated manual intervention. Continuity had previously been configured; after provider failures, task/process bookkeeping and continuation became unreliable. The first exact state transition responsible for that degradation has not been established. I am not claiming every provider failure killed every worker.

Related third-party reports describe a similar 20× account / official-web capacity pattern: https://github.com/Wei-Shaw/sub2api/issues/6739.

Is there a known current Codex incident or a supported diagnostic/recovery path for this **Full-fails / Light-works, official Work-also-fails** pattern? Separately, what is the supported way to preserve and resume a task after the overload retry budget expires, without replaying already completed tools?

I am not asserting an anti-abuse action, a shared physical server pool, or a particular server-side root cause. This English report was prepared with AI assistance from my observations and saved audits.

## Evidence boundary and continuity correction

The account owner reports that continuous execution had already been configured and running before the first Provider Error. Inconsistent task/process coordination and premature final responses followed that failure. Subsequent turns ending as `complete` do not establish why previously configured continuity broke. The exact first state transition has not been established by a before/after comparison.

The detailed success log begins on 9 September at approximately 15:56, whereas some earlier logs contain errors only. The 1,987 successes and the 151 overload failures must not be combined into an overall failure rate for 8–10 September.

The original comparison used an existing Work conversation. A fresh short-prompt Work conversation on the affected account remains a useful additional control for GPT-5.6/GPT-6. The latest update above records a confirmed openai-codex / gpt-5.5 overload receipt after initial activity in a new Hermes session. Reliable recovery remains unverified.

Public materials here are a curated subset of the retained audits, not the eight raw logs or the full session export. No source-file checksum has been independently added by this repository. Source line references refer to the retained files, not line numbers in this repository.
