# Procedure: how this skill grades a plan package

## Read order

Read the package in this order, every time, and write down what each
step says before moving on. The order matters because three of the
seven checks compare the plan against something that must already be
in hand when the plan is read: grade nothing until step 5.

1. Read the repo-facts block. Note the stated contribution policy and,
   if there is one, what the AI policy asks for and whether it reaches
   issue comments or only pull requests.
2. Read the issue title and body. Write one sentence naming the
   behavior being reported: what the reporter did, and what went wrong.
   That sentence is the boundary `bounded-scope` measures against.
3. Read the thread highlights. Note every explicit direction from a
   maintainer or owner: a culprit named, a file isolated, a patch
   posted, an approach rejected, a test requested. Note also when there
   is none.
4. Read the repro-evidence block. Write down what it pins: the steps,
   the observed output, and above all each control or negative run and
   what that control rules out. This is the fact set `grounded-cause`
   and `decisive-test` are read against, so it is read before the plan,
   not after. Reading the plan first makes a confident diagnosis feel
   supported before you know what the evidence actually shows.
5. Read the candidate plan, then the candidate plan comment. Now begin
   grading.

## Evidence gathering

For each check, pull the fact from exactly this place and record it as
a quote or a short paraphrase with its location. Use
`references/evidence-guide.md` for what good looks like in each family.

1. **grounded-cause**: pull the plan's cause sentence, and pull each
   control run from the repro-evidence block. Record the pair. If a
   control holds a factor constant and the behavior still changes, that
   factor is ruled out; record which factor.
2. **bounded-scope**: list every change the plan commits to, one line
   each, from its scope statement, its named files, and its approach
   steps. Mark each line `needed` or `extra` against the behavior
   sentence from read-order step 2. Record separately anything the plan
   explicitly defers or names not in scope; deferrals are not changes.
3. **executable-start**: record the file paths or code sites the plan
   names, and the one approach it commits to. Record any place the plan
   leaves a choice open instead of making it.
4. **decisive-test**: record what the test plan says to run, and the
   specific outcome it says to look for. Record whether that outcome
   would read differently before and after the fix.
5. **thread-direction**: from the notes taken in read-order step 3,
   record the strongest explicit maintainer direction. Then record
   whether the plan comment mentions it, builds on it, or argues with
   it. If step 3 found no direction, record "no explicit direction in
   thread".
6. **repo-policy**: from the notes taken in read-order step 1, record
   what the policy asks of an issue comment. Then record what the plan
   comment does about it. Treat the package as AI-assisted work.
7. **stated-unknowns**: record each risk or open question the plan
   states, and any claim it asserts that the evidence does not support.

In eval mode every one of these lives in the bundle text. Do not fetch
anything. If a family is genuinely absent from the bundle, record
"absent" rather than inferring it from a neighbouring section.

**In live mode the bundle does not exist, so assemble its five blocks
first, in this order, before running read-order step 1.** Each block is
built once and then read exactly as an eval bundle's block would be.

1. **Repo facts.** From the checkout the student is working in, not the
   network: read the contribution guide (check `CONTRIBUTING.md`,
   `docs/CONTRIBUTING.md`, and `.github/CONTRIBUTING.md`, in that order,
   and record which path actually exists), the issue and pull-request
   templates under `.github/`, and any `AI_POLICY.md` or equivalent.
   Record what the policy asks and of whom: issue comments, pull
   requests, or both. Where the guide says nothing about AI, record
   "no stated AI policy" rather than leaving the family blank.
2. **Issue.** Fetch the issue named by the student's URL: `gh issue view
   <number> --repo <owner>/<repo> --json title,body,state,labels` when
   the `gh` CLI is available, otherwise `GET
   https://api.github.com/repos/<owner>/<repo>/issues/<number>` over the
   public API. Record title, body, state, and labels. Before fetching,
   confirm `<owner>/<repo>` matches the repo named in `scope.md`; if it
   does not, stop without grading.
