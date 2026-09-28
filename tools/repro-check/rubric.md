# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| issue-match | Issue description and trigger, compared with the repro report's setup, steps, and output excerpt or other artifact | Pass if the attempt exercises the issue's stated trigger and the artifact shows the same observable behavior, or the report clearly says it could not be reproduced after a relevant attempt. Fail if it demonstrates a different error/symptom, skips or changes the trigger without explaining the difference, or calls an adjacent result a reproduction. | required |
| rerunnable | Repro report's environment record, commands/configuration, steps, and any referenced files or external resources | Pass if a stranger can establish the issue-relevant starting state and follow the trigger. Inputs already given in a self-contained public issue may be referenced rather than copied; omitted details fail only when they prevent reaching the trigger or materially affect its result. Private prerequisites or unexplained missing setup fail. For cannot-reproduce, the attempted setup and trigger must still be repeatable. | required |
| environment-fidelity | Repro report's OS, app/runtime/tool versions, configuration, and comparison with the issue's stated environment | Pass if details that can affect the result are recorded and any version/platform/configuration deviation from the issue is acknowledged; fail if a material mismatch is hidden or its output is presented as an exact reproduction. | required |
| evidence-support | Repro report's output, logs, screenshots, measurements, control run, and expected-versus-actual description | Pass if the artifact makes the reported behavior observable and the prose describes what it actually shows. A cannot-reproduce passes when the attempt and its actual result are evidenced and the report does not claim the issue was confirmed. Fail for assertion without an artifact, an artifact that does not show the claimed behavior, or an unsupported root-cause claim presented as fact. | required |
| repo-comms | Claim comment and repro comment, read with the repo-facts block's issue-template asks and contribution/AI policy | Pass if the claim identifies this issue and a concrete next investigation step without promising a fix or date; the report includes template-requested information when it is needed to identify or assess the reproduction; and any AI disclosure explicitly required by policy is present. Omission of ancillary diagnostics does not fail an otherwise assessable package. No disclosure is required when policy is silent or has no disclosure requirement. Fail for generic claim boilerplate, unsupported certainty or timeline, missing material template information, or missing required AI disclosure. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept if every required check passes. Reject if any required check fails or is
unclear. Preferred checks, if added later, never change the verdict. In claim-only
live mode, checks that require the repro report are not yet applicable and are
excluded from the verdict, as specified in `SKILL.md`.
