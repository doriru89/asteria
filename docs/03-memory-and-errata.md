# 03 · Memory and errata

*Distilled from the architecture and evaluation reviews, the external-precedent survey, the operating rules of the running workbench, and the September 2026 design of the inbox and the Chronicle.*

## Why memory gets the strictest gate

In a system run by one person with many agents, almost every action is reversible: a file edit sits in git, a draft can be thrown away, a research run can be redone. The one write that outlives the session and shapes every later one is a write to persistent memory. A wrong fact there is not a bug to fix later. It is a bias that every future agent inherits.

So memory is tiered, and the top tier is gated.

| Tier | Lifetime | Who writes | Gate |
|---|---|---|---|
| Session | one conversation | any agent | none; it is disposable |
| Project and domain | the life of a piece of work | agents, during work | schema check at write time |
| Persistent canon | indefinite | nobody directly | proposals only, approved by the human |

**The proposal model.** An agent that believes something belongs in canon does not write it. It stages a proposal: the proposed entry, its source, and why it matters. The human runs a review pass, approving, rejecting or editing each proposal. Only approved proposals are applied. The gate is cheap because the volume is low; most of what agents learn belongs in the project tier and stays there.

Two refinements came from the external survey of memory systems for coding agents. Each memory entry carries a scope (this project, the platform, general) so reads can filter and one project's facts do not bleed into another. Each entry also carries a confidence label: verified, inferred or unverified.

## Retrieval: graded, bounded, logged

Recall is hybrid, lexical (BM25) plus vectors. Three rules keep it honest.

- **It is graded as a blocking check.** A golden set of 65 queries with known correct hits is run as part of the reconciliation suite. If retrieval quality drops below the threshold, the check fails and commits are blocked. Retrieval is treated as a contract with a test, not as a feature that works until someone notices it does not.
- **It abstains rather than pads.** When a query shares no meaningful terms with any candidate, nothing is injected. Injecting weakly related memory is worse than injecting nothing, because the agent cannot tell the difference and will reason from it.
- **It logs every decision with a reason.** Each injection and each abstention is written to a log. Injection volume and abstention rate can be reconstructed afterwards, so "how much memory did the agent see" is a question with an answer.

The resident context (constitution, memory index) is loaded by the harness at session start, so hooks cannot meter it. Keeping it small is a content decision made by the human, not a code problem. The survey's one borrowed pattern here: when context must be cut, replace a whole section with a one-line pointer and report what was dropped. Never truncate silently.

## Errata: a failure becomes a check

The immune organ is hooks plus errata. The rule that defines it:

> Every failure that repeats becomes an errata entry and a write-time check that blocks.

An errata entry is a short record of a failure mode: what went wrong, why, and how to apply the lesson. There are more than seventy. But an entry alone is prose, and prose does not change agent behaviour. What changes behaviour is the check: a hook that runs when the agent tries to write and refuses the write if the pattern recurs, with the errata text as the error message.

This is the push channel. Memory is pulled when an agent asks a question. Errata are pushed into the agent's path whether or not it asked. The architecture review was firm that the two must stay separate organs; a merged store would lose the push.

## Hard constraints only

The design treats advisory warnings as theater, on the strength of its own history. A blocking check (exit code 2, write refused) is obeyed every time. An advisory check (exit code 0, message printed) is obeyed almost never, because the agent is mid-task and the message is one more line in a scroll it is not reading. Two consequences are built in:

- A check that adds a new advisory-only rule is itself a blocking failure. The system refuses to grow a layer of warnings.
- A rule that fires more than a set number of times a day for several days is flagged as noise and blocks until it is fixed or removed. A rule that cries wolf trains both the human and the agents to ignore the whole layer.

Where a hook cannot see something, the design says so rather than pretending. Shell commands are a partial blind spot for write-time checks, so the heavy checks run at commit time, where the full diff is visible. Fast and partial at write time, slow and complete at commit.

## The recurrence probe

The evaluation review found the immune organ's blind spot: it captured failures but never measured whether a fix held. Dozens of rules, and for all but one there was no evidence of whether the pattern had recurred since the rule was written. The capture loop was running; the effect loop was not.

The one exception was a rule with its own health check: a detector that scanned real session logs for the failure's signature and reported how many times it had reappeared since the rule's last revision. The review generalised that into the recurrence probe.

