# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

recentbontipiece

---

## Posted upstream

**Claim comment**

[Pending: after posting, replace this note with the comment's permalink. The accepted
live-checked text to post is:

Hi, I’d like to investigate this documentation issue. I’ll verify the hybrid retriever’s
default weights and score normalization against `docs/ARCHITECTURE.md`, then report the
exact behavior with a small worked example. I’ll share what I can confirm before proposing
any documentation change.]

**Reproduction comment**

[Pending: complete and verify the reproduction, run the full-package live check, and post
the report. Then add the comment permalink and exact posted text here.]

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Initial full run: `agreement: 18/20 scored items  (bar: 18/20: PASS)`; `pkg-05`
	and `pkg-12` were false rejects.
2. Targeted rerun: `agreement: 2/2 scored items`; both packages matched gold `accept`.
3. Canary rerun: `agreement: 3/3 scored items`; `pkg-20` matched gold `reject`.
4. Confirming full run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`;
	all five categories matched.

**Package analysis**

`pkg-05`: the initial rubric decided `reject`, while the gold label was `accept`. The
package's observed parser output was `Expecting value: line 1 column 1 (char 0)` after
running `conda env update --quiet --json -f env.yml 2>/dev/null`. The initial evaluation
recorded `pkg-05  accept  reject  NO  failed: rerunnable, repo-comms`. I treated the
unquoted `env.yml` contents and ancillary `conda info`/`conda list` diagnostics as
blockers, although the public issue supplies the command and the report shows the stdout
warning breaking JSON. The revision narrowed those checks to omissions that prevent
reaching or assessing the behavior; the final run records `pkg-05  accept  accept  yes`.

**Check rationale**

`| repo-comms | Claim comment and repro comment, read with the repo-facts block's issue-template asks and contribution/AI policy | Pass if the claim identifies this issue and a concrete next investigation step without promising a fix or date; the report includes template-requested information when it is needed to identify or assess the reproduction; and any AI disclosure explicitly required by policy is present. Omission of ancillary diagnostics does not fail an otherwise assessable package. No disclosure is required when policy is silent or has no disclosure requirement. Fail for generic claim boilerplate, unsupported certainty or timeline, missing material template information, or missing required AI disclosure. | required |`

I narrowed the earlier rule that required every listed template field. The eval showed
ancillary diagnostics can be absent while behavior remains directly assessable; material
report information and explicit disclosure requirements still gate the verdict.

**Trade-offs**

The revised threshold accepts `pkg-05` and `pkg-12`, which the initial rubric rejected for
missing ancillary details despite clear artifacts. This creates some risk of accepting a
report that omits useful diagnostics, but only when the omission does not block reproducing
or assessing the behavior. I re-ran `pkg-20` as the disclosure-category canary after
loosening the checks; it remained rejected because the required AI-use disclosure is
missing. The final run records `pkg-20  reject  reject  yes` and `disclosure 1/1`; the
wrong-target and unfollowable categories also remained fully matched.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
