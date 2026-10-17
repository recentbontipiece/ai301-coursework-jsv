# Voice guide: how I talk upstream

## Who I am in threads

I'm an AI301 student learning to contribute to Python and TypeScript projects;
for this issue I'm checking the documented hybrid-retrieval behavior against
the implementation. I will separate what I observed from what I still need to
investigate, and I won't claim experience or results I don't have.

## Rules I write by

### Rule: Name the concrete work

Tie the comment to the issue's specific behavior and say what I will inspect
next; don't use a generic claim that could fit any issue.

- Wrong: "I'd love to help with this, assigning myself."
- Right: "I'd like to investigate the missing hybrid-scoring explanation by comparing the docs with the retriever implementation."

### Rule: Promise investigation, not a fix

I can commit to checking and reporting what I find, but not to a solution,
timeline, or successful reproduction before I have evidence.

- Wrong: "I'll fix this and open a PR tomorrow."
- Right: "I'll trace the scoring path and report what I can verify before proposing a documentation change."

### Rule: Keep claims within the evidence

Describe the environment and observed result precisely; label explanations as
hypotheses until the code or a controlled test supports them.

- Wrong: "This proves the scoring is wrong for everyone."
- Right: "In this checkout, the implementation weights vector and keyword scores this way; I haven't tested other configurations."

### Rule: Follow the repo's disclosure rule

I will follow the issue and contribution instructions, including an AI-use
disclosure when the repository explicitly asks for one.

- Wrong: "The policy doesn't matter because this is only a small report."
- Right: "I used an AI assistant to help organize this report; I ran the steps and checked the cited behavior myself."

### Rule: Commit to an approach, not an outcome

A plan comment commits me to a direction in front of the people who maintain
the code, which my claim and repro comments did not. I state the change I will
make and the bounds I will hold, and I mark what I have not verified as open.
I do not promise that the approach will be accepted, or that it lands by a date.

- Wrong: "This will fix #11 and I'll have the PR up tomorrow."
- Right: "I plan to document the scoring in the RAG section of `docs/ARCHITECTURE.md`, scoped to that section, and I'll report back when the branch is ready for review."

### Rule: Answer the direction already in the thread

If a maintainer has already pointed at a file, named an approach, or rejected
one, my plan comment says what I am doing with it — adopting it, building on
it, or why I am going elsewhere. I never post a plan that reads as though the
thread were empty.

- Wrong: posting my own approach with no mention of the maintainer suggestion sitting above it.
- Right: "Following the direction given above — documenting the formula in the RAG section rather than changing the defaults."

### Rule: A PR title says what changes, not that I worked on it

A reviewer decides in a few seconds whether to open my PR. The title names
the change and where it lands, in the repo's commit style, and nothing about
effort or enthusiasm.

- Wrong: "Fixed the docs issue!!"
- Right: "docs(rag): document the hybrid retrieval scoring formula"

### Rule: The description promises exactly what the diff contains

Every sentence about the change has to be true of the diff. I list each change
I made, and I say plainly what I left out or added beyond the plan, so a
reviewer never finds something I didn't mention.

- Wrong: "Implements the plan exactly; no other changes." (over a diff with one more sentence than the plan named)
- Right: "One addition beyond the plan: a clause noting that a zero batch maximum scores 0. It is recorded in the plan's Deviations."

### Rule: A shortfall is stated, not apologized for

When a check failed or something is not done, I say what happened and why, in
one plain sentence, without hedging or apologizing.

- Wrong: "Sorry, I couldn't really get the tests to work, hopefully it's fine."
- Right: "`make test-integration` reports no tests ran; Path Review has no integration tests yet."

## Things I never post

- I never promise a fix, merge, or deadline before I know what the work requires.
- I never say I reproduced a behavior unless my own output shows that behavior.
- I never turn a hypothesis into a root-cause claim or generalize one run to every user.
- I never copy another person's reproduction as my own or omit a disclosure the repo requires.
- I never widen a plan past the issue to look thorough; work I am not doing, I say I am not doing.
- I never tick a checkbox or claim coverage the test evidence does not show.
