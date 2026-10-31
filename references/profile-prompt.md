# Build a reader profile from Claude Code history

Claude only runs this after you say it may read your Claude Code history. You can also run it yourself: paste it into a Claude Code session started in your home directory.

Before you run it, fill in the first line with the kind of work you want to grow into, in your own words. That line decides your learning lane, and it's the one thing your history can't tell Claude.

```
The kind of work I want to grow into: <write 1–3 sentences, and say your level today>

Build a profile of me for another Claude that annotates engineering news for me.
Read ~/.claude/CLAUDE.md, any CLAUDE.md files under my repos, the auto-memory
files under ~/.claude/projects/*/memory/, and skim my recent session transcripts
in ~/.claude/projects/ (last ~60 days).

Write ~/daily-diff-profile.md. Start it with my growth goal above, in my words.
Then add these sections:
1. What I actually work on: repos, languages, subsystems, recurring tasks
2. Concepts I know cold: things I never need explained
3. Concepts I asked about or struggled with: where I needed help
4. Tools and stack: build systems, debuggers, editors, infra
5. Current side projects and their status
6. Topics I keep coming back to, ranked by how often

Rules: be specific and cite which project each point comes from.
Exclude secrets, tokens, hostnames, customer names, and anything
confidential to my employer. Keep it under 600 words.
```

Read the file before you use it. Section 3 matters most: it tells the annotator what to explain and what to skip.
