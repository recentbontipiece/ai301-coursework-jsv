# Scan notes

## 2026-11-08 scan (375 unique results → 135 after drops + one-per-repo → top 5 graded)

- Ranking: Q1 and Q2 share rank key 0, and with fewer-comments then most-recent as tie-breaks, the top 5 were all issues opened *minutes* before the scan, 0 comments, mostly in tiny/new repos (0, 0, 3, 23 stars; PixelMorph was created the same day). Nothing in the strategy rewards repo scale or issue maturity. Consider: Q1 strictly before Q2, a minimum issue age (e.g. createdAt older than 3 days), or `--sort created --order asc` in a window.
- All five top picks came from Q2 (docs); no Q1 bug made the cut. The "Q1 and Q2 first" tie lets docs swamp bugs.
- rubric `not-claimed` still cites "the Path Review house rule in scope.md" (claim comments don't count). scope.md now says claims count again. Rubric wording is out of date: claim comments should block too.
- rubric `repo-liveness` passed a repo created the same day (4 commits, all today). "3 of last 5 commits within a year" can't tell a new repo from a maintained one.
- No rubric check for skills the work needs beyond code: two accepts are translations (Spanish, Korean) and one needs Windows + Visual Studio (I'm on macOS). Fit can't reject, so these go through.
- agentswarm-cli#4 passes every required check but is an AI-run "first passing submission wins" bounty with a 5-day deadline and an AI reviewer. That is not the human contribution workflow F4 is after. Candidate for a "human maintainer reviews PRs" required check.
- pr-agent#4773 had an open PR (#4774) opened ~45 min after the issue: the search ran on an issue that was already claimed. Assignee-only drop at ranking misses this (expected; the rubric caught it).

## 2026-11-08 scan 2 (strategy v2)

- The v2 fixes worked on what they targeted: all 5 picks are Python bugs (Q1), in org repos with 52 to 8,088 stars, opened by MEMBER/COLLABORATOR, 1 to 3 weeks old. No translations, no same-day repos.
- New top friction: 3 of 5 had an OPEN linked PR (adwize#55, EvoScientist#755, repowise#4097). The rubric caught all three, but each one wasted a grading slot. GitHub search supports `-linked:pr` and `no:assignee` qualifiers, so next strategy: add `-- -linked:pr` to every query. That's free and drops them at search.
- One user (Mr-Neutr0n) had open PRs on two of the five, opened 2026-11-06. "good first issue" in active repos gets swept within days, so the 7-day age floor is in tension with freshness. Watch whether `-linked:pr` alone is enough.
- Rubric `not-claimed` passed pydynox#559 even though vanshaj1999 commented "I'd like to take this one too. I have a small fix ready" and the issue body asks people to "comment here first so we can assign it". The house-rule wording ("student claim comments do not count") is the bug. Patch: an unanswered claim comment within N days blocks, unless a maintainer released it.
- gaia#5281 passed every check, but the deliverable is a logo glyph (image design) plus .cjs tests. No rubric or ranking check asks "is the work mostly code in my languages?" Fit can only rank it, which is correct, but worth a preferred check.

## 2026-11-08 patches after scan 2

- rubric `not-claimed` patched: claim comments count again (live if within 30 days and not withdrawn/released by a maintainer); PRs mentioned in comments count. Old version kept as rubric.v1.md. Unit 1 regression: 20/20 (claimed 4/4), bar 18/20 PASS, saved to ai301-unit1-starter/eval/eval-run.txt.
- strategy: every query now ends with `-- -linked:pr`. Verified: Q1 still returns 100 results, and adwize#53, EvoScientist#650, repowise#4089 no longer appear.

## 2026-11-08 scan 3 (strategy v3, rubric v2: 377 unique → 192 after drops → 111 after one-per-repo → top 5 graded)

- The rubric v2 patch works in the wild: pydynox#559 came back at rank 1 and is now rejected on not-claimed (vanshaj1999's "I'd like to take this one too. I have a small fix ready", 2026-11-04, no maintainer reply).
- `-linked:pr` worked: none of the 5 had an open linked PR. But 3 of 5 still died on not-claimed, this time from claim *comments* (pydynox#559, hls4ml#376, alchemy-logging#104). Search can't see comment text, so this is the new slot-waster. Low comment counts (1-2) don't help when the one comment is the claim.
- hivecommons "hive" bot comments showed up on 2 of 5 (spyre-inference#672, alchemy-logging#104): "bring your own coding agent", claim through an external relay, and "Hive runs CI and merges green PRs ... automatically". That's the second time (after agentswarm-cli#4 in scan 1) that an auto-merge, no-human-review workflow got through. spyre#672 passes every required check anyway. Time to make "a human maintainer reviews PRs" a required rubric check. Hive claims made through the relay also aren't visible on the issue thread, so not-claimed can't see them.
- spyre-inference#672 needs IBM Spyre accelerator hardware and a compiler-backend error ("Spyre backend does not support: Multi-arg pointwise with mixed EA") to reproduce. That's squarely on my avoid list (accelerators, compilers), but the title filter only catches literal "gpu"/"compiler"/"kernel". The rubric doesn't ask "can I reproduce this on my machine?" either.
- hls4ml#376: spacenets7's "I'd like to investigate ... Is anyone currently working on this?" plus a posted result was graded as a live claim. That's borderline next to the rubric's examples ("I'd like to work on this"); consider listing "investigating / working on it" and "posted partial results" explicitly. hls4ml is also FPGA/HLS fixed-point work (avoid line).
- gaia#5281 came back from scan 2 and was accepted again. Same caveat as before: the work is mostly an image asset (logo glyph PNGs) plus .cjs tests, not Python.