- **Input:** an errata rule, a signature detector over real session logs, and the date the rule was last revised.
- **Output:** one line per rule: rule, revision date, recurrences since revision, and the number of sessions scanned. The denominator is mandatory. Zero findings over zero sessions is not a pass.
- **Failure:** any recurrence after revision blocks commit. A rule with no detector is reported as unverified in the same report, not omitted.
- **Threshold:** a rule earns a probe once it has recurred twice after being fixed. Below that, fixing it again is cheaper than instrumenting it.

The principle behind it: an unfalsifiable rule is not a rule. If there is no observation that could show a rule has failed, the rule is a wish.

The review also set a hard ordering constraint. At the time, roughly forty percent of the existing reconciliation checks were red. Adding a measurement layer on top of a red suite is building a floor over a hole; the new alarms drown in the same fatigue. No judgment-quality layer until the structural checks are green.

## The rule budget

Rules have a ceiling. Past a certain count, the marginal rule has negative value: it bloats every agent's context, it is not enforced, and it teaches the agents that rules are optional. The workbench learned this when its engineering criteria were cut from twenty-eight to fourteen, and then applied the same cut to its own planning protocol the same day.

The order of operations when a new rule seems needed: delete an existing one, then demote one from resident to on-demand, then merge two, and only then add. Any new mechanism in the co-engineering design is capped at five in total; adding a sixth requires removing one first.

The same reasoning rejected a cost dashboard. An agent recording and judging its own spend is self-review. What was adopted instead is a hard token budget per chain that stops the chain when exceeded: a kill switch, not a chart.

## The inbox: capture is free, sorting is a rhythm

The cost of tidying is what kills capture, so capture does not tidy. Chat pastes and voice memos land in `inbox.md` as they came. Sorting is a separate step the person triggers ("/inbox"), and its output is a changeset: one card per item with the source fragment, the destination, create-or-merge, and the links to add. The person approves all at once or edits item by item. It is a pull request to the vault.

| What arrived | Where it goes | What the AI may do |
|---|---|---|
| My own thought | `notes/`, verbatim | add frontmatter, a one-line title, wikilinks |
| Other people's content (an article, a screenshot) | `raw/` as received; compiled into `wiki/` | write the compiled page, cite the raw original |
| A thought about running work | that project's card or session | attach it; never put it in the wiki, which is for stable concepts |
| Something I learned | a spark candidate | draft the core line; I write or confirm it |

## Author on two axes

Every file carries `author: me / co / ai`. In practice the field splits into two: `input` says whose idea it was, `words` says who wrote the text. My sentence touched up by the AI is `me / ai`; a card drafted by the AI from the ledger is `ai / ai`; a spark whose core line I wrote myself is `me / me`. The split exists so the vault can be filtered on either axis, and so my own words and generated words never blur. Whether three values are enough is one of the open decisions in [02](02-architecture.md).

## Chronicle and Spark

The Chronicle is the record of everything: a ledger appended by hooks, one card per session, and the capsules cut out as side quests. It judges nothing, and it can be rebuilt from the JSONL. Because every host leaves the same card, it is also the foundation for working with other people's AIs later.

Spark is the learning layer on top: one file per thing understood, with a core line written by the person and a maturity state (new → known → promoted to a rule in `program/`). The spark index is generated from frontmatter and injected at session start, so a learning session can open with "what you already know" instead of starting from zero. Spark is tool-neutral markdown; nothing in it depends on which AI wrote it. The quest log, the running list of what is open and what branched off, is the bridge between the two.

## Three beats of governance

Checks run at three rhythms. Write time, by hook, sees one file and blocks on schema, forbidden targets and path literals. Commit time sees the whole diff and blocks on cross-file consistency and review size. A periodic sweep sees the whole repository and the ledger, finds orphans, stale pages and rule-to-enforcer drift, and is the only beat with a person in it and the only one allowed semantic judgment. [06](06-self-knowledge.md) has the full table.

## What runs in the private workbench today

Honesty about status, as the reviews required. These are operating rules of the private workbench; the Asteria design in [02](02-architecture.md) adopts the tiering, the proposal model and write review, and leaves the rest as workbench practice.

- Running: tiered memory, the proposal model, hybrid retrieval with the golden-set check, write-time errata hooks, the advisory-theater and chronic-noise checks, commit-time heavy checks.
- Designed, partly installed: scope and confidence labels on memory entries, the abstention gate with logged reasons.
- Designed, not installed: the recurrence probe as a general mechanism (one rule has its own), the token kill switch.
