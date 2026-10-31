---
name: daily-diff-annotator
description: Turn today's The Daily Diff (tdd.cat) edition into a page written for you, with stories sorted by closeness to your work, explained in your terms, worked examples and claim checks.
---

# Daily Diff Annotator

The Daily Diff (https://tdd.cat/, curated by Arpit Bhayani) publishes about 30 high-signal
engineering stories a day. Each summary is dense: it assumes you already know every field
it touches. This skill rebuilds one edition as a page for one specific reader:

- Stories are grouped by how close they are to the reader's work, not by HN rank.
- Each story gets a plain one-liner, an "In your terms" line that links it to things the
  reader already knows, and a worked example with real numbers or code.
- Stories in the area the reader is growing into get a concept primer, explained from
  zero.
- Where a summary's closing line claims more than its source shows, a claim check puts the
  two side by side, like a diff.
- A "Connecting the dots" section links stories that relate to or contradict each other,
  and a glossary collects every term defined on the page.

The skill works on other multi-story pages too (another newsletter, a Hacker News front
page) if the user points at one. For a single blog post, build the same page with one
card.

## Step 1. Get the reader's profile

Personalisation is the whole point, so get this right before fetching anything.

**Ask first. Read nothing about the user until they say which sources you may use.** This
applies even when you can see memory tools or a `~/.claude` folder. If the user attached
or named a profile file (for example `daily-diff-profile.md`), that file is their approval
for that file only: use it and skip the question.

Otherwise, ask once which sources to build the profile from, and let them pick any
combination:

1. **Claude Code history:** `~/.claude/CLAUDE.md`, project `CLAUDE.md` files, auto-memory
   files and recent session transcripts. Only possible inside Claude Code.
2. **Claude chat app memory:** what Claude's memory has stored about their work, skills,
   projects and learning goals. Only possible where you have memory tools.
3. **A profile file** they paste or attach.
4. **A few questions** answered right now.

Offer only the sources available where you're running, and say in one line what each one
would read. Each app sees only its own history: Claude Code can't read the chat app's
memory, and the chat app can't read Claude Code sessions. If the user wants the other
app's history too, tell them how to bring it in as a profile file: in Claude Code, ask the
chat app to "write a profile of me from what you remember about my work, skills, projects
and what I'm learning, leaving out anything personal" and save it as
`daily-diff-profile.md`; in the chat app, run `references/profile-prompt.md` in Claude
Code and attach the file it writes. Then merge it with the sources they approved here.
Then wait for the answer. If the user doesn't answer or isn't there, use option 4 when you
can ask, and otherwise stop and explain that you need their approval to build a profile.

Once they approve:

- **Claude Code history:** use the prompt in `references/profile-prompt.md`. Read only the
  locations they agreed to.
- **Memory:** read only files about work, skills, projects and learning goals. Skip
  personal, family, health and financial files.
- **Questions:** ask these in one message:
  - What do you work on day to day (stack, systems, recurring tasks)?
  - What do you know cold, so I never explain it?
  - What are you learning or still finding hard?
  - What kind of work do you want to grow into, and what's your level in it today?
  - Any side projects I can tie stories to?

Show the user the profile you built before using it, and let them correct it. Offer to
save it as `daily-diff-profile.md` so later runs don't need to read their history again.
On later runs, use the saved profile and don't re-read any source unless they ask.

You need these fields: daily work, knows cold, still learning, growth area (the kind of
work they want to do, in their own words), side projects, and any unit or currency
preference. The growth area decides the learning lane, so if no source states it, ask for
it. Never put secrets, tokens, hostnames, customer names or anything confidential into the
profile or the page.

## Step 2. Fetch the edition

- Fetch https://tdd.cat/ for the latest edition, or the date the user asked for from
  https://tdd.cat/archive/. https://tdd.cat/llms.txt and https://tdd.cat/rss.xml are also
  available.
- Collect, for every story: title, source URL, discussion URL, author, the "Why read" line
  and the full summary text. Note the edition date and story count.
- If the site can't be fetched, say so plainly. Never rebuild an edition from memory.

## Step 3. Sort stories into lanes

Define 3–5 lanes from the profile, closest to the reader first. A typical set:

| id | Name it like | What goes in |
|---|---|---|
| `core` | "Your lane" | The reader's own field. Read at full speed. |
| `learn` | e.g. "Distributed systems, gently" | Stories that teach the growth area. Mark this lane `learn: true`. |
| one or two adjacent lanes | e.g. "AI infra is a storage problem", "Agents, decoded" | Fields the reader meets often but doesn't work in. Frame them through the reader's own field where that's honest. |
| `skim` | "Skim" | Everything else, one line each. |

Every story goes in exactly one lane. Aim for 2–4 stories in the learn lane. If fewer fit
naturally, attach a concept primer to the best-fitting story in another lane.

## Step 4. Write each card

For every non-skim story, write:

- `what`: what happened, in two or three short, plain sentences.
- `yours`: the link to the reader's real work or projects. Name the actual thing (their
  PR, their side project, the tool they use) when the profile supports it. If the only
  link would be forced, say what the story is a good example of instead.
