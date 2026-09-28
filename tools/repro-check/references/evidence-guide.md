# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

The environment record is in the repro report, usually its opening lines. Check
the operating system and versions of the affected app, runtime, browser, driver,
or other relevant dependency, plus issue-specific settings such as locale,
configuration, and build/source version. It is sufficient when a reader can
identify the conditions that may change the result and see any difference from
the issue reporter's environment acknowledged rather than silently treated as
equivalent.

## Steps

Steps live in the repro report after the environment. Follow them from a clean
or explicitly described starting state through the issue's actual trigger;
look for exact commands, inputs, settings, and required files. A stranger should
not need private data, an unstated local setup, or guesswork to reach the test
condition. When the issue itself contains a complete public input or command,
the report may refer to it and say it was run verbatim instead of duplicating
it. Missing package or configuration details matter when they can change the
trigger or result, not merely because they are absent. A cannot-reproduce report
still needs to make the attempted setup and trigger repeatable.

## Behavior shown

Artifacts are the quoted command output, logs, screenshots, measurements, or
control-run results in the repro report. Read them against the issue's stated
trigger and expected behavior: they should expose the same observable symptom,
not merely show that the program ran or produced a different error. A control is
useful when it isolates a condition named by the issue. For a cannot-reproduce,
the artifacts should show what the attempt actually did and the report must not
describe it as a successful reproduction.

## Honesty

Compare every factual claim in the claim and repro comments with the issue and
the report's artifacts. The report should distinguish observed behavior from
expected behavior and hypotheses, state when the issue did not reproduce, and
limit conclusions to the tested environment. A root-cause theory is a theory
unless the cited evidence establishes it; one machine or run does not support
claims about all users or every environment.

## Comms

Read the claim comment in the package against the issue title/body and thread;
read the repro comment against the repo-facts block's bug-report template and
contribution policy. Specific communication names the issue's actual behavior
and a plausible next investigation step, without promising a fix or deadline.
Include template-requested fields that materially identify or assess the
reproduction; ancillary diagnostics may be omitted when the package remains
assessable from its other evidence. Follow the exact AI-use policy: disclose
when it requires disclosure, but do not invent a disclosure requirement where
the policy has none. Generic praise, "same here," unsupported certainty, and
unrequested priority demands do not substitute for issue-specific work.
