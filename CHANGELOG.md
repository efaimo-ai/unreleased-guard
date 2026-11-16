# Changelog

All notable changes to unreleased-guard are documented here. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/); this project uses
[Semantic Versioning](https://semver.org/).

## [0.1.0] - 2026-11-16

> **Published without provenance.** This is the first version of `unreleased-guard` on
> npm, and it came from an authenticated session: npm's trusted publishing is
> configured from an existing package's settings page, so it cannot create a
> package that has never been published. From the next version the release
> workflow publishes it, with SLSA provenance naming the commit that built it.

### Added

- **The skill is installable.** `npx unreleased-guard` copies `SKILL.md` and its
  `references/` into `./.claude/skills/unreleased-guard/`, with `--global`, `--dir`,
  `--check`, `--print`, `--uninstall` and `--force`. The installer reads every
  byte back after writing it and fails if what landed is not what it wrote, and
  it prints how many files it installed rather than only that it succeeded.
- Three exit codes rather than two: 0 installed or already identical, 1 present
  and different, 2 could not run at all. "It differs" and "I could not do this"
  are different answers and collapsing them is how a broken install reads as a
  clean one.
- CI on four cells (Ubuntu and Windows, Node 20 and 24), including an end to end
  run that packs the package the way npm would, installs it from that tarball,
  and installs the skill from there.

### Changed

- New "Find the gap first": the commands that show the last tag, the commits
  since, and what npm, PyPI and GitHub Releases serve, and asking which version
  is published when there is no shell.
- One rule on when annotations come off, in `SKILL.md` and
  `references/the-gap.md` alike: before the tag, as late as possible, which is
  the commit right before it or the tag commit itself when the tooling tags
  that exact commit. The two used to disagree about the tag commit.
- The description names public docs for software ahead of its release, and
  tag or publish preparation, instead of any changelog edit.
- "What this is not" is down to the one sentence that changes what an agent
  does.

### Notes

- The package version and the version in `SKILL.md` frontmatter are one number,
  pinned by a test.

[0.1.0]: https://github.com/efaimo-ai/unreleased-guard/releases/tag/v0.1.0
