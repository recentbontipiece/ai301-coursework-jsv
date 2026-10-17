# Evidence guide: where evidence lives in a PR package

Each family below maps to one harness category. A package that fails
none of them is a clear accept.

## Plan fidelity (harness category: silent-drift)

**Where it lives.** In a bundle: the `## Plan context` block (the
"Plan:" paragraph's scope sentence, its "Not in scope:" list, its
"Files:" list, its "Test plan:", and any "Deviation note" or deferral),
read against the `### Diff` under `## Candidate PR` (each `+++ b/`
path and each `@@` hunk) and the `### Description`'s claims. In live
mode: `plan.md` (its `## Scope`, `## Files`, `## Approach`, and `##
Deviations` sections) read against `git diff main...HEAD`, and the
description in `pr_draft.md`.

**What good looks like.** Every file in the diff is on the plan's
files list or named in a deviation note, every hunk does something the
plan's scope or approach commits to, and everything the plan promises
(including a docs change or a test) is in the diff or openly deferred.
An honest deviation re-ties a mismatch: a deferral named in the plan's
note and restated in the description is in bounds. Silent drift runs
both ways: more than the plan (a refactor, a rename pass, a new config
option or flag, a second file rewritten) or less than the plan (a
promised docs update missing) with no note. A description claiming
"exactly as planned" or "no changes beyond the plan" over a diff that
has more, or claiming docs or tests the diff lacks, is drift, not a
comms slip.

## Test evidence (harness category: not-tested)

**Where it lives.** In a bundle: the `### Test evidence` section,
read against the plan context's "Test plan:" and "Repro evidence"
(the triggering inputs and the expected-after), and the repo-facts
block's named test command. In live mode: `test_evidence.md` (repro
re-run on `main` for before and on the branch for after, plus `make
test-unit`, `make test-integration`, `make lint`, `make typecheck`
output), read against the test plan in `plan.md`.

**What good looks like.** The plan's own repro re-run on the inputs
that trigger the bug, with a before and an after that read
differently (exit code, printed value, error text gone) and the after
matching the plan's expected-after; every failure mode the test plan
names gets its own run; and at least one named suite or check command
with its result visible (pass count or output). Not decisive: "tested
locally, works now", "verified on my machine", "cargo test passes"
with no repro; evidence on a control or a different path (a GET when
the bug is in POST, the single-file case when the bug needs two
files); a new test that covers something other than the fix. A
failing check reported honestly with its reason is still evidence.

## Diff quality (harness category: unreviewable)

**Where it lives.** In a bundle: every hunk of the `### Diff`, and the
`### Commits` list. In live mode: `git diff main...HEAD` and `git log
--oneline main..HEAD`.

**What good looks like.** The fix is visible in the first read, its
test sits beside it, and nothing else rides along. Debris tells, each
one enough to fail: a debug print or `eprintln!("DEBUG ...")` left in;
a commented-out line or an earlier attempt kept as comments; a dead
helper (an `_unused_...` name, `#[allow(dead_code)]`); a TODO added on
an unrelated line; re-indent, reformat, import-reorder hunks, or
identical lines removed and re-added; edits to files the fix does not
need. Commit messages like "wip", "fix", "misc cleanups while
debugging" often point to where the debris is. In live mode,
`plan.md`, `comment.md`, `pr_draft.md`, or `test_evidence.md`
appearing in the diff is debris.

## Standards and comms (harness category: standards-wall)

**Where it lives.** In a bundle: the `## Repo facts` block's
"pull requests:" line (template sections, required checklist,
issue-link line, CHANGELOG or whatsnew entry, title convention) and
"contribution policy:" line (any AI policy), read against the
`### Title`, `### Description`, and the diff's file list; maintainer
direction lives in `## Thread highlights` (entries marked `OWNER`,
`MEMBER`, `COLLABORATOR`). In live mode: `.github/PULL_REQUEST_TEMPLATE.md`
(Summary, Issue with `Closes #`, Changes, Testing checklist,
Screenshots / Demo, Notes for Reviewers) and `docs/CONTRIBUTING.md`,
read against `pr_draft.md`; the course's AI-use disclosure goes in
Notes for Reviewers.

**What good looks like.** Each section or checklist the repo names is
present with real content (an inline label like "Problem:" counts as a
section); a required "closes #N"/"fixes #N" line is there; a required
CHANGELOG or `whatsnew` file shows up in the diff's file list; where
the AI policy requires disclosure, the description says AI was used
and, if asked, which tool and how much; where it asks for own words,
the description reads as a person writing. Asks a PR's text cannot
show (a signed CLA, a vouch flow, CI status) are out of reach and not
graded. No template and no stated asks means nothing to meet. Whether
the description's claims are true of the diff is plan fidelity, above.
