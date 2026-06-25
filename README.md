# skills

A few [Claude Code](https://claude.com/claude-code) / agent skills I use and
wanted to share. Each folder is a self-contained skill — a `SKILL.md` with
YAML frontmatter (`name`, `description`) and the prompt/method underneath.

They're agent-neutral Markdown, so they also work with anything that reads a
skills/ directory (e.g. Codex).

## The skills

| Skill | What it does |
|-------|--------------|
| [`trim`](trim/SKILL.md) | Trim anything dense — a UI, a module/API/config, a doc, a feature set — down to what earns its place. Names the one job, ranks elements least-useful-first, lets you pick the cut line, then implements the cut cleanly. |
| [`technology`](technology/SKILL.md) | A technical sparring partner that analyzes architecture / design / testing / data-model decisions through the principles of 15 software-engineering books. Surfaces trade-offs and blind spots, then recommends. |
| [`ux`](ux/SKILL.md) | A UX advisor grounded in the principles of 26 books on UX, design, psychology, engagement, and learning science. *(In German.)* |

## Install

Symlink (or copy) any skill folder into your agent's skills directory:

```sh
# Claude Code
ln -s "$PWD/trim" ~/.claude/skills/trim

# Codex
ln -s "$PWD/trim" ~/.codex/skills/trim
```

Then invoke it (e.g. `/trim`) or just describe the task — the `description`
field tells the agent when to reach for it.

## On the book-backed skills

`technology` and `ux` distill principles from books into a condensed reference +
a reading list of the sources. They contain my own condensed notes and method —
**not** the book texts. If a principle helps you, buy the book; the authors
earned it.

## License

[MIT](LICENSE) — the skill prompts/method are mine to share. Book titles,
authors, and their ideas belong to their respective authors.
