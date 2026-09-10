# Same Builder session resumes after switching to Pro Light

[Home](../README.md) · [Structured observations and reset ledger](../evidence/builder-pro-light-20260910.json) · [Recovery task](https://github.com/gv1983us-commits/codex-provider-incident/issues/1)

**Latest owner report, 10 September 2026: connecting the existing Pro Light account to Hermes allowed work to resume in the same large Builder session. A new empty session was not required for this reported recovery. The affected Pro Full subscription remains unresolved.**

## Observed comparison

| Stage | Account and surface | Evidence |
| --- | --- | --- |
| Earlier failures | Pro Full → Hermes → Codex | Retained log audit and later exact GPT-5.5 `overloaded` / Astra `unknown` receipts |
| Earlier control | Pro Light → official ChatGPT Work | Owner-reported working comparison on the same home computer/network/VPN |
| New control | Pro Light connected to Hermes | Owner reports work resuming directly inside the same existing Builder session |

The owner identifies Builder as the session associated with the approximately **21 MB `session-20260909.json` export** already discussed. This identifies the retained session history; it does not measure active context tokens or the size of a single provider request. The old export was not re-audited for this update.

The owner reports that Jarvis is preserving the interrupted work and shutting down the external server. This is a report of work in progress, not a verified completion receipt. The owner estimates only a couple of hours of resources remain on Light; there is no measured duration or live quota read behind that estimate.

## Diagnostic consequence

The reported result strengthens the account-dependent availability pattern: the existing session could continue through Hermes using another account. It weighs against the session history being intrinsically unable to continue under all accounts.

The exact new model, reasoning settings, transport payload, session identifier and recovery timestamp are not supplied. Account selection and elapsed time changed, and equality of the outgoing requests is not established. The observation therefore does not by itself isolate quota, routing or request-admission behavior, nor determine intent.

**Keep the Full-account incident open.** Record useful progress on Light as temporary continuation, preserve completed work, and retain the earlier Full failures. A replacement account working does not restore the paid capability on the affected account.

## Pro Light reset ledger — corrected from the screenshot

**Three reset credits received, three used, zero available.** The owner supplied a screenshot of the reset-history panel with **History / Past 30 days** selected and **Available 0** visible. This corrects the previous provisional total of four applications.

All displayed times are **GMT+3**. The year 2026 comes from the conversation date; the cropped panel displays only day and month.

| Date | Time | Exact history label |
| --- | --- | --- |
| 22 August | 03:28 | `Reset received` |
| 23 August | 20:24 | `Reset used` |
| 4 September | 08:39 | `Reset received` |
| 4 September | 10:02 | `Reset used` |
| 5 September | 07:21 | `Reset received` |
| 5 September | 17:33 | `Reset used` |

**Owner clarification:** one original reset credit and two additional credits were provided. The owner applied the original and first additional reset; the provider applied the second additional reset, taking the remaining allowance from **97% to 100%**. That is a reported increase of **3 percentage points**, not a newly measured full allowance of additional consumption. The history screenshot does not display the initiator or the before/after percentage; these remain the owner's observations.

Source: owner-supplied image `9a33238a-7dfc-4867-80ad-3f69c5782c18.png`, visually read in this conversation. The owner identifies the account as Pro Light; the account name is not visible in this crop. The exact capture timestamp is not supplied. Only the relevant history entries are transcribed publicly.

The prior chat recollection of a reset used on **30 August** remains in the original transcript, but it is not added as a fourth application: the current visible history shows **23 August, 4 September and 5 September** as the three used dates. The earlier four-application summary in this repository is superseded by this screenshot and the owner's correction.

`Available 0` concerns saved reset credits, **not** current weekly allowance. A separate retained screenshot analysis recorded Pro Light at **45% weekly allowance on 10 September, 11:53–12:01 Moscow**; that is a historical snapshot. Its **41% and two available resets** describe the other affected Pro account and do not belong in this Light ledger. The current weekly allowance has not been read.

## По-русски

**После подключения Pro Light заработала та же большая сессия Builder.** Это более сильное сравнение, чем отдельный ответ в новом чате: прежняя история оказалась пригодна для продолжения. По сообщению владельца, Джарвис консервирует работу; завершение ещё не подтверждено. Full не восстановлена — работа временно продолжена за счёт другой учётки.

**Исправление по скриншоту: 3 кредита получено, 3 использовано, доступно 0.** Выдачи: 22 августа, 4 и 5 сентября. Использования: 23 августа, 4 и 5 сентября. По уточнению владельца, два применения были ручными, одно — со стороны провайдера, с изменением остатка 97% → 100%. Инициатор и проценты на этом скриншоте не показаны. Прежний итог «четыре» исправлен. Нулевой остаток reset-кредитов не означает нулевой недельный лимит.
