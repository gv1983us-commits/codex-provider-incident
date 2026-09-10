# Sol completes conservation on Light; earlier Astra banner reappears

[Home](../README.md) · [Structured record](../evidence/conservation-20260910.json) · [Account-switch comparison](ACCOUNT-SWITCH-2026-09-10.md)

**The owner supplied a Jarvis report of completed conservation and then clarified that Sol completed the current work; the Astra error banner shown below it was old.** The preceding account-switch report places this work in the same Builder session using Pro Light. The exact Sol wire model and reasoning effort have not been supplied.

## Completed work, as reported

The relayed report places the parent in **`QUIESCED_SAFE`**, with retained storage, checkpoints and a handoff for resumption. It does not report an accepted release or promotion. This is a report of a completed preservation stage, not completion of the overall build task. The underlying storage, process state and hashes have not been independently checked by this repository.

The account-switch observation has therefore progressed from reported initial activity to a report of a concrete safe stopping point. **The affected Pro Full subscription remains unresolved.**

## Why the later banner is not a new failure count

| Field | Earlier receipt | Later displayed receipt |
| --- | --- | --- |
| Time, UTC | 14:17:45.876 | 15:11:27.300 |
| Provider / model | `openai-codex / gpt-6-astra-900k` | Same |
| Layer / code / retryable | `provider / unknown / true` | Same |
| Request ID in error text | `2a438cad-4c97-43d4-abc8-77ce4e4ac03d` | Same |
| Error message | Generic request-processing error | Identical |

Both times are on **10 September 2026** and differ by **53 minutes 41.424 seconds**. This is a difference between displayed timestamps, not a measurement of useful work, outage length or another request's execution time.

The owner explicitly corrected the interpretation: **the banner was old; Sol had just completed the work**. Record the later receipt as a reported redisplay of the earlier error. It is not added as a new Sol failure, new Light-account failure, or independently established provider request. The original Astra failure remains preserved.

The supplied text does not explain why the displayed time changed. A future focused UI investigation could distinguish the original error event, display/export time and the current model/turn identifier. No particular frontend or transport implementation fault is established here.

## По-русски

**Sol на Pro Light завершил консервацию той же Builder-сессии — по переданному отчёту и уточнению владельца.** Parent остановлен в `QUIESCED_SAFE`; принятия релиза и promotion нет. Фактические диски и процессы из этой сессии расследования не перепроверялись.

Плашка Astra с временем 18:11:27.300 по Москве повторяет прежний request ID и весь текст ошибки. Владелец подтвердил, что это старая плашка. **Нового отказа Sol или Light по ней не фиксируем.**
