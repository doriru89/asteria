# 02 · Architecture

*Distilled from the framework draft (v2, August 2026) and the architecture review.*

## One vocabulary

The first blueprint described the same parts three ways: as organs, as stars and constellations, and as numbered layers. Three mapping tables for one system is the opposite of elegance, because it adds to what a reader must hold in their head. The framework killed the astronomy metaphor and kept a two-field coordinate: the layer number says where something lives, the organ or port name says which one it is.

```
L0  Constants   the skeleton: one config owns every path
L1  Organs      memory · recall · immune · spine · structure
L2  Ports       CODE · RESEARCH · KNOWLEDGE · ANALYSIS · STUDIO · INSPIRATION
L3  Data        notes vault · session transcripts · work items · generated outputs
```

Dependencies point downward only. A port may import an organ; an organ may read constants; nothing imports sideways, and nothing in L1 knows what L2 does with it.

## The five organs

| Organ | What it is | Why it is its own thing |
|---|---|---|
| **memory** | A tiered store: session, project, persistent. Read at query time. | The persistent tier is the one place where an agent's write outlives the session, so it gets the strictest gate: proposals only. |
| **recall** | Hybrid retrieval over memory and notes, lexical plus vector. | Retrieval quality is graded against a golden set as a blocking check. If the grade drops, commits stop. |
| **immune** | Hooks plus errata. Runs at write time and can block. | It is the push channel. Memory is pulled when asked; immunity is pushed whether or not anyone asked. Merging the two would lose the push. |
| **spine** | The session rhythm: plan, review, capture, handoff. | Without it there is no relay between sessions, and the chronicle has no raw material. |
| **structure** | The graph of what exists and what points at what. | Out-edges are written by authors (true when written). In-edges are derived by the doctor (true globally, changes as the graph changes). Hand-written in-edges always drift. |

The architecture review made one ruling that is easy to get wrong: errata and memory are not one module. They differ in delivery timing. Memory is pulled at query time, which fires when something asks. Errata are pushed at write time, which fires every time. A merged "knowledge store" would quietly drop the push channel and turn every hard rule into advice.

## The six ports

Ports are the surfaces where work happens: CODE, RESEARCH, KNOWLEDGE, ANALYSIS, STUDIO and INSPIRATION. Each port is a set of skills and agents that consume organs and produce outputs in L3. CODE and RESEARCH are core because nothing else can be built without them. The other four are secondary: losing one loses a category of output, and the system still boots.

Two things the operator asked for turned out not to be ports at all. "Hook architecture by lifecycle" is a field on the immune organ, not a new axis. "Skill and agent management with periodic review" is one line in the prune report, not a module. The review's rule: before adding a module, ask which existing organ already owns the judgment.

## The six laws

1. **Dependencies only point downward.** Enforced by an import-graph test, not by a sentence in a document.
2. **Paths live only in config.** A path literal anywhere else is a defect.
3. **Every contract ships with a check.** A contract without a check is documentation.
4. **Everything enters through the admission gate.** New skills, agents and hooks are resolved for closure before they are wired in.
5. **Interfaces are thin and late-bound.** Parts talk through files and config, not through shared code.
6. **Immunity is per organ.** Each organ carries its own hooks and errata, so replacing one does not disarm the others.

## The six governance devices

| Device | Job | Status at review |
|---|---|---|
| doctor | Checks structural integrity against every contract | planned as the first build step |
| admission resolver | Resolves closure for anything entering the system | in the blueprint |
| prune report | Periodic push of what has stopped earning its place | in the blueprint |
| errata loop | Failure becomes entry becomes check | running |
| rule-to-enforcer reconciliation | Asserts that every declared rule has a live enforcer | added by the review |
| recurrence probe | Measures whether a fixed rule stays fixed | added by the review |

The fifth device came out of the review's single most important finding, described next.

## Stable versus volatile, and the leak

The operator's claim: the stable layer is governance and the shape of the data; the volatile layer is hooks and tools; the architecture's value is letting the volatile layer be replaced without shaking the stable one.

The review tested it and found the dependency runs the other way.

```
Assumed:   stable layer ──(independent)──  volatile layer      swapping is safe
Actual:    stable layer ──(only while)──>  some blocking hook exists in the volatile layer
                                           otherwise it silently decays to prose
```

Concretely, a data-shape rule in the constitution said "every link an agent writes must pass an existence check, enforced by hook X." The blueprint had marked hook X for removal, on the grounds that no incident had ever been traced to it. Had that removal gone through, the rule would still have been in the constitution and would have stopped being a rule. A blocking check fires every time; a rule with no check fires never. The failure mode of "swap a part" in the governance layer is not an error. It is a rule evaporating in silence.

The fix was cheap and already implied by the third law: every stable-layer rule gets a declarative entry in config and an enforcer in the hook layer, and the doctor asserts the two match one-to-one. A rule that still exists while its enforcer is gone is a failure. Only then are the parts really swappable.

A second finding from the same review: the blueprint's hand-written table of hooks had drifted from reality within weeks, by roughly one entry every four days. Hand-written inventories of live things always rot. The blueprint should be a projection of what the doctor finds, not a document someone keeps in sync.

## Consumption: three levels of disclosure

The operator wanted a blueprint that is elegant but can be consumed without reading all of it every time. The answer is three levels, each with its own stability promise.

| Level | Contents | Size | When read | Stability |
|---|---|---|---|---|
| Resident | Thesis, four-layer coordinate, six laws, device list | one page | every session | almost never changes; changes are amendments |
| Contract face | Per module, about ten lines: inputs, outputs, success criterion, what it assumes others provide | two pages | when work crosses a module boundary | the only reporting layer |
| Module card | Full text of one module plus its teaching fields | as needed | only when entering that module | changes freely |

The rule that falls out: a change at the contract level wakes the operator; a change at the card level does not. The same boundary decides what an agent may keep doing while a decision is paused (see [04](04-human-in-the-loop.md)).

## Elegance as a test, not an adjective

Four acceptance questions turn "elegant" into something that can fail.

1. **Honest primitives.** List every primitive (five at most). Rename and move ten random cross-module references. All ten must still resolve, and each primitive's identity must come from content or an immutable id, never from a path or filename.
2. **Few, general composition rules.** Count the allowed ways to connect parts (five at most). Find three combinations nobody enumerated at design time and run them without editing any central file.
3. **Traceable results.** Pick five numbers the system produced. For each, walk back within two hops to a rerunnable command and a commit. Rerun; the result must match.
4. **A borrowed mental model.** Give someone who has never read the blueprint only the vocabulary and ask them to predict five things: where a feature goes, how two ports share code, what to check first when something breaks, what never changes, who may change it. Four of five correct, without reading the implementation.

At the time of the review, the fourth test was failing because of the three-vocabulary problem described at the top of this page. That is why vocabulary pruning was the first thing done.