3. **Thread highlights.** Fetch the comments
   (`gh issue view <number> --comments`, or the `/comments` endpoint).
   For each, record the date, the author, and the author's association
   (`OWNER`, `MEMBER`, `COLLABORATOR`, `CONTRIBUTOR`, `NONE`), because
   the association is what distinguishes maintainer direction from a
   passer-by's suggestion. Record the student's own comments as theirs,
   not as thread direction.
4. **Repro evidence.** Take the student's own posted repro comment on
   that issue, identified by their GitHub username among the comments
   gathered in step 3. On the house issue, where the student has no
   posted repro of their own, take instead the house repro pack exactly
   as the drafts quote it; if the drafts quote no repro evidence at all,
   record the family as "absent" and let the checks that need it grade
   accordingly.
5. **Plan and plan comment.** Read `plan.md` and the draft comment file
   from the student's working directory. Read only those two files plus
   what they quote: the package is what a maintainer will see on the
   thread, so other files in the directory are not evidence, however
   relevant they look.

If a live fetch fails or a source is unreachable, record the family as
"absent" and say so in the summary. Do not substitute the live GitHub
page's current state for a block you could not build, and do not fill a
gap from your own knowledge of the repository.

## Check execution

1. Execute the checks in the order they appear in the rubric's table:
   grounded-cause, bounded-scope, executable-start, decisive-test,
   thread-direction, repo-policy, stated-unknowns.
2. Grade each check only from the evidence recorded for it in the
   previous stage. Do not re-read the package for a check whose
   evidence is already recorded; re-read only when the recorded note is
   too thin to apply the pass condition.
3. Apply the pass condition as written. If the condition is met, the
   grade is `pass`, even when the package feels weak elsewhere. If the
   condition is not met, the grade is `fail`. Note the tension in the
   summary rather than overriding the condition.
4. Grade every check, including checks after one has already failed. A
   full set of grades is the feedback; stopping early hides the rest.
5. When the evidence a check needs is recorded as "absent", grade the
   check `unclear`, not `fail` and not `pass`. The verdict rule decides
   what `unclear` costs.
6. One check never borrows another's evidence. A strong diagnosis does
   not pass `decisive-test`, and a thorough plan does not pass
   `repo-policy`. Where the thread and the repro evidence disagree,
   `grounded-cause` follows the evidence and `thread-direction` follows
   the thread; both grades stand as recorded.
7. For every check, write one evidence line naming the fact or quote
   that decided it. A check with no evidence line is not graded.

## Verdict assembly

1. Collect the grades of the `required` checks: grounded-cause,
   bounded-scope, executable-start, decisive-test, thread-direction,
   repo-policy.
2. Convert each `unclear` among them to `fail`, per the rubric's
   verdict rule.
3. If all six are now `pass`, the verdict is `accept`. If any is
   `fail`, the verdict is `reject`. There is no third verdict and no
   weighing of how many failed.
4. The `preferred` check `stated-unknowns` is reported with its grade
   and evidence line but takes no part in steps 1 through 3.
5. On `reject`, the deciding check is the first `required` check that
   failed, in the table order of step 1. Quote that check's evidence
   line in the readable summary so the reader knows what held the
   package.
6. **Live mode only: report house-rule and voice-guide findings
   alongside the verdict, never inside it.** After the verdict is
   settled, check the package against `scope.md`'s house rules (branch
   naming, no piggybacked plans, branch on the student's own fork) and
   against `voice-guide.md`'s rules. Report each conflict in the
   readable summary, quoting the rule and naming what in the package
   breaks it. These are advisory: no house rule and no voice rule
   changes a check grade or the verdict, because the rubric is the only
   thing that grades. Where a house rule conflicts with the repo's own
   stated convention, report both and say which source each comes from
   rather than picking a winner.
7. Emit the fenced JSON block last, containing every check with its
   grade and evidence line, and the verdict. Nothing follows the JSON
   block. The JSON carries rubric checks only: house-rule and
   voice-guide findings stay in the readable summary above it.
