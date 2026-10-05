# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

recentbontipiece

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/11#issuecomment-6004282206

I'd like to investigate and document the hybrid retrieval scoring formula that's missing from `docs/ARCHITECTURE.md`.

**What I found.** At `2f4e82f` (upstream `main`), the "RAG System (`rag/`)" section of `docs/ARCHITECTURE.md` names only "vector similarity + BM25 keyword" and stops there. The implementation in `rag/retriever/hybrid.py` has more detail: `HybridRetriever.__init__` takes `vector_weight=0.7` and `keyword_weight=0.3` as constructor defaults (lines 18–19), and `retrieve` blends them with `blended_score = vector_weight * vector_score + keyword_weight * keyword_score` (line 101), where each side is normalized independently against the batch maximum before blending (lines 73–74, 86, 96). A zero batch maximum yields `0` rather than a division error, and a chunk returned by only one search keeps `0.0` on the other side.

This is a documentation gap, not a runtime bug — the fork and upstream are the same commit, so there is no source difference to reproduce.

**My plan.** Add a "Hybrid scoring" paragraph to the "RAG System (`rag/`)" section of `docs/ARCHITECTURE.md` only — the weighted-sum formula, the `0.7`/`0.3` defaults and that they are constructor arguments, the per-batch normalization of each score family, the zero-maximum guard, the single-source `0.0` case, and one short illustrative example. I will not touch `rag/retriever/hybrid.py`, the seeded `all_chunks` defect from issue #6, `min_score` filtering, the generator, the evaluator, or any other doc page.

**Test.** Re-run my reproduction reads against the built change: `grep -nE '0\.7|0\.3|normaliz|weighted sum' docs/ARCHITECTURE.md` should go from zero matches to matches at the lines where the formula now lives. I will also run `make lint && make typecheck && make test-unit` before opening the pull request in Unit 4 — those targets need `.venv`, which this clone does not yet have, so CI greenness is still outstanding and I will not claim it yet.

**Risks.** The worked example is illustrative, not from a real retrieval run. The "per-batch" normalization reading is my interpretation of the code and not confirmed as intended design. Another student may plan the same issue — that does not block mine per the Path Review house rules, and my plan is built from my own reproduction.

I used an AI assistant to help structure this plan; I read the code and docs at the same commit myself and am committing to the specific docs-only edit described above.

---

## Your branch

**Branch**

`docs/11-hybrid-scoring-formula`

**Evidence**

My Unit 2 reproduction read the documented behaviour and the implementing code
at the same commit and showed the documentation gap. Re-running the same reads
against the built change is the test. Both runs are on
`recentbontipiece/pathreview-ai301-fa26-s3`; the before is at `2f4e82f`
(upstream `main`), the after is commit `0d3336f` on branch
`docs/11-hybrid-scoring-formula` (10 insertions to `docs/ARCHITECTURE.md`,
pushed to the fork).

**Before**

```
$ git log --oneline -1
2f4e82f chore: track five more manifest entries against the tracker

$ sed -n '/^### RAG System/,/^### Safety Layer/p' docs/ARCHITECTURE.md
### RAG System (`rag/`)
Hybrid retrieval (vector similarity + BM25 keyword) fetches relevant context from the user's ingested documents. The generator uses prompt templates to produce structured, evidence-based feedback. The evaluator scores retrieval relevance and generation faithfulness.

### Safety Layer (`safety/`)

$ grep -nE '0\.7|0\.3|normaliz|weighted sum' docs/ARCHITECTURE.md
$ echo $?
1
```

The grep printed no lines and exited `1`: at this commit the architecture page
names neither the weights, nor normalization, nor the blend.

**After**

```
$ sed -n '/^### RAG System/,/^### Safety Layer/p' docs/ARCHITECTURE.md
### RAG System (`rag/`)
Hybrid retrieval (vector similarity + BM25 keyword) fetches relevant context from the user's ingested documents. The generator uses prompt templates to produce structured, evidence-based feedback. The evaluator scores retrieval relevance and generation faithfulness.

**Hybrid scoring.** `HybridRetriever` combines the two searches into a single ranking with a weighted sum of normalized scores:

```
blended_score = vector_weight * vector_score + keyword_weight * keyword_score
```

`vector_weight` and `keyword_weight` are constructor arguments on `HybridRetriever` and default to `0.7` and `0.3`. Each side is normalized independently before blending: `vector_score` is the chunk's raw similarity divided by the highest similarity in the current result set, and `keyword_score` is its BM25 score divided by the highest BM25 score in the current result set. Normalization is therefore per query rather than against a fixed scale, so the same chunk can normalize differently depending on what else the query returned; a maximum of zero yields a score of `0` rather than a division error. A chunk returned by only one of the two searches keeps `0.0` for the side that did not return it instead of being dropped, so it can still rank if its one score is strong enough.

For example — illustrating the formula, not values taken from a retrieval run — a chunk whose raw vector score is `0.8` where the batch maximum is `1.0`, and whose BM25 score is `6` where the batch maximum is `12`, normalizes to `0.8` and `0.5`. At the default weights that blends to `0.7 * 0.8 + 0.3 * 0.5 = 0.71`.

### Safety Layer (`safety/`)

$ grep -nE '0\.7|0\.3|normaliz|weighted sum' docs/ARCHITECTURE.md
62:**Hybrid scoring.** `HybridRetriever` combines the two searches into a single ranking with a weighted sum of normalized scores:
68:`vector_weight` and `keyword_weight` are constructor arguments on `HybridRetriever` and default to `0.7` and `0.3`. Each side is normalized independently before blending: `vector_score` is the chunk's raw similarity divided by the highest similarity in the current result set, and `keyword_score` is its BM25 score divided by the highest BM25 score in the current result set. Normalization is therefore per query rather than against a fixed scale, so the same chunk can normalize differently depending on what else the query returned; a maximum of zero yields a score of `0` rather than a division error. A chunk returned by only one of the two searches keeps `0.0` for the side that did not return it instead of being dropped, so it can still rank if its one score is strong enough.
70:For example — illustrating the formula, not values taken from a retrieval run — a chunk whose raw vector score is `0.8` where the batch maximum is `1.0`, and whose BM25 score is `6` where the batch maximum is `12`, normalizes to `0.8` and `0.5`. At the default weights that blends to `0.7 * 0.8 + 0.3 * 0.5 = 0.71`.
```

