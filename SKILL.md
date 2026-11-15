---
name: unreleased-guard
description: Use when writing or editing public docs for software whose repository is ahead of its latest published release, and when preparing a tag or a registry publish - a README, docs site, changelog entry, help text, example or captured command output. Readers install the published version, so docs written from the working tree describe flags they cannot run. Covers measuring the gap, inline "unreleased" notes and when to remove them, freezing captured output until the publish, and a check that ties docs to the released version.
license: Apache-2.0
metadata:
  version: "0.1.0"
  homepage: "https://efaimo.com"
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

## Find the gap first

    git describe --tags --abbrev=0                                 # last tag
    git log --oneline "$(git describe --tags --abbrev=0)"..HEAD    # unreleased commits
    npm view <pkg> version                                         # what npm serves
    pip index versions <pkg>                                       # what PyPI serves
    gh release view --json tagName,publishedAt                     # latest GitHub release

If the registry, the last tag and the manifest agree and nothing follows the
tag, there is no gap and nothing here applies. If you cannot run these (no
shell), ask which version is published before writing the copy.

## The move

1. **Make sure no two builds carry the same version.** If the last tag is 0.1.0
   and something merges, bump the tree to 0.1.1 immediately; otherwise two
   different pieces of software both call themselves 0.1.0 and no output can be
   traced to either one. Where release tooling picks the number at release time
   (release-please, changesets), make development builds self-identify instead,
   with a `-dev` suffix or a commit stamp.
2. **Annotate every line of copy that describes unreleased behaviour, inline,
   where it sits.** Not in a tracking issue and not in a list at the bottom.
   The annotation has to be in the reader's eye at the moment they would
   otherwise treat the line as shipped.
3. **State the gap once, at the top of whatever a fresh contributor reads
   first.** This is the single fact that most often makes a newcomer write
   something false, and they cannot infer it from the code.
4. **Strip the annotations before the tag, as late as possible.** Registries
   and release pages render the tagged tree: an annotation still there at the
   tag ships verbatim, and one removed after the tag never reaches the
   artifact. Do it in the commit right before the tag, or in the tag commit
   itself if your tooling tags that exact commit.
5. **Tie the claim to the released version with a check.** Something that reads
   the published version from one place and fails when public copy disagrees.
   Prose discipline decays across sessions; a check does not.
6. **After publishing, re-measure everything that quoted the old version.** The
   gap collapses on publish, which means every capture, badge, and number that
   was correct yesterday is now describing the wrong release.

What each of those six steps looks like in a real repository, including the ones
that were learned by getting them wrong, is in
[references/the-gap.md](references/the-gap.md).

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

## Scope

Document work in progress as much as you like. The inline annotation is what
makes it safe, and step 4 is what takes it off at the right moment.

<!-- generated:siblings -->

## Siblings

Every skill in this set is about a report that was true about the wrong thing. The set: https://efaimo.com/skills

- `claim-sweep` - the second half of step 6: publishing collapses the gap and every artifact quoting the old version has to be found.
- `red-before-green` - step 5 asks for a check rather than a convention, and that check has to be watched failing first.

<!-- /generated:siblings -->
