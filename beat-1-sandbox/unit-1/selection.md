# Unit 1 Issue Selection

## Selected issue

**Issue link**

[codepath/pathreview-ai301-fa26-s3#11](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/11)

**Verdict output**

The issue is a focused documentation task: explain the hybrid retrieval scoring formula and default weights in `docs/ARCHITECTURE.md`, with an example. It is labeled `good first issue`, names one file, and estimates 2–3 hours. GitHub shows it is open, unassigned, and has no branches or pull requests.

```json
{
   "item": "codepath/pathreview-ai301-fa26-s3#11",
   "checks": [
      {"name": "repo-liveness", "grade": "pass", "evidence": "The Path Review repository is active and the issue is currently open."},
      {"name": "not-claimed", "grade": "pass", "evidence": "GitHub shows no assignee and no branches or pull requests connected to issue #11."},
      {"name": "policy-allow", "grade": "pass", "evidence": "The repository contribution guidance requires review, testing, and CI but does not ban AI-assisted contributions."},
      {"name": "bounded-scope", "grade": "pass", "evidence": "The issue names one documentation file and one specific missing explanation with an example."},
      {"name": "well-specified", "grade": "pass", "evidence": "The issue identifies the desired formula, default weights, target file, and example."}
   ],
   "verdict": "accept"
}
```

## Eval iterations

**Run history**

The complete run reported: `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

**Issue analysis**

For `issue-12`, the rubric's decision was `reject`, matching the gold label `reject`. The bundle says the repository's contributing documentation bans AI-generated code and documentation outright, so the required `policy-allow` check fails even though the issue passes the liveness, claim, and scope checks.

**Check rationale**

The rubric check is: `The policy does not contain an outright ban on AI-assisted contributions (e.g. "we do not accept AI-generated code or documentation"). A policy that requires disclosure, personal understanding, testing, or human review passes; a policy that merely "discourages" AI passes; a repo with no AI statement passes.`

This check separates a repository that permits responsible assisted work from one that explicitly prohibits it. It treats disclosure and human review as compatible with contribution while treating an outright ban as disqualifying.

**Trade-offs**

This policy check can reject an otherwise excellent first issue when the repository bans AI assistance, as it did for `issue-12`. It also accepts repositories with no AI statement, so it cannot determine whether a maintainer would welcome AI in practice. That is intentional: the check follows written repository policy and avoids inventing a stricter rule than the evidence supports.

## Selection rationale

The issue fits my interest in technical documentation and RAG systems, and the estimated 2–3 hour scope fits a focused first contribution. It names one file and asks for one explanation, so I can verify the result against `rag/retriever/hybrid.py`.

The verdict correctly identified that the issue is active, unclaimed, bounded, and specific enough to plan. I also weighed the fact that the exact example and wording are left to the contributor; the rubric cannot judge how easy it will be to explain the formula clearly or how much investigation the existing implementation will require.

Claiming should be straightforward because GitHub shows no assignee and no linked branch or pull request. The main difficulty is that another student may comment on the issue before I do, so I should claim it promptly and follow the repository's contribution workflow.
