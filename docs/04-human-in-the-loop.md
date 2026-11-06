# 04 · Human in the loop

*Distilled from the human-factors review and the framework draft's judgment-loop section.*

## The problem with "keep a human in the loop"

Every agent system claims a human in the loop. In practice the human is either a bottleneck (asked to approve everything, so they approve everything) or a rubber stamp (asked to approve everything, so they stop reading). Neither is judgment. The design question is not whether to ask the human, but which decisions to ask about, what the agent does while waiting, and how to make the asking produce a better human over time.

The operator's own framing: "Seven decisions are really seven side quests. Each one is a chance to learn what I need in order to judge." The review agreed with the goal and disagreed with the rule.

## Recurrence is the gate, risk is not

The intuition is that important decisions deserve learning. Three strands of evidence point elsewhere.

- Information has value only when it can change an action, and is worth acquiring only when its value times the number of future uses exceeds its cost (value-of-information analysis, Howard 1966).
- Explaining things to someone who already holds the schema lowers their performance; a rule of "always explain" becomes a tax as the operator gets better (expertise reversal, Kalyuga and Sweller 2003).
- Full automation collapses situational awareness and slows recovery after failure, while moderate involvement preserves it (Endsley and Kiris 1995). Involvement is graded, not binary.

The consequence is counterintuitive: **how often a decision will recur is the admission criterion; how risky it is, is not.**

| | One-off | Will recur |
|---|---|---|
| **Low risk** | Delegate outright, no explanation | Three sentences. Repetition will teach it. |
| **High risk** | Three sentences plus a verification aid. Do not teach. | **Upgrade to a side quest.** The only entry. |

The high-risk one-off cell is the trap. It feels like the decision most worth learning from, and it is pure loss: the cost of learning is paid once and there is nothing to amortise it over. What a high-risk one-off decision needs is care, not a lesson. The agent provides a verification aid so the human can check the recommendation, not more explanation so they trust it.

The three inputs are mechanical. Blast radius comes from the dependency graph (how many downstream nodes an edit touches). Reversibility is three questions (schema or migration touched? data deleted, money spent or anything published? downstream already written against the old contract?). Recurrence is read off whether the decision is about a contract or an instance; contracts always recur.

## The throughput answer

Two hard caps, not suggestions:

- **At most one upgraded side quest is admitted per session.** A second qualifying decision goes to a pending pool, ordered by an existing priority rule. Admission and ordering are separate gates and are never merged.
- **Work in flight is capped at three problems.** Each slot must hold a genuine, worked answer; placeholders do not count. Slots must differ enough from one another to be worth the human's attention.

Expected outcome, from the review: of seven decisions in a session, one or two pass the gate. That is the whole throughput answer. The learning clause stops being a bomb and becomes a budget.

## The pause protocol

When a decision is admitted for upgrade, the agent does three things and then freezes.

1. Writes three mutually exclusive options, a recommendation, and under each option what the agent would do next.
2. Writes a capsule for the human whose first line is the next concrete action on return ("write X into section Y of Z"), not a summary of what was discussed. Sessions close and goals decay; prospective cues outperform retrospective ones (Trafton 2003, Monk 2008).
3. Registers the resume cue where the main line of work will meet it.

While the human is away, the agent neither idles nor guesses. The same dependency query that computed blast radius draws the boundary:

- Inside the frozen node's impact set: no progress. That would be deciding on the human's behalf.
- Outside it, sibling modules: work must continue.
- The frozen node itself: only verification aids may be prepared. The goal is to let the human check the recommendation, not to make them trust it. The best intervention supports verification, not transparency (Schemmer et al. 2023).

## Trust per decision class, not a level for the person

The operator said: "I am a person with levels. Some decisions need me to go and get more experience first." The review agreed that trust should be modelled explicitly and disagreed with the shape.

A single global level is decoration and is harmful. Adding badges and leaderboards to a course lowered intrinsic motivation, satisfaction and final grades (Hanus and Fox 2015). Competence belongs to the person; a decision class is a unit of work; the two form a matrix. One scalar for "the operator's level" is a category error.

What was designed instead is an entrustment value per decision class, borrowed from medical training (ten Cate's entrustable professional activities), with a currency date. Its main job is to make the agent say less over time, not to make the human feel good.

- Low entrustment: the agent must include a teaching layer.
- High entrustment: the agent must stop explaining that class of decision and give the conclusion. This fade is mandatory, because explanation at that point is a net loss.
- Stale currency date: automatic downgrade by one step. Currency is recent activity, not a one-time exam.

Three disciplines: never render it as a score or progress bar; never self-assess (a level rises only after a real decision was made and later confirmed); it can go down.

This mechanism is designed and deliberately not installed. The precondition is at least ten real decision records, gathered through the existing gates, before the fields and thresholds are calibrated.

## Evidence first

One rule in this area is a hook rather than a design, and it blocks.

The human cannot see what the agent sees. Tool output is collapsed by default in the interface; a question's preview is truncated and cannot scroll. So an agent that runs a scan and then immediately asks "which of these should I keep?" is asking a blind decision. The rule: the evidence the decision needs (samples, comparisons, counts, links) goes into the message body as a table or list, and the message ends there. The decision is asked on the next turn, carrying only the decision. A hook refuses a decision prompt that points at evidence the human has not been shown.

A second standing rule for anything brought to the human for a ruling: it must carry the strongest counter-position and why it did not win. A proposal with no opposition recorded is not ready to be ruled on. This also builds the decision record the entrustment mechanism needs.

## How it would rot

The review listed the likely failure modes with an early indicator for each, so they can be checked rather than felt.

| Failure | Indicator |
|---|---|
| The pending pool becomes a graveyard | Median age of pending side quests exceeds fourteen days, or admissions outpace completions by half for three sessions |
| Entrustment inflates; the ledger becomes a trophy case | No downgrade has ever happened, or values drift up while currency dates do not refresh |
| Learning theater | In a decision class, the human's ruling differs from the agent's recommendation close to zero percent of the time. Learning that changes nothing has, by definition, not happened. |
| Pause becomes the default; throughput collapses | More than half of sessions contain a frozen decision, or median time from freeze to resume exceeds three days |

The third indicator is the one that matters most. It is the only mechanical test that can falsify "I learned something," and it is the definition of value of information.

## The honest counter-evidence

Two findings weigh against the thesis and are kept on record. A mining study of code review across thousands of repositories did not detect a knowledge-transfer effect from reviewing others' work. And "just-in-time" learning has not been shown to beat "just-in-case" learning on knowledge or applied skill.

So the value of learn-for-judgment rests entirely on amortisation over recurrence, not on a claim that learning while working is more effective. Applied to one-off decisions, the literature predicts that the throughput is spent and nothing comes back. The critique and the mechanism are the same thing: the admission gate is what the critique looks like when it is executable.
