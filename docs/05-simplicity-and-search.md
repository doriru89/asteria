# 05 · Simplicity and search

*Distilled from the co-engineering philosophy (v2, August 2026), which was itself produced by four independently conditioned research agents and a point-by-point ruling by the operator.*

## The premise that died

Richard Gabriel's 1991 essay "Worse is Better" argued that the MIT school (interface simplicity, correctness and completeness first) lost to the Unix school (implementation simplicity first) for one economic reason: implementation cost dominated everything. "The right thing" lost because it was too expensive, not because it was wrong.

AI pushes the marginal cost of implementation toward zero. The premise dissolves. The operator's reversal: after the productivity explosion, the best thing is affordable for the first time. The two schools can merge: aim for the lowest total complexity (willing to accept a larger single component if the interaction complexity is lower), while keeping modularity, whose job has changed.

Affordable is not the same as obtained. Historically "the right thing" was expensive in two places, and they map exactly onto the two problems of co-engineering:

| Historical cost | Problem | Content |
|---|---|---|
| Confirming it is right | **Verification** | The agent self-orchestrates until mechanical checks pass; the human only signs at the end, and is asked for insight only when the value of information is high |
| Finding the right thing | **Search** | Guarantee that the agent has searched the space of framings, rather than iterated to a local optimum inside the framing it was handed |

## Simplicity is a goal, never a criterion

The central ruling of the philosophy, in four parts.

1. **Simplicity is the goal, at the scale of the whole.** The architecture exists for it.
2. **At the scale of a single decision, simplicity is banned as a criterion.** "Is this simple enough?" invites premature simplification. The goal can be simplicity; the input never is.
3. **A gate question is legal if and only if its answer comes from a command's output.** "Is the diff over 400 lines?" is legal. "Is this simple enough?" is not. This turns the two-scale rule into something an agent can follow.
4. **Good review is not simplicity. It is the foundation of simplicity.** The only resource that does not scale is human attention. A system that cannot be reviewed cannot be ruled on, and so can never converge to simple.

From this, modularity gets a new job: **a module is the unit of human review.** The empirical support is that code review is most effective at two to four hundred lines, defect detection collapses above four hundred, and a single review session caps at about an hour. "A module is what fits in one review" becomes mechanisable for the first time: a blocking gate of at most 400 lines of diff per review unit. One gate, not a stack of proxies; cognitive-complexity metrics correlate with line count and do not beat it.

## Three layers and a learning line

```
problem 1 ─[diverge → converge → decide]─┐
problem 2 ─[diverge → converge → decide]─┼─> merge (machine proposes, human rules) ─> judgment compounds
problem 3 ─[diverge → converge → decide]─┘
  ↑ in flight ≤ 3, each slot a genuine worked answer
  ↑ learning line throughout: differently conditioned parallel work is the teaching material
```

The purpose of running problems in parallel is not throughput. It is learning. Give the human several genuinely independent, sufficiently different directions, and the human learns from the sum of what the agents found, with judgment compounding at the highest rate. Recording what was learned is the other half of the compounding.

## Parallel is not independent

The key correction in the divergence layer: several fresh sessions of the same model sample the same distribution. Human diversity comes from fragmented knowledge; a language model has none of that. Blind evaluation fixes anchoring on the judging side and does nothing about homogeneity on the generating side.

What has been tried and does not work: adding debate rounds (the positions converge and get more alike), raising temperature (the sampling distribution has already collapsed), "be creative" prompts and creative-expert personas, and using a model as a reframing adviser (a controlled study found zero improvement and a widening gap between novices and experts).

What works: **condition each slot differently.** Top-down from constraints, bottom-up from existing parts, external precedents, outside framings. The philosophy document itself was produced this way: four research agents, four conditionings, one synthesis, one ruling. Diversity is a property of the archive, not the generator. The criterion that separates candidates is defined before generation, not read off afterwards.

## Divergence in three tiers, by stake

Every module getting double generation all the time was tried and rejected as too expensive. Divergence is triggered by stake instead.

| Tier | Trigger | Mechanism |
|---|---|---|
| **Everyday: the deadlock gate** | A build-and-verify loop has been red for N consecutive rounds | Force one representation change using two explicit operators: relax a constraint, or decompose into blocks. Produce one candidate in the new representation, return to the loop. Not a prose self-question; that version was tried and failed. |
| **Rulings: the counter-position** | Any framing brought to the human for a ruling | Must carry the strongest counter and why it did not win. The cheapest standing divergence discipline, and it produces the decision record as a side effect. |
| **Shape-class decisions** | A decision is judged to be about the shape of the problem | A framing round before the solution round; each slot conditioned differently; the difference criterion applied at the level of method steps (embedding distance was tried and gamed); each finalist gets a cheap empirical probe, because a real shadow run has overturned blind evaluation before. |

The layer carries its own abolition condition: if the default framing wins at least ninety percent of the time, the divergence layer is theater and is downgraded to a periodic sweep.

## Convergence: naming what already exists

