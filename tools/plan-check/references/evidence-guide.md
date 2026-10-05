# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives.** In an eval package, the plan's cause is the
`## Candidate plan` section's opening claim, usually a sentence
beginning "Diagnosis:" or naming the mechanism directly. The behavior
that cause must explain is in the `## Repro evidence` block: its
numbered steps, its pasted output, and in particular its control runs,
which appear after the main steps under headings like "Control run",
"Second control", or a bullet list of variants. Live, the cause is in
`plan.md`'s diagnosis section and the behavior is in the student's
posted repro comment on the issue thread.

**What good looks like.** The stated cause accounts for the output the
repro evidence actually shows, and survives every control in it. A
control exists to rule something out: when the repro runs the same
input with one factor removed and the failure disappears, that factor
is implicated; when the failure persists, that factor is cleared. A
diagnosis that names a cleared factor is wrong however confidently it
is written. The issue author and the thread may both have named that
cleared factor; the evidence still wins. A grounded diagnosis usually
says so out loud, in the shape of "both controls fit: no background, no
subtraction; width 2, no overshoot."

## Scope

**Where it lives.** The plan's scope statement, usually a line or
paragraph starting "Scope" or "Change, one fix", together with its
"Not in scope" or "defers" line, the file list, and every numbered step
of its approach. The boundary it is measured against is the behavior in
the `## Issue` section.

**What good looks like.** One bounded change: the edit that stops the
reported behavior, the places that edit touches, and a regression test.
A not-in-scope line that names the tempting adjacent work and declines
it is a strong signal, not padding. Deferral is in bounds: a plan that
fixes one of two reported variants and says which one it is leaving and
why is still bounded. A drive-by rewrite looks different: the correct
small fix is present but arrives wrapped in a migration, a redesign, a
new option, a framework, or a test-harness port the issue never
mentioned. Judge what the plan commits to doing, not how much prose it
spends saying it.

## Executability

**Where it lives.** The plan's file list (paths, sometimes with line
numbers) and its approach section, where it names the mechanism it will
use and the order of work.

**What good looks like.** A named file or code site, and one chosen
approach. A stranger reading it could open the right file and make the
first edit without messaging the author. Terse qualifies: "saturating
clamps at both subtraction sites in `src/printer.rs`, plus a width-1
regression test" is executable in one line. What is not executable is a
decision still open at posting time: a layer not yet chosen, a fix
location given as "upstream or vendored, whichever is easier", an
approach given as "profile and optimize", or a file list that is a
direction to search rather than a place to edit.

## Test plan

**Where it lives.** The plan's test plan line or section, read against
the `## Repro evidence` block's steps and the artifacts it produced
(commands, exit codes, pasted output, fixture names).

**What good looks like.** It names something concrete to run and the
specific observable that will read differently after the fix: the repro
command re-run with exit 0 instead of 101, a named fixture now matching
three spellings, a script run against a reference terminal, both fuzz
cases passing plus the control unchanged. The giveaway for a weak test
plan is that its stated outcome would read exactly the same before the
fix as after: "run the full test suite", "should feel fast", "nothing
else should feel broken" all describe a state, not a change. Re-running
the original repro and saying what should now appear is almost always
the strongest available test, because the evidence for the bug and the
evidence for the fix are then the same artifact.

## Honesty

**Where it lives.** The plan's risk, unknown, or open-question lines,
typically at the end; and, after a build, the `## Deviations` section.
Also anywhere the plan makes a confident assertion, which is where
false certainty hides.

**What good looks like.** The things the author has not measured are
written as things not yet measured, with what would be done if they
turn out badly: an unbenchmarked cost flagged with a fallback, an
open question handed to review. False confidence reads as flat
assertion of something the evidence never established, and it is
usually paired with length and polish. A recorded deviation says what
changed from the posted plan and why; a deviation that exists only in
the diff is not recorded at all.

## Comms

**Where it lives.** The `## Candidate plan comment` section, read
against two places: the `## Thread highlights` block (each entry
carries a date, a username, and a role in parentheses, so `OWNER` and
`MEMBER` entries are the maintainer signals), and the `## Repo facts`
block's bug-report template asks and contribution policy, including any
stated AI policy.

**What good looks like.** Thread-aware means the comment shows it read
the thread: it adopts the direction an owner already gave, or builds on
a culprit an owner already isolated, or says plainly why it is going
elsewhere. Where a maintainer has isolated a file and posted a patched
binary to test, a comment proposing an unrelated workaround without
mentioning any of it is not thread-aware, however polite it is. Where
the highlights carry no maintainer direction at all, there is nothing
to engage and the comment cannot fail on this.

On policy, read what the block says the policy asks and who it asks it
of. These differ sharply across packages and the difference decides the
check: some state no AI policy; some reach only pull requests; one
states explicitly that there is no disclosure ask for issue comments;
some ask that comments to maintainers be written by a human in their
own words; and one requires that all AI usage in any form be disclosed,
naming the tool and the extent of the assistance. Treat every package
as AI-assisted work, then ask only what that repo's stated policy asks
of an issue comment. A comment that reads as a person writing satisfies
an own-words ask; only an explicit disclosure satisfies a disclosure
ask; and where the policy asks nothing of issue comments, a comment
with no disclosure is fine.