The grep went from zero matches to three, which is the observable the plan
named. The documented values match `rag/retriever/hybrid.py` at the same commit:
`vector_weight: float = 0.7` and `keyword_weight: float = 0.3` (lines 18-19),
and the `vector_scores_max` / `keyword_scores_max` divisions (lines 73-74).

**Scope check**

```
$ git status --short
?? comment.md
?? plan.md

$ git show --stat HEAD
 docs/ARCHITECTURE.md | 10 ++++++++++
 1 file changed, 10 insertions(+)
```

The edit was committed to `docs/ARCHITECTURE.md` (10 insertions) as commit
`0d3336f`; `plan.md` and `comment.md` stay untracked and out of the branch on
purpose.

**Not run:** `make lint && make typecheck && make test-unit`, which the plan
commits to as a CI guard. Those targets require `.venv` and `make setup` has not
been run in this clone, so they are outstanding rather than passing. This is a
documentation-only diff touching no Python, but I have not demonstrated that the
checks are green and am not claiming it. Recorded under Deviations in `plan.md`
and to be run before the Unit 4 pull request.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2
   unbuildable 3/3  wrong-cause 4/4`. The only disagreement was `pkg-14`.

One full run occurred. I did not re-run after it: the bar and the category floor
were both met, and the single disagreement was on a package whose deferral the
gold note itself calls arguable, so a revision would have risked packages that
already agreed. My reasoning for stopping is under Trade-offs.

**Package analysis**

`pkg-14` (zellij-org/zellij#5174). My rubric decided `reject`; the gold label is
`accept`. The run recorded `pkg-14  clear-accept  accept  reject  NO  failed:
executable-start, stated-unknowns`.

My `executable-start` check requires the plan to name "the specific file or code
site it will change". `pkg-14` names subsystems rather than files — "the client
attach/reattach path in `zellij-server` (session connection handling) and
`zellij-client`'s terminal query issuance" — and then says "exact functions to be
pinned in the PR after tracing the query issuance with debug logs". My check read
that trailing clause as a decision left open at posting time, which is the same
shape it fails `pkg-18` for ("fix upstream or vendored, whichever is easier").

Reading the package again, the gold label is defensible and my check is the thing
that is strict. The approach itself is decided — drain pending OSC color-query
responses in the reattach handshake before pane input is wired, bounded to the
Unix client path — and the author states the tracing method already works ("the
leak's origin is visible in `zellij --debug` output"). So a stranger could in fact
start: open the client connection handling in `zellij-server`, run the debug
trace, and land on the function. What is deferred is the address, not the
decision. `pkg-18` defers the decision itself. My pass condition does not
currently separate those two cases, and `pkg-14` is where that shows.

**Check rationale**

`| executable-start | The files or code sites the plan names, and the approach it commits to. | The plan names the specific file or code site it will change and commits to one approach; where it raises alternatives, it picks one. Someone who has never seen this code could begin the first edit without asking the author a question. Brevity is not a fail and missing headings are not a fail; an unmade decision is ("profile and see", "gocui or tcell, not sure", "upstream or vendored, whichever is easier"). | required |`

It reads that way because the failure it exists to catch is an unmade decision,
not a short write-up. The eval set punishes a check that grades shape: `pkg-02` is
a clear accept whose entire plan is five short paragraphs, so any condition
keyed to length, headings, or section count would reject it. I therefore wrote
the condition around a single outcome — could a stranger begin the first edit
without asking the author anything — and anchored it with three quoted phrases
from the unbuildable packages rather than with an adjective like "detailed".

What I rejected in favour of it was a structural version ("the plan lists the
files it will touch"), which `pkg-10` would have passed by naming a directory
while still saying "profile and optimize" as its entire approach.

**Trade-offs**

The anchor phrase "names the specific file or code site" is what `executable-start`
gives up, and `pkg-14` is the package whose result it changes: gold `accept`, my
`reject`, because the plan names two crates and defers exact functions to a debug
trace it already has working.

I accept that miss rather than loosening the check. Loosening it to accept
subsystem-level naming would reach `pkg-10` ("profile-and-optimize with no
files"), `pkg-17` ("gocui? tcell? not sure") and `pkg-18` ("recover() somewhere"),
all three of which my rubric currently gets right and all three of which name
areas without committing to a site. That trades one agreed package for three at
risk, against a bar I had already cleared at 19/20 with every category matched.
The gold note for `pkg-14` calls its deferral "arguable" and the unit page says
four of the twenty could reasonably go either way, so I read this as the known
cost of a check tuned to catch the unbuildable family cleanly rather than as a
defect to fix.

The case it will keep missing, stated plainly: a plan that has genuinely decided
its approach and has a working method for finding the exact call site, but has
not yet run it. My check cannot currently tell that apart from a plan that has
decided nothing, because both look like a missing file path.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
