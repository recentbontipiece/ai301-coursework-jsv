# Contribution: amd/gaia#5281: macOS menu-bar icon is a plain square, not the GAIA logo

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** John Vallon
**GitHub Username:** recentbontipiece
**Issue:** https://github.com/amd/gaia/issues/5281
**Pull Request:** not opened yet (waiting for maintainer reply to claim and repro, 2026-11-08)

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: grew issue-select into issue-scout, wrote my search strategy, and patched my rubric after three wild scans (2026-11-08)**

I turned my issue-select skill into issue-scout, which searches all of
GitHub for me. My first scan found only brand-new tiny projects, so I
changed my search to skip them and to prefer bugs. My second scan
showed my rubric still ignored people's "I'll take this" comments, a
leftover classroom rule, so I fixed it and re-tested it against the
Unit 1 test set (20/20). My third scan found the issue I claimed.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched all of GitHub with issue-scout: four `gh search issues`
queries for "good first issue" labels (Python bugs, Python docs, any
Python issue, TypeScript), ranked by my strategy's free filters, with
the top five graded by my rubric. amd/gaia#5281 came up in scan 2 and
again in scan 3, and both times my scout accepted it: the repo is
active (all five latest commits on the scan day, 1.6k stars), nobody is
assigned, no PR is open, the contribution policy has no AI ban, and it
is one bounded change with an acceptance checklist. In scan 3 it was
the only accepted issue I could realistically take. The other accept
(spyre-inference#672) needs IBM accelerator hardware and auto-merges
PRs with no human review. It fits my profile because it's an active
project with real maintainers, it's a bug fix with a clear finish line,
and it needs a Mac, which I have. The downside: the work is image files
and JavaScript tests, not Python, which is the language I most wanted
to practise. I want to see how a real project reviews code from someone
new, and how template images and the project's asset tests work.

As a lead Android developer, I review code and work with other leads to
maintain our codebase, but this is the first time in my life I've posted
on an AMD project, and that felt weird. After reviewing the issue, I
think I have a good understanding of what the problem is and how to fix
it, and that's why I want this fix merged. How fast people claim "good
first issues" didn't surprise me at all: that's expected for open-source
projects.

The scout fell short in the wild in three ways, and each became a patch.
Scan 1's ranking rewarded newness, so it surfaced same-day toy repos.
My rubric still ignored claim comments, so it accepted pydynox#559,
which someone had already claimed. And 3 of 5 grading slots went to
issues with open linked PRs. I fixed all three (see Toolkit). One gap
is still open: nothing in my rubric checks that a human reviews PRs.
An auto-merge bot setup passed in scan 1 (agentswarm-cli#4) and again
in scan 3 (spyre-inference#672).

Claim comment (2026-11-08): https://github.com/amd/gaia/issues/5281#issuecomment-6728341739

**Toolkit**

- Tool: issue-scout, run as `claude "issue-scout: run a scan"` three
  times. It follows my `search-strategy.md` (queries + ranking filters,
  top 5) and grades with my `rubric.md`.
- What it produced: scan 3 graded 5 of 111 ranked survivors (377 unique
  results). It accepted amd/gaia#5281 with every required check passing:
  repo-liveness, not-claimed, policy-allow, bounded-scope, and
  well-specified.
- Difficulties: scan 1's top 5 were all minutes-old issues in tiny repos.
  The rubric's `not-claimed` check still followed the classroom rule and
  passed a claimed issue. Search can't see claim comments, so 3 of 5
  scan-3 slots still failed on them. And no check catches bot-only
  review.
- What changed: strategy v2 (date windows, issue# >= 50,
  translation/Windows/classroom drops, strict Q1>Q2>Q3>Q4, prefer
  MEMBER/COLLABORATOR), then `-linked:pr` on every query. The rubric's
  `not-claimed` now counts recent claim comments (Unit 1 regression
  20/20). I'd add a required "a human maintainer reviews PRs" check
  next.

---

## Understanding the Issue

**Problem Description**

GAIA's desktop app puts an icon in the macOS menu bar. The two image
files it uses there are placeholders, not the GAIA logo. They are
solid-white squares. The Linux tray had the same kind of placeholder (a
red square, #5268), which was fixed in #5273, but the macOS files were
left as they were.

**Expected Behavior**

The menu bar shows the GAIA logo as a proper macOS *template image*: a
black glyph on a transparent background, which macOS tints
automatically for light and dark menu bars.

**Current Behavior**

Both template files are a single colour: every pixel is opaque white.
macOS draws template images from their alpha channel, so a fully
opaque image should show as a filled square. I have confirmed the files,
not yet the rendered menu bar (see Reproduction).

**Affected Components**

- `src/gaia/apps/webui/assets/tray-iconTemplate.png` and `tray-iconTemplate@2x.png`:
  the placeholder assets.
- `src/gaia/apps/webui/services/tray-manager.cjs`: `ICON_SPECS.darwin`
  (lines 84–90) loads those two files at 16/32 px, and `_iconFor()`
  calls `setTemplateImage(true)` (line 651).
- `src/gaia/apps/webui/services/tray-icons.cjs`: draws the state badges
  on top of the logo, in black for template images.
- `tests/electron/tray-manager.test.cjs` and `tests/electron/_helpers/tray-icon-png.cjs`:
  the #5273 placeholder check, which currently runs only on the Linux
  assets.
- `src/gaia/apps/webui/assets/tray-icon-active*.png`: unused red
  placeholders the issue asks to remove (`git grep` finds no references).

**Toolkit**

- Tool: Claude Code reading the clone, plus `gh` for the linked PRs
  (#5273, #5354). No course skill covers this phase.
- What it produced: the file list above, traced from `ICON_SPECS.darwin`
  to the assets and to the existing #5273 asset check.
- Difficulties: a merged PR (#5354) cross-references the issue, which
  made it look fixed. I had to check that it never touched the template
  files.
- What changed: nothing.

---

## Reproduction Process

**Environment Setup**

macOS 26.2 (arm64), Node v24.5.0, Git 2.40.0. I cloned `amd/gaia` at
`main` = `03b04eea08f089e76a2d3cdc38d9877db21cb122` (2026-11-08).
The check needs only Node and the repo's own test helper, with no `npm
install` and no app build. The one challenge was the clone itself: the
repo is large, and a full clone took several minutes. I have not yet
built and run the desktop app.

**Steps to Reproduce**

1. From the repo root, run the #5273 placeholder check
   (`assertTrayLogoPng` in `tests/electron/_helpers/tray-icon-png.cjs`)
   on `tray-icon.png` (32 px), `tray-iconTemplate.png` (16 px), and
   `tray-iconTemplate@2x.png` (32 px), using a short `node -e` script
   (full command in my repro comment).
2. Decode both template files with the helper's `decodeRgbaPng` and list
   every distinct RGBA value.
3. Observed result: `tray-icon.png` passes. Both template files fail with
   "corner (0,0) is opaque — expected the round logo on a transparent
   background, not a filled square". Each template file contains exactly
   one value, `ffffffff` (opaque white). `tray-manager.cjs` lines 84–90
   confirm these are the files macOS loads.

**Reproduction Evidence**

- **Commit showing reproduction:** none yet. I reproduced on an
  unmodified clone of upstream `main` (`03b04ee`), so there is no fork
  commit. Planned: fork, and commit a failing test that runs the #5273
  check on the macOS template files.
- **Screenshots/logs:** the command output is posted in my repro comment:
  https://github.com/amd/gaia/issues/5281#issuecomment-6728581084
- **My findings:** confirmed at the asset level. Both macOS template
  files are flat opaque-white squares with no glyph and no transparency,
  and they fail the same check #5273 added for Linux. Not yet confirmed:
  the rendered menu bar (needs a full app build). Open question I asked
  the maintainer: `ICON_SPECS.darwin` uses 16/32 px, while the issue
  suggests about 18/36 or 22/44.

**Toolkit**

- Tool: Claude Code ran the reproduction in my clone and drafted both
  comments; I read them and posted them myself.
- What it produced: the two-command, Node-only reproduction above, and
  a posted repro comment that separates what is confirmed (assets) from
  what is not (menu-bar rendering).
- Difficulties: my posted claim first promised "a PR within the next
  week", which breaks my voice guide ("I never promise a fix, merge, or
  deadline"). I edited the comment to remove it. Also, repro-check's
  live mode can't grade this package: its staff `scope.md` still allows
  only the Path Review repo.
- What changed: I edited the claim comment upstream. Next time I'll hold
  the claim draft against my voice guide before posting, not after.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
