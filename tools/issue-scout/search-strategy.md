# Search strategy: where your scout looks, and why

<!--
THIS IS THE PART YOU WRITE. The scout in SKILL.md executes whatever
queries and filters you define here. It ships empty on purpose: the
strategy is your judgment about where YOUR first contribution lives,
and a scan without authored queries is just someone else's defaults.

A filled strategy must contain:

1. At least one query in the Queries section. Each query is a real
   `gh search issues` invocation, and each should trace to a sentence
   of your fit profile in scope.md: if you cannot say which sentence
   produced a query, the query is probably not yours. Search breadth
   is the point at this stage: prefer generous limits and let the
   ranking filters do the narrowing.

2. At least one ranking filter in the Ranking filters section. A
   filter is a condition or an ordering over fields the search
   results already carry (last activity date, assignee presence,
   label set, comment count, repo). It must be checkable from the
   result fields alone: if deciding it would need opening the issue
   page, it belongs in your rubric, not here. Ranking is the free
   stage; keep it free. Make at least one filter an ordering, or
   state your tie-break: with drop conditions alone, the scout takes
   the top of whatever order the searches happened to return, and
   the graded candidates stop being traceable to you.

3. The top-N line, kept or deliberately changed.
-->

## Queries

<!-- Real gh search invocations, one per line, each with a comment
naming the fit-profile sentence it came from. Example SHAPE (write
your own; these values belong to nobody):

gh search issues --label "good first issue" --language python --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url
-->

All queries share one JSON field list (call it FIELDS):
`title,number,repository,labels,assignees,commentsCount,updatedAt,createdAt,authorAssociation,isLocked,url`

Every query uses `--archived=false` (a hard-dead repo can never pass
repo-liveness, so fetching it wastes a slot) and `--sort updated` (so
the 100 results are the most recently active ones, per F4).

Every query also carries two date windows, written below as
`--created "<=AGE7"` and `--updated ">=LIVE90"`, where AGE7 is the scan
date minus 7 days and LIVE90 is the scan date minus 90 days, both as
YYYY-MM-DD (on macOS: `date -v-7d +%F`, `date -v-90d +%F`).
Why (scan 1, 2026-11-08): `--sort updated` alone filled all 100 slots
with issues opened minutes earlier, mostly in repos created that week,
and one of them already had a PR opened 45 minutes after the issue. A
week-old floor gives claims time to surface and keeps the slots for
issues that survived contact with a community.

**Q1** (F1 Python first + F3 bugs over features):

    gh search issues --label "good first issue" --label bug --language python --state open --archived=false --created "<=AGE7" --updated ">=LIVE90" --sort updated --limit 100 --json FIELDS -- -linked:pr

**Q2** (F1 Python first + F3 docs fixes over features):

    gh search issues --label "good first issue" --label documentation --language python --state open --archived=false --created "<=AGE7" --updated ">=LIVE90" --sort updated --limit 100 --json FIELDS -- -linked:pr

**Q3** (F1 Python first, any kind of issue, so Q1/Q2 label spelling
differences don't hide good candidates):

    gh search issues --label "good first issue" --language python --state open --archived=false --created "<=AGE7" --updated ">=LIVE90" --sort updated --limit 100 --json FIELDS -- -linked:pr

**Q4** (F2 TypeScript as the fallback language):

    gh search issues --label "good first issue" --language typescript --state open --archived=false --created "<=AGE7" --updated ">=LIVE90" --sort updated --limit 100 --json FIELDS -- -linked:pr

Every query ends with `-- -linked:pr` (GitHub search qualifier: no
linked pull request). Why (scan 2, 2026-11-08): 3 of the 5 graded
slots went to issues with an open linked PR, which the rubric then
rejected. Dropping them at search costs nothing.

Combine all four result sets and drop duplicates by `url`. A duplicate
keeps the **lowest-numbered** query that surfaced it as its origin
(Q1 beats Q3, so a Python bug stays a Python bug).

## Ranking filters

Applied in the order written, over the result fields only. "Today" is
the date the scan runs.

**Drops:**

1. Drop any result whose `assignees` is non-empty (someone else got
   there first; scope.md: claims count again).
2. Drop any result with `isLocked: true` (a newcomer can't comment to
   claim it).
3. Drop any result whose `updatedAt` is more than 90 days before today
   (F4: live projects).
4. Drop any result with `commentsCount` of 15 or more (long threads
   usually mean design debate or a queue of claimants; F3 and the
   "avoid design discussion" line).
5. Drop any result with a label (case-insensitive) containing any of:
   `question`, `discussion`, `rfc`, `proposal`, `design`, `epic`,
   `tracking`, `meta`, `umbrella`, `duplicate`, `invalid`, `wontfix`,
   `needs triage`, `blocked` (avoid line: design rather than change; or
   not actionable yet).
6. Drop any result whose `title` (case-insensitive) contains any of:
   `tracking`, `megaissue`, `meta:`, `[rfc]`, `cuda`, `gpu`, `kernel`,
   `firmware`, `crypto`, `compiler` (avoid line: umbrella issues and
   unfamiliar domains).
6a. Drop any result whose `title` or any label (case-insensitive)
   contains `translat`, `i18n`, `l10n`, `locali`, `windows`, or
   `visual studio` (avoid line: work that needs a skill or platform I
   don't have. Scan 1 surfaced Spanish and Korean translations and a
   Windows/Visual Studio build guide; I write English and use macOS.)
6b. Drop any result whose issue `number` is below 50 (F4: a project
   whose issue tracker hasn't reached #50 is too young to show it has
   maintainers who review outside PRs. Scan 1's PixelMorph#3,
   agentswarm-cli#4, and factreel#16 were all new or tiny repos. The
   search fields have no star count, so the issue number is the free
   stand-in for project scale.)
6c. Drop any result whose `repository` name contains `pathreview`
   (scope.md: the classroom is behind me. Those repos are other
   cohorts' Path Review sandboxes, and their issues are reserved for
   those students.)
7. One per repo: if several survivors share a `repository`, keep only
   the one ranked highest by the ordering below (five picks from five
   repos beat five picks from one repo that might fail repo-liveness
   or policy-allow together).

**Ordering** (sort keys, first key wins; later keys break ties):

1. Origin query, strictly: Q1, then Q2, then Q3, then Q4 (F1 Python
   before F2 TypeScript; F3 bugs, then docs, then any kind). Scan 1
   tied Q1 and Q2, and docs issues took all five slots.
2. `authorAssociation`: `MEMBER` or `COLLABORATOR` first, then
   `OWNER`, then anything else (a maintainer-opened issue was scoped by
   someone who knows the code: F3. MEMBER/COLLABORATOR means more than
   one person maintains the project, while OWNER is often a solo repo:
   F4).
3. Fewer `commentsCount` first (less churn, fewer would-be claimants).
4. More recent `updatedAt` first (F4).
5. Final tie-break: `url` in alphabetical order, so the order is fully
   deterministic.

## How many to grade

Grade the top **5** ranked results per scan.

<!-- Five is the default because grading is the only stage that costs
money: about $0.20 per candidate, so a scan is about $1, and you can
iterate the strategy several times for less than one full regression
run. You need one good issue, not ten. Raising this number is
allowed, but it is a decision you state here in a sentence, with the
cost you are accepting; it is not a number you bump because a scan
disappointed you once. -->
