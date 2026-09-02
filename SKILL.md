---
name: unreleased-guard
description: Use whenever public-facing copy is written or changed while the working tree is ahead of what users can actually get - a README, docs site, changelog, landing page, help text, example, or captured command output - and before cutting a release or publishing to a registry. The repository is the convenient source of truth and the wrong one; readers install the last published version, so copy generated from the working tree promises flags and behaviour that nobody can run.
license: Apache-2.0
metadata:
  version: "0.1.0"
  homepage: "https://efaimo.ai"
  verified_against: "2026-09-03"
---

# unreleased-guard

The moment a feature merges, the repository can do something the published
artifact cannot. That gap stays open until you publish, which is often weeks,
and every document written in the meantime is written by someone reading the
working tree while every reader is running the registry.

Nothing warns you. The code is right, the docs are right about the code, the
tests pass, and the README on the package page describes a flag that errors out
for anyone who installs it. This is not a mistake people make once; it is the
default outcome of the most reasonable habit in software, which is to document
what is in front of you.

## The standard

**Public copy describes what a reader can actually get today, not what the
repository can do.** Concretely: what `npm install` fetches, what the app store
is serving, what the docs URL renders. Not what your branch does.

Everything below exists to keep that one sentence true across a gap that opens
by itself.

## The move

1. **Make sure no two builds carry the same version.** If the last tag is 0.1.0
   and something merges, bump the tree to 0.1.1 immediately; otherwise two
   different pieces of software both call themselves 0.1.0 and no output can be
   traced to either one. If your release tooling computes the version at release
   time, as release-please and changesets do, the tree cannot carry the next
   number and bumping is the wrong instruction. The requirement is the same
   underneath: make development builds self-identify, with a `-dev` suffix or a
   commit stamp, so that no artifact is ambiguous about which side of the gap it
   came from.
2. **Annotate every line of copy that describes unreleased behaviour, inline,
   where it sits.** Not in a tracking issue and not in a list at the bottom.
   The annotation has to be in the reader's eye at the moment they would
   otherwise treat the line as shipped.
3. **State the gap once, at the top of whatever a fresh contributor reads
   first.** This is the single fact that most often makes a newcomer write
   something false, and they cannot infer it from the code.
4. **Strip the annotations in the commit BEFORE the tag.** Not in the tag
   commit. Registries and release pages render the tagged tree, so an
   annotation still present at the tag is published verbatim, and one removed
   after the tag never reaches the artifact at all. There is exactly one commit
   where it is right.
5. **Tie the claim to the released version with a check.** Something that reads
   the published version from one place and fails when public copy disagrees.
   Prose discipline decays across sessions; a check does not.
6. **After publishing, re-measure everything that quoted the old version.** The
   gap collapses on publish, which means every capture, badge, and number that
   was correct yesterday is now describing the wrong release.

## The trap inside the trap: regenerated output

If your docs quote real command output, and you regenerate that output from the
working tree while the copy around it describes the published version, you have
stamped the unreleased version into a document about the released one. The
numbers may be identical and it is still wrong, because the version line in the
capture now contradicts the sentence above it.

So during an open gap, **captures are frozen**. Not "be careful when
regenerating": frozen, with the freeze written down where the person about to
regenerate will read it, and lifted only when the publish lands.

## The tells

- The last tag and the version in the manifest are the same, and commits have
  landed since.
- A document describes a flag, a subcommand, or a field, and you cannot say
  from memory which release introduced it.
- Copy was written in the same session as the feature it describes.
- A capture, screenshot, or transcript was regenerated for an unrelated reason.
- Someone asks "is that live yet?" about your own documentation.
- The changelog has an unreleased section and the README does not distinguish
  it.

## What this is not

This is not a rule against documenting work in progress. Write as much as you
like; the annotation is what makes it safe, and step 4 is what makes the
annotation disappear at exactly the right moment.

It is also not a versioning policy. Semantic versioning tells you what number
comes next. It says nothing about the interval during which your documentation
is describing software that no reader possesses, which is where this lives.

## Related

- `claim-sweep` is the second half of step 6: once the publish collapses the
  gap, every artifact that asserted the old version has to be found, including
  the checks that just went vacuously green.
- `red-before-green` is why step 5 says a check rather than a convention, and
  why that check has to be watched failing before it is trusted.
- [efaimo](https://github.com/efaimo-ai/efaimo) audits the quality and context
  cost of MCP servers and Agent Skills, including this one.
