# Plan: document the hybrid retrieval scoring in `docs/ARCHITECTURE.md`

- Issue: [codepath/pathreview-ai301-fa26-s3#11](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/11)
- Branch: `docs/11-hybrid-scoring-formula`
- Fork: `recentbontipiece/pathreview-ai301-fa26-s3`

## Reproduction evidence this plan builds on

Quoted from my Unit 2 reproduction comment on the issue
([permalink](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/11#issuecomment-5863902704)):

> Environment: macOS 26.2 (arm64), Git 2.40.0. I inspected the PathReview fork
> `recentbontipiece/pathreview-ai301-fa26-s3` at commit
> `2f4e82f52efbcfcc57d65b3fa5348672163ca088`, the same revision as upstream
> `codepath/pathreview-ai301-fa26-s3` `main`. This is a documentation check, so
> no application runtime or service is needed.
>
> Step 2 — read the architecture section, `sed -n '59,61p' docs/ARCHITECTURE.md`:
>
> ```text
> ### RAG System (`rag/`)
> Hybrid retrieval (vector similarity + BM25 keyword) fetches relevant context from the user's ingested documents. The generator uses prompt templates to produce structured, evidence-based feedback. The evaluator scores retrieval relevance and generation faithfulness.
> ```
>
> Step 3 — read the implementation, `sed -n '16,20p;72,115p' rag/retriever/hybrid.py`:
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
> Observed: the source implements separate max normalization and a weighted sum
> with defaults `0.7` and `0.3`; the architecture page only names vector
> similarity and BM25. The fork and upstream were the same commit, so the fork
> check did not introduce a source difference. This confirms the documentation
> gap in issue #11; it is not a runtime failure.

## Diagnosis

The gap is in the documentation, not the code. `docs/ARCHITECTURE.md`'s "RAG
System" section describes hybrid retrieval only as "vector similarity + BM25
keyword", and stops there. Everything a reader would need to predict a score is
in `rag/retriever/hybrid.py` and nowhere in the docs:

- the two weights are constructor defaults, `vector_weight=0.7` and
  `keyword_weight=0.3` (lines 18–19);
- each score family is normalized independently against the maximum **within
  that batch of results**, not against a fixed scale — `vector_scores_max` and
  `keyword_scores_max` (lines 73–74), each guarded so a zero maximum yields a
  score of 0;
- the blend is a plain weighted sum of those two normalized values,
  `blended_score = self.vector_weight * vector_score + self.keyword_weight * keyword_score`;
- a chunk found by only one of the two searches keeps `0.0` for the other side
  rather than being dropped.

The repro's step-2 and step-3 outputs are the whole basis for this: the
documented text and the implementing code were read at the same commit
(`2f4e82f`, confirmed equal to upstream `main`), so the difference between them
is a documentation omission and not drift between fork and upstream. The issue
asks for exactly this explanation plus an example, so the fix is a docs edit.

## Scope

**In scope:** the "RAG System (`rag/`)" section of `docs/ARCHITECTURE.md`.
I will add the scoring formula, the default weights, the per-batch
normalization behaviour, and one short worked example, and leave the rest of
that section as it reads today.

**Not in scope, deliberately:**

- Changing any behaviour in `rag/retriever/hybrid.py`. The defaults and the
  normalization strategy are the documented subject here, not the target.
- The ignored `all_chunks` fetch at `rag/retriever/hybrid.py:65`, which carries
  an in-code note marking it as the seeded defect for issue #6. It sits three
  lines above the code I am documenting and it is someone else's issue.
- `min_score` / `max_chunks` filtering, the generator, and the evaluator — the
  rest of the RAG section's sentences stay as they are.
- README.md, SETUP.md, and any other doc page.

## Files

- `docs/ARCHITECTURE.md` — the "RAG System (`rag/`)" section only.

No code files, no tests. This is a documentation-only change, so there is no
`@pytest.mark.xfail` marker to remove (`docs/CONTRIBUTING.md`'s seeded-bug rule
applies to code fixes; issue #11 has no failing test attached).

## Approach

1. Re-read `rag/retriever/hybrid.py` lines 16–20 and 72–115 at the commit I
   branch from, and confirm the defaults and the normalization are still what
   the repro recorded. If they have moved since `2f4e82f`, the documented values
   follow the code, and I say so in the PR.
2. In `docs/ARCHITECTURE.md`, expand the "RAG System (`rag/`)" section: keep the
   existing opening sentence, then add the scoring explanation — independent
   per-batch max normalization of each score family, the weighted sum, the
   `0.7` / `0.3` defaults and that they are constructor arguments, and the
   single-source case scoring `0.0` on the missing side.
3. Add the worked example from my repro comment, labelled as illustrative: a
   chunk with raw vector score `0.8` against a batch maximum of `1.0` and BM25
   `6` against a batch maximum of `12` normalizes to `0.8` and `0.5`, blending
   to `0.7 * 0.8 + 0.3 * 0.5 = 0.71`. State plainly that these numbers
   illustrate the formula and are not output from a retrieval run.
4. Confirm a docs-only diff leaves CI green, per `docs/CONTRIBUTING.md`'s "CI
   must be green" requirement. Run the read-only checks —
   `make lint && make typecheck && make test-unit` — rather than `make check`.
   `check` depends on `format`, which is `black .` across the whole repo and
   **writes files**; on a docs-only branch that could reformat Python the issue
   never touched, including files carrying seeded defects, which
   `docs/CONTRIBUTING.md` explicitly asks contributors not to bulk-rewrite.
   No Python changes here, so there is nothing for `black` to format anyway.

## Test plan

This is a documentation change, so the observable is the documentation itself.
Re-running my Unit 2 step 2 against the built change is the test.

Before (on `main`, and recorded in the repro comment above), the section names
no formula:

```sh
sed -n '/^### RAG System/,/^### Safety Layer/p' docs/ARCHITECTURE.md
grep -nE '0\.7|0\.3|normaliz|weighted sum' docs/ARCHITECTURE.md
```

Expected before: the first command prints the three-line section quoted in the
repro; the second prints **no matching lines**.

Expected after the change: the first command prints the section including the
weighted-sum formula and the worked example; the second prints matching lines
naming `0.7`, `0.3`, and normalization. The grep going from zero matches to
matches is the observable that differs.

Plus, as a regression guard on the repo's own bar:

```sh
make lint && make typecheck && make test-unit
git status --short
```

Expected after: the same result as before the change (green), and `git status`
showing `docs/ARCHITECTURE.md` as the only modified tracked file. These three
targets only read; `make check` is avoided deliberately because it depends on
`make format` (`black .`), which writes across the repo.

## Risks and unknowns

- **The example's numbers are illustrative, not measured.** I have not run a
  retrieval against a seeded corpus to produce real scores. I label them as
  illustrative in the doc for that reason. If a maintainer would rather see
  numbers from an actual run, that is a larger change than this issue asks for
  and I would do it as a follow-up.
- **"Per-batch" is my reading of the normalization, and I want it checked.**
  `vector_scores_max` is computed from the results of the current query, so the
  same chunk can normalize differently across queries. The code supports that
  reading; I have not confirmed it is the intended design rather than an
  artifact, so I describe the behaviour and do not call it a design decision.
- **Unverified whether the section wants the `min_score=0.3` default too.** The
  issue asks for the formula, weights, and an example. I am leaving the
  filtering threshold out to keep the edit bounded; if review wants it, it is
  one more sentence.
- **Another student may plan the same issue.** Per the Path Review house rules,
  that does not block me, and my plan is built from my own reproduction.

## Deviations

The documentation change itself held: the built edit is the one the plan
describes. The "RAG System (`rag/`)" section of `docs/ARCHITECTURE.md` now
carries the weighted-sum formula, the `0.7` / `0.3` constructor defaults, the
per-query normalization behaviour, the single-source `0.0` case, and the
illustrative `0.7 * 0.8 + 0.3 * 0.5 = 0.71` example. Nothing outside that
section was touched: `git diff --stat` reports `docs/ARCHITECTURE.md | 10
++++++++++`, one file changed, and `git status --short` shows no other
modified tracked file. The scope I declined — `rag/retriever/hybrid.py`, the
issue #6 `all_chunks` line, `min_score`, other docs — stayed declined.

Two things differ from the plan as written:

1. **I added a sentence the plan did not promise.** Approach step 2 listed the
   formula, the defaults, the normalization and the single-source case. While
   writing it I also documented that a batch maximum of zero yields a score of
   `0` rather than a division error, because `hybrid.py` guards both divisions
   with `if vector_scores_max > 0` / `if keyword_scores_max > 0` and a reader
   working from the formula alone would predict a crash there. It is one clause
   inside the same paragraph and still inside the section I scoped, but it is
   more than I said I would write, so I am recording it rather than letting it
   pass as part of the plan.

2. **The CI guard has not run yet.** Approach step 4 and the test plan commit to
   `make lint && make typecheck && make test-unit`. Those targets need `.venv`,
   which this clone does not have — `make setup` has never been run here — so
   the checks are still outstanding and I ran the documentation observables
   only. This is a documentation-only diff that touches no Python, so I do not
   expect a change in result, but I have not demonstrated that and I am not
   claiming it. I will run `make setup` followed by the three targets before
   opening the pull request in Unit 4, and report the output there.

The per-batch normalization reading I flagged as an open question in the plan
is unchanged: I documented it as observed behaviour rather than as a design
decision, and it is still waiting on a maintainer to confirm which it is.
