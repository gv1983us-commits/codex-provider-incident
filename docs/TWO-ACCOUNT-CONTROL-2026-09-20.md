# Two-account controlled comparison and dual-subscription impact — 20 September 2026

[Current status](STATUS-2026-09-20.md) · [Same-session account switch](ACCOUNT-SWITCH-2026-09-10.md) · [15 Sep cross-account comparison](COMMUNITY-FOLLOWUP-2026-09-15.md) · [Route matrix](../evidence/route-matrix.json)

## Why this comparison matters

The strongest owner-side control in this incident is not a single error receipt. It is the repeated comparison of **two paid ChatGPT accounts in the same physical environment**, across independent clients.

The public evidence already records the following controls:

- same Windows laptop;
- same home network / Internet path;
- same VPN configuration;
- Microsoft Edge and Yandex Browser both used;
- ChatGPT's native account switcher tested;
- Pro Full browser/Work failure observed with Hermes stopped;
- both Pro Full and Pro Light later tested through Hermes / openai-codex.

These controls do not reveal the provider's internal routing or policy. They do remove several simple local explanations.

## Controlled account / client matrix

| Account | Surface / client | Observed state |
| --- | --- | --- |
| **Pro Full 20x** | Hermes → `openai-codex` | Repeated provider failures across Astra, Sol and GPT-5.5; later large Sol failure sets and repeated 5/5 exhaustion. Short successes also occurred. |
| **Pro Full 20x** | Official ChatGPT Work / Codex execution surface | Capacity / unavailable behavior was owner-observed during the affected periods. On 15 Sep, the Edge control failed while the same Full account was also failing in Hermes. |
| **Pro Full 20x** | Ordinary ChatGPT conversation | Continued to work as reported. The incident therefore did **not** present as a total loss of all ChatGPT functionality on the account. |
| **Pro Light** | Official ChatGPT Work / direct ChatGPT control | Earlier comparison remained usable while Pro Full execution surfaces were failing. On 15 Sep, the Light/Yandex conversation continued while Full/Hermes and Full/Edge were failing. |
| **Pro Light** | Hermes → `openai-codex`, same existing Builder session | After switching Hermes to Light, the same large Builder session resumed without creating a new empty session; Sol later completed the preservation/conservation stage. |
| **Pro Light** | Hermes → `openai-codex`, later | Light later also returned a documented Sol `overloaded` error, so it was a temporary fallback/control rather than a durable workaround. |
| **Pro Light** | Direct ChatGPT, later 17 Sep | Three incomplete Sol turns were later documented from the public session graph. These are kept separate from Hermes transport counts. |

## 15 September simultaneous control

A particularly useful owner-reported comparison was recorded on **one Lenovo laptop**:

- **Pro Full / Hermes:** two Astra sessions were encountering errors.
- **Pro Full / Microsoft Edge / official ChatGPT:** the official-interface control also failed when Hermes failed.
- **Pro Light / Yandex Browser / official ChatGPT:** the Light conversation continued without Hermes.

This observation is not counted as additional provider log receipts because the browser path does not expose the same transport ledger. Its diagnostic value is different: **the same machine and network simultaneously showed different behavior for two paid accounts, while the affected Full account reproduced outside Hermes.**

That makes a purely Hermes-local, project-local, laptop-local, LAN-local, or VPN-local explanation insufficient for the full observed pattern.

It does **not** prove an account-specific safeguard, a particular backend pool, or provider intent. Only the provider can correlate the account-side routing/admission state.

## Same-session account switch

On 10 September the existing **Pro Light** account was connected to Hermes after the Pro Full path had become unreliable.

The owner reported that the **same large Builder session** resumed directly under Light. No new empty session was required. Sol subsequently completed the preservation stage to the reported `QUIESCED_SAFE` state.

This matters because the retained project/session state itself was able to continue under another account. It weighs against a simple explanation in which that Builder conversation or project history was intrinsically unusable regardless of account.

