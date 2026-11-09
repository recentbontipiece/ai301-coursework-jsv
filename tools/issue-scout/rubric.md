# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-liveness | Repo facts block: `archived:`, `last push to any branch:`, and `last 5 default-branch commits:` dates | `archived: no` AND `last push to any branch` is within 1 year of the capture date AND at least 3 of the last 5 default-branch commits are dated within 1 year of the capture date | required |
| not-claimed | Repo facts block: the "this issue: assignees: ...; linked PRs: ..." line; the Comments section (each comment's author, date, and text) for claim comments, maintainer replies, and PRs mentioned outside the formal link list | ALL of: (a) `assignees: none`; (b) no linked PR has state `open`; (c) no PR mentioned in the Comments section is open; (d) no live claim comment. A claim comment is a non-maintainer saying they will take the issue or have a fix ("I'd like to work on this", "I'll take this", "can I work on this", "I have a fix ready"). It is live if it was posted within 30 days of the capture date (live mode: today), UNLESS a later comment shows the claim ended: the claimant withdraws it, or a maintainer (OWNER/MEMBER/COLLABORATOR) declines it, releases it, or says it is open to others. A claim older than 30 days with no PR and no follow-up from the claimant counts as abandoned and does not block. | required |
| policy-allow | Repo facts block: the `contribution policy` line | The policy does not contain an outright ban on AI-assisted contributions (e.g. "we do not accept AI-generated code or documentation"). A policy that requires disclosure, personal understanding, testing, or human review passes; a policy that merely "discourages" AI passes; a repo with no AI statement passes. | required |
| scope-feasible | Issue body: the work it asks for (plus title and labels) | The issue describes one identifiable piece of work someone could start: a behavior, a gap, or a task you can point at. It fails only when there is no single piece of work in it: a codebase-wide checklist, an "improve X everywhere", or a container issue tracking many sub-tasks. My unit-1 signals for those still apply: the body lists 10+ bare issue references (e.g. "- #2580") with no specific task, the title/body says "megaissue"/"tracking", or the body says "across the codebase" / "PRs are welcome both big and small" / invites searching an entire label. An open design edge, a "TBD", an "etc.", a long thread, or several files involved never fails this check; that raises the difficulty note instead. | required |
| well-specified | The Issue body | The issue includes reproduction steps with expected behavior (bugs), concrete file paths or component names with desired outcomes (features/docs), or an acceptance-criteria checklist | preferred |

## Difficulty note

With each accepted candidate, report a one-line difficulty note in the
read-out summary:

- **straightforward**: the issue names where the change lives and the
  change is bounded.
- **moderate**: the location is mine to find, or several files are
  involved.
- **ambitious**: an open design edge (a "TBD", an undecided choice, a
  long unresolved design thread), or an unfamiliar subsystem. Name the
  edge, because claiming this issue means raising it with the
  maintainer early.

The note is information for the chooser, never a verdict input: it does
not appear in the fenced JSON block, it never changes accept or reject,
and in eval mode it is skipped entirely.

## Verdict rule

accept if every required check passes; preferred checks never change the verdict, they rank accepted issues; unclear counts as fail.
