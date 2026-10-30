---
name: transcript-decisions
description: >-
  Turn a conversation transcript into a proposed update of DECISIONS.md and
  wait for approval before writing. Use when the user provides a transcript,
  call notes, a chat export, or a recording text and wants decisions,
  constraints, or open questions logged for coding agents. On the approved
  write, ensure AGENTS.md contains the one line that tells the executing
  agent to read Current and Open in full and superseded history from the end.
---

# Transcript decisions

The transcript is evidence. `DECISIONS.md` is the record the executing agent must follow. Show the proposal first. Write files only after the user approves.

## Files

Use the repo the user names. If they only give a transcript path, use the git root that contains that file. If neither is clear, ask which repo before proposing.

| File | Role |
|---|---|
| Transcript path the user gave | Source. Read it. Do not copy it into `DECISIONS.md`. |
| `DECISIONS.md` at that repo root | Binding record: `## Current` and `## Open`. `## Superseded` stays here only while the file is small. |
| `DECISIONS-HISTORY.md` at that repo root | Superseded lines after a split. Newest at the bottom. Absent until a split. |
| `AGENTS.md` at that repo root | Must contain the check line below, once. |

## How to read a long file

Use the file read tool's `offset` and `limit`. A read with only the path returns the body into context. `offset` is the starting line (negative counts from the end). `limit` is how many lines to return. Grep for the `##` headings first when you need their line numbers; that search does not load the section body.

Count lines with `wc -l` before reading. Under 200 lines, one full read is fine.

At 200 lines or more:

- Read `## Current` and `## Open` in full, using `offset` and `limit` when a section is long. Latest-first is forbidden on these sections. An old line there is still in force. Chunks still accumulate in context, so a huge Current section still costs the whole section.
- Read superseded lines from the end, because new lines are appended. `offset` negative, `limit` about 150, then walk upward. Stop at 400 superseded lines, or sooner once the topic you are checking has a matching entry. Say so if older history was not scanned.
- If `## Current` plus `## Open` alone exceed 150 lines, still read them in full. On the next approved write, move every `## Superseded` line into `DECISIONS-HISTORY.md` and leave `DECISIONS.md` as Current and Open only.

`DECISIONS-HISTORY.md` uses the same end-first rule. It is not binding.

## Propose, then stop

1. Read the transcript.
2. Read `DECISIONS.md`, `DECISIONS-HISTORY.md`, and `AGENTS.md` using the rule above.
3. Post the proposal in chat. Do not create or edit files in this step.
4. Wait. On approval, write only the lines the user kept. A correction replaces your wording; it does not approve the rest. "No" writes nothing.

## What becomes a line

Keep a line only when the transcript settles it:

- **decision** — a choice someone agreed to
- **constraint** — a limit that stays in force
- **question** — still unresolved; never file this as a decision

Skip small talk, status, restatements of a line already marked current, and implementation detail the repo already shows. Shorten anything longer than one sentence. If the transcript contradicts a current line and nobody resolves it, propose a question. Do not supersede on silence.

## Proposal shape

```markdown
### Keep
- 2026-10-30 | decision | Billing stays on the v1 Stripe webhook. | from: calls/2026-10-30.txt
- 2026-10-30 | constraint | Do not add SSO this phase. | from: calls/2026-10-30.txt

### Supersede
- 2026-03-03 | decision | OAuth this quarter. → superseded 2026-10-30 by "Do not add SSO this phase."

### Open
- 2026-10-30 | question | Who owns the client digest copy? | from: calls/2026-10-30.txt

### Drop
- "We should look at SSO someday." — not agreed
```

`from:` is the transcript path. If the user pasted the transcript, use `from: chat`.

## Write the approved lines

Create `DECISIONS.md` on first use:

```markdown
# Decisions

## Current

## Open

## Superseded
```

Append approved decisions and constraints under `## Current`. Append approved questions under `## Open`. Move a superseded line from `## Current` to the end of the superseded record and append `superseded YYYY-MM-DD by "<new line>."` Do not delete it. After a split, that record is `DECISIONS-HISTORY.md`; before a split, it is `## Superseded`.

When an open question is answered, move that line to the superseded record with the same note, and add the answer under `## Current`.

## AGENTS.md check line

On the same approved write, make sure `AGENTS.md` contains this line exactly once:

`Read DECISIONS.md Current and Open in full before changing scope or behavior; read Superseded or DECISIONS-HISTORY.md from the end, newest first, and stop once the topic has a matching entry.`

If the file is missing, create it with that line only. If the file exists and the line is absent, append the line. If an older check line says only "Read DECISIONS.md before changing scope or behavior", replace that line with this one. If this line is already there, leave `AGENTS.md` unchanged.
