/woah

A Socratic learning loop for vibe coding. Bill & Ted style.
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
Claude.ai / Claude Desktop

Download SKILL.md from this repo
Package it as a .skill file (or use the bundled one in releases, if present)
Upload to Claude as a custom skill

Claude Code
Drop the woah/ folder into your skills directory. Claude will pick it up automatically.
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
