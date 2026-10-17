# Rubric: is this pull request ready to submit?

A PR package is ready when the diff is the plan (plus whatever the
plan's deviation notes and the description openly add or defer), the
description says what the diff actually does, the evidence shows the
reported behavior change on the path the plan named and the repo's own
checks were run, the diff contains the change and nothing riding
along, and the repo's stated PR asks and AI policy are met. Any one of
those missing holds the package. An honestly disclosed shortfall is
not a miss.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | Every file and hunk in the diff, listed and read against the plan's scope, not-in-scope list, files list, and approach steps, plus any deviation note in the plan context and any deferral or deviation stated in the PR description. | Every change in the diff is something the plan commits to, or is named in a deviation note or in the description as a deliberate addition; and every change the plan commits to (code, tests, docs) is in the diff, or is named as deferred or left out in a deviation note or the description. Fails on any undisclosed extra: a rewrite, rename pass, new option, new flag, new setting, or a touched file the plan never names. Fails on any undisclosed omission. Work the plan names as not in scope appearing in the diff fails even when it is adjacent or small. | required |
| description-matches-diff | Each factual claim the PR description makes about the change ("implements the plan exactly", "no functional changes outside X", "docs now document Y", "tests added for A, B, C", "CHANGELOG entry added"), each checked against the diff. | Every claim the description makes about what the diff contains, or does not contain, is true of the diff. A claim of fidelity ("exactly as planned", "no changes beyond the plan") over a diff with unplanned changes fails; a claim that something was added (docs, tests, changelog) when the diff does not contain it fails. A description that makes no false claim passes, however short it is. | required |
| decisive-evidence | The test-evidence section, read against the plan's test plan and the issue's or plan's repro steps. | The evidence re-runs the failing path the plan's test plan names (the issue's repro, on the inputs that trigger the bug) and shows an observable outcome that differs before and after (an exit code, printed output, a value, a rendered result), with the after matching the plan's expected-after. If the test plan names more than one failure mode or repro, each one is shown. Fails on "tested locally", "works now", "verified on my machine" with nothing shown; on evidence that runs only a control or the unchanged path; and on a new test that exercises something other than the fix. | required |
| repo-checks-run | The test-evidence section and description, read against the test command the plan's test plan names and the test or check command the repo facts ask for (cargo test, pytest, go test, yarn test, make targets). | The output or explicit result of at least one named suite or check command is shown (the command named, with a pass count, a passed/failed line, or the pasted output). A failing check reported honestly with its reason passes. Fails when no suite or check result appears, or when the evidence says only "tests pass" without naming what was run. | required |
| clean-diff | Every hunk of the diff, and the commit list. | The diff contains only the change and its tests or docs. Fails on any of: debug output left in (print, eprintln, console.log, dbg, "DEBUG"); commented-out code or a commented-out earlier attempt; a dead or unused function or variable added (including behind allow(dead_code) or an "unused" name); a stray TODO or FIXME added to unrelated code; formatting, re-indent, import-reorder, or re-printed identical lines in hunks the fix does not need; edits to files unrelated to the fix. Comments that explain the fix are not debris. | required |
| template-asks | The repo-facts block's pull-request line (PR template sections and checklist, contributing-guide asks such as an issue-link line, a CHANGELOG or whatsnew entry, a conventional-commit title), read against the PR title, description, and diff. | Every ask the repo states as part of a PR, and that the package can show, is visibly met: each named template section or checklist is present with real content (headings may be inline labels), the issue is linked where the template asks for a closes/fixes line, a required changelog or release-notes file entry appears in the diff, a required title convention is followed. Asks that cannot be shown in a PR's text or diff (a signed CLA, a vouch flow, a CI run, a code of conduct agreement) are not graded here. Where the repo states no PR template and no PR asks, this passes. | required |
| ai-disclosure | The repo-facts block's contribution policy, specifically any stated AI policy, read against the PR description. Treat every package as AI-assisted work. | The description does what the stated AI policy asks of a pull request. If the policy requires AI use to be disclosed (or to name the tool and extent), the description contains that disclosure. If the policy asks that text to maintainers be in the contributor's own words, the description reads as a person writing, and an explicit own-words statement satisfies it. If the repo states no AI policy, this passes. | required |
| thread-engagement | The thread highlights, specifically any explicit question, direction, or concern from a maintainer, owner, or member, read against the PR description. | Where a maintainer raised a question or gave direction the change bears on, the description addresses it or says why it goes another way. Where the thread carries no such direction, this passes. | preferred |
| commit-hygiene | The commit list. | Commit messages describe the change ("wip", "fix", "fmt + cleanup", "misc cleanups" fail), and follow the repo's stated commit convention if one is given. | preferred |

## Verdict rule

The verdict is `accept` if and only if every `required` check grades
`pass`. Any `required` check grading `fail` makes the verdict
`reject`. There is no weighing of how many failed.

An `unclear` grade on a `required` check counts as a `fail`: a PR whose
readiness cannot be verified from the package is not ready to submit.

`preferred` checks are graded and reported with an evidence line, but
never change the verdict, whatever their grade.

On `reject`, the deciding check is the first `required` check that
failed (counting `unclear` as failed), in table order. Its evidence
line is what the summary quotes as the reason to hold.
