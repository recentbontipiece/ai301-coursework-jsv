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

[Posted comment](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/11#issuecomment-5863822785)

Hi, I’d like to investigate this documentation issue. I’ll verify the hybrid retriever’s
default weights and score normalization against `docs/ARCHITECTURE.md`, then report the
exact behavior with a small worked example. I’ll share what I can confirm before proposing
any documentation change.

**Reproduction comment**

Status: **Draft, not posted.** Permalink: pending. The report below passed the live check;
post it, then replace this status with the comment permalink and exact posted text.

> Environment: macOS 26.2 (arm64), Git 2.40.0. I inspected the PathReview fork
> `recentbontipiece/pathreview-ai301-fa26-s3` at commit
> `2f4e82f52efbcfcc57d65b3fa5348672163ca088`, the same revision as upstream
> `codepath/pathreview-ai301-fa26-s3` `main`. This is a documentation check, so no
> application runtime or service is needed.
>
> Steps:
>
> 1. Clone the fork, pin the revision, and verify it matches upstream:
>
> ```sh
> git clone https://github.com/recentbontipiece/pathreview-ai301-fa26-s3.git
> cd pathreview-ai301-fa26-s3
> git checkout 2f4e82f52efbcfcc57d65b3fa5348672163ca088
> git remote add upstream https://github.com/codepath/pathreview-ai301-fa26-s3.git
> git fetch upstream main
> git rev-parse HEAD upstream/main
> ```
>
> Both revisions printed as `2f4e82f52efbcfcc57d65b3fa5348672163ca088`.
>
> 2. Read the architecture section:
>
> ```sh
> sed -n '59,61p' docs/ARCHITECTURE.md
> ```
>
> Observed output:
>
> ```text
> ### RAG System (`rag/`)
> Hybrid retrieval (vector similarity + BM25 keyword) fetches relevant context from the user's ingested documents. The generator uses prompt templates to produce structured, evidence-based feedback. The evaluator scores retrieval relevance and generation faithfulness.
> ```
>
> 3. Read the implementation:
>
> ```sh
> sed -n '16,20p;72,115p' rag/retriever/hybrid.py
> ```
>
> Relevant output excerpt:
>
> ```python
> vector_weight: float = 0.7,
> keyword_weight: float = 0.3,
> vector_scores_max = max([r["score"] for r in vector_results], default=1.0)
> keyword_scores_max = max([r.get("bm25_score", 0) for r in keyword_results], default=1.0)
> vector_score = vector_map[chunk_id]["score"] / vector_scores_max
> keyword_score = keyword_map[chunk_id].get("bm25_score", 0) / keyword_scores_max
> blended_score = self.vector_weight * vector_score + self.keyword_weight * keyword_score
> ```
>
> For an illustrative example, if a chunk's raw vector score is `0.8` out of a batch maximum
> of `1.0` and its BM25 score is `6` out of a batch maximum of `12`, its normalized scores
> are `0.8` and `0.5`; the default blend is `0.7 * 0.8 + 0.3 * 0.5 = 0.71`. These values
> illustrate the formula; they are not claimed as output from a retrieval run.
>
> Expected: as issue #11 requests, `docs/ARCHITECTURE.md` explains the scoring logic with an
> example, including normalization and default weights.
>
> Observed: the source implements separate max normalization and a weighted sum with
> defaults `0.7` and `0.3`; the architecture page only names vector similarity and BM25.
> The fork and upstream were the same commit, so the fork check did not introduce a source
> difference. This confirms the documentation gap in issue #11; it is not a runtime failure.

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
