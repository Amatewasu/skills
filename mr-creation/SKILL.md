---
name: mr-creation
version: 1.2.0
description: Draft or create a merge request with a ticket link, a concise explanation of what changed and why, an evidence-based completion checklist, a reviewer guide (reading order, points to challenge, how to test), and before/after screenshots or a screen recording. Use when the user asks for an MR template, MR description, a "how to review" for an MR, or to open/create a merge request.
---

# Merge Request Creation

Prepare an MR description from repository evidence, then create the MR only when the user explicitly asks to publish or open it.

The description serves two readers: someone who wants to know *what changes and why* (lead, PO, future archaeologist), and the reviewer who needs to know *where to look and how to check it*. Key changes answers the first at behavior level; How to review answers the second at file level. Keep them from overlapping: Key changes never names files, How to review never re-explains behavior.

## Workflow

1. Inspect the current branch, target branch, commits (`main..HEAD`), and diff (`main...HEAD`, not the working tree, which may be clean once everything is committed). Account for every material change included in the MR.
2. Resolve the ticket URL from the user's request, branch name, commits, or repository integrations. Read the ticket and copy its number and title into the link label using `<ticket number> - <ticket title>`. Ask for any value that cannot be resolved unambiguously.
3. Write a short explanation of what the MR does and why it is needed. Describe behavior and intent rather than listing files. Format for scanning: short bullets over paragraph blocks, bold key words (component, file, API, error message), and no text chunk longer than a few lines. Stay brief: 2 to 3 bullets, the problem and the answer to it. Details of the change belong in Key changes, not here.
4. When a Mermaid diagram helps a human understand the change faster than prose — a flow, state machine, sequence, or architecture shift — include one in the explanation section. Diagrams are welcome but optional; skip them when the change is simple or localized.
5. Build the key-changes checklist from the diff:
   - Each item names one material behavior change from the diff, nothing else.
   - Keep the list brief and focused on material behavior.
   - Use `- [x]` for changes present in the branch, `- [ ]` for known remaining work.
6. Build the **How to review** section for the reviewer:
   - **Suggested reading order**: 3 to 5 numbered blocks, one per layer, riskiest first (access control, data queries, migrations, permissions or money logic), then API, then client, then docs. Flag the riskiest block in its title, e.g. "(the part that matters)". Inside each block, one sub-bullet per file: name it and say *what to check* in one line, not what the code does. Point to non-obvious gotchas already commented in the code. Put tests in the block of the code they cover rather than in a block of their own. Fold pure renames and mechanical changes into a single "Rename-only changes" sub-bullet. A block with a single file can stay on one line.
   - **Points worth challenging**: deliberate trade-offs or known limits a reviewer might question (pagination, performance, backward compatibility, defaults). Omit when there are none.
   - **How to test**: automated test commands for the touched area, plus 2–4 manual scenarios a reviewer can click through. Keep commands short: prefer the repository's own tasks (`deno task`, `npm run`, `make`) and `-A` over long lists of permission flags; keep the env flags and options the repository requires (e.g. DB test switches).
7. Verification *claims* ("typecheck passes", "tests pass locally") never appear anywhere in the description, since the reviewer cannot trust them and they go stale. Verification *instructions* telling the reviewer how to check belong in How to test. Mention verification status only when it carries special meaning (tests skipped, known flaky test, unusual setup required).
8. Add preview evidence for user-facing visual changes:
   - Prefer paired before and after screenshots.
   - Use a screen recording when motion or a multi-step interaction communicates the change better.
   - When preview media is not ready, insert explicit replaceable placeholders such as `[Before screenshot]`, `[After screenshot]`, or `[Screen recording]`.
   - Omit the Preview section entirely for changes with no visible effect.
9. Produce the Markdown using the template below. Use only its sections. Never invent ticket URLs, ticket numbers, ticket titles, completed work, or preview assets.
10. After the description, outside of it, tell the user anything you noticed that does not belong in the MR but needs their attention: missing ticket, duplicate or fixup commits worth squashing, test commands you did not run (flag them as unverified), placeholders left to fill.
11. When the user explicitly requested MR creation, determine the repository host and target branch, create the MR with the prepared title and description, and return its URL. Otherwise, return the draft only.

## Template

```markdown
## Ticket

[<ticket number> - <ticket title>](https://ticket-url)

## What does this MR do and why?

<Short explanation of the behavior changed and its purpose. Prefer short bullets with **bold key words**; no long paragraph blocks. Optionally embed a Mermaid diagram here when it clarifies a flow, sequence, or architecture.>

## Key changes

- [x] <Verified completed change>
- [x] <Verified completed change>
- [ ] <Known remaining work, when applicable>

## How to review

### Suggested reading order

1. **<Riskiest area> (the part that matters)**
   - `path/to/file.ts`: <what to check>
   - `path/to/file.test.ts` covers <what>
2. **API**
   - `path/to/router.ts`: <what to check>
3. **Client**
   - `path/to/api.ts`: <what to check>
   - Rename-only changes: `A.tsx`, `b.ts`
4. **Docs**: `docs/feature.md`

### Points worth challenging

- <Trade-off or known limit>

### How to test

- Automated: `<command>`
- Manual:
  - <Scenario and expected result>
  - <Scenario and expected result>

## Preview

### Before

[Before screenshot]

### After

[After screenshot]

<!-- Replace the sections above with "### Screen recording" and `[Screen recording]` when more appropriate. -->
```

Omit empty optional subsections. Keep the final description concise and ready to paste or publish.
