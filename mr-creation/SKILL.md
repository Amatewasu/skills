---
name: mr-creation
version: 1.2.0
description: Draft or create a merge request with a conventional-commit title, a one-line TL;DR, a ticket link, a concise explanation of what changed and why, an evidence-based completion checklist, a blast radius line, and before/after screenshots or a screen recording. Use when the user asks for an MR template, MR description, or to open/create a merge request.
---

# Merge Request Creation

Prepare an MR description from repository evidence, then create the MR only when the user explicitly asks to publish or open it.

## Workflow

1. Inspect the current branch, target branch, commits, and diff. Account for every material change included in the MR.
2. Resolve the ticket URL from the user's request, branch name, commits, or repository integrations. Read the ticket and copy its number and title into the link label using `<ticket number> - <ticket title>`. Ask for any value that cannot be resolved unambiguously.
3. Write the MR title as a conventional commit message: `<type>(<optional scope>): <imperative summary in lowercase, no trailing period>`, e.g. `fix(editor): keep lane order stable when merging circuits`. Use standard types (`feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `style`, `perf`). The title becomes the commit message when the MR is merged with squash, so it must describe the whole MR, not a single commit.
4. Write the TL;DR: one line, plain language, stating what this MR changes and why it matters. A human who reads nothing else should still get the MR.
5. Assess the blast radius and output a single line in this exact style: `Blast radius: 🟢 Low / 🟡 Medium / 🟠 High / 🔴 Very high (brief human-readable reason)`. Base the level on how widely the change can affect behavior, shared dependencies, contracts, state, persistence, or external consumers. Keep the reason concrete, non-technical where possible, and under ~15 words.
6. Write a short explanation of what the MR does and why it is needed. Describe behavior and intent rather than listing files. Format for scanning: short bullets over paragraph blocks, bold key words (component, file, API, error message), and no text chunk longer than a few lines. Stay brief: the goal is a human grasps the MR quickly, not exhaustive coverage of the diff.
7. Build the key-changes checklist from the diff:
   - Each item names one material behavior change from the diff, nothing else.
   - Keep the list brief and focused on material behavior.
   - Verification statements ("typecheck passes", "tests pass locally") are never checklist items; mention verification only when it carries special meaning here (tests skipped, known flaky test, unusual setup required).
   - Use `- [x]` for changes present in the branch, `- [ ]` for known remaining work.
8. List points worth challenging: decisions, trade-offs, or assumptions in the change a reviewer could legitimately question, one bullet each. Only genuinely debatable points; omit the section when there are none.
9. When a Mermaid diagram helps a human understand the change faster than prose — a flow, state machine, sequence, or architecture shift — include one in the explanation section. Diagrams are welcome but optional; skip them when the change is simple or localized.
10. Add preview evidence for user-facing visual changes:
   - Prefer paired before and after screenshots.
   - Use a screen recording when motion or a multi-step interaction communicates the change better.
   - When preview media is not ready, insert explicit replaceable placeholders such as `[Before screenshot]`, `[After screenshot]`, or `[Screen recording]`.
11. Produce the Markdown using the template below. Use only its sections; do not add a Verification section. Never invent ticket URLs, ticket numbers, ticket titles, completed work, or preview assets. Also state the proposed conventional-commit MR title above the Markdown so it is ready to paste into the title field.
12. When the user explicitly requested MR creation, determine the repository host and target branch, create the MR with the prepared title and description, and return its URL. Otherwise, return the draft only.

## Template

```markdown
**TL;DR:** <One line, plain language: what this MR changes and why.>

Blast radius: <🟢 Low / 🟡 Medium / 🟠 High / 🔴 Very high> (<brief human-readable reason>)

## Ticket

[<ticket number> - <ticket title>](https://ticket-url)

## What does this MR do and why?

<Short explanation of the behavior changed and its purpose. Prefer short bullets with **bold key words**; no long paragraph blocks. Optionally embed a Mermaid diagram here when it clarifies a flow, sequence, or architecture.>

## Key changes

- [x] <Verified completed change>
- [x] <Verified completed change>
- [ ] <Known remaining work, when applicable>

## Points worth challenging

- <Decision, trade-off, or assumption a reviewer could legitimately question>

## Preview

### Before

[Before screenshot]

### After

[After screenshot]

<!-- Replace the sections above with "### Screen recording" and `[Screen recording]` when more appropriate. -->
```

Omit empty optional subsections, including Points worth challenging when nothing is genuinely debatable. Keep the final description concise and ready to paste or publish.
