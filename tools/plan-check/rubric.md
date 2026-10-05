# Rubric: is this plan ready to post and build from?

A plan package is ready when the cause it names survives the evidence,
the change it commits to is one bounded piece of work, a stranger
could start it, the test would actually show the fix landed, and the
comment answers the room it is being posted into. Any one of those
missing holds the package.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| grounded-cause | The plan's stated cause, read against the repro-evidence block's steps, outputs, and every control or negative run it reports. | The stated cause explains the behavior the repro evidence actually shows, and no control run in that block rules it out. If the evidence isolates one factor and the plan blames a different one, this fails. A cause the issue or the thread asserts confidently still fails here when the package's own evidence contradicts it: the evidence outranks the thread. | required |
| bounded-scope | Everything the plan commits to doing: its scope statement, the files or areas named, and each step of its approach, read against the behavior the issue reports. | Every change the plan commits to is needed either to fix the reported behavior or to make that fix safe to land. Work the plan explicitly defers, or names as not in scope, does not count against it. Refactors, migrations, redesigns, new options, or framework work the issue did not ask for do count against it, even when the core fix inside them is correct. | required |
| executable-start | The files or code sites the plan names, and the approach it commits to. | The plan names the specific file or code site it will change and commits to one approach; where it raises alternatives, it picks one. Someone who has never seen this code could begin the first edit without asking the author a question. Brevity is not a fail and missing headings are not a fail; an unmade decision is ("profile and see", "gocui or tcell, not sure", "upstream or vendored, whichever is easier"). | required |
| decisive-test | The plan's test plan, read against the repro evidence's steps and artifacts. | The test names a concrete thing to run and the specific observable that will differ once the fix lands: an exit code, a named output, a named test case, a rendered result. "Run the full test suite", "should feel fast", and "nothing else should feel broken" fail, because they would read the same before and after the fix. | required |
| thread-direction | The thread-highlights block, specifically any explicit direction, isolation, patch, or request from a maintainer or owner, read against the candidate plan comment. | Where the highlights carry explicit maintainer direction, the comment engages it: adopting it, building on it, or saying plainly why it is going another way. Proposing an unrelated workaround while the owner has already isolated the culprit or posted a patch to test fails. Where the highlights carry no such direction, this check passes. | required |
| repo-policy | The repo-facts block's stated contribution and AI policy, read against the candidate plan comment. Treat every package as AI-assisted work. | The comment does what the stated policy asks of an issue comment. If the policy requires AI use to be disclosed, the comment discloses it. If the policy asks that comments to maintainers be in the contributor's own words, the comment reads as a person writing. If the repo-facts block states no AI policy, or states a policy that reaches only pull requests and sets no disclosure ask for issue comments, this check passes. | required |
| stated-unknowns | The plan's risks, unknowns, and open questions. | Anything the plan has not actually verified is written as a risk or an open question rather than asserted as settled fact. | preferred |

## Verdict rule

The verdict is `accept` if and only if every `required` check grades
`pass`. A single `required` check grading `fail` makes the verdict
`reject`.

`preferred` checks never change the verdict. Grade and report
`stated-unknowns` honestly, but a fail there is feedback, not a hold.

An `unclear` grade on a `required` check counts as a `fail`: a plan
whose readiness cannot be verified from the package is not a plan that
is ready to build from. An `unclear` on a `preferred` check is
reported and otherwise ignored.

When the verdict is `reject`, quote in the output the evidence line of
the first `required` check that failed, in table order; that is the
deciding check.
