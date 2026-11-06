# 02 · Architecture: skeleton and flesh

*From the architecture decision record of 26 September 2026, which superseded the August framework draft and the sidecar design that preceded it. The [interactive demo](../demo.html) walks through the same design in eighteen steps.*

## One sentence

Asteria is the skeleton for co-engineering with AI: a set of stable interfaces, one git folder, and a front end the person builds themself. The AIs and tools are flesh, replaceable at any time. It connects sessions scattered across different AIs, the files those sessions produced, and the person's own knowledge, so the person can read it, learn from it, and turn one into two.

## Purpose, in priority order

1. **Co-engineering.** Use AI to break information barriers, learn to make trade-offs, pick the right tools, build good products.
2. **Legibility across sessions.** Several finished things may be one thing; one problem may have been split into several. Branches that went off to learn must come back.
3. **Learning (Spark).** AI amplifies the person's judgment, so the judgment has to keep improving.
4. **Zero-friction capture.** A thought pasted from a chat or spoken into a voice memo must not rot in a corner because sorting it costs too much, and must be reusable later.
5. **Self-awareness.** Asteria can read itself (its files, rules, code) and can fetch the code context of a project it is working on.
6. **Visual craft.** Nice to have; kept on a separate ledger.

What Asteria builds itself: the thin features the person wants, and the front end. Capture, retrieval and models are borrowed wherever something exists.

## The coordinate: L0 to L3

The layer number says where something lives; the name says which one it is. Dependencies point downward only.

```
L0  Constants   the skeleton: eight invariants, program/ as the single source of settings, the interface list
L1  Organs      memory · recall · immune · spine · structure
L2  Ports       CODE · RESEARCH · KNOWLEDGE · ANALYSIS · STUDIO · INSPIRATION   (the flesh plugs in here)
L3  Data        the notes vault · each host's transcripts · work items · outputs
                L3 returns to L1 through the spine
```

## L0 · The eight invariants

| # | Invariant | Meaning |
|---|---|---|
| 1 | **Files are the only truth** | Everything is markdown with frontmatter in one private git repository. Change it in one place and it changes everywhere. |
| 2 | **The skeleton defines interfaces, not implementations** | Which tool sits behind each interface can change any day. |
| 3 | **Structure at write time, not read time** | The closing card is drafted by the AI that did the work, at the end of the session while the context is fresh. Later sessions read cards, never raw transcripts. |
| 4 | **Vendor details stop at the adapter** | Hook names, paths and payload formats differ per host; only the adapter layer knows. |
| 5 | **Views only read files** | The front end can be rewritten forever and holds no data of its own. |
| 6 | **The human signs; divergence and convergence are human-triggered** | The AI proposes and stops. |
| 7 | **AI writes are visible** | Every AI write to the vault arrives as a changeset the person reviews (batch approval allowed). |
| 8 | **Provenance is legible** | Every file's frontmatter carries `author: me / co / ai`. The person's own words and generated text never blur. |

### program/ · one source for all settings

The one-line intent rule, the search protocol, preferences, tool configuration and the skills are written once, in `program/`, and a generator distributes them into each host's entry file (the Claude Code instructions file, the agents file), each host's skills directory and the MCP configuration. Entry files are written only by the script, never by hand.

```
edit program/ once (with a decision record) → generator → entry files · skills · MCP config → every tool's next session
```

## L1 · The five organs

**memory · what is remembered**

| Store | Author | Contents |
|---|---|---|
| `chronicle/cards/` | co (AI drafts, the person confirms) | One card per session: decided / still open / next / learned |
| `vault/sparks/` | co (the person writes the core line) | What I understood, with a maturity path: new → known → promoted to a rule in `program/` |
| `vault/wiki/` | ai | Concept pages compiled from outside material (the LLM-wiki pattern) |
| `vault/notes/` | me (verbatim) | My own thoughts sorted out of the inbox; the AI adds only frontmatter and links |

**recall · how it is found again.** Cheapest first, most expensive last:

