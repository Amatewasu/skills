# Skills

My personal agent skills — small, reusable disciplines I plug into my coding agents (Claude Code, Codex, Mistral Vibe, ...). Not a framework, just the practices I actually use every day. Fork them, hack them, make them your own.

## Skills

| Skill | Version | What it does |
| --- | --- | --- |
| [mr-creation](./mr-creation/SKILL.md) | 1.0.0 | Draft or create a merge request with a ticket link, a concise what/why, an evidence-based completion checklist, and before/after preview evidence. |

## Installation

```bash
npx skills@latest add Amatewasu/skills
```

Or copy a skill folder into your agent's skills directory (e.g. `/.agents/skills/`):

```bash
git clone https://github.com/Amatewasu/skills.git
cp -r skills/mr-creation ~/.agents/skills/
```

## Conventions

Each skill lives in its own folder with a `SKILL.md` containing a YAML front-matter (`name`, `description`, `version`) and the workflow instructions. Skill versions follow [semver](https://semver.org/) and are bumped in the front-matter `version` field.