Light later developed its own Codex failures, so this observation is a **time-bounded account-switch control**, not evidence that Light was permanently healthy.

## Scope: Codex / execution path, not ordinary ChatGPT chat

The owner-observed scope is important:

**Ordinary ChatGPT conversation remained usable. The disruptive failures were concentrated on Codex-backed / work-execution use: Hermes through `openai-codex`, and the official ChatGPT Work/Codex execution surface.**

This distinction should be preserved. The evidence does not support saying that the Pro Full account was completely unable to use ChatGPT. It supports saying that the **paid execution capability required for the owner's development work was intermittently or repeatedly unusable**, while ordinary chat could still answer.

Similarly, the repository does not claim that every official Work failure and every Hermes failure used an identical internal backend route. The cross-client reproduction establishes a broader provider-side/account-route pattern requiring provider correlation; it does not expose internal topology.

## Dual-subscription impact

The incident consumed resources from **two paid subscriptions**, not one.

### Pro Full 20x

Pro Full was purchased on **4 September 2026** for the primary development workload. During much of the period through 20 September, its Codex/work-execution path was not reliable enough for sustained work. Time that should have been used for the intended workload was instead spent reproducing failures, preserving interrupted work, collecting diagnostics and maintaining the incident.

### Pro Light

Pro Light was then used as:

- a live control against the affected Full account;
- a fallback route for official ChatGPT/Work;
- a replacement Hermes credential so the same Builder session could be preserved;
- a diagnostic comparison for separating client-local behavior from account/provider behavior.

That means Light's paid allowance and working time were also diverted into maintaining and diagnosing the Full-account incident. Light subsequently developed its own documented Codex/Direct ChatGPT failures, so it did not remain a dependable substitute.

### User impact statement

This is not a claim that both subscriptions were completely unavailable for two continuous weeks.

It is a narrower and more defensible statement:

> **Two paid subscriptions were materially consumed by the same incident period. Pro Full was the primary affected subscription; Pro Light was then spent as fallback capacity and a diagnostic control, and later also exhibited execution-path failures. As a result, paid time and usable resources from both subscriptions were diverted from the development work they were purchased to perform.**

This dual-subscription impact should be considered when determining any service credit, subscription extension, refund or other compensation path.

## What the comparison establishes

The combined evidence supports these conclusions:

1. **Hermes alone is insufficient to explain the incident.** Full-account failure reproduced in the official ChatGPT execution surface, including with Hermes stopped.
2. **The local machine/network alone is insufficient.** Different paid accounts showed different behavior on the same laptop/network environment.
3. **The retained Builder session alone is insufficient.** The same session resumed after switching Hermes from Full to Light.
4. **The issue is not equivalent to complete ChatGPT account failure.** Ordinary chat remained usable while the execution/Codex path was disrupted.
5. **Light was not a durable workaround.** It later produced its own Codex overload and Direct ChatGPT incomplete-turn evidence.
6. **The exact provider-side mechanism remains unknown.** The observations are consistent with account/model/route-dependent serving or admission behavior, but they do not prove a safeguard, routing class, backend pool or policy decision.

## Evidence limits

- The strongest simultaneous browser comparison is documented as **Full in Edge / Light in Yandex**, not as a fully instrumented same-browser A/B benchmark.
- The owner recalls additional native account-switcher testing, but this report does not elevate an uncorrelated same-browser recollection into a new transport-level fact.
- Browser/Work observations lack the request IDs and retry telemetry available in Hermes logs.
- Full and Light did not remain in fixed identical workload states over the entire incident period.
- Later failures on Light mean the two-account comparison is time-dependent, not a permanent healthy-account / unhealthy-account split.

The value of the control is therefore not “account A always fails and account B always works.” It is that **under the same local environment, account/client outcomes diverged repeatedly, and the primary affected Full account reproduced outside Hermes.**
