---
name: woah
description: Use when /woah is invoked, or when the user has just written/pasted code that works but can't explain why, hit unfamiliar syntax/behavior, sees a pattern in someone else's code they can't articulate, or wants to actually learn the concept before moving on. Not for quick syntax lookups or active time-pressured debugging.
---

# /woah — Socratic Learning Loop

A learning loop for vibe coding. The user hits something they don't fully understand, types `/woah`, and you walk them through it: Socratic probing → name the gap → save the concept → durable HTML guide → refine → fix the code. They leave with a real artifact and code that reflects what they just learned.

## When to use

- User just wrote or pasted code that works but they can't explain why
- They hit syntax or runtime behavior they don't recognize
- They see a pattern in someone else's code and can't articulate it
- They're about to copy-paste and want to actually learn the underlying idea

**Don't use when:**
- The ask is a quick syntax lookup — just answer it directly
- They're mid-debug under time pressure — finish the fix, run `/woah` after
- There's no concrete code to anchor against — the loop needs a target

## First-run setup

On first invocation in a project, check for `.woah/config.json` at the project root. If absent, ask the user — one question at a time — and persist:

1. **Knowledge directory** — where concept notes and HTML guides live (default: `./learning/`)
2. **Save format** — `markdown` or `json` for concept notes (default: `markdown`)
3. **Research trigger** — `auto-offer` | `manual` | `always-on` (default: `auto-offer`)
4. **Default tags** — e.g. languages and frameworks the user is working in

Write the answers to `.woah/config.json`. Never ask again unless the user invokes `/woah --reconfigure`.

## The loop

### 1. Probe (Socratic Q&A)

One question at a time. Start broad: *"What did you expect this line to do?"* Narrow based on each answer. The goal is to surface the user's actual mental model — not to test them, not to lecture.

Stop probing when you can name the specific gap in one sentence.

**Discipline:** No explanations during probing. Questions only.

### 2. Identify the gap

State the gap back in one sentence. Confirm with the user — sometimes the real gap is adjacent to the suspected one. Don't proceed until they agree.

### 3. Save the concept

Write a short concept note to the configured knowledge directory. Filename: `<slug>.<ext>` based on configured format.

Required fields (frontmatter for markdown, top-level keys for json):
- `tags` (from config defaults + any specific to this concept)
- `date`
- `source_file` and `source_line` (the code that triggered this)
- `gap` (one sentence)
- `corrected_model` (one to three sentences)
- `tiny_example` (one minimal code snippet)

### 4. Research + HTML guide

If the configured research trigger fires (or the user opts in), do focused research — web search + scanning the user's project for related context. Generate a self-contained HTML file in the knowledge directory: `<slug>.html`.

The guide includes:
- Plain-English explanation
- A worked example tied to the user's *actual* code (not a generic textbook example)
- One diagram if it genuinely helps (skip if forced)
- A "you'll know it clicked when…" check at the end

The HTML must be self-contained — inline CSS, no external assets. The user should be able to open it offline later.

### 5. Refine (5 buckets)

Offer the user a menu — they can pick one, several, or skip:

- **Format** — visual style, simpler language, restructure
- **Pitfalls** — common mistakes and why they happen
- **Exercises** — 1–3 practice problems with answers
- **Scope** — broaden to related concepts
- **Application** — apply the concept to other places in *this* codebase

Apply the chosen bucket(s), update the HTML in place. Loop back to the menu until the user is satisfied or skips.

### 6. Fix the code

Return to the original code that triggered `/woah`. Rewrite it so it reflects the corrected mental model. Show the diff. Explain *why* each change is there in terms of the gap from step 2. Let the user accept, reject, or edit.

## Output discipline

- One concept note + one HTML guide per `/woah` invocation
- The HTML guide is the durable artifact — treat it as something the user will re-read
- Probing means questions only — don't slip into explanation mode mid-loop
- If the user bails out of any step, save what you have and exit cleanly

## Install

**Claude Code:** copy this `woah/` folder into `~/.claude/skills/`. Claude picks it up on next launch.

**Claude.ai / Desktop:** zip the contents of `woah/` (so `SKILL.md` sits at the zip root), rename to `woah.skill`, upload as a custom skill.
