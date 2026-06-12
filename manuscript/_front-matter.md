---
title: "Cover · Preface · How to Read This Book"
part: 0
status: v3
written: 2026-06-06
author: 이민수
version: v3
---

# AI Workflow for Game Designers

**No Fabricated Numbers — A Six-Month Field Manual for Claude Code, Prompts, Validation, and Production Memory**

The 304 rules, 48 tools, and automation systems that a director with 24 years in the industry actually ran on a mid-sized (10–50 person) team — exactly as they were used

By Minsoo Lee · 2026

> Everything in this book is current as of the **first half of 2026**. Pricing, models, and features of AI tools change quickly, so please check each official page for the latest figures and installation steps.

---

## Distribution and Copyright

This book is released free of charge, in the hope that it will be read widely. But whether you are reading the Korean original or an English or Japanese translation, one fact must stay attached to it everywhere: the original author of this book is **Minsoo Lee (이민수)**.

- **License**: Creative Commons **Attribution-NonCommercial-ShareAlike 4.0** (CC BY-NC-SA 4.0)
- **Original author**: Minsoo Lee (이민수), 2026
- **Canonical edition**: `https://eremes81.github.io/game-design-ai-practice` (the author's GitHub Pages — the canonical edition, under the author's editorial control)

**You are free to do the following.** Personal study, noncommercial sharing and quotation, noncommercial translation, and use in internal study groups — provided you credit the original author, include a link to the canonical edition, and state clearly if you have modified the content or carried it into another language. Derivative works must be shared under the same terms.

**Please ask for permission first.** Commercial publication or sale, use as course material in paid classes, inclusion in a company's products or services, and any redistribution that removes the author credit.

The knowledge in this book is complete in this one volume, and it is free as it stands. If it helps you and you would like to support the author, the official e-book and the paid kits are a way to do that.

---

## Preface — A Book Written Three Times

This book was written three times.

The first version was not a book at all — it was an **internal company manual**. Over six months of running an AI workflow at work, I pinned decisions, tools, and procedures down in writing so the team would never have to ask about the same rule twice. It was not made for publication; it was an operations document, built up to cut down on daily repetition. That is where this book's concreteness comes from — it is not based on examples invented for a book, but on a manual that was actually in service.

The second was the **first manuscript that turned that manual into a book**. It ended up as a plausible-sounding survey of "look what AI can do." It had a lot of tables, and a lot of numbers demonstrating impact. Most of those numbers carried a note in small print: "illustrative figures." When I reread it, that was its biggest flaw. It talked about using AI without ever showing a real screen, and it talked about impact while citing made-up numbers. In moving the manual into book form, I had managed to lose the very concreteness that made it worth writing.

So I wrote the whole thing again, a third time. That is the text you are holding now. I brought back the concreteness of the internal manual, and I kept the book's principles simple.

First, **every chapter shows you a real session from start to finish.** The full prompt I typed, the raw output the AI produced, what I rejected in that output, and how I made it redo the work — all of it is here. No chapter ends with the sentence "AI takes care of it."

Second, **every number is one of three kinds.** A public standard anyone can verify (model token pricing, accessibility guidelines), a constant actually present in my system's code, or a value explicitly labeled "this is my estimate." There is not a single fabricated savings table. I made honesty the differentiator.

Third, **I quote, as is, the real system I ran in production for six months.** The 304 decision cards (atoms), the 48 tools (skills), the hook that automatically pulls in relevant memory on every input, the memory structure that lets one person run four people's worth of collaboration context — all of it. Not an abstract "some tool," but the actual file names, code, and scores, written down as they are. In the worked examples I masked only company and project names and the names of teammates; the concreteness of the workflow is untouched. (The company that gave permission to publish this book is named openly in the acknowledgments — because they agreed to it.)

I am a game designer with 24 years in the industry. I got my start doing QA and review on single-player games, and went on to direct an MMORPG that ran in dozens of countries, to work on the early development of a 200-person AAA MMORPG, and to handle live ops for a global mobile MMORPG — a career spent building RPGs, MMORPGs, and their variants. On projects like Ragnarok Online, Bless Online, and the MIR series, I served as director, design lead, and systems designer, and at times as PM; I also once founded a small mobile game company.

To be honest, I was made a director too early. After that came a long stretch back in the trenches: working data sheets and combat numbers hands-on as a systems designer, producing quests and NPCs line by line as a content designer, planning events, churning out new content. Many of the workflows in this book were born in that seat — the seat where you get your hands dirty rather than manage — out of the simple wish to somehow cut down on the repetition. Today I lead a mid-sized (10–50 person) team as the design director of an MMORPG in active development, but the tools in this book did not come from a director's management kit; they came from a practitioner's hands. That is why the system in this book is not theory but a working environment that runs every day. The book also covers, in exactly the same way, one small puzzle game I built alone at home — quoting that game's actual git commits and code.

AI will not replace a game designer's job. What it does is free your hands from the busywork. What you do with those hands is still up to you. I hope this book serves as a practical guide to that transition.

---

## How to Read This Book

You do not have to read this book in order from cover to cover. Pick the route that fits your situation. If terminals and installation are new to you, open to **1.0, "Before You Begin,"** before anything else — it is the chapter that eases the fear of the black screen first.

