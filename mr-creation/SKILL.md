---
name: mr-creation
description: Draft or create a merge request with a ticket link, a concise explanation of what changed and why, an evidence-based completion checklist, and before/after screenshots or a screen recording. Use when the user asks for an MR template, MR description, or to open/create a merge request.
---

# Merge Request Creation

Prepare an MR description from repository evidence, then create the MR only when the user explicitly asks to publish or open it.

## Workflow

1. Inspect the current branch, target branch, commits, and diff. Account for every material change included in the MR.
2. Resolve the ticket URL from the user's request, branch name, commits, or repository integrations. Read the ticket and copy its number and title into the link label using `<ticket number> - <ticket title>`. Ask for any value that cannot be resolved unambiguously.
3. Write a short explanation of what the MR does and why it is needed. Describe behavior and intent rather than listing files. Format for scanning: short bullets over paragraph blocks, bold key words (component, file, API, error message), and no text chunk longer than a few lines. Stay brief: the goal is a human grasps the MR quickly, not exhaustive coverage of the diff.
4. Build the key-changes checklist from the diff:
   - Each item names one material behavior change from the diff, nothing else.
   - Keep the list brief and focused on material behavior.
   - Verification statements ("typecheck passes", "tests pass locally") are never checklist items; mention verification only when it carries special meaning here (tests skipped, known flaky test, unusual setup required).
   - Use `- [x]` for changes present in the branch, `- [ ]` for known remaining work.
5. When a Mermaid diagram helps a human understand the change faster than prose — a flow, state machine, sequence, or architecture shift — include one in the explanation section. Diagrams are welcome but optional; skip them when the change is simple or localized.
6. Add preview evidence for user-facing visual changes:
   - Prefer paired before and after screenshots.
   - Use a screen recording when motion or a multi-step interaction communicates the change better.
   - When preview media is not ready, insert explicit replaceable placeholders such as `[Before screenshot]`, `[After screenshot]`, or `[Screen recording]`.
7. Produce the Markdown using the template below. Use only its sections; do not add a Verification section. Never invent ticket URLs, ticket numbers, ticket titles, completed work, or preview assets.
8. When the user explicitly requested MR creation, determine the repository host and target branch, create the MR with the prepared title and description, and return its URL. Otherwise, return the draft only.

## Template

`markdown
## Ticket

[<ticket number> - <ticket title>](https://ticket-url)

## What does this MR do and why?

<Short explanation of the behavior changed and its purpose. Prefer short bullets with **bold key words**; no long paragraph blocks. Optionally embed a Mermaid diagram here when it clarifies a flow, sequence, or architecture.>

## Key changes

- [x] <Verified completed change>
- [x] <Verified completed change>
- [ ] <Known remaining work, when applicable>

## Preview

### Before

[Before screenshot]

### After

[After screenshot]

<!-- Replace the sections above with "### Screen recording" and `[Screen recording]` when more appropriate. -->
`

Omit empty optional subsections. Keep the final description concise and ready to paste or publish.
