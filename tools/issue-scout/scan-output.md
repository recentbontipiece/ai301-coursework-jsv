# Scan output

## 2026-11-08 scan 1 (strategy v1)

Note: only the closing verdict line and JSON block were saved; the per-candidate summary lines were not captured.

Honestly, none of these is a great first issue for you. factreel#16 is the only reasonable one, and only if you write Spanish. I'd fix the ranking problems above and scan again.

```json
[
  {"item": "https://github.com/DeadY2J/factreel/issues/16", "checks": [
    {"name": "repo-liveness", "grade": "pass", "evidence": "archived: false; pushed 2026-11-08; all 5 last commits 2026-10-22..2026-11-08"},
    {"name": "not-claimed", "grade": "pass", "evidence": "assignees: []; linked PRs: []; no cross-referenced PRs; 0 comments"},
    {"name": "policy-allow", "grade": "pass", "evidence": "CONTRIBUTING.md and PR template contain no AI statement (silence passes)"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Translate 4 named docs into docs/es/ and link from README; no umbrella/TBD/churn condition met"},
    {"name": "well-specified", "grade": "pass", "evidence": "Names configuration.md, providers.md, scheduling.md, youtube.md plus a 'Done when' criterion"}],
   "verdict": "accept"},
  {"item": "https://github.com/GUURUUU/PixelMorph/issues/3", "checks": [
    {"name": "repo-liveness", "grade": "pass", "evidence": "archived: false; pushed 2026-11-08; 4 of 4 commits dated 2026-11-08 (>=3 within 1 year)"},
    {"name": "not-claimed", "grade": "pass", "evidence": "assignees: []; linked PRs: []; 0 comments"},
    {"name": "policy-allow", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI statement"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Single file: 'BestCam/README.md' build walkthrough; no fail condition met"},
    {"name": "well-specified", "grade": "pass", "evidence": "Names toolchain, install_bestcam.ps1 step, known pitfall, and target file BestCam/README.md"}],
   "verdict": "accept"},
  {"item": "https://github.com/andrewsiah/agentswarm-cli/issues/4", "checks": [
    {"name": "repo-liveness", "grade": "pass", "evidence": "archived: false; pushed 2026-11-08; last 5 commits 2026-10-29..2026-11-08"},
    {"name": "not-claimed", "grade": "pass", "evidence": "assignees: []; linked PRs: []; 0 comments"},
    {"name": "policy-allow", "grade": "pass", "evidence": "CONTRIBUTING.md: 'issues and reviews are run by the agentswarm.space project agent (AI)' - no ban"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "'Scope is README prose/navigation only': add README.ko.md + nav link"},
    {"name": "well-specified", "grade": "pass", "evidence": "Pinned pytest check command defines done; structure-preservation rules listed"}],
   "verdict": "accept"},
  {"item": "https://github.com/The-PR-Agent/pr-agent/issues/4773", "checks": [
    {"name": "repo-liveness", "grade": "pass", "evidence": "archived: false; pushed 2026-11-08; all 5 last commits 2026-11-08"},
    {"name": "not-claimed", "grade": "fail", "evidence": "linked PR https://github.com/The-PR-Agent/pr-agent/pull/4774 state OPEN (lyn1dev, 2026-11-08)"},
    {"name": "policy-allow", "grade": "pass", "evidence": "CONTRIBUTING.md no AI ban; AGENTS.md present (guidance for coding agents)"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Add a :::note to docs/docs/tools/improve.mdx and review.md"},
    {"name": "well-specified", "grade": "pass", "evidence": "Exact note text, file paths, and 'Done when: the docs build ... passes'"}],
   "verdict": "reject"},
  {"item": "https://github.com/dmarx/luria/issues/400", "checks": [
    {"name": "repo-liveness", "grade": "pass", "evidence": "archived: false; pushed 2026-11-04; all 5 last commits 2026-11-04"},
    {"name": "not-claimed", "grade": "pass", "evidence": "assignees: []; linked PRs: []; 0 comments"},
    {"name": "policy-allow", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI ban; CLAUDE.md present"},
    {"name": "bounded-scope", "grade": "fail", "evidence": "Body is only 'nice case study for multiple realizability, hierarchical latent semantics...' - no specific change described"},
    {"name": "well-specified", "grade": "fail", "evidence": "No repro, file paths, outcome, or acceptance criteria"}],
   "verdict": "reject"}
]
```

## 2026-11-08 scan 2 (strategy v2: date windows, issue# >= 50, skill/platform + classroom drops, strict Q1>Q2>Q3>Q4, MEMBER/COLLABORATOR > OWNER)

400 results -> 373 unique -> drops (assignee 89, number<50 62, comments>=15 17, skill/platform 14, classroom 7, label 5, title 1) -> 105 after one-per-repo -> top 5 graded. All five from Q1 (Python + bug).