| Route | Path | Who It Is For |
|---|---|---|
| The adoption route | 1.0 (setup) → Part 1 (adoption) → Part 2 (information architecture) → one part for your own discipline | Game designers just starting with AI tools |
| The full route | Parts 1–2 → discipline parts (3–15) → process (16–19) → operations (20–24) | Leads designing team-wide adoption |
| The indie/solo route | 1.0 (setup) → Parts 1–2 → Part 23 (personal game development) → each chapter's "Solo Scale-Down" | Developers building alone or as a hobby, with no team |
| The general-work route | Parts 1–2 → Part 17 (meeting notes) → Part 16 (collaboration) → Part 18 (decision-making) → Parts 21–22 (self-improvement and governance) | Designers, PMs, and office workers outside games |
| The problem-solving route | Appendix index → backward to the relevant chapter | Readers with a problem to solve right now |

Every chapter ends with a "Try It Yourself" section. The goal is not a chapter you read and close, but one that gets your hands moving at least one step in your own environment, today.

One more word for readers who work outside games. Many of this book's workflows — turning meeting notes into decisions, tracing a decision's ripple effects, verification gates (this book's term for a quality gate where a human or a checker verifies output), cost management, copyright and ethics — work as is, with no connection to games. **Feel free to read "game design" as your own job.** The "Beyond Games" box in each chapter is the bridge, and if you are short on time, the 90-minute express course (17.1 → 16.2 → 22.1 → 21.1) alone will let you feel the core skeleton with your own hands.

Let me draw one distinction here. In this book, "solo" is used in two senses. One is the **solo director** — a lead who carries several people's worth of collaboration context alone. The other is the **solo or hobbyist developer making a game by themselves**. The "Solo Scale-Down" at the end of each chapter is for the latter: it lays out how to take just that chapter's core with no team and no company folder.

You can read the book straight through as a single volume, or split it into two halves — Parts 1–15, "foundations and disciplines," and Parts 16–24, "process and operations" — and start with whichever you need. And most of the code in this book runs as is with no external dependencies, using only the Python standard library. Only a few tools, such as the relation graph, need packages beyond the standard library (networkx, PyYAML), and in those spots a one-line install (`pip install …`) sits right next to the code. Apart from those cases, there is nothing extra to download — you can copy a code block and run it right away to see for yourself.

If a term stumps you, do not stop there — move on for now. If the black terminal feels foreign, that is not a flaw in the tool but a matter of familiarity, and Chapter 1.0 and Part 1 will close that distance with you.

---

## The Easiest Way to Use This Book

Finally, a tip on the fastest way to put this book to work: feed the book itself, whole, to an AI tool like Claude Code.

This book was not written for human readers alone. The full prompts, code, and verification procedures in each chapter are written in a form an AI can understand and reproduce directly. So, in your own project folder, you can hand this book to an AI — as a PDF or as plain text — and ask something like, "Read the consistency-check patterns in this book and build a checker that fits our data sheets." The AI will then set up that chapter's workflow adapted to your environment. Both routes are open: building along chapter by chapter with your own hands, or handing the whole book to an AI and building together.

One thing does not change, though. The final call on what to adopt and what to reject is — as this book repeats from beginning to end — still yours. Even if you have an AI read the book and install the system, the seat where a human reviews what that system produces stays occupied by a human. Even the easiest way to use this book runs on that principle.

---

## One Promise

No table in this book contains a number inflated to persuade you. Instead of overstating the impact, I show you the **structure** that produces it. Move the same structure into your own project, and you will measure your own numbers. That is the most honest help this book can offer.

---

## The Promise Behind the Word "Reconstruction"

Throughout this book, the outputs in worked transcripts often carry the label **"reconstruction"** — for example, "Step 3 — Claude's Output (Reconstruction)." Let me make one precise promise, just once, about what that word preserves and what it touches. A book that puts honesty on its cover must not leave this spot, of all spots, blurry.

"Reconstruction" does **not mean invented; it means an actual session, edited.** The boundary runs like this.

| Preserved Verbatim | Edited |
|---|---|
| The full prompt I typed — in a form you can copy and use directly | Company, project, NPC, and teammate proper names → book-safe anonymization (IP protection) |
| The **structure and the failures** of the AI's output — the off-target candidates, the spots where it quietly bent a rule, the back-and-forth where I rejected and reran it | Length — side branches that do not fit the text are trimmed into "excerpts" |
| Code, constants, and verification values — kept as is, so they can be reproduced by running them yourself | Line breaks, spacing, and other typesetting adjustments for the page |

Put another way, no reconstructed output **adds an inflated number or a success that never happened.** It was anonymized, excerpted, and tidied for the page — nothing more. Nowhere has a failed output been rewritten as a success; if anything, I deliberately left the failures in, because they are what show you what a human rejects. (By contrast, anywhere code execution results or system logs are marked "measured" or "quoted verbatim," they were carried over without editing.)

*Translator's note: In this edition, the prompts and outputs inside code blocks have been translated into English for readability. All worked transcripts in this edition are translations of the original Korean sessions — not re-runs performed in English. The verbatim Korean originals — the artifacts this book promises not to retouch — are preserved as is in the Korean edition. Code syntax, identifiers, numbers, and verification values are untouched. Currency conversions are approximate, at roughly 1,500 won to the US dollar as of mid-2026. Where the Korean text itself is the subject — dialogue registers, honorifics — or where a transcript is explicitly quoted verbatim, the original Korean is kept with an English gloss.*

---
