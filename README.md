# transcript-decisions

Cursor skill for **business decision records (BDRs)**.

An [ADR](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) captures a technical choice. A **BDR** captures a product or business choice: what we decided to build or change, what stays in force, what is still open. This skill turns a transcript into proposed BDR lines in `DECISIONS.md` and writes only the lines you approve. On that write it adds one line to `AGENTS.md` so later agents read current and open entries in full, and superseded history from the end.

It pairs with [intention-spec](https://github.com/djordjep/intention-spec): the HTML intention page is the picture of a substantial proposal; `DECISIONS.md` is the BDR ledger. Spec, intention, and ticket remain three jobs — this skill does not invent intention pages or tickets. A client call often yields a BDR first, then the page is updated. When you own the idea, you may write the intention page first and derive BDR lines from that page afterward. If the picture also forces a technical choice, that belongs in an ADR (not here).

## Install

Personal skill, available in every project:

```bash
git clone https://github.com/djordjep/transcript-decisions.git ~/.cursor/skills/transcript-decisions
```

Project skill, this repo only:

```bash
mkdir -p .cursor/skills
git clone https://github.com/djordjep/transcript-decisions.git .cursor/skills/transcript-decisions
```

The skill file is `SKILL.md`.

## See also

- [intention-spec](https://github.com/djordjep/intention-spec)
- [An intention spec — workflow for the agentic dev cycle](https://djordjepuzic.com/blog/posts/2026-11-13-intention-spec/)
