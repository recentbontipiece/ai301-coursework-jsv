# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm an AI301 student learning to contribute to Python and TypeScript projects;
for this issue I'm checking the documented hybrid-retrieval behavior against
the implementation. I will separate what I observed from what I still need to
investigate, and I won't claim experience or results I don't have.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

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

## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->

- I never promise a fix, merge, or deadline before I know what the work requires.
- I never say I reproduced a behavior unless my own output shows that behavior.
- I never turn a hypothesis into a root-cause claim or generalize one run to every user.
- I never copy another person's reproduction as my own or omit a disclosure the repo requires.
