# Contributing to this investigation

[Home](README.md) · [Open tasks](https://github.com/gv1983us-commits/codex-provider-incident/issues) · [Upstream incident](https://github.com/NousResearch/hermes-agent/issues/107307)

The immediate objective is reliable Codex-backed Hermes operation on the affected Full account. Useful contributions are small reproductions, effective-route checks, code-path analysis and focused recovery patches.

## Report an observation

Use the observation issue form. Include:
- Timestamp with timezone; client and exact model identifier.
- Account tier as displayed, without account identifiers.
- Whether the request was ordinary Conversation, official Work or Hermes Codex.
- Exact failure text or a precisely scoped success.
- Installed Hermes revision and relevant customizations, if applicable.
- Whether tools ran and whether the model continued after their result.

Label an explanation as a hypothesis until evidence supports it. Different 429, overload, authentication, timeout and unsupported-model errors should remain separate. Other users' similar symptoms do not establish one common internal cause.

## Work on a task

Comment with the path you will investigate and the smallest evidence excerpt needed. A task can be coordinated here even when upstream assignment is unavailable. Public issue participation and fork-based PRs do not require access to the account owner's runtime.

For a proposed code change:
1. Identify the upstream revision and relevant customized behavior.
2. Describe the failing state transition and expected result.
3. Provide a focused diff or link to an upstream PR.
4. Demonstrate it on a harmless fixture or mocked provider failure.
5. State what still needs verification on the affected setup.

There is no Hermes source fork or deployed recovery patch in this repository yet. A proposal can be submitted under `proposals/` with reproduction steps and a patch against a named upstream commit. Do not claim a fix based only on a changed final-answer prompt.

## Public evidence scope

Only publish material needed to reproduce or understand the incident. Review excerpts for OAuth tokens, cookies, authorization headers, account identifiers, private prompts and internal project material. Do not attach an entire raw session or configuration file as a shortcut.

Preserve dates and evidence provenance. Corrections should say what changed in the interpretation instead of rewriting historical observations as if a later finding had always been known.

English and Russian contributions are welcome. AI-assisted analysis should be checked against evidence; source authorship and review status must remain clear.
