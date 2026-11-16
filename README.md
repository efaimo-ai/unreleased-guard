# unreleased-guard

[![npm](https://img.shields.io/npm/v/unreleased-guard?color=0b7285&label=npm)](https://www.npmjs.com/package/unreleased-guard)
[![license](https://img.shields.io/badge/license-Apache--2.0-0b7285)](LICENSE)
[![grade](https://img.shields.io/badge/efaimo%20check--skill-A%20(100)-0b7285)](https://efaimo.com/skills)
[![house-style](https://github.com/efaimo-ai/unreleased-guard/actions/workflows/house-style.yml/badge.svg)](https://github.com/efaimo-ai/unreleased-guard/actions/workflows/house-style.yml)

An Agent Skill. **Public copy describes what a reader can actually get, not what
your working tree can do.** This is the discipline for the interval in between,
which opens by itself the moment a feature merges and closes only when you
publish.

```
Skill: unreleased-guard
```

No runtime, no dependencies, nothing to configure: `SKILL.md` plus one
reference file, and any way of getting that directory onto your skills path
works.

<!-- generated:install -->

## Install

```sh
# into ./.claude/skills/unreleased-guard/
npx unreleased-guard

# into ~/.claude/skills/unreleased-guard/, for every project
npx unreleased-guard --global

# installed already, and still current?
npx unreleased-guard --check
```

The package is the skill: `SKILL.md` and its `references/`, nothing else. The
installer copies them, reads every byte back, and fails if what landed is not
what it wrote. It refuses to overwrite a directory whose contents differ unless
you pass `--force`, and installing the same version twice is a success rather
than a conflict.

Or take it by hand. It is markdown; `npx unreleased-guard --print` writes `SKILL.md` to
stdout, and the repository is the whole thing.

<!-- /generated:install -->

## The gap, which opens by itself

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/efaimo-ai/unreleased-guard/main/assets/diagram-6f6c0e6bf3-dark.png">
  <img alt="Copy generated from the working tree on main, which has the new flag, promises the reader behaviour nobody can run, because the reader gets the last published release, which does not have it." src="https://raw.githubusercontent.com/efaimo-ai/unreleased-guard/main/assets/diagram-6f6c0e6bf3-light.png" width="533">
</picture>

<details>
<summary>Mermaid source</summary>

```text
flowchart TD
    accDescr: Copy generated from the working tree on main, which has the new flag, promises the reader behaviour nobody can run, because the reader gets the last published release, which does not have it.
    M["working tree<br/><b>main</b><br/><i>has the new flag</i>"]
    P["last published release<br/><b>what a reader installs</b><br/><i>does not</i>"]
    M -->|"copy is generated from here<br/>because it is convenient"| C["README, changelog,<br/>landing page,<br/>captured output"]
    C -->|"promises behaviour<br/>nobody can run"| R(["the reader"])
    P -->|"is what the reader<br/>actually gets"| R
    classDef pass fill:#0b728522,stroke:#0b7285;
    classDef fail fill:#c9282822,stroke:#c92828;
    class P pass;
    class M,C fail;
```

</details>

The gap opens the moment a feature merges and closes only on publish. Every
document written inside it is wrong on arrival, and no gate notices, because the
working tree agrees with itself.

## The problem

The repository is the most convenient source of truth and the wrong one. Anyone
writing documentation is reading the working tree; everyone reading it is
running the last published version. Between merge and publish those are
different software, and nothing warns you, because the code is right, the docs
are right about the code, and the tests pass.

The result is a package page promising a flag that errors out for whoever
installs it. It is not a mistake people make once. It is the default outcome of
documenting what is in front of you.

## The move

1. Bump the version the moment the tree diverges from a tag. Two builds calling
   themselves 0.1.0 make every capture untraceable.
2. Annotate unreleased behaviour inline, where the line sits, so a reader
   skimming for something to copy hits the annotation first.
3. State the gap once, at the top of whatever a new contributor reads first.
4. Strip the annotations **before** the tag, as late as possible: the commit
   right before it, or the tag commit itself when your tooling tags that exact
   commit. Registries render the tagged tree.
5. Tie the claim to the released version with a check, not a convention.
6. After publishing, re-measure everything that quoted the old version.

## The part that is easy to miss

If your docs quote real command output, regenerating a capture during an open
gap stamps the unreleased version into a document about the released one. The
numbers can be identical and it is still wrong. During a gap, captures are
frozen, and the freeze has to be written where the regeneration happens rather
than where the document lives, because that regeneration is almost always done
for an unrelated reason by someone not thinking about the gap.

## Provenance

Every step here is from a CLI that has been through four releases with this gap
open each time, including the one that produced the rule about stripping before
the tag. `references/the-gap.md` has the mechanism, the check, and the table of
the three moments the annotations can come off.

The skill starts by measuring the gap (last tag, unreleased commits, what the
registry serves) and asks which version is published when it has no shell to
look.

## License

Apache-2.0. See `LICENSE` and `NOTICE`.


<!-- generated:pipeline -->

## What installing it does to a session

A skill is not free just because it is markdown. Its frontmatter is loaded at
the start of every session for every skill you have installed, whether or not it
ever fires.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/efaimo-ai/unreleased-guard/main/assets/diagram-4aa00f8445-dark.png">
  <img alt="Installing puts SKILL.md and references/ in .claude/skills/unreleased-guard/. The frontmatter enters your context window in every session, the body only when the skill triggers, and the references only if the agent reads them." src="https://raw.githubusercontent.com/efaimo-ai/unreleased-guard/main/assets/diagram-4aa00f8445-light.png" width="756">
</picture>

<details>
<summary>Mermaid source</summary>

```text
flowchart TD
    accDescr: Installing puts SKILL.md and references/ in .claude/skills/unreleased-guard/. The frontmatter enters your context window in every session, the body only when the skill triggers, and the references only if the agent reads them.
    N["npx unreleased-guard"] --> D[/".claude/skills/unreleased-guard/"/]
    D --> M["frontmatter<br/><b>every session, always</b>"]
    D --> B["SKILL.md body<br/><i>only when it triggers</i>"]
    D --> R["references/<br/><i>only if the agent reads them</i>"]
    M --> S(["your context window"])
    B -.->|"on trigger"| S
    R -.->|"on demand"| S
    classDef always fill:#c9282822,stroke:#c92828,stroke-width:1px;
    classDef lazy fill:#0b728522,stroke:#0b7285,stroke-width:1px;
    class M always;
    class B,R lazy;
```

</details>

In this skill's case, measured by [efaimo](https://github.com/efaimo-ai/efaimo) `weigh` (v0.6.0, 2026-11-02):
**96 tokens always resident**, 1,143 when it triggers, 997 across 1 reference file if the agent reads to the end.

<!-- /generated:pipeline -->

<!-- generated:set -->

## The set

Every skill in this set is about a report that was true about the wrong thing.

| skill | something reported | what the report was really about |
|---|---|---|
| [`red-before-green`](https://github.com/efaimo-ai/red-before-green) | a check said clean | whether it ran at all |
| [`denominator`](https://github.com/efaimo-ai/denominator) | a check said clean | how much of the world it saw |
| [`read-it-back`](https://github.com/efaimo-ai/read-it-back) | a write said done | whether it applied |
| [`claim-sweep`](https://github.com/efaimo-ai/claim-sweep) | a change said done | everything else still asserting the old value |
| **`unreleased-guard`** | a document said true | which version it is true of |
| [`honest-chart`](https://github.com/efaimo-ai/honest-chart) | a picture said the data | whether its geometry is proportional |
| [`mcp-stateless-migration`](https://github.com/efaimo-ai/mcp-stateless-migration) | a server said ok | which revision it speaks |
| [`efaimo`](https://github.com/efaimo-ai/efaimo) | a tool said A(100) | what a grade certifies, and what it costs |

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/efaimo-ai/unreleased-guard/main/assets/diagram-9f9ac0d509-dark.png">
  <img alt="Every skill in the set and the efaimo CLI, with an edge for each handoff between two of them. unreleased-guard is highlighted." src="https://raw.githubusercontent.com/efaimo-ai/unreleased-guard/main/assets/diagram-9f9ac0d509-light.png" width="758">
</picture>

<details>
<summary>Mermaid source</summary>

```text
graph TD
    accDescr: Every skill in the set and the efaimo CLI, with an edge for each handoff between two of them. unreleased-guard is highlighted.
    red_before_green["red-before-green"]
    denominator["denominator"]
    read_it_back["read-it-back"]
    claim_sweep["claim-sweep"]
    unreleased_guard["unreleased-guard"]
    honest_chart["honest-chart"]
    mcp_stateless_migration["mcp-stateless-migration"]
    efaimo["efaimo"]
    red_before_green --- denominator
    red_before_green --- read_it_back
    denominator --- claim_sweep
    read_it_back --- claim_sweep
    claim_sweep --- red_before_green
    claim_sweep --- unreleased_guard
    unreleased_guard --- red_before_green
    honest_chart --- red_before_green
    honest_chart --- read_it_back
    mcp_stateless_migration --- unreleased_guard
    mcp_stateless_migration --- red_before_green
    efaimo --- denominator
    efaimo --- mcp_stateless_migration
    classDef self fill:#0b728533,stroke:#0b7285,stroke-width:2px;
    class unreleased_guard self;
```

</details>

Each edge is a real handoff, not a category: the reason one skill points at
another is written into it at [efaimo.com/skills](https://efaimo.com/skills), and
in the `Siblings` section of every `SKILL.md`. All of them are graded and
weighed by [`efaimo`](https://github.com/efaimo-ai/efaimo), the CLI that measures
what an agent loads.

<!-- /generated:set -->

## License

Apache-2.0. See [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE).
