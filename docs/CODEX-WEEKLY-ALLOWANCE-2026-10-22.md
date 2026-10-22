# Codex: переходы Pro → Plus → новый Pro, недельная квота и задержки Lenovo/MSI

Обновлено **22 октября 2026 года**. Время собственных логов — **UTC+03:00**. Основной инцидент: [issue #6](https://github.com/gv1983us-commits/codex-provider-incident/issues/6). [Машиночитаемая сводка](https://github.com/gv1983us-commits/codex-provider-incident/blob/main/evidence/allowance-stalls-20261022.json).

## Новые выводы и уточнения

**Нынешний Pro $200 куплен на другой учётной записи относительно прежнего Pro/Plus. При этом Lenovo и MSI сейчас используют одну подписку и общий бюджет — это ранее прямо подтвердил владелец.** Эти два факта совместимы: сменился исторический аккаунт подписки, а не возникли два независимых текущих бюджета.

### Хронология со слов владельца

| Этап | Что известно | Граница подтверждения |
|---|---|---|
| Первый Pro $200 на Lenovo | Владелец сообщал примерно о пяти сутках работы на недельном объёме в период Sol 5.6 | Историческая оценка; теперь получены session/model aggregates с 8 сентября, но не полный per-call журнал и не выписка лимита |
| Переход Lenovo на Plus **3 или 4 октября** | Почти две недели расход выглядел как на прежнем Pro $200; работа продолжалась, мелкие provider errors заметно не мешали | Дата начала имеет неопределённость в один день; точный момент изменения фактически применённого лимита не установлен |
| Поздняя часть Plus | Недельный объём резко стал заканчиваться примерно за сутки, затем начали расходоваться credits | Нельзя описывать весь Plus-период как одинаковый «один день на неделю» |
| Новый Pro $200 на другой учётке, со старта MSI | По словам владельца, долгие ожидания вернулись сразу; текущая подписка общая для MSI и Lenovo | Это новая покупка на другой учётке, а не обычный upgrade прежней Plus-подписки |
| **22 октября, около 11:46** | Обе машины получили структурированный `usage_limit_reached`, `plan_type=pro`, окно **10 080 минут** | Подтверждено логами; значения reset перекрываются в диапазоне 27 октября 11:44:36–38, а не побайтно одинаковы |

Первый сохранённый main-loop timestamp MSI — **20 октября 12:04:02.662**. Он служит техническим ориентиром сравнения, но не заменяет дату чека, активации тарифа или переключения авторизации. Владелец ранее оценил длительность нового Pro до исчерпания примерно в 2,5 суток; это его наблюдение, не длительность, вычисленная от первого сохранённого успешного вызова.

### 1. Старый и новый Pro $200 могут иметь разный включённый объём

Проверенная 22 октября [официальная страница OpenAI о Pro](https://help.openai.com/en/articles/9793128-about-chatgpt-pro-tiers) сообщает: новые Pro $200 без права на сохранённые прежние условия имеют меньший включённый объём при той же цене. Старые условия доступны до **29 октября 2026** для подходящих аккаунтов, имевших активный Pro $200 в установленном окне **22–29 сентября**, пока Pro активен. Возобновление на том же подходящем аккаунте тоже может сохранять это право.

Поэтому одинаковая цена первой и новой подписки не даёт основания считать их недельные объёмы одинаковыми. Новая учётка относительно прежнего аккаунта делает это различие существенным. **Фактический entitlement, выданный этой учётке при покупке, всё ещё не прочитан из личного кабинета или провайдерского журнала.** Мы не присваиваем ему самостоятельно метку 10x/20x и не объявляем коэффициент снижения. Текущая документация не заменяет доказательство условий конкретной покупки.

### 2. Для аномального Plus найден предметный публичный аналог

В [openai/codex #54677, комментарии от 20 октября](https://github.com/openai/codex/issues/54677#issuecomment-6296212737) другой пользователь описывает переход **Pro $200 → Pro $100 6 октября**, резкий провал остатка **16 октября** и последующее письмо OpenAI **“Correcting your usage limit”**. По его опубликованному пересказу письма, прежний большой лимит ошибочно сохранялся после перехода, затем уже накопленное использование зачли относительно меньшего лимита. Он также сообщил о добавленных компенсирующих credits.

Мы прочитали публичный комментарий; оригинал его письма и счёт не проверяли. Наш случай относится к **Plus**, а не к его Pro $100. Но последовательность «старый объём сохраняется после понижения → позднее резко меняется доступный остаток» очень близка к сообщению владельца Lenovo. Это **сильная гипотеза для проверки старого аккаунта**, а не установленный диагноз нашего аккаунта. Она не объясняет автоматически зависания на новом Pro и не доказывает ошибочность всех новых списаний.

### 3. Зависания до исчерпания квоты — самостоятельная часть инцидента

На MSI подтверждён один пользовательский ход **61 минута 19,3 секунды**, закончившийся внешним `complete`, с **тремя 300-секундными watchdog timeout** внутри. Это произошло до первого зафиксированного недельного отказа около 11:46. Подробные якоря и границы расчёта приведены ниже.

Независимые первичные сообщения:

- [OpenAI #45030, 12 сентября](https://github.com/openai/codex/issues/45030): автор сообщает, что Pro часто прерывает рабочие задачи, а Plus на том же компьютере с похожими задачами работает устойчивее.
- [OpenAI #56114, 22 октября](https://github.com/openai/codex/issues/56114): ошибки capacity следуют за одним Pro-аккаунтом на другую машину; другой аккаунт работает в исходной среде, при этом у проблемного аккаунта остаётся allowance.
- [Hermes #131743](https://github.com/NousResearch/hermes-agent/issues/131743): открытое расследование 300s first-progress stalls. В [комментарии 16 октября](https://github.com/NousResearch/hermes-agent/issues/131743#issuecomment-6212372634) есть независимый длинный эпизод с таймаутами. Связанные #133901 и #133963 относятся к накопленному ожиданию восстановления и корреляции физических попыток.

Сходство симптомов обосновывает сопоставление. Оно не доказывает единую причину или намеренное ограничение Pro. Статус PR #133933, #139410 и #138206 проверен 22 октября: предложения открыты и не merged; обещать, что любое из них уже исправляет наш случай, нельзя.

### 4. Статистика Lenovo получена: 8 сентября — 22 октября

**22 октября получена прямая offline-выгрузка счётчиков Lenovo.** [Подробное дополнение](https://github.com/gv1983us-commits/codex-provider-incident/blob/main/docs/LENOVO-RETAINED-USAGE-2026-10-22.md) и [агрегаты JSON](https://github.com/gv1983us-commits/codex-provider-incident/blob/main/evidence/lenovo-retained-usage-20261022.json) заменяют прежнее ограничение «полная статистика ещё не получена».

Три базы содержат 1 188 сессий. Их сохранённые счётчики дают **115 531 main + 3 604 auxiliary = 119 135 вызовов**; 119 123 явно помечены openai-codex, восемь без provider attribution, четыре относятся к другим маршрутам. История начинается 8 сентября; наличие более ранних данных этим не подтверждается. Межбазовых пересечений session IDs не найдено, но это сумма локальных счётчиков, не независимая дедупликация всех физических запросов провайдера.

Во всех сессиях main-строки session_model_usage точно совпали с sessions по шести полям: calls, input, output, cache read/write, reasoning. Auxiliary составляет отдельную добавку в модельной таблице. **Таблицы целиком не складываются.**

**Уточнение области прежних log-итогов:** завершённые API-вызовы включают часть background_review. Для Sol 6: 9 944 main + 229 review = 10 173 log completions; для Sol 6.1: 166 + 18 = 184. Полный вход, выход и cache-read тоже точно совпали. Поэтому прибавление всей DB auxiliary-суммы к прежнему log total повторно посчитает часть нагрузки. Термин completed calls ниже обозначает журнал завершений, а не только DB main.

DB input_tokens — вход без кеша; log in= — полный prompt. Для всех трёх баз полный вход равен **23 479 321 960**, cache-read — **22 282 305 456 (94,90%)**, выход — **70 324 713**. Reasoning уже является детализацией выхода и повторно не прибавляется.

Для тарифного сравнения сохраняется ограничение: три длинные сессии с 28 944 main calls (25,06% основной базы) пересекают границы 3–4 октября либо не имеют окончания. Их lifetime totals нельзя целиком отнести к одному тарифу или дню. first_seen/last_seen содержат времена accounting writes и границы исторических session-агрегатов, не полноценную хронологию каждого вызова. Отдельно отмечено появление Sol 6 и Sol 6.1 на Lenovo 20 октября, подтверждённое логами.

Нулевые денежные поля included-учёта не означают нулевые списания недельного allowance или купленных credits. Account/plan attribution и provider debit ledger в выгрузке отсутствуют. Механизм upstream проверен по MSI d923a5e; локальные изменения Lenovo c7c2595…dirty неизвестны.

### Что предъявляется поставщикам

OpenAI: проверить старый переход Pro → Plus на известную ошибку сохранения прежнего лимита; установить размер и начальный остаток **новой** Pro-подписки; сверить реальные списания по текущему общему аккаунту; отдельно расследовать доступность до недельного исчерпания и учёт failed/partial responses.

Hermes: присоединить Windows-эпизод к существующему расследованию stalls; обеспечить понятную связь между пользовательским ходом, физическими попытками и восстановлением; использовать существующий DB-учёт auxiliary routes при сверке; отдельно объяснить короткие повторы после терминального `usage_limit_reached` с многодневным reset.

Публичная JSON-сводка содержит необходимые агрегаты и временные якоря без response/session IDs, частных путей, содержания задач и реквизитов аккаунтов. **Числа повторно сверены по CSV/JSON: 18 арифметических проверок сошлись.** Никакие новые inference-прогоны для этого не выполнялись.

---

## 1. Technical evidence for issue #6

### 22 October — shared Pro weekly quota exhausted on both devices; reconciled Lenovo/MSI execution evidence

#### Owner context and impact

The owner reports approximately five days of usable included weekly allowance during the earlier Pro $200 / Sol 5.6 period. Lenovo moved to Plus on October 3 or 4. For almost two weeks, allowance consumption appeared similar to the former Pro plan; only later did the weekly allowance begin lasting about one day and purchased credits get used. The current Pro $200 was newly purchased on a different account at MSI startup; the owner estimates approximately 2.5 days until exhaustion. Both devices now share that current subscription. The exact starting balance and entitlement are not contained in these logs. The owner reports full historical Lenovo statistics exist; their export has not been included in the recovered evidence used here.

The owner explicitly confirms **one Codex subscription and one shared allowance pool on Lenovo and MSI**. The baseline work assigned on Lenovo continued across all three periods. One working Jarvis stream was added on MSI. The owner reports lower useful output and estimates provider-related disruption at roughly three quarters of working time on MSI, with similar disruption on Lenovo. That estimate remains separate from measured client telemetry.

Both batches have now been analyzed: **nine Lenovo logs and four MSI logs**. No new Hermes inference, diagnostic replay or configuration change was required. The account-level exhaustion is established; its exact deduction history remains a provider-side reconciliation question.

#### A. Matching seven-day Pro exhaustion on both devices

| Event, 22 October | Lenovo | MSI |
|---|---|---|
| Last retained completed call | **11:46:06.247** | **11:46:26.453** |
| First recorded HTTP 429 attempt | **11:46:10.786** | **11:46:28.962** |
| First structured error payload | 11:46:10.789 | 11:46:28.964 |
| Error type | `usage_limit_reached` | `usage_limit_reached` |
| Plan family | `pro` | `pro` |
| `limit_window_minutes` | **10080** | **10080** |
| Recorded reset, UTC+03:00 | 27 Oct **11:44:36–38** | 27 Oct **11:44:36–37** |

The first failures are **18.176 seconds apart in the recorded timestamps**; both payload sets identify a seven-day Pro window with overlapping reset ranges differing by up to two seconds. This is consistent with the owner's explicit shared-account confirmation. Clock synchronization uncertainty is unmeasured, and timestamps alone are not used to identify an account. A completion timestamp does not identify the moment a request was admitted or charged.

First MSI payload, `agent(1).log` line **19682**, mirrored at `errors(1).log` line **626**:

```json
{
  "type": "usage_limit_reached",
  "plan_type": "pro",
  "limit_window_minutes": 10080,
  "resets_at": 1793090676,
  "resets_in_seconds": 431887
}
```

The first Lenovo payload is `agent.log` line **25558**, mirror `errors.log` line **6471**. The package preserves 8 allowlisted Lenovo payload records and 22 MSI payload records. `pro` identifies the plan family; the $200 price is owner-reported. These errors do not disclose entitlement size, starting balance after upgrade, model weights or previous deductions.

#### B. Both published call/error counts now reproduce independently

The unchanged common window is **20 October 12:04:02.662 → 22 October 10:57:18.578**, inclusive. The earlier boundary was the first retained MSI main-loop record, not a verified purchase timestamp. MSI's first retained successful Codex completion occurs later, at **20 October 14:12:51.359**.

| Metric | Lenovo common | MSI common | Combined common | Lenovo new delta | MSI new delta |
|---|---:|---:|---:|---:|---:|
| Completed conversation-loop records | **10,259** | **2,618** | **12,877** | 219 | 79 |
| Failed attempts | **237** | **9** | **246** | 15 | 33 |
| Main-loop retry notices | **213** | **7** | **220** | 10 | 22 |
| HTTP 429 quota-exhaustion attempts | 0 | 0 | 0 | 10 | 33 |

Delta means strictly after the old common-window endpoint and is bounded by each supplied file's actual end. MSI agent/error records continue through **12:25**; its GUI ends at **11:49**. The shorter GUI retention cannot exclude later background failures.

Across all retained successful-call data there are **19,417 unique response IDs**: 16,720 Lenovo and 2,697 MSI. No IDs overlap between devices. The retained success windows differ, so the all-retained total is not a balanced historical comparison. Input/output totals and cache denominators passed arithmetic checks; every success marker was parsed. Exact agent/error mirrors, retry notices and terminal failure summaries are not counted as extra model attempts.

#### C. What the added MSI stream contributed

Common-window completed-call telemetry:

| Device | Calls | Input tokens | Output tokens | Explicitly cached / all input, lower bound |
|---|---:|---:|---:|---:|
| Lenovo | 10,259 | 1,969,147,623 | 5,210,871 | 95.46% |
| MSI | 2,618 | 440,058,362 | 2,705,613 | 96.96% |
| **Combined** | **12,877** | **2,409,205,985** | **7,916,484** | **95.74%** |

MSI accounts for **20.33% of completed calls, 18.27% of input and 34.18% of output** in this recorded population. Relative to Lenovo alone in the same window, MSI adds **25.52% more calls and 51.92% more output tokens**. The added stream is therefore material even though it is one owner-facing workflow and experienced stalls. These are token/call shares, **not shares of deducted weekly allowance**.

From the common start through the last retained successes, including the new delta:

| Device | Calls | Input tokens | Output tokens | Explicitly cached / all input, lower bound |
|---|---:|---:|---:|---:|
| Lenovo | 10,478 | 2,016,474,421 | 5,315,060 | 95.23% |
| MSI | 2,697 | 458,476,561 | 2,805,594 | 97.04% |
| **Combined** | **13,175** | **2,474,950,982** | **8,120,654** | **95.57%** |

Input includes repeatedly supplied context and cached prefixes. It is not unique new text. The successful-call census excludes unreported auxiliary usage, failed/partial response usage and any other account activity absent from these files. Output is the logged `out` counter; no separate per-call reasoning breakdown is present.

#### D. Effective models and auxiliary routing

Common-window models across both machines:

| Recorded model | Calls | Input | Output |
|---|---:|---:|---:|
| `gpt-5.6-sol-900k` | 121 | 24,410,571 | 59,637 |
| `gpt-6-sol-900k` | 9,954 | 1,908,133,702 | 5,093,911 |
| `gpt-6.1-sol` | 6 | 100,945 | 815 |
| `gpt-6.1-sol-900k` | 2,796 | 476,560,767 | 2,762,121 |

**MSI:** all 2,697 recorded completions are in the 6.1 family: 2,691 `gpt-6.1-sol-900k` and six `gpt-6.1-sol`. The six unsuffixed calls belong to one CLI session, not an extra model family. At **20 October 14:11:39.845**, `agent(1).log` line **166** records a resolved client reasoning setting of `medium`. This is a client switch-time setting, not per-request attestation of every requested/served effort. No `service_tier` field was found.

MSI common-window auxiliary route selections include **36 approval, 12 compression, two goal_judge and one title_generation** records, all in the 6.1 family. Two further goal_judge selections occur in the delta. These are route-selection events, not a ledger of confirmed billed completions; their returned token usage is not recorded. Resolver auto-detect records are separate and are not added as extra calls.

**Lenovo:** the common-window completed-call log counts remain 9,954 on 6 Sol, 184 on 6.1, and 121 on 5.6. The 184 6.1 completions belong to one Telegram session. Separately, 175 desktop `goal_judge` route-selection records choose 6.1 in the common window. A new desktop session at **21 October 15:58:32.640** explicitly selects 6 Sol through `source=client override` despite a 6.1 profile default (`gui.log`, line **12745**). Thus this is a mixed session/auxiliary configuration; it is not evidence of hidden backend remapping.

#### E. Internal generation increased substantially

Two fully retained calendar days provide a bounded descriptive comparison. October 19 is Lenovo before the added MSI stream, during the owner-attributed Plus period; it is **not the earlier Pro month**.

| Recorded metric | Lenovo, 19 Oct | Lenovo, 21 Oct | MSI, 21 Oct | Combined, 21 Oct |
|---|---:|---:|---:|---:|
| Completed calls | 4,839 | 6,549 | 1,219 | **7,768** |
| Input tokens | 834,877,005 | 1,202,436,991 | 208,100,240 | **1,410,537,231** |
| Output tokens | 2,243,745 | 3,431,677 | 1,353,379 | **4,785,056** |

The combined October 21 population has **1.605 times the calls, 1.690 times the input and 2.133 times the output** of Lenovo on October 19. Lenovo itself contributes a 52.9% output increase; MSI adds another 1.353 million output tokens that day. Lenovo's explicit subagent starts also increase from 58 to 138.

This supports a concrete mechanism by which unchanged owner-level assignments and reduced useful output can coexist with more internal model processing. It does **not** establish the cause of that growth, identical task workloads, a 2.13x allowance charge, or an explanation of the earlier Pro-to-current-Pro duration ratio. No quota ratio is inferred from output alone.

#### F. A one-hour MSI turn hides three 300-second stalls behind `complete`

MSI `gui(1).log` line **268** finishes a turn at **22 October 10:56:15.549** with `status=complete`, `error_retained=False`, and duration **3,679.3 seconds — 61 minutes 19.3 seconds**. Its inferred start is approximately **09:54:56.249**.

All three previously reported MSI watchdog failures occur inside this same turn and session:

| Watchdog time | Meaning | Next completed call | Recorded latency of that call |
|---|---|---|---:|
| 10:01:11.806 | No substantive progress for 300s | #150 at 10:06:18.529 | **606.8s** |
| 10:29:23.104 | No substantive progress for 300s | #157 at 10:33:51.361 | **568.5s** |
| 10:43:19.519 | No substantive progress for 300s | #160 at 10:46:15.464 | **476.2s** |

Sources in `agent(1).log`: **18536 → 18556**, **18688 → 18709**, **18780 → 18807**. These are absence-of-substantive-progress watchdogs, not proof that no SSE events arrived at all. All recorded models are 6.1.

The three affected calls account for **1,651.5 seconds (27 minutes 31.5 seconds)** of recorded call latency, about 44.9% of this GUI turn. Their duration includes processing and recovery, so 44.9% is **not a downtime fraction**. The three 300-second thresholds are already within these call intervals and must not be added again. Across the entire turn there are 15 completed calls (#150–164), with 3,385.1 seconds of recorded latency including processing and waiting.

This is a specific demonstration of degraded execution hidden by a final `complete` status. The broader owner estimate of 75% lost working time remains unmeasured. The logs do not classify every second as useful work, waiting, redundant work or recovery.

#### G. Cache and recovery conclusions

The common-window MSI completed-call input is at least **96.96% explicitly cached**; Lenovo is at least **95.46%**. Combined, **95.74% of all recorded input** is explicitly cached, or 96.82% among records with cache fields. The 301 records without a cache field contain 27,002,835 input tokens. Missing cache fields are not assigned zero. Combined uncached input is bounded at **75,742,862–102,745,697** tokens within the recorded population.

These measurements do not support a general successful-call cache collapse. They do not prove correct caching on every request, auxiliary route or partial response. The earlier source-level cache-affinity hypothesis remains unproven for the installed runtime and should not be presented as the established cause.

After weekly exhaustion, Lenovo records **two chains of five attempts** and MSI **eleven chains of three attempts**. Combined: **43 quota-error attempts and 30 associated retry notices**. MSI's last rejection is at **12:25:01.776**, nearly 39 minutes after its first. Eight MSI chains share a thread name but have distinct session fingerprints; a thread name alone does not establish whether a scheduler or user initiated them.

The repeated short retries are an actionable quota-error recovery problem. They do not establish charges for rejected attempts and cannot explain how the allowance was depleted before the first rejection. GUI reflections and terminal summaries are not extra requests.

#### Requested resolution

1. **OpenAI: reconcile the shared seven-day Pro bucket.** State the entitlement and starting balance of the newly purchased Pro subscription on a different account, reset boundary, carried consumption if any, and deductions through October 22 11:46. Attribute the supplied Lenovo/MSI activity and any other consumers of the same account bucket using provider records.
2. **OpenAI: explain effective accounting.** Reconcile actual model mix, context, cached input, output/reasoning, auxiliary calls and requested/served tier. Explain any relevant changes between the earlier and current Pro periods. Public API or purchased-credit prices are not a conversion table for included weekly allowance.
3. **OpenAI: explain failed/partial-response deductions.** Determine whether any recorded failed, timed-out or cancelled attempts consumed allowance, in what quantity, and whether deductions were reversed. Missing local usage is not a zero-usage finding.
4. **Hermes: reconcile all inference routes.** Use existing state DB auxiliary accounting where available, and expose effective model, reasoning and tier together with returned usage for main, auxiliary and recovery paths. The text-log census has incomplete auxiliary coverage: the new Lenovo DB export proves that background_review already overlaps completed-call records. Pinned MSI code implements separate auxiliary DB accounting; do not add its whole total to log counts. Explain the observed Lenovo client override / auxiliary split and any accounting gaps, without treating missing text-log fields as proof that Hermes never records auxiliary usage.
5. **Hermes: distinguish terminal quota exhaustion from transient failure.** Surface the reset and suppress short retries for structured `usage_limit_reached` on a seven-day window. Preserve recovery appropriate to overload or transport errors.
6. **Remedy:** provide an account-specific explanation. Correct entitlement/metering defects and restore incorrectly consumed allowance if found. If deductions are expected, explain them with quantities the owner can reconcile.

The owner is not asked to buy credits or run additional inference to obtain this reconciliation. The September 30 operational closure of [issue #1](https://github.com/gv1983us-commits/codex-provider-incident/issues/1#issuecomment-5913330753) remains valid for its stated continuity criterion and did not resolve this October allowance question.

## 2. Что это говорит о разнице в цене

Проверенная 22 октября [официальная таблица](https://learn.chatgpt.com/docs/pricing) даёт следующие **Standard-ставки покупных credits на миллион токенов**:

| Модель | Некэшированный input | Cached input | Output |
|---|---:|---:|---:|
| Sol 5.6 | 100 | 10 | 500 |
| Sol 6 | 50 | 5 | 250 |
| Sol 6.1 | 50 | 2,5 | 250 |

По этим ставкам 6.1 дешевле 5.6. Документация отдельно предупреждает, что credit prices не определяют включённое использование подписки. Поэтому таблица не объясняет списание недельной квоты Pro, а наблюдаемые 2,13 раза по output не превращаются в 2,13 раза по оплате. Реальная сравнительная стоимость одинаковых задач также зависит от количества обработки; одинаковых повторов задач здесь нет.

Fast и другие режимы по логам не установлены: `service_tier` отсутствует. Для MSI есть resolved medium при переключении, но нет per-call подтверждения requested/served effort. Исходный вопрос «6.1 сама по себе съедает вдвое больше?» **не доказан ни логами, ни опубликованной таблицей credits**. При этом ускоренное исчерпание, задержки и ухудшение полезного результата остаются предметом инцидента.

## 3. Дополнительная техническая сводка

### Контекст и compression

| Наблюдаемая metadata | Lenovo | MSI |
|---|---|---|
| Максимальный input в общем окне | 448 385 | 449 060 |
| Медианный input в общем окне | 168 520 | 149 657 |
| Compression attempts в общем окне | 26: 24 committed, 2 explicit_interrupt | 12: все committed |
| Наблюдаемые context / threshold | 900 000/450 000; 872 000/436 000; 500 000/375 000 | 900 000/450 000 — 4; 500 000/375 000 — 8 |

Compression token fields имеют `token_count_method=estimate_rough` и не добавляются к provider usage. Суффикс `-900k`, клиентский предел контекста, размер фактического input и единицы списания — разные величины. Длительности compression также не объявляются чистым простоем.

### Ошибки и внешние статусы

В общем окне MSI имеет 9 attempts: три overload APIError, один отдельный non-retryable APIError, один HTTP 403, один HTTP 503 и три progress watchdog TimeoutError. Lenovo имеет 237 attempts; расширенная классификация по `error_type` даёт 207 APIConnectionError, 14 APIError, 5 TimeoutError, 3 APITimeoutError, 5 PermissionDeniedError, 2 InternalServerError и 1 ReadError. Более ранние 149 connection-category records в issue относились к более узкому текстовому правилу; общий счётчик 237 не изменился.

GUI в общем окне: Lenovo 291 finishes (285 complete, 1 interrupted, 5 error); MSI 79 (77 complete, 1 interrupted, 1 auth error). После границы появляются два Lenovo и три MSI GUI outcomes по rate_limit. Количество внешних complete не равно числу полезных выполненных задач и не исключает восстановленные ошибки внутри turn.

В common на MSI явно запущено 36 subagent sessions при одной основной desktop session; CLI-запуски учитываются отдельно. Это совместимо с сообщением владельца об одном рабочем потоке Jarvis. На Lenovo в том же окне 198 явно стартовавших subagent sessions. Эти числа не превращаются в количество отдельных пользовательских заданий.

### Покрытие источников

| Источник | Фактическое покрытие |
|---|---|
| Lenovo agent-ротации | 18.10 23:04:46.396 → 22.10 11:51:54.702 |
| Lenovo successful calls | 18.10 23:04:56.513 → 22.10 11:46:06.247 |
| Lenovo offline DB counters | 08.09 09:02:23 → 22.10 11:46:06; session/model aggregates |
| Lenovo errors-ротации | С 01.10; full retained failed-attempt census 645 |
| MSI agent | 20.10 12:04:02.662 → 22.10 12:25:02.197 |
| MSI successful calls | 20.10 14:12:51.359 → 22.10 11:46:26.453 |
| MSI errors | 20.10 12:04:18.197 → 22.10 12:25:01.780 |
| MSI GUI | 20.10 12:04:08.206 → 22.10 11:49:37.650 |

Более ранние GUI/desktop записи Lenovo не содержат сопоставимого журнала successful-call usage за первый Pro-период. Нельзя достроить такой журнал из одних ошибок. Метки времени воспроизводятся из источников; UTC+3 следует ранее установленной интерпретации, отдельный NTP-аудит не проводился.

## 4. Что осталось выяснить

**Обе машины уже разобраны; полная сохранённая выгрузка счётчиков Lenovo теперь получена и проверена.** Дополнение расширяет историю до 8 сентября и устанавливает main/auxiliary/log overlap. Следующий недостающий слой — движение недельного остатка и купленных credits с фактически применёнными условиями аккаунта. Публикация имеющихся результатов не зависит от получения ответа провайдера. Сохранённый снимок остатка сразу после покупки помог бы сузить неизвестный начальный баланс, но отсутствие снимка не мешает предъявить подтверждённое исчерпание.

Сохраняются четыре границы вывода:

1. Рост внутренней генерации и добавка MSI измерены; причина роста и полезность каждой операции по метаданным не установлены.
2. Успешные вызовы не охватывают полную auxiliary/partial/server-side usage. Доли токенов не равны долям списания квоты.
3. Первоначальные пять суток Pro и нынешние примерно 2,5 суток — наблюдения владельца. Получена выгрузка сохранённых session/model counters с 8 сентября, но нет одинаковых задач, полного per-call журнала старого периода и начальных балансов для контролируемого сравнения.
4. Не доказаны завышенное списание за ошибки, массовая потеря кэша, скрытая подмена модели или включение Fast. Эти объяснения не подставляются вместо provider reconciliation.

Пакет **CODEX-SHARED-BUDGET-EVIDENCE-2026-10-22.zip** содержит манифест 13 оригиналов, совместный CSV из 19 417 completed-call records, отдельные технические JSON по машинам, quota payloads, retry chains, метаданные маршрутизации и lifecycle, а также скрипты воспроизведения основных счётчиков. Исходные логи, содержимое задач, ответы модели, вывод инструментов и auth-файлы не включены. Session IDs заменены псевдонимами/отпечатками; response IDs сохранены для серверной корреляции.

Исходные логи не изменены. Публичный incident #6 и адресные дополнения OpenAI/Hermes опубликованы; точные ссылки приведены ниже. Изменение конфигурации, обновление Hermes и дополнительные модельные прогоны не выполнялись.


## 5. Публикации и конкретный механизм повторов Hermes

Материалы опубликованы 22 октября 2026 года; содержание каждого из пяти комментариев прочитано обратно и совпало с отправленным текстом.

| Площадка | Опубликованное дополнение |
|---|---|
| Наш инцидент #6 | [Хронология, общий бюджет и проверенные измерения](https://github.com/gv1983us-commits/codex-provider-incident/issues/6#issuecomment-6338958052) |
| OpenAI #54677 | [Переход Pro → Plus и отдельная новая Pro-подписка](https://github.com/openai/codex/issues/54677#issuecomment-6338959315) |
| OpenAI #45030 | [Долгие ожидания после перехода на новый Pro](https://github.com/openai/codex/issues/45030#issuecomment-6338960289) |
| Hermes #131743 | [Один ход 61м19,3с с тремя 300s stalls](https://github.com/NousResearch/hermes-agent/issues/131743#issuecomment-6338970984) |
| Hermes #41590 | [Короткие повторы после недельного исчерпания квоты](https://github.com/NousResearch/hermes-agent/issues/41590#issuecomment-6338979182) |

[Основной коммит с отчётом и исправленными статусами](https://github.com/gv1983us-commits/codex-provider-incident/commit/e39b3b430b6c62bdddbbc0ec42928eb16881ad75). Исходные логи и реквизиты аккаунтов не публиковались.

### Почему недельный отказ повторяется через секунды: проверка исходника MSI

В ревизии `d923a5eb487a17c0a68801bba9ef0f56c81040a8` прослеживается конкретное сочетание условий:

1. [Классификатор HTTP 429](https://github.com/NousResearch/hermes-agent/blob/d923a5eb487a17c0a68801bba9ef0f56c81040a8/agent/error_classifier.py#L1073-L1088) относит quota wall с сигналом reset к категории `rate_limit`.
2. [Проверка сигнала](https://github.com/NousResearch/hermes-agent/blob/d923a5eb487a17c0a68801bba9ef0f56c81040a8/agent/error_classifier.py#L1238-L1268) принимает поля `resets_at` / `resets_in_seconds`, в том числе с многодневным горизонтом.
3. [Извлечение паузы для повтора](https://github.com/NousResearch/hermes-agent/blob/d923a5eb487a17c0a68801bba9ef0f56c81040a8/agent/retry_utils.py#L185-L195) использует `Retry-After` / `retry_after`; другие поля reset сами по себе эту паузу не задают.
4. [Расчёт backoff](https://github.com/NousResearch/hermes-agent/blob/d923a5eb487a17c0a68801bba9ef0f56c81040a8/agent/turn_recovery.py#L1401-L1435) допускает короткие jittered delays и отдельно формирует текст с временем reset.

Это согласуется с наблюдаемым MSI payload `resets_in_seconds=431887` и ближайшими паузами **2,588314876с** и **4,666180805с**. Перед нами объяснение на уровне прочитанного исходника, согласованное с логами. Фактически загруженный runtime и переопределения плагинов не трассировались; Lenovo имеет другую ревизию.

Нужно различать долгосрочное исчерпание allowance и краткое ограничение частоты запросов, прекращать бесполезные немедленные повторы исчерпанного маршрута и показывать реальный reset. Альтернативный маршрут может использоваться только в рамках уже выбранной пользователем конфигурации.

Этот механизм относится к **повторам после отказа по квоте**. Он не объясняет её предшествующее исчерпание или более ранние 300s stalls. Старый Hermes #97315 с бесконечным rotate-to-self циклом отличается от наших ограниченных цепочек 3/5; регрессия того старого бага здесь не заявляется.


