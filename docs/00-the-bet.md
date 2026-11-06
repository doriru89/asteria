# 00 · The bet

*From the thesis lecture of September 2026. Claims marked "verified" had two or more independent sources at the time; "judgment" is the author's reasoning.*

## Two compounding curves

Compounding means this round's result becomes next round's capital. Working with AI every day grows two kinds of capital.

| | AI knows you better | You get stronger |
|---|---|---|
| Where the capital lives | On the platform's servers: the assistant's memory of you, the connectors it holds | In your head: judgment, the ability to tell good from bad, what you can now do without help |
| Can you take it with you | Partly. Memory import between assistants exists, but it is one-time and lossy | You do not need to. It follows you to any tool |
| Who has an incentive to grow it | Every platform, because the more you depend on it the less you leave | Almost nobody, because it runs against retention |

Most products climb the first curve. Asteria is a bet on the second: a workbench that handles daily work and life with AI while making sure what was learned actually lands in the person, and keeps landing. It is built for people who are not content to be fed results and want to stand at the front of their own field. Writing code is only the simplest case.

The honest evidence cuts both ways (verified):

- A twelve-week study of 240 students taught one group to hand low-level writing tasks to AI and keep the high-level thinking. That group's critical-thinking gains were larger.
- A 120-person randomised trial found that students who learned with AI assistance retained less on a surprise test 45 days later (57.5% against 68.5%). An EEG study of essay writing found the lowest brain engagement in the group using a chatbot, and participants who could not recall what they had written. The researchers called it cognitive debt.

AI itself does not decide whether you get stronger or weaker. What decides is where the saved effort goes. Asteria's real job is to decide, on your behalf and by rule, which moments are worth your own effort and which go to the AI.

## What AI makes cheap, and what stays expensive

If something becomes cheap, everyone has it and it stops being anyone's advantage. So the place to spend effort is where things stay expensive.

The thesis author's original claim was that AI makes compatibility easy. Half true. Technical compatibility, writing the adapter, is approaching zero cost: open protocols for connecting AI to tools reach tens of millions of downloads a month, and one small team wrote adapters for six coding agents and put them behind one interface. Permission compatibility, whether the other side lets you in, is getting more expensive: an iMessage bridge was shut down within days of launch; a coding tool lost direct access to a model provider with under five days' notice; a workplace chat platform rewrote its API terms to forbid bulk export, indexing and feeding data to language models. Data is now the raw material of AI, and every platform is closing the door.

| Getting cheap | Still expensive, and who holds it |
|---|---|
| Interfaces: two leading agent desktops already look nearly identical | Authorisation and trust: what the user lets the AI touch (held by the few companies people already trust) |
| The AI's memory of you: importable, viewable, editable | The entry point: the app you open every day anyway (held by the large platforms) |
| Doing things for you: agents that drive a browser or a desktop | Attention: how much one person can seriously read in a day (held by each person) |
| | What is in your head: cannot be exported or installed, only grown (held only by you) |

The consequence for Asteria: keep the data in your own files on your own machine, and choose external dependencies that nobody can switch off: your own files, official command-line tools, open protocols. The AIs are replaceable suppliers.

## Three questions instead of a maturity ladder

A layered framework forces completeness and invites form-filling. For a personal AI product three questions are enough: who is it for, what accumulates, and who owns what accumulates.

| Product | For whom | What accumulates | Owned by |
|---|---|---|---|
| A large social platform's assistant | Everyone, people who do not want effort | The AI's knowledge of you and the things it did for you | The platform |
| An enterprise work agent | Office workers | The workflows it has run and the office software it can reach | The vendor |
| A frontier assistant with memory and connectors | Knowledge workers | The AI's knowledge of you | The vendor, exportable |
| Anki | People facing a high-stakes exam | What you have memorised | You |
| **Asteria** | People who want to grow and stand at the front | Your judgment, plus the context you hold yourself | You |

Side by side, Asteria is the only row that is both for people with intent and owned entirely by the user. Anki is the nearest neighbour; it manages memory but not judgment.

## Precedents for building for people who want to grow

- **Anki** (verified): roughly three million active users on the Android client alone; close to seventy percent of medical students use it. The counter-example is the same fact: they use it because an exam forces them. Self-directed adults mostly stop after a few weeks. Lesson: hardcore users scale when there is a reason that forces effort. For Asteria that reason may be "I have an important decision to make".
- **Obsidian** (single source for the numbers): a team of about ten, no venture money, profitable on sync and publish. The counter-example is that much of its growth came from a plugin ecosystem that is hard to copy. Lesson: hardcore users pay, and pay steadily. Asteria does not need mass scale to stand.
- **Roam** (verified): a seed round at a valuation around twenty-five times the median of its year, a cult hashtag, then slowing development, a paid-only tier and data held on its own servers. Users drifted to Obsidian and Logseq. Lesson: a hardcore community's excitement does not retain it; steady iteration and confidence in one's own data do.
- **Tools for thought** (verified): a well-known 2019 essay asked why the technology industry barely invests in tools that strengthen thinking. The counter-example is a language-learning app that reached mass scale by making effort feel like play. Lesson: the need is real and the business is hard. Serve a few people well first, starting with yourself.
- **Logseq's rewrite** (verified): moving from files to a block database took over two years and cost the freedom to edit notes with any editor, in exchange for typed blocks and validated properties. Obsidian stayed with files and an index beside them, and thrived. Lesson: storage decides which features are easy, not who wins. In the AI era, files gain one more advantage: models are already good at reading and writing them. Asteria borrows the rule "a tag is a type, a property has a format" for its frontmatter and keeps files as storage.

One more observation (judgment): the people in the AI world who want to improve are also the fastest to switch tools. Keeping them means never locking their accumulation inside any tool. Because they can leave at any time, they are willing to stay.

## Three contradictions left open

1. **The market is small.** The tools-for-thought essay and Roam's history both say thinking tools are hard to make into large businesses. Anki and Obsidian are the only two precedents that hold.
2. **The target users switch tools fastest.** Keeping them requires portable accumulation, which also makes the product itself easier to replace.
3. **The author does not want effort everywhere either.** If "the moments worth your effort" are picked badly, Asteria becomes one more burden. The triage gate in [04](04-human-in-the-loop.md) is the answer, and it is the part most likely to be wrong.

The one-line conclusion: Asteria bets on the human side of compounding, the curve the large platforms will not build because it runs against their retention logic. It can stand in three places: picking the moments worth your effort, keeping your context in your own hands, and treating the AI as a supplier you can swap at any time.
