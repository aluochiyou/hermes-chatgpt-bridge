---
name: quota-smart-codex
description: Run a quota-conscious coding workflow where ChatGPT Web plans and reviews through a least-privilege connector while Codex executes, tests, and recovers locally. Use when the user wants to conserve Codex usage without treating it as a quota bypass.
---

# Quota-smart Codex

Use this skill to separate reasoning from execution without weakening verification.

## Outcome

Keep Codex responsible for local writes, terminal commands, tests, Git operations, and recovery. Use ChatGPT Web for requirements analysis, plans, architectural trade-offs, and independent review when a working connector is available.

This workflow reallocates work across product surfaces the user already has. It does **not** bypass, increase, pool, or guarantee any ChatGPT or Codex quota.

## Default routing

1. Start with a read-only workspace connector. Let ChatGPT inspect only the files, Git state, and released test outputs it needs.
2. Ask ChatGPT for a compact plan that names files, rationale, and acceptance checks.
3. Make changes locally in Codex, then run the requested deterministic checks.
4. Record a compact execution summary and have ChatGPT review the actual diff and test status through the connector.
5. Finish only when verification passes or the user accepts a stated limitation.

Use a write-capable connector such as DevSpace or CodexPro only when the user explicitly asks ChatGPT to edit files or run commands. Before that escalation, state that the web client gains write and/or command authority and narrow its allowed workspace roots.

## Context discipline

- Do not paste whole files, diffs, terminal logs, credentials, or private keys into the web chat. Let the connector retrieve minimal relevant context.
- Keep control messages short: task, task state, changed-file count, checks, and next question. The connector is the data plane; the chat is the control plane.
- Maintain one active ChatGPT conversation per workspace unless the existing one is broken or the user requests a new conversation.
- Treat ChatGPT output as advice until local checks verify it.

## Task protocol

For non-trivial work, maintain this sequence:

`PLAN → EXECUTING → EXECUTED → REVIEW → DONE | REVISE | BLOCKED`

At `PLAN`, require concrete files, reasons, and checks. At `EXECUTED`, provide only the high-level result and direct the reviewer to the connector. At `REVIEW`, inspect the real diff and test record, not a pasted summary.

## Recovery boundaries

- If a ChatGPT connector fails, first run its documented health check and repair its local side. Do not recreate connectors just because a browser interaction timed out.
- A temporary public address changes after the local bridge restarts. Replace only the matching workspace connector after confirming the new endpoint is healthy.
- Stop automated browser configuration after two materially different failed attempts at the same configuration step. Give the user one explicit manual action, then resume verification.
- On Windows proxy or UDP-restricted networks, diagnose the browser-control process and tunnel separately before changing connector configuration.

## Security checklist

- One workspace per connector whenever possible.
- Deny secret paths and keep local-only configuration out of the repository.
- Prefer OAuth or an equivalent scoped authentication mechanism for public endpoints.
- Never claim a connection is working merely because a server returned an HTTP status. Verify a real workspace read in the intended ChatGPT conversation.
