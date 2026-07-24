---
name: deep-dive
description: Interview the user about a software project and produce a short Architecture Decision Record (ADR.md) covering DB, API, Frontend, Env, and architecture-level changes. Use this whenever the user gives a skill+prompt combo asking to "deep dive", "interview me about this project", "figure out what needs to change", or wants a scoping/ADR document before starting work — whether the codebase is greenfield (empty/new) or brownfield (existing project with files). Also trigger on explicit mentions of "ADR", "architecture decision record", "deep-dive", or requests to scope out a feature/change before coding it. Always run the interview and file exploration steps before writing the document — do not skip straight to writing the ADR from the prompt alone.
---

# Deep Dive

Turn a rough prompt into a short, crisp ADR (Architecture Decision Record) by interviewing the user and reading their project files, then writing `docs/ADR.md`. The goal is a document a busy engineer can read in under two minutes — bullets and small tables, plain language, no fluff.

## Workflow

Follow these steps in order. Don't skip the interview or the file exploration — the ADR should reflect what's actually in the project and what the user actually confirmed, not a guess from the prompt alone.

### Step 1: Establish greenfield vs. brownfield

Check the working directory (or the directory the user points to) for existing project files (`package.json`, `requirements.txt`, `go.mod`, existing `src/`, `docs/`, `README`, `.git`, etc.).

- Files found → **brownfield**. Skim the key files (README, package manifest, folder structure, existing `docs/ADR.md` if present) to understand the current stack and conventions before asking questions.
- Nothing found → **greenfield**. Skip file exploration and go straight to the interview.

If it's ambiguous (e.g. you're in a chat interface with no filesystem context, or the user hasn't mentioned a project directory), just ask.

For brownfield, keep what you skim in mind through the interview: if the user later describes how something currently behaves and the code says otherwise, that's worth surfacing (see Step 2).

### Step 2: Interview the user

Use the prompt they already gave as your starting point — don't re-ask anything it already answers. Then fill the gaps. At minimum, understand:

1. What they're trying to achieve (the actual goal, in their words, not yours)
2. Who/what this affects — new feature, refactor, bug fix, migration, integration, etc.
3. Any hard constraints (must ship by X, must use Y stack, can't touch Z)

This is an active interview, not a form to fill in — push back where it sharpens the scope:

- **Press on vague terms.** If the user says something like "add user roles" or "sync the data," ask what that means concretely before it goes in the ADR. A term that's fuzzy now becomes a wrong assumption baked into the doc later.
- **Probe one edge case.** For the core piece of the change, float a concrete scenario ("what happens if a user cancels mid-sync?") rather than accepting the happy path alone. One well-chosen scenario usually surfaces a constraint the user hadn't stated.
- **Flag contradictions with the code.** If brownfield exploration showed the codebase doing something different from what the user just described, say so directly rather than writing down the user's version as fact.

Prefer the `ask_user_input_v0` tool for quick multiple-choice questions where the options are genuinely enumerable (e.g. "greenfield or brownfield?", "which environment does this target?"). For open-ended discovery ("what are you actually trying to build?"), ask in plain conversational text instead — don't force free-form answers into buttons.

Keep the interview tight: 1-2 rounds of questions is normal. This is a scoping exercise, not a full requirements-gathering session. If the user's prompt was already detailed and specific, a single confirming question may be enough.

### Step 3: Map the goal to change categories

Based on the confirmed goal (and, for brownfield, the actual files), decide which of these categories genuinely apply. Not every deep dive touches all five — only include a section if there's a real change to report:

- **DB change** — new/altered tables, columns, indexes, migrations, data model shifts
- **API change** — new/changed endpoints, request/response shapes, versioning, auth changes
- **Front End change** — new screens/components, state changes, routing, UX flow changes
- **Env change** — new env vars, secrets, config, infra, deployment changes
- **Major arch change** — anything structural: new service, new dependency, changed data flow, changed pattern (e.g. sync→async, monolith→split)

If a category doesn't apply, leave it out of the doc entirely rather than writing "N/A."

### Step 4: Write the ADR

Create (or overwrite) `docs/ADR.md` using the template in `assets/ADR_template.md`. Rules for the writing itself:

- **Crisp, not comprehensive.** Bullets and small tables only — avoid long paragraphs.
- **Plain language.** Explain each change like you're telling a teammate over coffee, not writing a spec. No jargon unless the project already uses it.
- **Short tables** for anything with more than one attribute per item (e.g. a DB table needs Field / Type / Notes columns). A single bullet is fine for simple, single-fact changes.
- **State the "why" in one line per section**, not a full justification essay.
- If genuinely nothing changes in a category, omit that section — don't pad the doc.

Make the `docs/` folder if it doesn't exist yet, then write the file.

### Step 5: Deliver

Show the user the ADR content (or the file, depending on environment) and briefly flag anything you're still unsure about — better to flag an open question in the doc than to guess and write it as fact.

See `ADR_template.md` for the exact structure to fill in. Only include the sections that actually apply (per Step 3) — the template shows the full set, not a required set.