Three independent ecosystems converged on the same minimal shape for the verification half: a while-loop, a blocking gate (failure feeds back until green; a non-blocking warning is theater), and an evidence pack. The running workbench already had this shape, so the convergence layer is mostly a naming exercise, with three missing pieces:

1. **A unified evidence-pack contract.** Every claim binds to one rerunnable command and its output.
2. **One review-cost gate.** At most 400 lines of diff per review unit, blocking.
3. **A circuit breaker.** An iteration cap and a token kill switch. A gate that is red forever is an infinite loop, a known failure mode, and the precondition for letting an agent run hard.

**Producer is never verifier**, stated precisely: self-correction without an external signal makes performance worse; loops with an external signal work. The verifier must hold a signal the producer lacks: test execution, a fresh context (the same error is fixable as external input and invisible in one's own output), or a different model. A review by the same model in the same context is theater.

## The shadow of compression

A high-level synthesis is a win for review bandwidth and a loss for learning and trust: the agent decides unilaterally what survives, and the human never sees what was simplified away. Producer-is-not-verifier applies to the synthesis itself. Two compensations, both wiring of existing mechanisms rather than new ones:

- Everything that gets synthesised is first written to disk with its source trail. The audit channel always exists; the synthesis is an entry point, not the whole.
- When a highly compressed synthesis is handed to the human for a ruling, the agent must attach one or two candidate side quests: what to read or do, how long it takes, what question to bring back. The human should not have to collide with a decision point to discover there is something to learn.

## Failure boundaries

The philosophy names the conditions under which it stops applying.

1. **Process is promised, outcome is not.** "Guaranteed to find the global optimum" is unfalsifiable. What can be promised: K genuinely different framings collided, every mechanical gate green, a human ruled.
2. **The divergence layer abolishes itself** when the default framing wins at least ninety percent of the time.
3. **The root cause of the search problem is motivation, not capability.** An agent feels no scarcity, so it does not search on its own; scarcity must be manufactured by the protocol. This is a falsifiable claim: if a model generation is measured producing genuinely different framings unprompted, the layer is downgraded.
4. **The protocol obeys its own rule budget.** New mechanisms introduced by this document: five. Adding one requires deleting one.
5. **Ruling is never fully automated.** The final choice of framing is always a person's, because only the person knows what question they wanted to ask.

## /idea: an open-ended building decision

The search discipline above became a procedure in October 2026 for decisions that do not yet have a plan.

1. **Interview down to a one-pager.** The AI asks until the question, the constraints and the success test fit on one page the person signs.
2. **Fresh agents with different stances.** Each agent gets the one-pager and one stance card (top-down from constraints, bottom-up from existing parts, an external precedent, a deliberately opposed framing). None sees the others.
3. **Compare.** The results are laid side by side on the criterion the one-pager fixed before generation, with where they agree and where they split.
4. **Optionally, demo and council.** The two or three survivors are prototyped cheaply and a council of differently-stanced agents critiques each.

There are four stops where the person looks and chooses: after the one-pager, after the stances are picked, after the comparison, and after the demos. The AI never recommends an option unless asked. The procedure exists because an AI handed a framing optimises inside it; the stances are the manufactured scarcity that the failure-boundary section above says the protocol must supply.

## A sidecar example

The software stack behind the workbench follows one rule: each piece runs where it is cheapest and talks over one plain interface. A vault plugin needs similarity search; it does not embed a model. It calls a small local similarity server over HTTP; that server calls a local embedding model. Three processes, one interface each, any of the three replaceable without touching the other two. The same shape holds for the hosts: Claude Code and Codex are processes that read and write files; the adapter translates their hook payloads; nothing else knows which host ran.

## The feedback ladder

Verification is a ladder, and each rung is cheaper and faster than the one above it. The rule is to climb only as far as the change needs.

| Rung | Catches | Cost |
|---|---|---|
| types | the wrong shape | seconds, every save |
| lint | the known bad patterns | seconds, every save |
| tests | the wrong behaviour | minutes, every commit |
| screenshot | the wrong look, in the real browser | a minute, every visible change |
| human | the wrong thing | the person's attention, only at the gate |

The producer-is-not-verifier rule applies at every rung: the signal must come from somewhere the producer could not have faked it. A screenshot taken by the agent that wrote the page is still external evidence; the agent's description of the page is not.

## The episode this was learned from

The adversarial review of the plan found that the planning protocol had fifteen locked decisions, six amendments, and had shipped zero code over several weeks. The workbench's own errata already recorded this pattern (locking decisions without a step that implements them) at half the size. The response was to apply the rule budget to the protocol itself: the most expensive part of the method (every module written twice, blind, by two fresh agents, then reconciled) was deleted; the one-page resident line replaced "read everything every session"; the first build step was cut from five parts to three. The only addition was one fifteen-line check. A protocol that cannot pass its own rule budget is theater, and the test is: are there more than ten locked rows, more than one page of must-read, any enforcement that has never fired, and an overhead-to-work ratio approaching one to one.
