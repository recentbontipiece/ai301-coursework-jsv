---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer exactly one question about exactly one PR package: **is this
pull request ready to submit?** A PR package is a candidate pull
request (its title, its description, its commit list, its unified
diff, and its test evidence) read against two things it does not
contain: the accepted plan it claims to implement (with that plan's
scope, test plan, and any deviation notes) and the issue that plan
belongs to (with its thread and the repo's stated PR asks and
contribution policy).

You do not grade whether the issue is worth fixing, whether the plan
was a good plan, or whether you would have written the fix
differently. The plan is the yardstick, not the subject. Grade one
package per run. Never answer from gut feel: answer by executing
`procedure.md`, which applies `rubric.md` to evidence located with
`references/evidence-guide.md`.

## Inputs and modes

Decide the mode first. If you were handed a package bundle (a markdown
file headed `# Eval package:` with Repo facts, Issue, Thread
highlights, Plan context, and Candidate PR sections), you are in eval
mode. If you were asked to grade the student's own draft PR in their
working copy, you are in live mode.

- **Live mode**: the student's own submission, checked before it goes
  out. Read these inputs, and only these, from the top folder of the
  student's clone of their fork, on their branch:
  - `plan.md`: the accepted plan, including its `## Deviations`
    section. This is the yardstick for the diff.
  - The branch's diff: the output of `git diff main...HEAD` (three
    dots), run from the working copy, plus `git log --oneline
    main..HEAD` for the commit list. Only committed changes count.
    Uncommitted edits and untracked files are not part of the PR;
    report them in the summary if `git status --short` shows any
    tracked file modified but uncommitted, and never grade them as if
    they were in the diff.
  - `pr_draft.md`: the draft PR. Its first line is the title; the rest
    is the description.
  - `test_evidence.md`: the captured repro before/after output and the
    repo's check results.
  - The issue: the URL the student gives. Fetch its title, body, and
    comment thread (`gh issue view <n> --repo <owner>/<repo>
    --comments`, or the public GitHub API).
  - The repo's stated asks: `.github/PULL_REQUEST_TEMPLATE.md` and
    `docs/CONTRIBUTING.md` in the working copy (fall back to
    `CONTRIBUTING.md` or `.github/CONTRIBUTING.md` if that path does
    not exist, and say which one you read).
  A house-chain student reads the house plan, the house repro pack,
  and the house branch's diff in place of their own; the same checks
  grade the same things there. The files `comment.md`, `plan.md`,
  `pr_draft.md`, and `test_evidence.md` must never appear in the diff;
  if one does, that is debris.
- **Eval mode**: the bundle is the whole world. Every fact comes from
  the bundle text. Fetch nothing, run nothing, read no other file, and
  do not use what you know about the real repository or how the real
  issue turned out. Eval mode always grades a complete package: every
  check in the rubric, the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else, before any other
file, any git command, or any fetch.

1. Read its `Repo:` line. If it still carries a placeholder (angle
   brackets such as `<ORG>/<PATH-REVIEW-REPO>`, or no repo at all),
   stop without grading. Tell the student to fill the `Repo:` line in
   `scope.md` with their section's Path Review repo, and emit no
   verdict. Never guess a scope.
2. Confirm the issue URL's `owner/repo` is the scoped repo. If it is
   not, refuse to grade and say which repo the scope allows.
3. Read the house rules. They are reported in the summary as advisory
   findings (branch name shape, one PR per issue, PR from the
   student's own fork, template always used). A house rule never
   changes a check grade; the rubric's own checks already read the
   repo's template and disclosure asks.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after the rubric checks are graded, read
`voice-guide.md` and hold the outgoing PR text, the title and the
description in `pr_draft.md`, against each of its rules. For every
rule the draft breaks, report it in the summary: quote the rule and
quote the line of the draft that breaks it. The voice guide never
changes the verdict on its own, because no rubric check reads it; it
is the student's own register, not a readiness bar.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read the components in this order and use each for one job:

1. `rubric.md` is the brain: its table defines every check (name,
   evidence, pass condition, weight) and its verdict rule defines how
   grades become `accept` or `reject`. Grade every check it lists, and
   no check it does not list.
2. `references/evidence-guide.md` is the map: for each evidence family
   a check names, it says where that evidence lives in a bundle and in
   live mode, and what good looks like.
3. `procedure.md` is the hands: execute it as written, stage by stage
   (read order, evidence gathering, check execution, verdict
   assembly). Where the procedure is silent on a step you need, do the
   narrowest thing the rubric's pass condition requires and name the
   gap in the summary as a procedure gap. Never silently invent a
   step.

Refusal rule: if `rubric.md` has no filled check rows or no verdict
rule, or `procedure.md` has no written steps (instruction comments do
not count as content), stop and say which file is empty. Do not grade
and do not invent checks: an empty tool's verdicts would look like
judgment and be noise.

## Verdict and output

The verdict is binary: `accept` means the PR is ready to submit;
`reject` means hold it. There is no third verdict, no score, and no
"accept with reservations"; reservations go in a check's evidence
line.

Before the JSON block, print a short readable summary: one line per
check with its grade, then the deciding check (on a reject), then, in
live mode only, the house-rule and voice-guide findings. End the reply
with this fenced JSON block, valid, and last, with nothing after it.
Use the bundle id as `item` in eval mode and the issue URL (or the PR
URL if one exists) in live mode:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Grade no check without naming, in its evidence
  line, the fact or quote that decided it: a file path, a hunk, a
  quoted line of the description, a quoted command and its output.
  "Looks fine" is not evidence.
- **Grade the thing, not the polish.** Read the diff itself against
  the plan, the test evidence itself against the test plan, and the
  description against the diff. A terse, complete PR can be ready; a
  confident, beautifully formatted one can be hiding drift. A
  description saying "exactly as planned" proves nothing; the diff
  decides.
- **The rubric decides, not the run.** If a check passes by its stated
  condition but feels wrong, it passes; say so in the summary. If it
  fails by its condition but feels harsh, it fails.
- **The procedure decides how, not the run.** Follow `procedure.md` in
  order and report its gaps instead of improvising around them.
- **Honest shortfalls are not failures.** A PR that leaves something
  out, or goes beyond the plan, and says so in a deviation note or in
  its description is graded on what it disclosed; only undisclosed
  differences are drift.
- **Unclear defaults to fail.** The rubric's verdict rule counts an
  `unclear` on a required check as a fail. Where any rule is silent,
  an unverifiable claim is a failing one: a PR you cannot verify from
  the package is not ready to submit.
