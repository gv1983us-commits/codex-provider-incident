# Evidence and provenance

[Home](../README.md) · [Incident](../docs/INCIDENT.md)

## What is published here

| File | Evidence type | How to interpret it |
| --- | --- | --- |
| [excerpts.json](excerpts.json) | Two historical log message fields, a later UI transcription and a new GPT-5.5 receipt | Original source metadata is separate. The new receipt confirms provider/model; screenshot labels alone do not. |
| [audit-summary.json](audit-summary.json) | Derived counts and chronology | Transcribed from the retained audits; the raw logs are not bundled |
| [route-matrix.json](route-matrix.json) | Account owner's browser and Hermes observations | Unknown model IDs or timestamps remain unknown |

The source corpus consists of eight logs spanning 8–10 September and a saved session export of 21,492,423 bytes / 4,400 messages. A retained index contains 260 evidence records: 259 from source logs plus one later user error receipt. The receipt was not added to the historical 181 Codex / 151 overload counters.

Deduplication used **timestamp including milliseconds + logger + message**. Buffered Desktop output is not a reliable measure of retry frequency.

Counts apply to different scopes:
- **181 Codex main-loop failed attempts:** 151 overload + 20 request-processing errors + 7 rate-limit events + 2 first-event timeouts + 1 unsupported-model error.
- **11 auxiliary overload attempts:** separate from the 181 main-loop failures.
- **1,987 successful Codex calls:** only the detailed success window, 9 September after 15:56 through 10 September 10:40:37, UTC+3.
- The later Terra and GPT-5.5 browser observations do not change those historical counters.

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