1. **Inject.** A new session starts with the last few cards of the same project plus the spark index.
2. **Index and grep.** `_index.md`, the wiki index, the vault's command-line interface. At personal scale this layer is usually enough.
3. **Knowledge graph.** A graph tool run over the Asteria repository itself, so it can read its own rules and the relations between its files.
4. **Code graph.** A code-graph tool run over a project repository, for code context while building.
5. **Vector retrieval.** Added only when the scale really demands it.

**immune · what is defended against.** Tiering (the most sensitive material lives outside the repository and is deny-listed for AI reads); write review (invariant 7); a periodic wiki lint for contradictions, stale pages and orphans; and a premise audit that keeps claims and verified facts apart.

**spine · the Chronicle.** Three hooks (SessionStart, Stop, PostToolUse) append one line each to `chronicle/ledger.jsonl`: session, agent, event, file path, time. Claude Code and Codex expose hooks of the same names; only the adapter handles payload differences. Transcripts are not copied: each host stores its own locally, and the ledger records the path. Provenance (files created, files modified) is derived from the ledger; a document's frontmatter carries `source_session`, so opening a new session against a document finds the conversation that produced it at once. The person opens projects; sessions with no project go to an inbox, archive after thirty days, and can be pulled back. Hosted assistants with no file access write cards only through one narrow MCP door.

Chronicle is the record of everything and can be rebuilt from the JSONL. It is also the foundation for working with other people's AIs later: a session on any host leaves the same card. Spark is the learning layer on top, and the quest log is the bridge between them. Spark is tool-neutral markdown; nothing in it depends on which AI wrote it.

**structure · one format.**

```yaml
# the minimal frontmatter every markdown file shares
type: session | spark | note | wiki | decision
author: me | co | ai
project: artopath            # may be empty
topics: [[domain-a]], [[domain-b]]
source_session: 2026-09-26-xxx
created: 2026-09-26
```

Links are wikilinks. Vault-specific syntax follows the vault's published conventions so the relationship graph is never broken.

## L2 · The six ports

| Port | In / out | Current flesh |
|---|---|---|
| **CODE** | write code, change projects | Claude Code, the Codex app, plus a code-graph tool |
| **RESEARCH** | search, read the web, verify | web search, browser automation, the premise audit |
| **KNOWLEDGE** | store and compile knowledge | the vault, the smart inbox (below) |
| **ANALYSIS** | analyse, compare, decide | Claude Code or a hosted assistant, with one-sentence intent, fan-out and merge |
| **STUDIO** | design and prototypes | a design tool; parallel prototypes later |
| **INSPIRATION** | loose ideas coming in | chat pastes and voice memos → `inbox.md` |

Rule for a new port: write down what comes in and what goes out before choosing the flesh.

## L3 · Data

| Name | What | Where |
|---|---|---|
| the vault | the person's notes | the vault folder |
| transcripts | each host's raw conversations | each host's local store; the ledger holds the path |
| work items | tasks and boards | to be confirmed |
| outputs | files a session produced | the project repositories, linked to sessions by the ledger |

## The smart inbox: Spark's capture end

Goal: the person pastes and speaks freely; the AI splits, groups by topic, files and links; the person sees all of it.

```
chat paste · voice memo → inbox.md (verbatim)
  → "/inbox" → AI splits · groups · finds existing notes → changeset (review view)
  → approve / edit / return →  notes/ (my thoughts, verbatim)
                               wiki/ (compiled from outside material; original kept in raw/)
                               project or Chronicle (thoughts about running work)
                               sparks/ candidate (I confirm the core line)
```

Routing rules: my own thoughts go to `notes/` verbatim, with only frontmatter, wikilinks and a one-line title added. Other people's content goes to `raw/` as received and is compiled into `wiki/`. Thoughts about running work attach to that project or session, never to the wiki, because the wiki is for stable concepts, not changing work context. Things I learned become a spark candidate whose core line I write or confirm.

The review view is the first front end Asteria builds: one card per item showing the source fragment, the destination, create-or-merge, and the links to add. Approve all with one click or edit item by item. It is, in effect, a pull request to the vault.

## The ritual

```
me   → AI       start: one line of intent (the AI names the session)
files → AI      inject: recent cards · spark index · program settings
                (I trigger divergence or convergence when needed)
AI   → files    hooks record: session, touched files
AI   → me       end: draft the closing card
me   → files    change one line, confirm; if something was learned, one spark
```