Accepted, in fit order:
1. ferrumio/pydynox#559: dataclass init=False fields break MapAttribute/JSONAttribute round-trip. Q1; MEMBER-opened, 1 comment, updated 2026-11-04. Fit: pure Python bug in a library, runnable repro, named files and existing test files (F1, F3). Caveat: vanshaj1999 commented 2026-11-04 "I'd like to take this one too. I have a small fix ready". The rubric as written ignores claim comments (Path Review wording), so it passes, but by scope.md this issue is claimed.
2. amd/gaia#5281: macOS menu-bar template icons are flat placeholders. Q1; COLLABORATOR-opened, 1 comment (bot), updated 2026-11-03. Fit: active org project (F4), bounded with an acceptance checklist, needs a Mac (have one). Weaker on F1: the work is PNG assets + Electron .cjs tests, not Python. Verified still open on main: tray-iconTemplate.png is 79 bytes.

Rejected:
3. Adwize/adwize-oss#53: not-claimed (open linked PR #55 by Mr-Neutr0n). Q1; MEMBER, 0 comments.
4. EvoScientist/EvoScientist#650: not-claimed (open linked PR #755 by Mr-Neutr0n). Q1; MEMBER, 1 comment.
5. repowise-dev/repowise#4089: not-claimed (open linked PR #4097 by aazminu). Q1; MEMBER, 1 comment.

```json
[
  {"item": "https://github.com/ferrumio/pydynox/issues/559", "checks": [
    {"name": "repo-liveness", "grade": "pass", "evidence": "archived: false; pushed 2026-11-06; all 5 last default-branch commits 2026-11-06"},
    {"name": "not-claimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; claim comment by vanshaj1999 (2026-11-04) ignored per rubric's house-rule wording"},
    {"name": "policy-allow", "grade": "pass", "evidence": "CONTRIBUTING.md: 'We appreciate GenAI contributions, but we care about quality' (conditions, not a ban)"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "One bug in MapAttribute.deserialize()/JSONAttribute._from_dict(); no umbrella, churn, or TBD condition"},
    {"name": "well-specified", "grade": "pass", "evidence": "Runnable repro, expected vs actual TypeError, files primitives.py/special.py and tests test_map_typed.py/test_json_typed.py named"}],
   "verdict": "accept"},
  {"item": "https://github.com/amd/gaia/issues/5281", "checks": [
    {"name": "repo-liveness", "grade": "pass", "evidence": "archived: false; pushed 2026-11-08; all 5 last default-branch commits 2026-11-08"},
    {"name": "not-claimed", "grade": "pass", "evidence": "assignees: none; no linked PR; only cross-ref PR #5354 is MERGED and left tray-iconTemplate*.png untouched; sole comment is github-actions"},
    {"name": "policy-allow", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI statement; AGENTS.md sets coordination rules for coding agents (no ban)"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Replace two macOS template PNGs, extend #5273 asset tests, remove unused tray-icon-active placeholders"},
    {"name": "well-specified", "grade": "pass", "evidence": "'## Acceptance' checklist names tray-iconTemplate.png/@2x.png, sizes, light/dark check, and tests"}],
   "verdict": "accept"},
  {"item": "https://github.com/Adwize/adwize-oss/issues/53", "checks": [
    {"name": "repo-liveness", "grade": "pass", "evidence": "archived: false; pushed 2026-10-24; last 5 commits 2026-10-15..2026-10-24"},
    {"name": "not-claimed", "grade": "fail", "evidence": "linked PR https://github.com/Adwize/adwize-oss/pull/55 state OPEN (Mr-Neutr0n, 2026-11-06)"},
    {"name": "policy-allow", "grade": "pass", "evidence": "CONTRIBUTING.md no AI statement; AGENTS.md present (agent reference)"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "'Smallest fix': install.sh next-steps output, PATH guard, fish line, README note"},
    {"name": "well-specified", "grade": "pass", "evidence": "Repro of 'adwize: command not found', cause in install.sh, 4-step fix list"}],
   "verdict": "reject"},
  {"item": "https://github.com/EvoScientist/EvoScientist/issues/650", "checks": [
    {"name": "repo-liveness", "grade": "pass", "evidence": "archived: false; pushed 2026-11-08; all 5 last commits 2026-11-06"},
    {"name": "not-claimed", "grade": "fail", "evidence": "linked PR https://github.com/EvoScientist/EvoScientist/pull/755 state OPEN (Mr-Neutr0n, 2026-11-06)"},
    {"name": "policy-allow", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI ban"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Remove langgraph_dev_file_persistence setting from config/settings.py and langgraph_dev/manager.py; maintainer confirmed fix 2026-11-05"},
    {"name": "well-specified", "grade": "pass", "evidence": "3-step repro and file-by-file expected change list"}],
   "verdict": "reject"},
  {"item": "https://github.com/repowise-dev/repowise/issues/4089", "checks": [
    {"name": "repo-liveness", "grade": "pass", "evidence": "archived: false; pushed 2026-11-08; all 5 last commits 2026-11-08"},
    {"name": "not-claimed", "grade": "fail", "evidence": "linked PR https://github.com/repowise-dev/repowise/pull/4097 state OPEN (aazminu, 2026-10-18)"},
    {"name": "policy-allow", "grade": "pass", "evidence": ".github/CONTRIBUTING.md has no AI ban (agent setup instructions only)"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Fix _UNKNOWN_ONLY_HINT in tool_health/projection.py:36-40 and MCP_TOOLS.md key list"},
    {"name": "well-specified", "grade": "pass", "evidence": "File:line references, mechanism, and repro output"}],
   "verdict": "reject"}
]
```
