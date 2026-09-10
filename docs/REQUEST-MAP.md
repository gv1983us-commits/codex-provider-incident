# From Hermes controls to Codex requests

[Home](../README.md) · [Observed session](SESSION-2026-09-10.md) · [Recovery procedure](RECOVERY.md)

**Purpose:** choose a useful next recovery probe from the controls already tested. The target remains sustained Jarvis work on the existing Full subscription.

## Evidence boundary

The screenshot's short build label `9fd44b4` resolves to public Hermes commit **[`9fd44b4dfc44138b9e5d5689acb56c438364ff7b`](https://github.com/NousResearch/hermes-agent/commit/9fd44b4dfc44138b9e5d5689acb56c438364ff7b)**, dated 8 September 2026. The implementation findings below use that pinned snapshot. This is a matching reference revision, not verification that the installed runtime has no local changes, user provider plugins or request overrides.

The session export captures saved configuration and tool results, not the final outgoing HTTP body. Separate three things: **selected UI value → implementation transformation → observed request**. Only the first two are currently available for these settings.

## Which controls change which fields?

| Selected setting | Standard transformation in the reference snapshot | Consequence for the investigation |
| --- | --- | --- |
| `gpt-5.6-sol-900k` versus `gpt-5.6-sol` | Both produce `model: gpt-5.6-sol` | This does not select two distinct model IDs at the provider. Context policy and consequently retained input can still differ. |
| `gpt-6-astra-900k` | Produces `model: gpt-6-astra` | The successful session's `900k` suffix is a Hermes context variant. |
| GPT-5.5, `max` or `ultra` | Both clamp to `reasoning.effort: xhigh` | These selections do not create separate effort values in the standard path. |
| GPT-5.6 or Astra, `ultra` | Clamps to `max` | Compare the effective effort, rather than the UI word alone. |
| Astra, `medium` | Remains `medium` | This is a distinct effort candidate, but one successful run does not establish causality. |
| Session `service_tier: normal` | Normal mode disables the fast override; the config loader resolves it to `None` | This is not evidence that the Full subscription was downgraded or assigned a particular server pool. |

Sources: [context-variant definitions](https://github.com/NousResearch/hermes-agent/blob/9fd44b4dfc44138b9e5d5689acb56c438364ff7b/agent/model_metadata.py), [effort vocabularies and clamping](https://github.com/NousResearch/hermes-agent/blob/9fd44b4dfc44138b9e5d5689acb56c438364ff7b/agent/reasoning_effort.py), [request construction](https://github.com/NousResearch/hermes-agent/blob/9fd44b4dfc44138b9e5d5689acb56c438364ff7b/agent/transports/codex.py), [normal-mode loading](https://github.com/NousResearch/hermes-agent/blob/9fd44b4dfc44138b9e5d5689acb56c438364ff7b/gateway/run_config_loaders.py), [session fast-mode setter](https://github.com/NousResearch/hermes-agent/blob/9fd44b4dfc44138b9e5d5689acb56c438364ff7b/tui_gateway/methods_config_set.py).

The transport consults provider-declared effort support before its fallback vocabulary and merges `request_overrides` after constructing fields. The [bundled Codex profile](https://github.com/NousResearch/hermes-agent/blob/9fd44b4dfc44138b9e5d5689acb56c438364ff7b/plugins/model-providers/openai-codex/__init__.py) does not declare a separate effort vocabulary. Installed overrides still need checking before describing these source-derived values as captured requests. These mappings apply to Hermes; they do not establish how ChatGPT Work translates its own controls.

## The next recovery step

The demonstrated anchor is **Full → Hermes → openai-codex → Astra 900k / Medium / normal**, which completed five tool calls and a final answer at 12:18:50 UTC. Preserve that session and its state. Let it perform one bounded, useful portion of the Jarvis task, retaining completed tool results. Do not simultaneously change its model, context variant, effort and fast mode or introduce a parallel probe workload.

If it completes, continue useful work and record the observation duration and any failures. A repeated synthetic test is unnecessary while the actual task is progressing. After that, inspect which auxiliary, compression and explicit worker routes the task actually uses; the successful main route does not prove they match.

If it overloads again, retain the failed step and collect only a focused request/error record:

| Field | What it resolves |
| --- | --- |
| UTC start/end, local turn identifier, attempt number | Whether observations belong to the same call and retry sequence |
| Call role: main, summary, approval, vision or worker | Whether the rejected request actually used the selected main route |
| Effective provider, endpoint origin/path, model, reasoning effort and service tier (including absent fields) | What reached the transport after selection and overrides |
| Input-item/tool counts and request size, if already available | Whether a different payload accompanied the apparent same-model test |
| Error type/code, HTTP status if available, upstream response request ID if supplied | What the provider actually returned, without inventing missing diagnostics |
| Last completed tool result and pending next step | What must resume without replaying a completed action |

Use opaque local labels for account/session comparison; omit credentials, raw account IDs and private message/tool bodies. The client-generated `x-client-request-id` in this transport is related to cache/session scope; do not present it as a unique upstream response ID.

Only if reasoning remains a concrete unresolved variable, use a small sequential **Medium → XHigh → Medium** comparison with the same harmless task and equivalent starting context. State the time-order limitation: intermittent capacity can mimic an effort effect. Stop at the normal bounded retry limit. This comparison has **not** been run.

## Where we can intervene

- **Request construction and routing:** identify effective values and route overrides, then use a supported configuration that demonstrably completes work.
- **Continuation after rejection:** preserve the pending model step and completed tool results; resume when a request succeeds. This is tracked in [task 2](https://github.com/gv1983us-commits/codex-provider-incident/issues/2).
- **Compression failure:** retain recoverable history and make the fallback path observable, tracked in [task 3](https://github.com/gv1983us-commits/codex-provider-incident/issues/3).

The source also exposes a fast/priority request control. Its presence is not proof that the affected subscription will accept or benefit from it, and the documentation describes increased consumption/cost. It was not enabled or used as a recovery claim here. See [the pinned configuration documentation](https://github.com/NousResearch/hermes-agent/blob/9fd44b4dfc44138b9e5d5689acb56c438364ff7b/website/docs/user-guide/configuration.md).

The exact provider-side capacity/admission mechanism remains unknown. No runtime patch or new live Full-account test was performed as part of this source review.

## По-русски

Продолжаем от уже сработавшей Astra Medium. В проверенном исходном коде `900k` снимается с имени модели перед отправкой, а `Max` и `Ultra` у 5.5 оба превращаются в `xhigh`. Поэтому часть прежних переключений не разделяет варианты так, как кажется по интерфейсу. Сначала наблюдаем следующий полезный шаг в той же сессии. При отказе нужны параметры конкретного вызова и место остановки — особенно роль вызова, фактическая модель и reasoning. Полный экспорт заново для этого не требуется.
