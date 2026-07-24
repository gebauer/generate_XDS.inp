# generate_XDS.INP

An automatic, **read-only mirror** of `generate_XDS.INP` — the shell script that
writes an `XDS.INP` file by reading the headers of a set of diffraction frames.

> **The authoritative version lives on the XDSwiki:**
> <https://wiki.uni-konstanz.de/xds/index.php/Generate_XDS.INP>
>
> This repository is not a fork. Its content is extracted verbatim from that
> page. If the two ever disagree, the wiki is right and this mirror has a bug.

## Why this exists

The wiki holds only the current version of the script, inside a collapsed code
block. That is fine for downloading it, and awkward for everything else:

- seeing what actually changed between two versions
- `git blame` on a line, to find which revision introduced it and why
- searching the history for a detector, a serial number or a keyword
- pinning a known-good version into a processing pipeline

A git history gives you all of that. That is the whole purpose of this
repository: a convenience for people who want to read or work with the code.

## Using it

```sh
git clone https://github.com/gebauer/generate_XDS.inp.git
cd generate_XDS.inp

git tag -l                              # every mirrored version
git diff v1.23 v1.24                    # what changed between two versions
git log --oneline                       # version history, newest first
git log -p -L '/Bruker-sfrm/,+40:generate_XDS.INP'   # history of one section
git show v1.26:generate_XDS.INP > generate_XDS.INP   # extract a pinned version
```

Provenance for any commit:

```sh
git log -1 --format='%B' v1.24
```

```
1.24: Implementation for ID30B Pilatus CdTe detector

Script-Version: 1.24
Script-Date:    14-Mar-2026
Script-Author:  Jan Gebauer
Wiki-Revision:  4761
Wiki-User:      Kay
Wiki-Comment:   /* The script */ rev 1.24
Wiki-URL:       https://wiki.uni-konstanz.de/xds/index.php?title=Generate_XDS.INP&oldid=4761
```

For actually running the script, prefer the official download rather than this
mirror:

```sh
wget https://wiki.uni-konstanz.de/pub/linux_bin/generate_XDS.INP
chmod a+x generate_XDS.INP
```

## Pull requests are not accepted here

Please do **not** send patches to this repository. It is rebuilt from the wiki,
so any commit made here would be ignored or overwritten on the next sync.

`generate_XDS.INP` is maintained by **Kay Diederichs** (University of Konstanz),
with contributions from many people across the crystallography community — new
detectors, beamline-specific rotation axes, bug fixes. Send improvements and bug
reports to him, or raise them on the XDSwiki, which is the route every entry in
the script's changelog took.

Documentation and contact: <https://wiki.uni-konstanz.de/xds/>

Issues about the *mirror itself* — a broken sync, a missing version, a wrong
attribution — are welcome here.

## How the mirror is updated

A scheduled job runs once a day and:

1. asks the MediaWiki API for revisions of the page newer than the last one
   already mirrored,
2. extracts the script from the code block under the *The script* heading and
   validates it (`#!/bin/bash` … `# end of generate_XDS.INP`, plus `bash -n`),
3. commits when the `REVISION=` line changes, and creates an annotated tag
   `vX.YZ` for that version.

Consecutive wiki edits that belong to the same version are collapsed into that
version's final state, so one tag means one released version. An edit that
changes the script *without* bumping `REVISION=` becomes a follow-up commit with
no new tag.

Each commit carries the trailers shown above; `Wiki-Revision` is also what the
sync job reads to know where it left off. Commit dates are the timestamps of the
wiki edits, so the history reflects when each version actually appeared.

The mirror starts at version **1.23** (28 Feb 2026). Earlier versions exist in
the wiki's own page history but were never mirrored.

## Authorship in the commits

Every commit is authored by the mirroring job, not by a person. The people who
wrote the code are credited in a `Script-Author` trailer instead, taken from the
`# revision …` changelog line inside the script:

```sh
git log --format='%ad  %(trailers:key=Script-Author,valueonly)  %s' --date=short
```

This is deliberate. A git author field requires an email address, and the
contributors to `generate_XDS.INP` never published one for use here — most have
no involvement with this repository at all. The credit is theirs; the commits
are not.

If you are named in this history and want the attribution corrected or removed,
open an issue.

## Copyright

`generate_XDS.INP` is the work of Kay Diederichs and the contributors listed in
its changelog. The script itself carries no licence text; it is distributed for
public use from the XDSwiki. This mirror claims no rights over it, adds nothing
to it, and exists only to make the published versions easier to read.