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

**Unit 6: installed the voice guard with new upstream rules, and wrote a memory file for my gaia clone (2026-11-08)**

I merged my three voice-guide copies into one, added rules for claims,
questions and pings, and installed the guard hook. The wiring test was
stopped as expected. The guard then caught my already-posted claim: it
broke my new rule "A first step and a report-back, never a date for the
fix", so I rewrote the draft until it passed, which showed me the guard
works best before posting, not after. My `CLAUDE.local.md` in the gaia
clone (58 lines, kept out of git) records the tray-icon files, the PR
rules, gaia's "no Claude attribution" rule, and the tray test command,
which I ran myself (135 tests pass on `main`, even with the blank icons,
which shows the test gap). What fell short: my `claude` command broke
during an update and had to be reinstalled before the guard could run.

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
  well-specified. In Unit 6 I re-graded it with the patched rubric: still
  accept, with the difficulty note **moderate**. The files are named, but
  the fix touches two images, the #5273 test helper and the tray tests,
  and the icon size (16/32 vs 18/36 or 22/44) is an open question I've
  already asked. The other pick, spyre-inference#672, was rated
  **ambitious** (it needs IBM hardware I don't have).
- Difficulties: scan 1's top 5 were all minutes-old issues in tiny repos.
  The rubric's `not-claimed` check still followed the classroom rule and
  passed a claimed issue. Search can't see claim comments, so 3 of 5
  scan-3 slots still failed on them. And no check catches bot-only
  review.
- What changed: strategy v2 (date windows, issue# >= 50,
  translation/Windows/classroom drops, strict Q1>Q2>Q3>Q4, prefer
  MEMBER/COLLABORATOR), then `-linked:pr` on every query. The rubric's
  `not-claimed` now counts recent claim comments (Unit 1 regression
  20/20). Unit 6 scope patch: I replaced my strict `bounded-scope` check
  with `scope-feasible`, which only rejects issues that aren't one piece
  of work. Open design questions and "TBD"s now give a difficulty note
  (straightforward / moderate / ambitious) instead of a reject. I'd still
  add a required "a human maintainer reviews PRs" check, and make
  `not-claimed` say "other than my own claim".

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

- **Commit showing reproduction:** none. I reproduced on an unmodified
  clone of upstream `main` (`03b04ee`).
- **Screenshots/logs:** the command output is posted in my repro comment:
  https://github.com/amd/gaia/issues/5281#issuecomment-6728581084
- **My findings:** confirmed at the asset level. Both macOS template
  files are flat opaque-white squares with no glyph and no transparency,
  and they fail the same check #5273 added for Linux. Not yet confirmed:
  the rendered menu bar (needs a full app build). Open question I asked
  the maintainer: `ICON_SPECS.darwin` uses 16/32 px, while the issue
  suggests about 18/36 or 22/44.

**Toolkit**

- Tool: repro-check in live mode, with the new real-repo scope, grading
  my claim draft and repro report together against my rubric and voice
  guide. Claude Code ran the reproduction and drafted both comments; I
  read them and posted them myself.
- What it produced: accept, with all five required checks passing
  (issue-match, rerunnable, environment-fidelity, evidence-support,
  repo-comms). Its key reading: my output shows the same flat-white
  files the issue describes, and the report says plainly that I haven't
  seen the menu bar itself.
- Difficulties: I posted the claim and the repro comment before Unit 6's
  tools existed. My first claim promised "a PR within the next week",
  which I edited out. The Unit 6 instructions say the report shouldn't
  go on the issue at all, but mine was already up. Running repro-check
  from a script (`claude -p`) failed because that session couldn't read
  the skill's files, so I ran it inside an interactive session instead.
- What changed: I now check every draft with the voice guard and
  repro-check before it goes upstream, not after. The rest of the proof
  will go in my PR description.

---

## Solution Approach

**Analysis**

On macOS, `src/gaia/apps/webui/services/tray-manager.cjs` picks its icons
from `ICON_SPECS.darwin` (lines 84-90): `tray-iconTemplate.png` at 16 px
and `tray-iconTemplate@2x.png` at 32 px. `_iconFor()` loads them, draws
the state badge with `drawStateIcon(..., { template: true })` from
`tray-icons.cjs`, and calls `setTemplateImage(true)` (line 651). macOS
draws a template image from its alpha channel only. Both files were
placeholders with every pixel opaque white (`ffffffff`), so the alpha
mask was a full square. Nothing caught it because the placeholder check
from #5273 (`assertTrayLogoPng` in
`tests/electron/_helpers/tray-icon-png.cjs`) only runs on the Linux
icons: the tray tests pass on `main` (135 tests) with the placeholders
still there. `tray-icon-active.png` and `@2x` are flat red placeholders
that nothing references.

**Proposed Solution**

Replace both template PNGs with the GAIA logo as a real template image:
a black glyph on a transparent background, made from the existing tray
logo (`tray-icon@2x.png`). It's the robot head with the visor cut out
and the two eyes kept, so it still reads as GAIA at 16 px. Add a test
check for template icons, because `assertTrayLogoPng` asks for 64+
colours, which a one-colour glyph can't have. Remove the unused
`tray-icon-active*.png` placeholders.

Not changing: the logic in `tray-manager.cjs` and `tray-icons.cjs`, the
badges, the Linux and Windows icons, or the sizes. I kept 16/32 px to
match `ICON_SPECS.darwin` and asked the maintainer about 18/36 or 22/44.
If they want the larger sizes, I'll change the spec in the same PR. No
new dependencies, and the script I used to make the PNGs stays out of
the PR.

