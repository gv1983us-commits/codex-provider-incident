# Direct ChatGPT Pro Light incomplete turns from the shared session graph — 17 September 2026

**Evidence status: session-graph confirmed; server-side cause unknown.** The public ChatGPT share page exposes a React Router stream containing a devalue-style JSON reference array. Decoding that array yields the conversation mapping, message identifiers, parent/child topology, timestamps, model slugs, node status and `end_turn` fields.

This receipt upgrades two same-day Direct ChatGPT observations from owner-reported waiting periods to reproducible session-graph evidence. It does **not** convert them into provider request logs: the share graph exposes no direct ChatGPT request ID, HTTP response, hidden retry ledger or server exception.

The Pro Light account tier and Yandex Browser client are supplied by the owner. The public session graph confirms the conversation events and model slug but does not encode those account/client fields.

## Source and integrity

| Field | Value |
| --- | --- |
| Public share | `https://chatgpt.com/share/6aabeaa8-4504-83ed-918d-b18808b38bc3` |
| Conversation ID | `6aabeaa8-4504-83ed-918d-b18808b38bc3` |
| HTML title | `ChatGPT - Связанные проекты и память` |
| Captured HTML size | 12,181,209 bytes |
| Captured HTML SHA-256 | `a6f97c1ae94c761cddaf0e05dc11d9852146fceb4511ffcf0d6626040100673d` |
| Decoded reference-array values | 189,037 |
| Conversation mapping nodes | 3,929 |
| Message nodes | 3,928 |
| Time zone below | Europe/Istanbul, UTC+03:00 |

The full 12 MB HTML and the private conversation text are intentionally not mirrored into this repository. The public receipt retains only the source hash, structural event fields, message IDs, timestamps, text lengths and text hashes needed to verify the two incidents.

## Case 1 — morning incomplete turn

| Event | Local time | Message ID | Structural result |
| --- | --- | --- | --- |
| User prompt | 10:24:48.658 | `7b512f96-a335-4aed-b91c-e3bbfb52ae61` | Turn begins |
| Non-empty assistant partial | 10:25:17.187 | `12d2f586-75fe-5ede-a2d8-53c617120a17` | 505 chars, `gpt-5.6-sol-wm`, `end_turn=false` |
| Last assistant activity | 10:26:35.271 | `77d802c1-98f0-4177-b37d-612fee57c6de` | Empty, `gpt-5.6-sol-wm`, `end_turn=false` |
| User interruption/complaint | 12:15:49.026 | `5189967c-2820-4b9d-a87d-92854157867b` | Its parent is the last non-final assistant node |
| Final reply to the new user message | 12:16:25.334 | `71a3b778-1fdd-5f58-a4af-9e4ed9be7948` | 538 chars, `end_turn=true` |

Between the original prompt and the next user message, the graph contains 13 assistant nodes and one redacted tool-result node. Twelve assistant nodes explicitly carry `end_turn=false`; none carries `end_turn=true`. One model node contains the 505-character partial answer, while subsequent model nodes are empty.

- Prompt → user interruption/complaint: **6,660.368 s = 1:51:00.368**.
- Last assistant activity → user interruption/complaint: **6,553.755 s = 1:49:13.755**.
- New user message → first final reply: **36.308 s**.

The following user message at 12:17:16.468 explicitly records that no answer appeared and the owner pressed stop. A repeated 4,701-character answer then completed at 12:17:40.028 with `end_turn=true`.

## Case 2 — afternoon incomplete turn

| Event | Local time | Message ID | Structural result |
| --- | --- | --- | --- |
| User prompt | 12:21:27.771 | `deb352be-621e-4712-bf00-5b6902cef313` | Turn begins |
| Non-empty assistant partial | 12:21:54.754 | `d774120c-b990-5f87-a423-10aaf08c8a0b` | 440 chars, `gpt-5.6-sol-wm`, `end_turn=false` |
| Last assistant activity | 12:24:13.909 | `25ff8369-5071-40dc-98b0-6c6fe6bc7887` | Empty, `gpt-5.6-sol-wm`, `end_turn=false` |
| User complaint/re-prompt | 15:53:37.266 | `59f48060-fd84-4c67-9894-04ce351b7d4f` | Its parent is the last non-final assistant node |
| Final reply to the new user message | 15:54:02.835 | `c6d5a401-2d77-5982-81c8-7da50d729b82` | 1,012 chars, `end_turn=true` |

Between the original prompt and the next user message, the graph contains 17 assistant nodes. All 17 carry `end_turn=false`; none carries `end_turn=true`. One contains the 440-character partial answer and the remaining nodes are empty.

- Prompt → user complaint/re-prompt: **12,729.495 s = 3:32:09.495**.
- Last assistant activity → user complaint/re-prompt: **12,563.356 s = 3:29:23.356**.
- New user message → first final reply: **25.570 s**.

## Important status semantic

The affected assistant nodes are labelled `status=finished_successfully` even while `end_turn=false`, including the final empty nodes to which the next user messages are attached. Therefore an individual node's `finished_successfully` value is **not** proof that the user turn completed or that a final response was delivered.

For incident handling, completion must be evaluated at turn level: a terminal assistant event, `end_turn=true`, delivery acknowledgement where available, and visible client completion must not be collapsed into one generic node status.

## What this confirms

1. Both affected Direct ChatGPT Pro Light prompts entered assistant processing on `gpt-5.6-sol-wm`.
2. Both produced a non-empty partial model node followed by repeated empty, non-final assistant nodes.
3. Neither original turn produced an `end_turn=true` assistant node before the owner interrupted or re-prompted.
4. The user-visible incomplete-turn windows were at least 1:51:00.368 and 3:32:09.495.
5. A new user message caused a final answer to appear in 36.308 s and 25.570 s respectively, so the conversation itself was not permanently dead.
6. These events occurred in Direct ChatGPT, outside the Hermes request path, on the same date as the separately logged Hermes/Codex degradation.

## What this does not prove

- The share graph does not expose the Direct ChatGPT provider request IDs, HTTP status, retry count, stream disconnect reason or server exception.
- The elapsed windows are lower-bound user-visible stall intervals, not measured model compute or single-request durations.
- This evidence cannot identify the failing OpenAI component or prove that Direct ChatGPT and Hermes used the same serving pool or backend route.
- Temporal coexistence justifies provider-side correlation; it does not by itself establish a common cause.
- The Pro Light tier and Yandex Browser attribution come from the owner, not from fields exposed in the share graph.

## Machine-readable receipt

The minimized receipt is [`evidence/direct-chatgpt-share-timing-20260917.json`](../evidence/direct-chatgpt-share-timing-20260917.json). It includes the two event chains, exact durations, structural counts, source integrity fields, confirmed interpretations and explicit non-claims.
