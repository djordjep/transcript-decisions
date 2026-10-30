# transcript-decisions

Cursor skill. Give the agent a transcript. It proposes updates to `DECISIONS.md` and writes only the lines you approve. On that write it adds one line to `AGENTS.md` so later agents read current and open entries in full, and superseded history from the end.

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
