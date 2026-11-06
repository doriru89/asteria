# Asteria

**A personal agent workbench where agents do the volume and one human signs the decisions. The skeleton is a set of stable interfaces and one git folder of plain files; the AIs are replaceable flesh.**

Asteria is the design behind a private workbench built on Claude Code and run daily by one person: 135 skills, 37 subagent roles, and 2,100+ commits since March 2026. This repository is its public shell. It holds the bet, the architecture and the human-in-the-loop design, written in English from the working design records of September 2026 and the five expert reviews that preceded them. The code itself stays private, because it is wrapped around one person's memory, notes and work. The ideas are the transferable part.

**Start with the [interactive demo](https://doriru89.github.io/asteria/demo.html).** Eighteen steps, one fictional project, every screen names the file it reads. Then the [bet](docs/00-the-bet.md) for why, and the [human-in-the-loop design](docs/04-human-in-the-loop.md) for the question most teams are asking right now: what may an agent do on its own, and how does a human stay in charge without becoming a rubber stamp?

## The problem

A single agent on a single task is a solved problem. Dozens of agents over months, working on your own files and your own memory, is not. Three things go wrong, and they are the same three things that go wrong inside companies adopting agents:

- **You get pushed along.** The agent is faster than you, so it proposes the next step before you have judged the last one. Decisions get made by momentum.
- **You learn nothing.** Every decision is a chance to learn why. Under throughput pressure that chance is skipped, so the human's judgment stops improving while the system keeps getting more complex.
- **Work scatters.** Outputs land in chats, side files and half-finished plans, and nothing can be found or traced later.

Asteria is one person's working answer to all three.

## The thesis

> Asteria is a workbench for one person building with AI. It treats three failure modes: being pushed along, learning nothing, and work scattered everywhere. It makes three guarantees: everything is a plain, searchable, traceable file; any part can be swapped whole without shaking the rest; and before a decision is signed, the work pauses and the decision becomes something learned. Success is measured, not felt: daily work happens here, and the operator can say why any module exists.

Each guarantee maps to a mechanism, and each mechanism has a check that can fail. The thesis also carries its own sunset conditions. If the operator's daily work drifts back out of the workbench, that counts as the thesis being wrong, not as a discipline problem.

## Six ideas that carry the design

1. **Agents do the volume, a human signs the decisions.** Agents research, draft, build and verify in loops. A small set of decisions is routed to the human through a triage gate, and the loop pauses there. Which decisions qualify is decided by rule, not by feel: how often the decision will recur, how much it touches, and whether it can be undone. [docs/04](docs/04-human-in-the-loop.md)

2. **Skeleton and flesh.** Three folders (`program/` settings written once, `chronicle/` the ledger and one card per session, `vault/` the notes), two hooks that do the defaults, three skills that do the deliberate. Files are the only truth, views only read files, every AI write to the vault arrives as a changeset, and every file says whose idea and whose words it holds. [docs/02](docs/02-architecture.md)

3. **Errata are checks, not advice.** Every failure that repeats becomes an errata entry (70+ so far) and a check that runs at write time and blocks. A warning that cannot block is treated as theater: in practice a blocking check fires every time and an advisory one changes nothing. [docs/03](docs/03-memory-and-errata.md)

4. **Producer is never verifier.** The agent that wrote something does not grade it. Verification is done by a test run, a fresh context, a different model or a human: something that holds a signal the producer did not have. [docs/05](docs/05-simplicity-and-search.md)

5. **Simplicity is a goal, never a criterion.** "Is this simple enough?" is banned as a gate question because it invites premature simplification. Gates only ask questions a command can answer: diff size, exit codes, check counts. Simplicity is read off afterwards, as a lagging indicator. [docs/05](docs/05-simplicity-and-search.md)

6. **The system measures what it loads.** A rule exists only if some prompt loads it. Load traces are coverage for prompts, in-edges are derived from readers instead of hand-written, and a rule with no readers fails an orphan gate. Prose may hold only where authority lives, how to query it, why, and the invariants. [docs/06](docs/06-self-knowledge.md)

## Module map

One coordinate system. The layer number says where something lives; the organ or port name says which one it is. The flesh plugs in at L2.

```
L0  Constants   eight invariants · program/ is the one source of settings · the interface list
L1  Organs      memory · recall · immune · spine · structure
L2  Ports       CODE · RESEARCH · KNOWLEDGE · ANALYSIS · STUDIO · INSPIRATION
L3  Data        the vault · each host's transcripts · work items · outputs
```

On disk it is three folders:

| Folder | Holds | Written by |
|---|---|---|
| `program/` | the one-line intent rule, preferences, the search protocol, the three skills | the person, once; a script distributes it into every host's entry files |
| `chronicle/` | `ledger.jsonl` appended by hooks; `cards/` one per session; `briefs/` side-quest capsules | hooks and a cheap model; the person edits one line |
| `vault/` | `inbox.md`, `notes/`, `wiki/`, `sparks/`, `decisions/`, `raw/` | the AI, through changesets the person approves |

| Organ | Job | One-line contract |
|---|---|---|
| memory | cards, sparks, wiki, notes | the person's words and generated words never blur |
| recall | inject → index and grep → knowledge graph → code graph → vectors when needed | cheapest first; in the private workbench retrieval is graded against a 65-query golden set as a blocking check |
| immune | tiering, write review, wiki lint, premise audit | a repeated failure becomes a blocking check |
| spine | the Chronicle: three hooks, one ledger, one card per session | any document leads back to its conversation in thirty seconds |
| structure | one frontmatter for every file | `type`, `author`, `project`, `source_session`; in-edges are derived, never hand-registered |

## Documents

| Page | What it covers |
|---|---|
| [Demo](https://doriru89.github.io/asteria/demo.html) | Eighteen interactive steps: a session, a side quest, the inbox as a pull request, the gate, the Chronicle, the load trace |
| [00 · The bet](docs/00-the-bet.md) | Two compounding curves, what AI makes cheap, three questions instead of a ladder, precedents, three open contradictions |
| [01 · Thesis](docs/01-thesis.md) | The problem, the three guarantees, measurable success, sunset conditions |
| [02 · Architecture](docs/02-architecture.md) | Skeleton and flesh: eight invariants, three folders, the recall stack, the smart inbox, the ritual, seven open decisions |
| [03 · Memory and errata](docs/03-memory-and-errata.md) | Tiered memory, the inbox as a pull request, author on two axes, Chronicle and Spark, errata as blocking checks |
| [04 · Human in the loop](docs/04-human-in-the-loop.md) | Recurrence is the gate, the pause protocol, five human verbs, changeset trust levels, evidence first |
| [05 · Simplicity and search](docs/05-simplicity-and-search.md) | Why simplicity is a lagging indicator, diverge/converge/decide, producer ≠ verifier, /idea, the feedback ladder |
| [06 · Self-knowledge](docs/06-self-knowledge.md) | Three kinds of edge, load traces as coverage, skills without a registry, a taxonomy of facts, prosthetics |

## What this repository is not

- It is not the workbench. The code, memory and notes stay private.
- It is not a framework to install. Several ideas transfer; the implementation is one person's.
- It is not finished. The design was reviewed from five angles (product, architecture, adversarial critic, evaluation, human factors) in August 2026 and restated as skeleton and flesh in September 2026. Seven decisions are still open and the demo says which answers it assumed. Mechanisms that run only in the private workbench are marked as such.

## Author

Austin Zheng · [portfolio](https://doriru89.github.io/az-portfolio/) · [GitHub](https://github.com/doriru89)

## Licence

MIT. Use the ideas, the structure and the text freely; attribution is all that is asked.
