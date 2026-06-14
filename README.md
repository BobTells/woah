> [!WARNING]
> **Early prototype.** A personal experiment, shared rough — expect sharp edges and breaking changes.

/woah

A Socratic learning loop for vibe coding.
You're vibe coding. You hit something you don't fully understand. You type /woah and Claude walks you through it — adaptive Socratic questioning, saves the concept, generates an HTML guide, lets you refine it, and closes the loop by fixing the code.
What it does

/woah  →  probe (Socratic Q&A)
       →  identify the gap
       →  save the concept
       →  research + HTML guide
       →  refine (5 buckets)
       →  fix the code
The whole point is the loop. You leave with:

A durable HTML guide you can re-read later
A concept note (optional) for indexing
Code that reflects what you just learned

Refinement buckets
After the guide is generated, you can evolve it by picking from:

Format — visual style, simpler language
Pitfalls — common mistakes
Exercises — practice problems
Scope — broader context, related concepts
Application — how it applies to your codebase

Install

Claude Code
Tell Claude: "Install the woah skill from github.com/BobTells/woah into my .claude/skills folder."
Then restart Claude Code and run /woah.

Claude.ai / Claude Desktop
Download woah/SKILL.md from this repo, zip the woah folder so SKILL.md sits at the zip root, rename to woah.skill, and upload via Settings → Capabilities → Skills.
Setup
On first run in a new project, the skill creates .woah/config and asks:

Where saved knowledge lives (./learning/, ~/knowledge/, wherever)
Save format (markdown, JSON, etc.)
Research trigger style (auto-offer, manual, always-on)
Default tags (e.g. languages/frameworks)

After that it just works.
When to use

You wrote something that works but you don't know why
You see a pattern in someone else's code you can't explain
You're about to copy-paste and want to actually learn first
You hit a syntax or behavior you don't recognize

When NOT to use

Quick syntax lookups (just ask)
Time-pressure debugging (do it after the fix)
No codebase context (the skill needs something concrete to probe against)

License
MIT. Use it, fork it, share it.
