<h1 align="center">rilis</h1>

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](CHANGELOG.md)
[![No dependencies](https://img.shields.io/badge/dependencies-none-brightgreen.svg)](#)

**Never guess a version number again.** A discipline for AI agents (and the
humans reviewing them) covering version numbers, changelogs, and README sync.

*rilis* is Indonesian for "release". Version confusion is rarely about the
number itself. It happens because no rule was ever decided. This skill is the
rule, decided once: **SemVer for the number, Keep a Changelog for the log,
one source of truth for the value.**

## The decision tree (the whole point)

```
Does it break the old way?        → MAJOR   2.4.1 → 3.0.0
No. Does it add a capability?     → MINOR   2.4.1 → 2.5.0
No. Fixes and polish only?        → PATCH   2.4.1 → 2.4.2
```

Plus: pre-1.0 rules, pre-release suffixes, and when in doubt take the bigger
bump. MAJOR bumps always need human confirmation: breaking changes are a
product decision, not a technical one.

## What else it enforces

- **One source of truth.** The version lives in exactly one place
  (`package.json`, `VERSION`, SKILL.md frontmatter, git tag). Badges and docs
  reference it, never store their own copy.
- **Changelog written when things change, not at release time.** Every change
  lands in `## [Unreleased]` in the same commit, under Added / Changed /
  Deprecated / Removed / Fixed / Security. At release, Unreleased just gets
  renamed to `[X.Y.Z] - YYYY-MM-DD`.
- **Atomic releases.** Bump + changelog + README + tag in one step. No silent
  bumps, no tag without a changelog entry, no entry without a date.
- **Human entries.** Changelog lines describe impact on users, never
  copy-pasted commit messages.

Full rules, 0.x handling, anti-patterns, and a Do & Don't table: [`SKILL.md`](SKILL.md).

## Usage

**Chat triggers:** `versi berapa` · `bump versi` · `rilis` · `changelog` ·
`update versi` · `naik versi` · `versioning`

**Automatic:** whenever the agent finishes a shippable change, prepares a
release, or spots version numbers disagreeing across files.

## Install

Copy the `rilis/` folder into your harness's skills directory
(`~/.claude/skills/`, `~/.cursor/skills/`, `~/.gemini/skills/`, etc.).
Pure markdown. No hooks, plugins, MCP servers, or scripts.

## Project structure

```
rilis/
├── SKILL.md        # SemVer decision tree, single source of truth,
│                   #   changelog discipline, atomic release steps
├── README.md       # You are here
├── CHANGELOG.md    # Version history (Keep a Changelog, obviously)
└── LICENSE         # MIT
```

## License

MIT. See [LICENSE](LICENSE).
