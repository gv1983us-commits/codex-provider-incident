# Evidence and provenance

## New corpus — 15 September 2026

[log-audit-20260915.json](log-audit-20260915.json) publishes the 118-entry Codex failed-attempt ledger, a 1,013-entry diagnostic index, selected exact message excerpts, the five source-file hashes and the two observation windows. [Current report](../docs/CONTINUING-INCIDENT-2026-09-15.md).

The last 48 hours contain 100 failed Codex attempts. This is a separate window after repository commit `4a398261`, including some 11–12 September events already discussed in issue #1. It must not be added wholesale to the older counters below. The 12 September clean-install and retained-session scope corrections remain applicable. Source line references identify the retained raw files by the hashes in this new corpus.

## Earlier evidence guide — 10 September

[Home](../README.md) · [Incident](../docs/INCIDENT.md)

## What is published here

| File | Evidence type | How to interpret it |
| --- | --- | --- |
| [excerpts.json](excerpts.json) | Two historical log message fields, a later UI transcription and a new GPT-5.5 receipt | Original source metadata is separate. The new receipt confirms provider/model; screenshot labels alone do not. |
| [astra-20260910T141745876Z.json](astra-20260910T141745876Z.json) | Later exact Astra `unknown` error details, including request ID | Separate from the 29-message export and original audit; approximately ten additional tools are an owner report, not an audited count |
| [audit-summary.json](audit-summary.json) | Derived counts and chronology | Transcribed from the retained audits; the raw logs are not bundled |
| [builder-pro-light-20260910.json](builder-pro-light-20260910.json) | Owner-reported same-Builder resumption; reset history transcribed from owner screenshot | Three received, three used, Available 0; initiator and 97% → 100% change are owner observations |
| [conservation-20260910.json](conservation-20260910.json) | Owner-supplied conservation report and explicit stale-banner correction | Sol completion is reported; identical Astra request ID at a later displayed time is not a new failure count |
| [light-sol-20260910T152154745Z.json](light-sol-20260910T152154745Z.json) | Exact Sol `overloaded` receipt; owner attributes it to Light | Separate from the stale Astra redisplay and original audit counters |
| [route-matrix.json](route-matrix.json) | Account owner's browser and Hermes observations | Unknown model IDs or timestamps remain unknown |

The original 8–10 September source corpus consists of eight logs spanning 8–10 September and a saved session export of 21,492,423 bytes / 4,400 messages. A retained index contains 260 evidence records: 259 from source logs plus one later user error receipt. The receipt was not added to the historical 181 Codex / 151 overload counters.

Deduplication used **timestamp including milliseconds + logger + message**. Buffered Desktop output is not a reliable measure of retry frequency.

Counts apply to different scopes:
- **181 Codex main-loop failed attempts:** 151 overload + 20 request-processing errors + 7 rate-limit events + 2 first-event timeouts + 1 unsupported-model error.
- **11 auxiliary overload attempts:** separate from the 181 main-loop failures.
- **1,987 successful Codex calls:** only the detailed success window, 9 September after 15:56 through 10 September 10:40:37, UTC+3.
- The later Terra and GPT-5.5 browser observations do not change those historical counters.

## Fresh-session export, including a later successful phase

[Reviewed event data](session-20260910-reviewed.json) and the [narrative review](../docs/SESSION-2026-09-10.md) cover the new **98,545-byte, 29-message export**, with its source SHA-256. All 29 events are represented with source IDs, times and tool pairing; private payloads are omitted.

Astra 900k / Medium completes five tool calls and a final answer in 51.33 seconds. This is a scoped success after the earlier GPT-5.5 failure; sustained recovery remains unverified. The new export contains no literal overload receipt. Keep it separate from the original 260-record index and the later pasted receipt.

## New receipt after the original audit

At **2026-09-10T12:06:26.863Z**, an owner-supplied Hermes error receipt explicitly names **openai-codex / gpt-5.5 / overloaded**, with `retryable: true`. [Exact receipt](https://github.com/gv1983us-commits/codex-provider-incident/issues/1#issuecomment-5618436481). It follows an initial report of very slow activity in a new GPT-5.5 Ultra session and a subsequent overload screenshot.

This is a new evidence item outside the original eight-log corpus and 260-record index. Historical counters remain unchanged. The receipt has no HTTP status, endpoint, request ID, call role or retry-attempt count. The screenshot and receipt have distinct displayed times and are recorded separately.

## Provenance already public

1. [Primary incident report](https://github.com/NousResearch/hermes-agent/issues/107307).
2. [Exact selected messages and retained-evidence description](https://github.com/NousResearch/hermes-agent/issues/107307#issuecomment-5617927854).
3. [Related independent user reports](https://github.com/Wei-Shaw/sub2api/issues/6739).

The owner's two retained narrative audits are named `HERMES_PROVIDER_ERRORS_AUDIT_2026-09-10.md` and `HERMES_SESSION_FORENSICS_2026-09-10.md`. Those full private audit files are not published here. The exact installed Hermes commit and customization delta remain missing reproduction inputs.

## Request the smallest useful additional sample

Open or comment on a task with:
1. The code path or hypothesis being tested.
2. The timestamp range and fields required.
3. The expected observation that would support or reject the hypothesis.

A focused, reviewed excerpt can then be supplied. Full session exports, credentials, cookies, OAuth files, private task text and internal Jarvis architecture are unnecessary for an initial reproduction.

Keep the distinction between **observed**, **derived**, **hypothesis** and **not yet tested** when adding evidence. Preserve prior observations with their dates; a later successful response does not erase a historical failure.
