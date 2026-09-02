# unreleased-guard

An Agent Skill. **Public copy describes what a reader can actually get, not what
your working tree can do.** This is the discipline for the interval in between,
which opens by itself the moment a feature merges and closes only when you
publish.

```
Skill: unreleased-guard
```

Drop the directory into your skills path. Nothing to install, no dependencies:
`SKILL.md` plus one reference file.

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
4. Strip the annotations in the commit **before** the tag. Registries render the
   tagged tree, so there is exactly one commit where this is right.
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
open each time, including the one that produced the rule about the commit before
the tag. `references/the-gap.md` has the mechanism, the check, and the table of
three moments where only one works.

## License

Apache-2.0. See `LICENSE` and `NOTICE`.

Part of [efaimo ai](https://efaimo.ai). Its sibling skills are
[claim-sweep](https://github.com/efaimo-ai/claim-sweep),
[red-before-green](https://github.com/efaimo-ai/red-before-green),
[honest-chart](https://github.com/efaimo-ai/honest-chart) and
[read-back](https://github.com/efaimo-ai/read-back);
[efaimo](https://github.com/efaimo-ai/efaimo) is the CLI that audits the quality
and context cost of MCP servers and Agent Skills, including this one.
