# Robo Refine and Polish

A Claude Code skill that keeps a long [`roborev`](#requirements) refine loop
honest.

`/roborev-refine` runs the review → fix → re-review cycle. **Robo Refine and
Polish** is the bookkeeping that turns that cycle into a *converging* process: a private
ledger that tracks every finding across iterations and reviewers, so you can
tell a regression from a repeat from a loop, defend a deliberate design call
without re-litigating it every iteration, handle a reviewer going offline
without faking coverage, and decide — honestly — when to stop.

Without the ledger, a long loop is how you end up "fixing" the same finding
three times, missing a regression you introduced two iterations ago, and
silently re-arguing a decision the next agent run can't see.

## What it gives you

- **A typed ledger table** — one row per finding, across all iterations and all
  reviewer agents, with a `Type` column that forces the distinction between
  `NEW`, `REGRESSION`, `REPEAT`, and `LOOP`.
- **Multi-reviewer discipline** — run `claude-code`, `codex`, and `pi` as
  separate jobs so "one reviewer flagged it, another didn't" stays as signal.
- **A deliberate-pushback list** — the mechanism that lets a multi-agent loop
  converge when reviewers can't see prior turns; a defended decision stops being
  an infinite loop.
- **An explicit convergence criterion and iteration budget** — including the
  honest "budget stop" for an unbounded tail of Low-severity findings.
- **Coverage caveats** — when a subscription reviewer goes down, that's a
  recorded limitation, never a silent drop to one agent.

## Requirements

This skill builds on **[roborev](https://roborev.io)** — continuous code review
for AI coding agents — and its `/roborev-refine` loop. roborev is the loop runner
this discipline sits on top of: the ledger is keyed to roborev's job ids, agent
names, and Pass/Fail verdicts. Install it first (see the
[roborev installation guide](https://roborev.io/installation/) and
[quick start](https://roborev.io/quickstart/)); without it, the methodology still
reads, but the commands in this skill won't run.

## Installation

This repo is its own plugin marketplace, so install it in two steps — add the
marketplace, then install the plugin:

```sh
/plugin marketplace add jesserobbins/refine-and-polish
/plugin install refine-and-polish@refine-and-polish
```

The skill then activates automatically when you run a multi-iteration roborev
refine loop, or you can invoke it explicitly with `/refine-and-polish`.

## Usage

Use it whenever a refine loop will run for more than a couple of iterations —
especially with two or more reviewer agents, or when you've been asked to "loop
until convergent", "address every finding", or "iterate to convergence". The
skill walks you through creating the ledger, typing each finding, planning each
iteration before you run it, and writing an honest closing when the loop
converges or hits its budget.

See [`skills/refine-and-polish/SKILL.md`](./skills/refine-and-polish/SKILL.md) for the
full discipline.

## License

[MIT](./LICENSE)
