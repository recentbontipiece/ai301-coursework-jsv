# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-liveness | Repo facts block: `archived:`, `last push to any branch:`, and `last 5 default-branch commits:` dates | `archived: no` AND `last push to any branch` is within 1 year of the capture date AND at least 3 of the last 5 default-branch commits are dated within 1 year of the capture date | required |
| not-claimed | Repo facts block: the "this issue: assignees: ...; linked PRs: ..." line; also the Comments section for PRs mentioned outside the formal link list | `assignees: none` AND no linked PR has state `open`. Per the Path Review house rule in scope.md, student claim comments in the thread ("I'd like to work on this") do not count; only an explicit assignee or an open linked PR blocks. | required |
| policy-allow | Repo facts block: the `contribution policy` line | The policy does not contain an outright ban on AI-assisted contributions (e.g. "we do not accept AI-generated code or documentation"). A policy that requires disclosure, personal understanding, testing, or human review passes; a policy that merely "discourages" AI passes; a repo with no AI statement passes. | required |
| bounded-scope | Issue body, title, labels, the "Comments (N total)" line, the issue open date vs capture date, and the Comments section | The issue describes one specific, bounded change. FAIL if ANY is true: (a) a tracking/mega-issue — the body contains 10+ bare numbered issue references (e.g. "- #2580") with no specific task, or the title/body says "megaissue"/"tracking"; (b) a codebase-wide umbrella — the body contains "across the codebase", "the codebase", or "PRs are welcome both big and small", or invites searching an entire label; (c) long-abandoned design churn — the comment count is 30+ AND the issue was open more than 1 year before capture AND the comment thread shows back-and-forth design debate or repeated claim/unclaim cycles with no maintainer giving a definitive resolution; (d) undefined scope — the body contains "TBD" or "to be decided" for a key implementation detail (e.g. the core asset, spec, or design decision) that must be resolved before the work can be planned | required |
| well-specified | The Issue body | The issue includes reproduction steps with expected behavior (bugs), concrete file paths or component names with desired outcomes (features/docs), or an acceptance-criteria checklist | preferred |

## Verdict rule

accept if every required check passes; preferred checks never change the verdict, they rank accepted issues; unclear counts as fail.
