# Procedure: how this tool grades a PR package

## Read order

Read the package in this order, every time, and write down what each
step says before moving on. Grade nothing until step 6. The order
matters because the load-bearing checks are side-by-sides: the plan
must be in hand before the diff is opened, and the diff must be in
hand before the description is read. Reading the description first
lets a confident "implements the plan exactly" color how the diff
looks.

1. **Repo facts.** Note the pull-request line word for word: each
   template section or checklist item it names, each contributing
   ask (issue-link line, CHANGELOG or whatsnew entry, title
   convention, tests). Note the contribution policy's AI rule, if
   any, and what it asks (disclose, name tool and extent, own words),
   or "no stated AI policy".
2. **Issue and thread.** Write one sentence naming the reported
   behavior. From the thread highlights, note each explicit question,
   direction, or concern from an `OWNER`, `MEMBER`, or
   `COLLABORATOR`, or note "no maintainer direction".
3. **Plan context.** Before any other part of the PR, write down:
   (a) the scope: each change the plan commits to, one line each,
   including tests and docs; (b) the files list; (c) the not-in-scope
   list; (d) every deviation or deferral note; (e) the test plan: what
   to run, on which inputs, which failure modes, the expected-after,
   and the suite command it names.
4. **Diff and commits.** List every file the diff touches. For each
   hunk, write one line saying what it does. Then list the commits.
5. **Description and title, then test evidence.** Read the title and
   description; list each factual claim about the change and each
   section or label present. Then read the test-evidence section; note
   each command shown, its before and after output, and any suite
   result.
6. Begin evidence gathering.

## Evidence gathering

For each check, pull the fact from exactly this place and record it
with its location. Use `references/evidence-guide.md` for what good
looks like.

1. **plan-fidelity**: pair the hunk list (read-order step 4) against
   the plan's scope list (step 3a). Mark each hunk `planned`,
   `disclosed` (named in a deviation note or the description as an
   addition), or `extra`. Then walk the scope list the other way: mark
   each planned change `present`, `deferred-disclosed`, or `missing`.
   Record every `extra` and every `missing`, and any hunk touching a
   file on the not-in-scope list.
2. **description-matches-diff**: pair each description claim (step 5)
   with the hunks that would make it true. Record each claim as `true`
   or `false` of the diff, quoting the claim. A claim about docs,
   tests, or a changelog is checked for the file in the diff.
3. **decisive-evidence**: pair the test plan's runs (step 3e) with the
   commands in the test evidence. For each failure mode or repro the
   plan names, record whether it was re-run on the triggering input,
   and whether before and after outputs are both shown or stated and
   differ. Record if the evidence runs only a control or a different
   path than the one the fix changes.
4. **repo-checks-run**: record each suite or check command shown with
   its result (pass count, passed/failed line, or pasted output), or
   record "none shown".
5. **clean-diff**: scan every added line and every hunk. Record each
   debris item with its file: debug output, commented-out code, dead
   or unused function, unrelated TODO, formatting or import churn,
   re-printed identical lines, unrelated file. Also record commit
   messages that admit debris ("while debugging", "cleanup").
6. **template-asks**: walk the asks recorded in read-order step 1. For
   each, record `met` (quote where), `unmet`, or `not-showable` (CLA,
   vouch flow, CI, code of conduct). For a file ask (CHANGELOG,
   whatsnew), look for that file path in the diff's file list.
7. **ai-disclosure**: from read-order step 1, record what the AI policy
   asks of a PR. Record the description's disclosure sentence, quoted,
   or "no disclosure".
8. **thread-engagement**: from read-order step 2, record the strongest
   maintainer direction and whether the description addresses it.
9. **commit-hygiene**: record the commit messages.

In eval mode all of this is in the bundle text; fetch nothing. If a
section is genuinely absent from the bundle (for example an empty
Commits list), record "absent" for the families that need it rather
than inferring from a neighboring section.

**In live mode, assemble the same five parts first**, then follow the
read order above exactly as for a bundle:

1. *Repo facts*: read `.github/PULL_REQUEST_TEMPLATE.md` and
   `docs/CONTRIBUTING.md` (or `CONTRIBUTING.md`,
   `.github/CONTRIBUTING.md`; record which exists). Record every
   template section and checklist line, and any AI policy. The course
   always asks for an AI-use disclosure in the PR's Notes for
   Reviewers section; record that as a stated ask.
2. *Issue and thread*: confirm the URL's `owner/repo` matches
   `scope.md`, then fetch with `gh issue view <n> --repo <owner>/<repo>
   --comments` (or the GitHub API), recording each comment's author
   and association.
3. *Plan context*: read `plan.md`, including `## Deviations`.
4. *Diff and commits*: run `git diff main...HEAD` and `git log
   --oneline main..HEAD`. Run `git status --short` and record any
   modified tracked file that is not committed.
5. *Description, title, test evidence*: read `pr_draft.md` (first line
   is the title) and `test_evidence.md`.

If a fetch fails, record the family as "absent" and say so. Never fill
a gap from memory of the repository.

## Check execution

1. Execute the checks in the rubric's table order: plan-fidelity,
   description-matches-diff, decisive-evidence, repo-checks-run,
   clean-diff, template-asks, ai-disclosure, thread-engagement,
   commit-hygiene.
2. Grade each check only from the evidence recorded for it. Re-read the
   package for a check only when its recorded note is too thin to
   apply the pass condition.
3. Apply the pass condition as written. Met is `pass`; not met is
   `fail`. If a grade feels wrong, keep it and note the tension in the
   summary.
4. Grade every check, even after one has failed.
5. When the evidence a check needs is recorded as "absent", grade it
   `unclear`.
6. One check never borrows another's evidence. Strong test evidence
   does not excuse an extra hunk; a clean diff does not excuse a
   missing changelog. A drift item is graded under plan-fidelity and
   a false claim about it under description-matches-diff; both stand.
7. Write one evidence line per check, naming the file, hunk, quote, or
   command that decided it.

## Verdict assembly

1. Collect the grades of the seven `required` checks: plan-fidelity,
   description-matches-diff, decisive-evidence, repo-checks-run,
   clean-diff, template-asks, ai-disclosure.
2. Convert each `unclear` among them to `fail`.
3. If all seven are `pass`, the verdict is `accept`. If any is `fail`,
   the verdict is `reject`.
4. `thread-engagement` and `commit-hygiene` are reported with grade and
   evidence line and take no part in steps 1-3.
5. On `reject`, the deciding check is the first failed `required` check
   in table order. Quote its evidence line in the summary.
6. Live mode only, after the verdict is fixed: report `scope.md` house
   rule findings (branch name `<type>/<issue-number>-<slug>`, one PR
   per issue, PR from own fork) and each `voice-guide.md` rule the
   title or description breaks, quoting the rule and the line. Report
   any uncommitted tracked change found in live step 4. None of these
   change a grade or the verdict.
7. Emit the fenced JSON block last, with every rubric check, its grade
   and evidence line, and the verdict. Nothing follows it.
