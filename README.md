## Disclaimer

This skill, including `SKILL.md`, the page template and some part of this README, was made
with the help of Claude Opus. I reviewed it, but it may still have mistakes, so check
anything important against the original stories.

# Daily Diff Annotator

A Claude skill that rebuilds today's edition of [The Daily Diff](https://tdd.cat/), the
engineering newspaper curated by [Arpit Bhayani](https://arpitbhayani.me/), as a page
written for you.

![Two story cards from an annotated edition, each with an "In your terms"
box](docs/banner.png)

Every morning I read The Daily Diff, which picks around 30 high-signal stories a day.
Right now I find the summaries dense. They assume you already know the concepts. (One day
I hope to keep up with them :P)

I made this to help me understand and internalize what I read. When I read a blog post, I
keep wondering how it relates to my work. Once I can see that link, I'm much more
motivated to read the article closely. It also gives me a starting point to have an AI
grill me on the concepts, which is how I learn best.



- **Sorted by distance from your work.** Your own field first, then the area you're
  learning, then adjacent fields, then a skim list.
- **In your terms.** Every story is linked to things you already know: your stack, your
  projects, your recent bugs.
- **Worked examples.** Real numbers or a short code snippet on every card, with the
  arithmetic checked.
- **Concept primers.** For the area you're growing into, one idea per card, explained from
  zero, with one approachable next read.
- **Claim checks.** Where a summary's last line claims more than its source shows, you see
  both, like a diff.
- **Connecting the dots, a glossary, read marks,** and light and dark themes.

## Sample

Here's the edition from 29 Oct 2026, annotated for me:
[see the full page](https://claude.ai/artifact/VawMamhKYGDLsGZBjPT33x).

![One story card from the sample page: a plain summary, an "In your terms" box, a worked
example and a list of terms](docs/sample-card.png)

## Install

- **claude.ai:** download this folder as a zip and upload it in the Skills section of your
  settings.
- **Claude Code:** copy the folder to `~/.claude/skills/daily-diff-annotator/`.

## Use

Ask Claude something like:

- "Annotate today's Daily Diff for me."
- "Do the Daily Diff from 25 Oct 2026."
- "Annotate this newsletter the same way: <url>"

The first time, Claude needs a profile of you. **It asks before reading anything.** You
choose where the profile comes from:

- your Claude Code history (CLAUDE.md files, auto memory, recent sessions)
- the memory in the Claude chat app
- a profile file you paste or attach
- a few questions you answer on the spot

You can pick more than one. Claude shows you the profile before using it, so you can fix
anything it got wrong, and offers to save it as `daily-diff-profile.md` so it doesn't need
to read your history again.

If your Claude can create scheduled tasks, ask it to run this every morning.

## Using both Claude Code and the Claude chat app

Each app only sees its own history. Claude Code can't read the memory from the Claude chat
app, and the chat app can't read your Claude Code sessions. If you use both, bring the
other one in as a profile file.

**Running the skill in Claude Code, and you also want what the chat app knows:**

1. In the Claude chat app, ask: "Write a profile of me from what you remember about my
   work, skills, projects and what I'm learning. Leave out anything personal."
2. Read it, then save it as `daily-diff-profile.md` in your home folder.
3. In Claude Code, run the skill. When it asks which sources to use, choose your Claude
   Code history and the profile file. It merges the two.

**Running the skill in the Claude chat app, and you also want your Claude Code history:**

1. In Claude Code, run the prompt in `references/profile-prompt.md`. Fill in the
   growth-goal line first.
2. Read the `~/daily-diff-profile.md` it writes.
3. In the Claude chat app, attach that file when you run the skill, and choose the chat
   app's memory too. It merges the two.

Either way, you only do this once. Keep the merged profile Claude shows you and reuse it.

## Get the best results

**It works best if you already use Claude Code to grill you on concepts.** When you ask
Claude Code to explain things, quiz you, or walk you through code you don't understand
yet, your session history records exactly where you struggled. The skill uses that to
decide what to explain from scratch and what to skip. If you only ever ask Claude Code to
write code, the profile will know your stack but not your gaps.

**Write down the kind of work you want to do.** Your history shows where you are. It can't
show where you want to go. Add one to three sentences in your own words about the work you
want to grow into, and your honest level in it today. That decides your learning lane, the
concept primers and the reading path. Put it at the top of `references/profile-prompt.md`
before you run it, or tell Claude when it asks.

### What a good profile looks like

Some lines from my profile are as follows:

```
## Where you want to grow

* Distributed systems, at a beginner level. You’re just starting on replication,
  consensus, consistency models and failure handling, and you still find them hard. You’re
  on solid ground with single-node storage internals.
* You’re also new to building with LLMs, models and agent harnesses (as of September
  2026).

## What I work on
allocator data structures (AVL and B-tree range trees, overlap comparators, `lower_bound`,
extent merging); lock granularity and weak-consistency contracts within one process;
BlueStore architecture (BlueFS/RocksDB, the freelist, fsck).

## Concepts I struggle with
complex distributed systems fundamentals, test infra (gtest parameterized names,
teuthology, negative controls); storage hardware (SSD flash translation layers, thin
provisioning, the discard thread)

## The latest topics that I worked on
1. Allocator correctness and stress testing (~14 sessions) · 2. Cloudflare/Next.js
   deployment for side projects (~6) · 3. BlueStore internals and history (4) · … · 7.
   Discard/TRIM and SSDs (2, rising)
```

Notice what makes it useful:

- The growth goal is stated plainly, with an honest level ("beginner") and a preferred
  learning style. That produced a "Distributed systems, gently" lane with concepts
  explained from zero.
- "Know cold" is specific, so the page never wastes a line explaining extent merging to
  someone who writes allocators.
- "Struggled with" names real topics from real sessions, so a story about SSDs gets a
  worked example on exactly the part that's new.
- The ranked topics show what's on the reader's mind this month, which shapes the top
  three picks.

## Files

- `SKILL.md`: the instructions Claude follows.
- `assets/template.html`: the page template. Claude fills only its data block.
- `references/profile-prompt.md`: a prompt to build your profile from Claude Code history.
- `docs/`: screenshots for this README. Not needed by the skill.

## Privacy

Claude reads only the sources you approve, and your profile stays with you. The page it
produces reveals details from the profile, so read it before you share it.

## Credit

All curation comes from The Daily Diff by Arpit Bhayani. This skill only rewrites the
explanations for one reader. Read and support the original at https://tdd.cat/.

## Disclaimer

This skill, including `SKILL.md`, the page template and this README, was made with the
help of Claude Opus. I reviewed it, but it may still have mistakes, so check anything
important against the original stories.
