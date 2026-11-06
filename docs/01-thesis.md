# 01 · Thesis

*Distilled from the product thesis input (August 2026) and the product and critic reviews of it.*

## Where it came from

The thesis was not written as a vision statement. It was written as a correction. The workbench already existed and was already running daily, with a long list of mechanisms: a tiered memory, a retrieval engine, a hook layer, an errata file, dozens of skills and subagent roles. What it lacked was an answer to three plain questions: what is this for, for whom, and what does success look like.

The operator's own diagnosis, in their raw input, had three parts.

- **Human-in-the-loop was missing.** "AI is genuinely useful, but the human-in-the-loop part is gone." Agents proposed and executed faster than decisions could be judged.
- **Learning was being squeezed out.** "I understand most AI terms roughly, but I cannot retain them and cannot make an informed choice." Every decision point was a chance to learn, and every one was skipped under pressure to move on.
- **Too many projects, no module boundaries.** Work was not split by granularity or by module, so nothing could be iterated cleanly, and nothing could be swapped without fear of breaking the whole.

The input also recorded the operator's rebuttal to an earlier challenge, which became the central architectural claim: the stable layer is the governance and the shape of the data; the volatile layer is the hooks and tools. The architecture exists so that the volatile layer can be replaced without shaking the stable one.

## The statement

> Asteria is a workbench for one person building with AI. It treats three failure modes: being pushed along, learning nothing, and work scattered everywhere. It makes three guarantees: everything is a plain, searchable, traceable file; any part can be swapped whole without shaking the rest; and before a decision is signed, the work pauses and the decision becomes something learned. Success is measured, not felt: daily work happens here, and the operator can say why any module exists.

The three guarantees map one-to-one onto the three complaints.

| Complaint | Guarantee | Mechanism |
|---|---|---|
| Work scatters | Plain, searchable, traceable files | Markdown and config as the only durable form; no database that files cannot reconstruct |
| Cannot swap parts safely | Any part replaceable whole | Layered dependencies, contracts with checks, paths only in config |
| Pushed along, learn nothing | Pause before signing, decision becomes learning | The judgment loop: triage gate, pause protocol, side quests |

One thing was deliberately left out of the statement. The operator described the end-state feeling they wanted: having built the infrastructure, to master a complex system and move through it lightly. That feeling is real but cannot be accepted or rejected by a test, so it stays out of the thesis. It is allowed to emerge as the compound result of the three guarantees.

## Success that can fail

The product review insisted that every promise in the thesis come with a test command, an oracle and a pass condition, and that no row be marked as passing before the instrument that measures it exists. The candidate scorecard:

| Promise | How it is tested |
|---|---|
| Any part swappable without shaking the rest | Replace one organ's implementation with a stub inside a worktree; only that organ's own contract checks may fail |
| Paths live only in config | A repository-wide grep for path literals outside config and tests returns zero, with any allowed exceptions named one by one |
| Adding a port needs no central edit | Create a dummy port; the diff outside its own directory touches at most one file, and that file is the config |
| Every executable has been looked at | A two-way reconciliation: zero unclaimed files, zero references to files that do not exist |
| Module cards double as teaching material | A fresh agent reads one card and answers three questions: why does this exist, what breaks without it, who consumes it |
| Daily work happens here | Over fourteen days, at least eighty percent of working sessions start in the workbench |
| Learning does not eat throughput | For every three modules frozen, at least one completed side quest; median session length grows by no more than a quarter |
| Resistance is real | Inject faults into three organs (missing dependency, bad input, timeout); all three exit non-zero and name the root cause; zero silent degradations |

Two of these rows need instruments that do not exist yet, a session counter and a quest log. The review's rule: do not mark those rows as passing.

## Sunset conditions

A thesis without conditions under which it is wrong is not falsifiable, and an unfalsifiable thesis cannot be improved. Five triggers were written down, each observable from a command:

1. If, across two review batches in a row, at least half the decisions brought to the human turned out to need only a three-sentence explanation, the learning clause is overweighted and side quests should be triggered by decision grade rather than by default.
2. If two or more modules have neither an existing implementation to survey nor a top-down contract, the method has a definitional gap and the thesis must explicitly allow a greenfield path.
3. If, in any thirty-day window, at least half of working sessions start outside the workbench, the workbench thesis has failed. This is a thesis error, not an execution error, and the fix is a rewrite, not more discipline.
4. If a third-party harness appears that can pass the first three scorecard rows, re-evaluate building versus adopting.
5. If any elegance test fails twice in a row and every proposed fix is rejected, that test may not apply here. Record an amendment and demote it; never keep it silently.

Anyone, including a fresh agent, may submit evidence that a trigger has fired, with the command output attached. Only the operator can change the thesis. A change is an amendment with a version bump. Frozen module cards are never unfrozen by a thesis change; they are tagged with the version they were frozen under, and any that now contradict the thesis go to a single reversal table that the operator rules on in one pass.

## What the reviews found wrong

The thesis was reviewed from five angles. Three findings changed it.

**The method had become the goal.** The earlier vision said: every module gets a frozen card, and the operator has looked at every executable. That is a method, and a rare one in being mechanically checkable. But by its own wording, writing all the cards would count as success even if the operator went back to working elsewhere. The product review demoted it to a method section and put the measurable thesis above it.

**New capabilities could never enter the plan.** The method defined its baseline as a survey of what already exists. Two of the capabilities the operator asked for, a session chronicle and a judgment-quality evaluation layer, did not exist yet. No existing code meant no baseline, which meant they could never be scheduled. Walking the full method would have produced a lean copy of the existing system with neither new capability. The fix was a greenfield path: a top-down contract plus a falsifiable probe, with "three external precedents" standing in for the missing baseline.

**Learning as written would eat throughput.** "Seven decisions means seven side quests" is a throughput bomb. The human-factors review replaced it with an admission rule: at most one upgraded side quest per session, and only for decisions that will recur. That rule is described in [04 · Human in the loop](04-human-in-the-loop.md).

The adversarial review added a finding the thesis keeps on record rather than hiding: at the time of review, the plan had fifteen locked decisions and six amendments and had not shipped a line of code. The workbench's own errata already named this pattern, and the plan was cut accordingly. That episode is described in [05 · Simplicity and search](05-simplicity-and-search.md).
