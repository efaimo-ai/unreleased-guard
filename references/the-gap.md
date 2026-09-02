# Working the gap: what each step looks like in a real repository

The rules in `SKILL.md` are short because they were expensive. This is the
mechanism behind each one, from a CLI that has been through four releases with
the gap open every time.

## Why "bump immediately" is not pedantry

A tree twenty commits past `v0.1.0` while still stamping `0.1.0` into every
generated file means a run produced by the working tree cannot be told apart
from one a reader could reproduce. That is fatal for any document that invites
readers to check its numbers, because the invitation is now false and there is
no way to see it from either artifact.

Bumping on divergence costs one line and makes every capture self-identifying
for the rest of the gap. The published number and the tree's number are then
different, which is uncomfortable, correct, and the entire point.

## What an inline annotation looks like

Beside the line, not below the document:

```markdown
- `--min-distinct <pct>` fails the run when fewer than pct percent of tools own
  a word no other tool has. _(unreleased)_
```

The test for a good annotation: someone skimming for a flag to copy-paste has
to hit it before they hit the flag. An entry in a tracking issue, a note in the
changelog, or a section at the bottom all fail that test, because the reader
who is about to make the mistake is not reading those.

## Why the commit before the tag

Three candidate moments, and only one works.

| when the annotations come off | what the registry renders |
|---|---|
| commit before the tag | clean copy, no annotations, describing the release |
| the tag commit itself | usually fine, but any tooling that tags an existing commit publishes the annotated tree |
| after the tag | the annotations ship, verbatim, on the package page |

The failure mode of "after" is silent for the author, since their tree looks
right, and permanent for the reader, since the registry renders the tag. The
package page will say "unreleased" next to a feature that is, at that moment,
released.

## The check, and why prose will not do

A rule that lives only in a document is enforced by whoever remembers it. Across
sessions and contributors that is nobody, reliably.

The shape that works is small: read the published version from exactly one
place, then assert that public copy agrees with it.

```js
const released = JSON.parse(readFileSync("site/release.json")).version;
const claimed = [...readme.matchAll(/\bv?(\d+\.\d+\.\d+)\b/g)].map((m) => m[1]);
if (!claimed.length) fail("the README names no version; this check is aimed at nothing");
if (!claimed.includes(released)) fail(`README says ${claimed.join(", ")}, released is ${released}`);
```

Two things about that snippet matter more than the comparison. **One source for
the released version**, so there is nothing to keep in sync. And **the empty
case fails**: a README that names no version at all would otherwise satisfy any
"does it disagree" test forever, which is a check that has stopped checking
while still reporting green. Watch it fail on purpose before you trust it.

## The freeze, and how it gets forgotten

During an open gap, regenerating a capture stamps the tree's version into a
document about the release. The reason this bites is that the regeneration is
almost never done for that document; it is done for an unrelated reason, by
someone who is not thinking about the gap.

So the freeze has to be written where the regeneration happens, not where the
document lives. In practice that means the same warning in more than one place:
the onboarding file, the doc itself, and ideally a check that reads the freeze
and enforces it.

## Publishing collapses everything, and that is when the second failure starts

The moment the artifact ships, three classes of thing become wrong at once:

1. Every annotation, which is now describing shipped behaviour.
2. Every capture and badge stamped with the previous version.
3. Every check whose premise was the previous version. These are the dangerous
   ones. A check comparing copy against release 0.1.2 does not fail when you
   ship 0.2.0; it keeps passing against a number nobody uses any more.

The third is why `claim-sweep` names the checkers explicitly among the artifacts
to re-aim. A publish is a claim flip, and a claim flip blinds the instruments
that were watching the old claim.