**Implementation Plan**

1. Branch `fix/5281-macos-tray-template-icon` off `upstream/main`.
2. Test first: add `assertTrayTemplatePng` (size, transparent corners,
   every visible pixel black, glyph covering 20-90%) to the test helper,
   plus a "macOS tray template asset" block in `tray-manager.test.cjs`.
   Confirm it fails on the old placeholders.
3. Regenerate both template PNGs from `tray-icon@2x.png` and confirm the
   tests pass.
4. Delete `tray-icon-active*.png` and confirm nothing references them.
5. Re-run my reproduction and every check in my ship manifest, and save
   the before/after output.
6. Check the real menu bar on my Mac if I can build the app; if not, say
   so in the PR.

Deviations (2026-11-08):
- Step 2: the macOS block needed the same `solidPng()` builder the Linux
  block had, so I moved the shared PNG helpers above both blocks instead
  of copying it. That's a test-file change the plan didn't name, still
  inside the same file.
- Step 3: the first icons had faint stray pixels around the edge, so the
  final version drops any pixel under 16% opacity before saving.

**Toolkit**

- Tool: plan-check, grading `plan.md` and `plan-comment.md` against the
  issue and my posted repro, using my procedure and rubric.
- What it produced: accept, with all six required checks passing
  (grounded-cause, bounded-scope, executable-start, decisive-test,
  thread-direction, repo-policy) and the preferred stated-unknowns check
  passing too. It used `tray-icon.png` passing the same check as the
  control that makes the cause believable.
- Difficulties: I first tried running it as `claude -p` from a script,
  which couldn't read my skill files, so I ran it in an interactive
  session. My rubric's bounded-scope check made me justify deleting the
  red placeholders; it passed only because the issue's own Acceptance
  list asks for it.
- What changed: nothing in the plan. The plan comment stays on my
  computer and isn't posted, as the real-repo scope says.

---

## Testing Strategy

Three observable checks, each run before and after the fix:

1. **My reproduction, re-run on the branch.** Before (`main` at
   `03b04ee`): both template files contain one value, `ffffffff`, and
   fail the #5273 check with "corner (0,0) is opaque". After (branch at
   `bc94fe5`): every visible pixel is black (`0,0,0`), the alpha channel
   carries the shape (82 and 156 levels), it covers 56% / 48% of the
   icon, and both files pass the new `assertTrayTemplatePng`.
2. **The new tests fail first, then pass.** With only the test commit
   (`4d093a2`), `npx jest tray-icons tray-manager` gave "2 failed, 139
   passed", the two failures being the real icons. With the new icons
   (`a746401`): "141 passed".
3. **The project's own checks from my ship manifest.** The full Electron
   suite (`cd tests/electron && npm test`, as CI runs it): 60 suites,
   1563 passed, 2 skipped. `util/lint.py --all` and `pytest tests/unit/`:
   pending until my Python `.venv` is set up.

Not covered yet: the real macOS menu bar in light and dark mode, which
needs the desktop app built. I checked rendered previews instead (the
glyph on a light and a dark background), and the PR says so.

**Toolkit**

- Tool: the repo's own Jest suite, my Node reproduction commands, and my
  ship manifest's check list.
- What it produced: a clear fail-then-pass on the new tests, the full
  Electron suite green, and before/after pixel output saved in
  `test_evidence.md`.
- Difficulties: my first "after" pixel printout was wrong. It converted
  colours to hex without leading zeros, so it showed alpha values as if
  they were colours. I caught it and re-ran it with plain RGB numbers.
  Lint and pytest need the project's Python environment, which I still
  have to set up myself.
- What changed: the evidence file uses the corrected printout.

---

## Implementation Notes

**2026-11-08**

Built a prototype of the macOS icon in a scratch folder first, from the
existing `tray-icon@2x.png`: the robot head's silhouette with the dark
visor cut out and the cyan eyes kept. Viewed on light and dark
backgrounds, it still reads as GAIA, so I wrote the plan around it.
plan-check accepted the plan.

**2026-11-08**

On branch `fix/5281-macos-tray-template-icon`: wrote the template check
and tests first (`4d093a2`). They failed on the two real icons, as
expected. Moved the shared PNG test helpers up so the Linux and macOS
blocks both use them, which I noted as a deviation. Generated the new
icons, dropping near-invisible stray pixels (`a746401`): 141 tray tests
pass. Removed the unused red `tray-icon-active*` placeholders
(`bc94fe5`). Commits follow gaia's `type(scope): summary` style with no
AI credit line, because gaia's CLAUDE.md forbids one.

**2026-11-08**

Drafted the PR description from gaia's template. My voice guard stopped
it: I had ticked "real-world evidence matched to the surface I changed",
but the real menu bar isn't captured yet. I unticked it and explained
why, and the guard then passed it.

**Toolkit**

- Tool: Claude Code on the branch, with my `CLAUDE.local.md` memory
  file loaded (the files involved and gaia's no-attribution rule), plus
  my voice guard on the PR draft.
- What it produced: three small commits (tests, icons, cleanup) and a
  PR draft that passes my voice rules.
- Difficulties: installing packages and creating the Python environment
  were blocked for the assistant, so those steps are mine to run. The guard
  caught a checkbox I shouldn't have ticked.
- What changed: I now run the voice guard on the PR draft before the
  ship-gate, not only at posting time.

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