- `example`: a worked example in a `<pre>` block, with concrete numbers or a short code
  snippet. Run every calculation in code before writing it down. Label illustrative
  figures as illustrative.
- `terms`: definitions for jargon this reader wouldn't know. Skip anything the profile
  says they know cold.
- `go: true` on 3–5 stories worth reading in the original. Everything else says "this card
  is enough".
- `concept` (learn lane): `name`, `def` (explained from zero), `near` (the same idea on a
  single machine or inside the reader's own system), and `read` (one approachable next
  read, like a book chapter, a classic paper or a well-known essay). Never invent a
  source.
- `claim`: only where the summary's closing line overstates its source. First element is
  the summary's claim, second a more careful version.

Skim rows get `what` (one line) and `why` (why it might still matter to this reader).

Then write the page-level parts:

- **Picks:** "If you only read three", with a one-line reason each, tied to the profile.
- **Connecting the dots:** 2–3 cards linking stories that relate or contradict each other,
  with ids.
- **Reading path** (optional): 3–6 items, approachable first, for the growth area. Reuse
  the previous one on reruns unless something changed.

### Writing rules

- Plain language. Short, active sentences. No hype.
- No asides set off with em-dashes, no "not X, but Y" framing, no "it's worth noting".
- Rewrite in your own words. Don't copy the summaries.
- Use only numbers from the edition or numbers you computed.
- Respect the reader's units and currency (convert money if they prefer another currency).

## Step 5. Build the page

Use `assets/template.html` next to this file. Copy it and replace only the block between
`DATA START` and `DATA END`: the `EDITION`, `LANES`, `TRACK` and `S` objects. Keep the CSS
and the render script unchanged. The comments in the data block describe every field.

If the template isn't available, build one self-contained HTML page with the same parts in
this order: masthead (date, story count, source link), a four-item legend, picks, a sticky
lane filter, the lanes of cards, Connecting the dots, a searchable glossary, and a
colophon. Support light and dark themes and phone widths.

Always keep the colophon crediting The Daily Diff and its curator, with a link to the
edition.

## Step 6. Check, then deliver

- Extract the inline script and run `node --check` on it when you have a shell.
- Confirm every story id is unique, every pick and dot id exists, and every link is a real
  URL from the edition.
- Deliver it as a published artifact page where you can, otherwise as an HTML file named
  `daily-diff-YYYY-MM-DD.html`. On later runs in the same place, update the same page so
  the link stays stable.
- Tell the user in two or three lines: the edition date, the three picks, and the concept
  of the day.
- The page reveals details from the reader's profile. If the user wants to share it, point
  that out once.

## Optional: run it every morning

If the user wants a daily edition and you can create scheduled tasks, offer one. Its
prompt must be self-contained: the saved, user-approved profile (scheduled runs never read
new sources about the user), the output location, and "if the edition date hasn't changed
since the last run, stop without notifying". If the user wants past editions kept, save
each one as its own dated page and keep a simple index of them.
