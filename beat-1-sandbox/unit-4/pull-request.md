# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

TODO: paste the PR link after opening it (https://github.com/codepath/pathreview-ai301-fa26-s3/pull/<number>)

**Branch**

`docs/11-hybrid-scoring-formula`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, 20 scored packages: **19/20**. Categories: clear-accept 6/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. The bar is 18/20 with every category matched, so this run passes. It is the run saved in `eval-run.txt` ("agreement: 19/20 scored items (bar: 18/20: PASS)").

That was my only eval run. I did not do partial `--only` runs, because the single miss (pkg-02) was something I chose to keep rather than fix (see Trade-offs).

**Package analysis**

**pkg-02** (Textualize/rich#4208, clear-accept). My rubric said **reject**; the gold label is **accept**.

Every check passed except `clean-diff`, whose evidence line was: "save_html and save_svg hunks remove and re-add identical lines (`html = self.export_html(` / `svg = self.export_svg(`, `theme=theme,`) when only `clear=clear` -> `clear=False` needed to change — re-printed identical lines."

The rubric read it that way because the diff really does show `-        html = self.export_html(` / `-            theme=theme,` removed and then re-added unchanged, around the one argument that changed. My `clean-diff` pass condition lists "re-printed identical lines in hunks the fix does not need" as debris, and the grader treated those lines as churn. I think the gold label is right. The repeated lines sit inside the exact statement the fix edits, so a reviewer reads them as part of the change, not as noise that buries it. That is different from pkg-18, where whole blocks of identical lines are re-printed in code the fix never touches. My wording ("hunks the fix does not need") was meant to draw that line, but it wasn't sharp enough to stop a grader from flagging repeated lines inside a hunk the fix *does* need.

**Check rationale**

> | clean-diff | Every hunk of the diff, and the commit list. | The diff contains only the change and its tests or docs. Fails on any of: debug output left in (print, eprintln, console.log, dbg, "DEBUG"); commented-out code or a commented-out earlier attempt; a dead or unused function or variable added (including behind allow(dead_code) or an "unused" name); a stray TODO or FIXME added to unrelated code; formatting, re-indent, import-reorder, or re-printed identical lines in hunks the fix does not need; edits to files unrelated to the fix. Comments that explain the fix are not debris. | required |

It reads as a list of concrete debris tells, not as "the diff is clean", because my Unit 3 lesson was that adjective-shaped checks make graders disagree. The unreviewable packages each fail on something different: pkg-12 on a commented-out first attempt and a dead `_unused_buffer_probe` fn, pkg-15 on a `DEBUG` `eprintln` and a dead fn behind `allow(dead_code)`, and pkg-18 on import churn and re-printed identical lines. Naming each tell lets a grader point at a line instead of judging an impression. Two things are carved out deliberately. "Comments that explain the fix are not debris" is there because good fixes, like pkg-11's, add an explanatory comment. "Hunks the fix does not need" is there so the formatting rule doesn't catch the fix itself. The check is `required` because the gold labels hold PRs whose fix is right but whose diff is buried (pkg-12, pkg-15, pkg-18).

**Trade-offs**

The re-printed-identical-lines clause in `clean-diff` cost me pkg-02, a clear accept. I kept it anyway, because it is the only clause that catches pkg-18, whose gold note calls the problem "two blocks of identical re-printed lines (pure churn)". Loosening it to ignore all repeated lines would risk flipping pkg-18 to accept. Unreviewable has only 3 packages, so I'd rather keep a check that catches all three and lose one clear accept (I still have 6/7 there) than make the category depend on a borderline call. If I loosen it later, the fix is to exempt unchanged lines inside the statement the fix edits. I would re-run `--only pkg-02,pkg-18,pkg-12,pkg-15`, with pkg-18 as the canary, before a confirming full run. I did not make that change for this submission, so the files uploaded to `tools/pr-precheck/` are the exact files fingerprinted in `eval-run.txt`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
