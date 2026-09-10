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

## Pro Light reset ledger: what can be counted

The owner currently reports **two manually applied resets and two provider-initiated resets during the partial month: four reported applications in total**. This is the owner's tally, not an independently retrieved account ledger. Exact dates for all four applications remain incomplete.

| Date | Retained evidence | Counting treatment |
| --- | --- | --- |
| 30 August | Owner explicitly says a reset credit was used on this date | One dated manual application within the reported tally |
| 4 September | Owner reports a new reset credit appearing; the contemporary transcript records a Full Reset expiry of 4 October | Issuance/availability, not proof of another application; do not add it as a fifth reset |
| 10 September, 11:53–12:01 Moscow | Retained screenshot analysis records Pro Light at 45% weekly allowance | Historical snapshot, not the current remaining balance |

Sources are the retained private texts `CHATGPT_SHARED_CHAT_SLOW_FIRST_RESPONSE_2026-09-07.md` (owner messages 32, 35, 37, 39 and 41 plus the contemporaneous screenshot interpretation) and `HERMES_SESSION_FORENSICS_2026-09-10.md` (10 September screenshot addendum). This public note paraphrases only the relevant account/reset observations. Earlier assistant explanations of provider internals are not treated as primary evidence.

The same screenshot addendum assigns **41% and two available resets to the other affected Pro account**. Those values must not be moved into the Light ledger. The total number of issued reset credits and the current available balance are not established. The owner clarified that “credit” here means a reset credit, not a monetary credit.

## По-русски

**После подключения Pro Light заработала та же большая сессия Builder.** Это более сильное сравнение, чем отдельный ответ в новом чате: прежняя история оказалась пригодна для продолжения. По сообщению владельца, Джарвис консервирует работу; завершение ещё не подтверждено. Full не восстановлена — работа временно продолжена за счёт другой учётки.

По текущему подсчёту владельца за неполный месяц было **2 ручных + 2 провайдерских сброса**. В старом чате датированы применение 30 августа и появление нового кредита 4 сентября. Выдачу кредита не прибавляем к числу применений. Утренние 45% Light не выдаём за текущий баланс; два доступных сброса на другом скриншоте относились к Full.