The inbox runs on its own rhythm: throw things in at any time, say "/inbox" periodically, review, write.

## The front end, thin first

| Stage | Experience | Reads |
|---|---|---|
| First | Inbox review: split → destination → links, one-click approve | `inbox.md`, the AI's changeset |
| Second | Sessions and outputs: what a session created and modified; jump from a file back to its session; cards across sessions | `ledger.jsonl`, `cards/` |
| Later | Star map and timeline, side-quest view, reactive documents | the same files |

Editing and preview borrow the vault app and the hosts' own side panels. Asteria builds only the cross-session part they lack.

## The sidecar that came before

The previous design record put a sidecar process at the centre: it owned the files and the event log, never the terminal or the agent. Adapters did three things (observe, inject, act), the event log was append-only, and every file had a schema. Of that design, Asteria keeps the division of ownership, the adapter signatures and the append-only ledger, and defers the twelve-event model, the raw-versus-semantic layering and the full authorisation matrix. The five human verbs it introduced (confirm, choose, ask, merge, approve) are described in [04](04-human-in-the-loop.md).

## Kept, changed, deferred

| | |
|---|---|
| **Kept** | Files are the only truth; vendor details stop at the adapter; sessions have a head and a tail; projects plus inbox plus thirty-day archive; the spark index and maturity path; the relationship graph belongs to the vault app; `program/` generates entry files; hosted assistants use one narrow door; sparks are confirmed by the person |
| **Changed in this version** | Dropped a commit-based session tracker (its unit is the git commit; Asteria's unit is the session and the document, and many knowledge sessions never commit) in favour of three thin hooks and a ledger; the front end promoted to a first-class citizen; added the smart inbox, the recall stack, the author field and write review; the whole design restated in the L0–L3 coordinate |
| **Deferred** | The twelve-event model; a host app-server adapter; selection anchors; reactive documents; the star map; twenty-agent parallel prototypes; the full authorisation matrix (only deny-read of the sensitive tier for now) |

## Seven open decisions

1. What exactly the vault and the work-item store are in L3.
2. My thoughts in the inbox: keep them verbatim with AI adding only structure (the default here), or let the AI rewrite them into tidy wiki pages.
3. Is `author` with three values (me / co / ai) enough?
4. Inbox rhythm: run only when I say so, or a daily changeset waiting for review.
5. Which front end first: inbox review, or sessions and outputs.
6. Should the closing card carry an "expected → actual" field, to train trade-offs.
7. Carried over: rename Chronicle? Are start and end hard gates or automatic defaults? What goes in the sensitive tier?

## Done criteria for the first month

- **Switching tools does not break anything.** The same task run in Claude Code and in Codex reads the same settings and the same cards.
- **Provenance.** Any document leads back to the conversation that produced it within thirty seconds.
- **Capture.** A week of chat pastes and voice memos all reach the inbox, and one review files them in under ten minutes.
- **Compounding.** At month end, four criteria are scored and decide the next front-end stage: future sessions are more relevant; answers fit me better; new things are explained better; I can review what I already know.

## Lessons kept from the August review

Two findings from the earlier blueprint's review still shape the design.

**A rule with no check evaporates silently.** The August blueprint had marked a hook for removal because no incident had ever been traced to it, while a constitutional rule still said "enforced by that hook". Had the removal gone through, the rule would have stayed in the text and stopped being a rule. The fix is the same in both designs: every stable rule has a declared enforcer, and a check asserts the two match. In Asteria this is why `program/` is generated, never hand-edited, and why invariant 7 is a review view rather than a sentence.

**Hand-written inventories of live things rot.** The blueprint's table of hooks drifted from reality by roughly one entry every four days. Nothing in Asteria is a hand-maintained list of what exists: the ledger is appended by hooks, indexes are generated from frontmatter, and the Chronicle views draw from the project and parent fields of the cards. [06](06-self-knowledge.md) takes this further.

Some mechanisms from that review (a recurrence probe for fixed rules, a block on advisory-only warnings) run in the private workbench today. They are operating rules of that workbench, not part of the Asteria design.
