# skills

A personal collection of [Claude Code](https://claude.com/claude-code) agent skills — reusable, version-controlled instructions that teach Claude how to do a specific job the same way every time.

## What's in here

```
skills/
└── engineering/
    └── deep-dive/
        ├── SKILL.md          # the skill definition Claude reads
        └── ADR_template.md   # template it fills in when writing the doc
```

Skills are grouped by category (`engineering/`, and more as the collection grows), one folder per skill. Each skill folder has a `SKILL.md` with YAML frontmatter (`name`, `description`) describing what it does and when Claude should trigger it, followed by the step-by-step instructions.

### `engineering/deep-dive`

Interviews you about a project and produces a short Architecture Decision Record (`docs/ADR.md`) covering DB, API, frontend, environment, and architecture-level changes — before any code gets written. Works on both greenfield (empty/new) and brownfield (existing) codebases, and actively pushes back on vague requirements and edge cases during the interview rather than just taking dictation.

## Adding these skills to another repo

This repo works as a skills source for [`npx skills`](https://github.com/vercel-labs/skills), a package manager for agent skills that treats GitHub repos as its registry.

From inside the target project:

```bash
# install every skill in this repo
npx skills@latest add heraju/skills

# install just one skill
npx skills@latest add heraju/skills --skill deep-dive

# install for a specific agent (e.g. claude-code, codex)
npx skills@latest add heraju/skills -a claude-code

# install to your user directory instead of the current project
npx skills@latest add heraju/skills -g
```

By default this installs into `./<agent>/skills/` in the target project (e.g. `.claude/skills/`); pass `-g`/`--global` to install to `~/<agent>/skills/` instead.

Other useful commands:

```bash
npx skills list              # see what's installed
npx skills update            # pull in updates from this repo
npx skills remove deep-dive  # remove a skill
```

## Adding a new skill here

1. Create `skills/<category>/<skill-name>/SKILL.md` with frontmatter:
   ```markdown
   ---
   name: skill-name
   description: What it does and when Claude should use it — be specific, this drives auto-triggering.
   ---
   ```
2. Write the workflow as ordered steps. Keep it concrete and opinionated — a skill's whole job is to remove ambiguity about how a task gets done.
3. Put any supporting files (templates, format specs) alongside the `SKILL.md` in the same folder.
