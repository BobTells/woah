---
name: woah
description: Use when /woah is invoked, or when the user has just written/pasted code that works but can't explain why, hit unfamiliar syntax/behavior, sees a pattern in someone else's code they can't articulate, or wants to actually learn the concept before moving on. Also valid for architectural / "how does this run" gaps that aren't tied to a single line. Not for quick syntax lookups or active time-pressured debugging.
---

# /woah — Socratic Learning Loop

A learning loop for vibe coding. The user hits something they don't fully understand, types `/woah`, and you walk them through it: Socratic probe → name the gap → save a concept index → durable HTML guide → close the loop. They leave with a real artifact and (if applicable) code that reflects what they just learned.

## When to use

- User just wrote or pasted code that works but they can't explain why
- They hit syntax or runtime behavior they don't recognize
- They see a pattern in someone else's code and can't articulate it
- They want to learn an idea or architecture, not just a single line
- They're about to copy-paste and want to actually learn the underlying idea

**Don't use when:**
- The ask is a quick syntax lookup — just answer it directly
- They're mid-debug under time pressure — finish the fix, run `/woah` after
- The conversation is design/strategy, not a learning gap — drop the loop and just talk

## First-run setup

On first invocation in a project, check for `.woah/config.json` at the project root. If absent, ask the user — **one explicit question at a time**, no defaults-by-default. Each question stands on its own; don't bundle.

1. **Knowledge directory** — where concept notes and HTML guides live (suggest `./learning/`, but ask)
2. **Save format** — `markdown` or `json` for concept notes
3. **Research trigger** — `auto-offer` (offer the HTML guide every time) | `manual` (only when user asks) | `always-on` (build without asking)
4. **Default tags** — list of tags applied to every concept note (e.g. languages, frameworks, project name)
5. **Default refinement** — list of refinement buckets applied automatically (any of `format` | `pitfalls` | `exercises` | `scope` | `application`, or `none`)

Persist answers to `.woah/config.json`. Re-run setup only on `/woah --reconfigure`.

Example config:
```json
{
  "knowledge_dir": "./learning/",
  "save_format": "markdown",
  "research_trigger": "auto-offer",
  "default_tags": ["pinky", "server", "web"],
  "default_refinement": ["pitfalls", "application"]
}
```

## The loop

### 1. Probe (Socratic Q&A)

One question at a time. Start broad: *"What did you expect this line to do?"* Narrow based on each answer. The goal is to surface the user's actual mental model — not to test them, not to lecture.

Stop probing when you can name the specific gap in one sentence.

**Discipline:** No explanations during probing. Questions only.

### 2. Identify the gap

State the gap back in one sentence. Confirm with the user — sometimes the real gap is adjacent to the suspected one. Don't proceed until they agree.

**Tiered source anchor.** Don't refuse the loop because there's no single line. Use the most specific anchor available:
- `file:line` — best, when there's an exact line that triggered this
- `file` — when it's a whole file's behavior
- `app/dir` — when it's a multi-file pattern (e.g. `dev/Apps/Maestro/code/backend/`)
- `concept` — when there's no source at all (architectural / theoretical)

Save the chosen tier in the concept note's frontmatter.

### 3. Confirm refinement direction (before research)

Read `default_refinement` from config. State it back: *"Applying defaults: pitfalls + application. Change?"*

User responses:
- **Accept** → carry those buckets into step 4 so the HTML is shaped correctly the first time
- **Override** → ask which bucket(s) for this loop only; don't write to config
- **/woah --refine** invoked → skip defaults, present the full menu interactively

This step replaces the old post-build refinement menu — no rebuild loop.

### 4. Save the concept note (lightweight index)

Write a *short* concept note to the configured knowledge directory. Filename: `<slug>.<ext>`.

The note is an **index entry**, not a duplicate of the HTML. Required fields only:
- `tags` (config defaults + any specific to this concept)
- `date`
- `anchor_type` (one of: `file_line` | `file` | `app_dir` | `concept`)
- `anchor` (the actual path/identifier; empty string if `concept`)
- `gap` (one sentence)
- `guide` (relative path to the HTML)

No `corrected_model` or `tiny_example` in the note — those live in the HTML, single source of truth. The note exists so `learning/` is greppable and indexable later.

### 5. Research + HTML guide

If `research_trigger` allows, do focused research — web search + scanning the user's project for related context. Generate a self-contained HTML file in the knowledge directory: `<slug>.html`.

Shape the HTML to the refinement buckets confirmed in step 3. The guide always includes:
- Plain-English explanation
- A worked example tied to the user's *actual* code or project (not a generic textbook example)
- One diagram if it genuinely helps (skip if forced)
- A "you'll know it clicked when…" check at the end

Plus the chosen bucket sections inline:
- **Format** — visual style/layout choices, simpler language
- **Pitfalls** — common mistakes and why they happen
- **Exercises** — 1–3 practice problems with answers
- **Scope** — broaden to related concepts
- **Application** — apply the concept to other places in *this* codebase

The HTML must be self-contained — inline CSS, no external assets. The user should be able to open it offline later.

### 6. Close the loop

Produce a concluding artifact appropriate to the gap type:

- **Code-bound gap** (e.g. user pasted a snippet they didn't understand): rewrite the code to reflect the corrected mental model. Show the diff. Explain *why* each change is there in terms of the gap from step 2. Let the user accept, reject, or edit.
- **Idea / architecture gap** (no specific code to rewrite): write a 2–4 line "next time you see X, try Y" take-away. Append it to the HTML as the final section. No diff.
- **Reading-comprehension gap** (user wants to be able to read this kind of code): the HTML's "how to read" section is the closure. Step 6 is just a sanity check that section exists.

Either way, the HTML is the durable reference; step 6 is the take-away.

## Output discipline

- One concept note + one HTML guide per `/woah` invocation
- Concept note is an index entry only — never duplicate HTML content there
- Probing means questions only — don't slip into explanation mode mid-loop
- If the user bails out of any step, save what you have and exit cleanly

## Install

**Claude Code:** copy this `woah/` folder into `~/.claude/skills/`. Or simply tell Claude: *"Install the woah skill from github.com/BobTells/woah into my .claude/skills folder."*

**Claude.ai / Desktop:** zip the contents of `woah/` (so `SKILL.md` sits at the zip root), rename to `woah.skill`, upload as a custom skill.
