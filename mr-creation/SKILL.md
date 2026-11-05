---
name: mr-creation
version: 1.5.0
description: Draft a concise, evidence-based merge request description with a conventional-commit title, ticket, blast radius, key changes, and visual evidence when relevant. Use when the user asks for an MR template or description, or asks to open or create an MR.
---

# Merge Request Creation

Write for a teammate who knows the product and is seeing this change for the first time. They should understand the outcome in one sentence and know where to focus their review after a quick scan.

## Editorial rules

- For a routine MR, aim for 100 to 150 words of prose or fewer, excluding headings, ticket links, and media. A simple fix can be much shorter. Use more space when a reviewer needs it to understand a migration, compatibility issue, or consequential trade-off.
- Give each section one job. The TL;DR states the outcome and why it matters. The blast radius names the reach. The explanation supplies the cause or rationale. The checklist identifies concrete behaviors a reviewer can verify.
- Use familiar product terms and short sentences. Name a function, file, or API only when it helps the reviewer locate a cause or inspect a contract. For example, prefer “Reopening a draft keeps its lane order” to “Improve lane-order persistence handling.”
- Before returning the draft, cut repeated claims, file inventories, routine plumbing, and commentary the diff already makes obvious. Keep any detail whose removal would change how someone understands or reviews the MR.

## Workflow

1. **Inspect the evidence.** Check the source and target branches, commits, and full diff. Account for every material behavior change. Group edits that produce one outcome.
2. **Resolve the ticket.** Find and read it using the request, branch, commits, or repository integration. Use the exact label `<ticket number> - <ticket title>`. Ask for any value that remains ambiguous.
3. **Write the title.** Use `<type>(<optional scope>): <imperative summary>` in lowercase, with no trailing period. Choose `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `style`, or `perf`. Describe the whole MR so the title works as its squash commit.
4. **Write the opening lines.** Aim for 20 words or fewer in the TL;DR. Reserve risk and affected scope for the blast radius. Choose its level based on reach across behavior, shared dependencies, contracts, persisted state, or external consumers. Name the affected surface in human terms and quantify it when evidence allows. Keep one blank line between the quote blocks.
5. **Explain what needs context.** For a fix, open with **Root cause** and identify the code path and decision that produced the symptom. One sentence is usually enough. Add rationale only where it helps explain the chosen behavior. Include a Mermaid diagram only if it makes a flow or state change quicker to grasp than prose.
6. **Build the checklist from the diff.** Use one item per material behavior, usually one to three items. Mark confirmed changes `[x]` and known remaining work `[ ]`. Keep routine test and typecheck results out. Mention an unusual verification gap briefly in the explanation when it affects review.
7. **Include optional sections selectively.** Add *Points worth challenging* only for a real decision a reviewer could reasonably contest. Add *Preview* for user-facing visual changes. Prefer paired screenshots of the same scenario, capturing the before state from pre-fix code and restoring the branch afterward. Use a recording for motion or multi-step interactions. If media is pending, use explicit placeholders.
8. **Return or publish.** Put the proposed title above the Markdown body. Use only the sections below and omit inapplicable optional sections. Never invent ticket details, completed work, or media. Create the MR only when the user explicitly asks to open or publish it. Confirm the host and target branch from repository evidence, then return the MR URL.

## Template

```markdown
> **TL;DR** <What changes and why it matters, in one sentence>

> **Blast radius** <🟢 Low / 🟡 Medium / 🟠 High / 🔴 Very high> · <affected surface> · <concrete scope>

## Ticket

[<ticket number> - <ticket title>](<ticket URL>)

## What does this MR do and why?

**Root cause** <For fixes, identify the code path and decision that caused the bug.>

<Add only the context needed to understand the change.>

## Key changes

- [x] <Concrete behavior confirmed by the diff>

## Points worth challenging

- <A genuine trade-off and its consequence>

## Preview

### Before

[Before screenshot]

### After

[After screenshot]
```

For a recording, replace the Before and After subsections with `### Screen recording` and `[Screen recording]`.
