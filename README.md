# Asteria

**A personal agent workbench where agents do the volume and one human signs the decisions.**

Asteria is the design behind a private workbench built on Claude Code and run daily by one person: 135 skills, 37 subagent roles, and 2,100+ commits since March 2026. This repository is its public shell. It holds the product thesis and the architecture, written in English from the working design documents and the five expert reviews that shaped them. The code itself stays private, because it is wrapped around one person's memory, notes and work. The ideas are the transferable part.

If you came here from the portfolio, start with the [thesis](docs/01-thesis.md), then the [human-in-the-loop design](docs/04-human-in-the-loop.md). Those two answer the question most teams are asking right now: what may an agent do on its own, and how does a human stay in charge without becoming a rubber stamp?

## The problem

A single agent on a single task is a solved problem. Dozens of agents over months, working on your own files and your own memory, is not. Three things go wrong, and they are the same three things that go wrong inside companies adopting agents:

- **You get pushed along.** The agent is faster than you, so it proposes the next step before you have judged the last one. Decisions get made by momentum.
- **You learn nothing.** Every decision is a chance to learn why. Under throughput pressure that chance is skipped, so the human's judgment stops improving while the system keeps getting more complex.
- **Work scatters.** Outputs land in chats, side files and half-finished plans, and nothing can be found or traced later.

Asteria is one person's working answer to all three.

## The thesis

> Asteria is a workbench for one person building with AI. It treats three failure modes: being pushed along, learning nothing, and work scattered everywhere. It makes three guarantees: everything is a plain, searchable, traceable file; any part can be swapped whole without shaking the rest; and before a decision is signed, the work pauses and the decision becomes something learned. Success is measured, not felt: daily work happens here, and the operator can say why any module exists.

Each guarantee maps to a mechanism, and each mechanism has a check that can fail. The thesis also carries its own sunset conditions. If the operator's daily work drifts back out of the workbench, that counts as the thesis being wrong, not as a discipline problem.

## Five ideas that carry the design

1. **Agents do the volume, a human signs the decisions.** Agents research, draft, build and verify in loops. A small set of decisions is routed to the human through a triage gate, and the loop pauses there. Which decisions qualify is decided by rule, not by feel: how often the decision will recur, how much it touches, and whether it can be undone. [docs/04](docs/04-human-in-the-loop.md)

2. **Tiered memory, with a proposal model for the top tier.** Session memory is disposable, project memory is working state, persistent memory is canon. Nothing writes to canon directly. Agents file proposals; the human approves or rejects them in a review pass. [docs/03](docs/03-memory-and-errata.md)

3. **Errata are checks, not advice.** Every failure that repeats becomes an errata entry (70+ so far) and a check that runs at write time and blocks. A warning that cannot block is treated as theater: in practice a blocking check fires every time and an advisory one changes nothing. [docs/03](docs/03-memory-and-errata.md)

4. **Producer is never verifier.** The agent that wrote something does not grade it. Verification is done by a test run, a fresh context, a different model or a human: something that holds a signal the producer did not have. [docs/05](docs/05-simplicity-and-search.md)

5. **Simplicity is a goal, never a criterion.** "Is this simple enough?" is banned as a gate question because it invites premature simplification. Gates only ask questions a command can answer: diff size, exit codes, check counts. Simplicity is read off afterwards, as a lagging indicator. [docs/05](docs/05-simplicity-and-search.md)

## Module map

One coordinate system. The layer number says where something lives; the organ or port name says which one it is.

```
L0  Constants   one config file owns every path; nothing else may hold one
L1  Organs      memory · recall · immune · spine · structure
L2  Ports       CODE · RESEARCH · KNOWLEDGE · ANALYSIS · STUDIO · INSPIRATION
L3  Data        notes vault · session transcripts · work items · generated outputs
```

| Organ | Job | One-line contract |
|---|---|---|
| memory | tiered store, pulled at query time | persistent writes are proposals |
| recall | hybrid retrieval, BM25 plus vectors | graded against a 65-query golden set as a blocking check |
| immune | hooks plus errata, pushed at write time | a repeated failure becomes a blocking check |
| spine | plan, review, capture | every session ends with a handoff the next one can resume from |
| structure | the graph of what exists | out-edges are written, in-edges are derived, never hand-registered |

Six laws govern how parts connect: dependencies only point downward; paths live only in config; every contract ships with a check; everything enters through an admission gate; interfaces are thin and late-bound; immunity is per organ. Six governance devices enforce them: a doctor that checks structure, an admission resolver, a prune report, the errata loop, a rule-to-enforcer reconciliation, and a recurrence probe. [docs/02](docs/02-architecture.md)

## Documents

| Page | What it covers |
|---|---|
| [01 · Thesis](docs/01-thesis.md) | The problem, the three guarantees, measurable success, sunset conditions |
| [02 · Architecture](docs/02-architecture.md) | Four layers, five organs, six ports, six laws, six governance devices, stable versus volatile |
| [03 · Memory and errata](docs/03-memory-and-errata.md) | Tiered memory, the proposal model, retrieval grading, errata as blocking checks, the recurrence probe |
| [04 · Human in the loop](docs/04-human-in-the-loop.md) | The triage gate, the pause protocol, trust levels per decision class, the evidence-first rule |
| [05 · Simplicity and search](docs/05-simplicity-and-search.md) | Why simplicity is a lagging indicator, diverge/converge/decide, producer ≠ verifier, the rule budget |

## What this repository is not

- It is not the workbench. The code, memory and notes stay private.
- It is not a framework to install. Several ideas transfer; the implementation is one person's.
- It is not finished. The design was reviewed from five angles (product, architecture, adversarial critic, evaluation, human factors) in August 2026, and some reviewed mechanisms are designed but deliberately not yet installed. Each document says which.

## Author

Austin Zheng · [portfolio](https://doriru89.github.io/az-portfolio/) · [GitHub](https://github.com/doriru89)

## Licence

MIT. Use the ideas, the structure and the text freely; attribution is all that is asked.
