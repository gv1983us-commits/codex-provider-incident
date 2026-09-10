<p align="center"><img src="assets/banner.svg" alt="Codex provider incident — September 2026. Evidence, recovery and collaboration." width="100%"></p>

# Codex provider incident · September 2026

**Goal: restore reliable Codex-backed Jarvis operation through Hermes on the affected Pro Full subscription.**

[Русская сводка](docs/README.ru.md) · [Evidence](evidence/README.md) · [Recovery procedure](docs/RECOVERY.md) · [Open tasks](https://github.com/gv1983us-commits/codex-provider-incident/issues) · [Hermes report](https://github.com/NousResearch/hermes-agent/issues/107307)

> [!IMPORTANT]
> **Incident open; sustained recovery unverified.** The new export confirms an **Astra 900k / Medium short text response and five-tool turn**, completed at **12:18:50 UTC**. GPT-5.5 previously returned a confirmed overload. Availability is intermittent; one successful diagnostic run does not establish reliable long-running Jarvis work.

## What works, what fails

| Account | Client / route | Model | Latest reported observation |
| --- | --- | --- | --- |
| Pro Full 20× | Hermes → Codex, earlier tests | Astra / Sol / Terra | 🔴 Earlier overloads; later Astra Medium success recorded below |
| Pro Full 20× | Hermes → Codex, later session phase | Astra 900k / Medium | 🟡 Text + five-tool turn completed; sustained work unverified |
| Pro Full 20× | Official ChatGPT Work | GPT-6 / GPT-5.6 | 🔴 Capacity banner |
| Pro Full 20× | Official ChatGPT Work | GPT-5.5; Max in latest report | 🟡 Earlier response; latest report very slow; no new browser failure established |
| Pro Full 20× | Hermes → Codex, new session | GPT-5.5, Ultra selected | 🔴 Initial activity, then overload; provider/model confirmed by receipt |
| Pro Full 20× | Ordinary ChatGPT Conversation | Model not recorded | 🟢 Conversation works |
| Pro Light | Official ChatGPT Work | Working session; exact model not recorded in this comparison | 🟢 Work works |

**Controls:** the same home Windows laptop, network and VPN; both Edge and Yandex tested; ChatGPT's built-in account switcher used in Yandex. The Full-account browser failure persists with Hermes shut down. The Pro Light account works in the same environment.

This narrows the failure to an **account/model/route-dependent pattern**. It does not identify an internal server pool, prove an account restriction, or establish the exact server-side cause. A short-prompt test in a fresh Work conversation on the affected account is still useful to separate conversation state from model/account routing.

The [new exact receipt](https://github.com/gv1983us-commits/codex-provider-incident/issues/1#issuecomment-5618436481) supersedes the initial optimistic GPT-5.5 Hermes report. It is recorded separately from the historical audit counters. The new export supplies message-level timings below; the cause of the earlier reported slowness is not isolated.

**New evidence:** [29-message session review](docs/SESSION-2026-09-10.md) and [reviewed event data](evidence/session-20260910-reviewed.json). Astra's five-tool analysis finished in **51.33 s**. Earlier delays include **32.50 s inside `hermes doctor`**; the comparison does not isolate reasoning effort as the cause.

## Evidence at a glance

| Finding | Scope |
| --- | --- |
| **151** main-loop overload failures | Deduplicated across eight session IDs: 102 Sol, 49 Astra |
| **11** auxiliary overload failures | Includes two failed context summaries |
| **3 attempts total** | Observed retry budget; no infinite rotation demonstrated |
| **1,987** successful Codex calls | Detailed log window, 9 Sep after 15:56 through 10 Sep 10:40:37, UTC+3 |
| **41%** weekly allowance remaining | Snapshot at 10:33 on 10 Sep, UTC+3; not a universal quota clearance |
| **850 → 82**, **328 → 102** messages | Active histories shortened after summary generation failed |

The counts have different scopes. **Do not compute an overall incident failure percentage from this table.** Earlier logs do not contain a complete record of successful calls. Terra is a later owner observation; it is not part of the historical 151 Sol/Astra count.

## Where we can act

1. **Recover a usable route.** GPT-5.5 also has a confirmed Codex overload receipt. Preserve that failure and require a complete model → tool → model cycle plus sustained useful progress before declaring any route recovered.
2. **Make routing consistent.** Inspect the live main model, explicit workers, auxiliary jobs, compression and fallback targets. A browser response or a changed dashboard default does not prove these routes changed.
3. **Preserve work through rejection.** Investigate retry exhaustion, safe resumption after completed tools, and compression commits when the new summary fails.
4. **Share a reproducible case.** Pin the installed Hermes revision and relevant customizations, then compare a small reproduction with upstream behavior.

The procedure and acceptance criteria are in [RECOVERY.md](docs/RECOVERY.md). No recovery patch has been deployed from this repository.

## Active tasks

| Task | Current owner / invitation | Status |
| --- | --- | --- |
| [#1 · Validate sustained Codex work](https://github.com/gv1983us-commits/codex-provider-incident/issues/1) | Account owner | Astra Medium short tool run passed; sustained recovery open |
| [#2 · Preserve continuation after retry exhaustion](https://github.com/gv1983us-commits/codex-provider-incident/issues/2) | `huklaa` invited; formal assignment pending | Awaiting contributor confirmation |
| [#3 · Check compression failure and recovery](https://github.com/gv1983us-commits/codex-provider-incident/issues/3) | Account owner coordinates; help wanted | Installed revision and code review needed |
| [#4 · Compare results with Sub2API](https://github.com/gv1983us-commits/codex-provider-incident/issues/4) | Project owner and two relevant reporters invited | Awaiting replies |

The [invitation and repository handoff](https://github.com/NousResearch/hermes-agent/issues/107307#issuecomment-5618225579) are posted in the original Hermes thread. These are coordination states, not completed recovery checks.

## Start here

| Material | Purpose |
| --- | --- |
| [Incident report](docs/INCIDENT.md) | Controlled comparison, chronology and known limitations |
| [Fresh-session review](docs/SESSION-2026-09-10.md) | GPT-5.5/Sol attempts, later Astra Medium tool success and measured intervals |
| [Controls → requests](docs/REQUEST-MAP.md) | Source-based model/effort mappings and the next bounded recovery observation |
| [Evidence guide](evidence/README.md) | Provenance and how to request a focused excerpt |
| [Exact excerpts](evidence/excerpts.json) | Historical log fields, a screenshot transcription and the new GPT-5.5 receipt |
| [Audit summary](evidence/audit-summary.json) | Counts and the final interruption sequence |
| [Route matrix](evidence/route-matrix.json) | Machine-readable observations, with unknowns preserved |
| [Recovery procedure](docs/RECOVERY.md) | Small tests, route coverage and completion criteria |
| [Contributing](CONTRIBUTING.md) | Report a result, investigate a code path, or propose a patch |

## Join the investigation

- **Have the same symptom?** [Submit a comparable observation](https://github.com/gv1983us-commits/codex-provider-incident/issues/new?template=observation.yml). Record the exact model, route, timestamp, account tier and outcome.
- **Can help with Hermes?** Pick a focused [open task](https://github.com/gv1983us-commits/codex-provider-incident/issues), say what you will investigate, and propose a small PR or diagnostic.
- **Have a working route?** Distinguish a text response from a successful tool loop and sustained work.

Coordination with Hermes remains in [NousResearch/hermes-agent#107307](https://github.com/NousResearch/hermes-agent/issues/107307). A related independent user-report thread is [Wei-Shaw/sub2api#6739](https://github.com/Wei-Shaw/sub2api/issues/6739); reports there are corroborating observations, not proof of a shared root cause.

This is an independent incident repository maintained by the affected account owner. English material was prepared with AI assistance from owner observations and retained audits. It contains a public evidence subset; full private sessions and Jarvis architecture are outside its scope.
