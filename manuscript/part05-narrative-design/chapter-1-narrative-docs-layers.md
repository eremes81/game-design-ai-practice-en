---
title: "5.1 The NarrativeDocs Layer 0–4 Structure"
part: 5
chapter: 1
status: v3
version: v3
written: 2026-05-24
author: 이민수
ip_check: done
---

# 5.1 The NarrativeDocs Layer 0–4 Structure

When I walked into the meeting room, a character's name was circled in red on the whiteboard. An NPC I'll call Kim. In one designer's side quest, he was "the adoptive father who took the protagonist in as a child." In chapter 3 of another designer's main quest, he was "the old comrade who betrayed the protagonist and left." Both documents had been approved a month earlier, and both were already in the build. We had even gotten a quote for voice recording.

It was nobody's fault. Both designers had read the worldbuilding document and consulted the character's lore. The problem was that the same character's lore was scattered across three different files, and nobody could say with certainty which one was "real." The worldbuilding document was a monolithic 70-page Word file; a search turned up Kim in eleven places. There was no way to tell which lines were decisions and which were memos.

What we decided after that meeting was to split NarrativeDocs into five layers. This chapter is the story of those five layers.

---

## 5.1.1 Why Start with the Most Abstract Discipline

There is a reason the discipline-by-discipline Layer decomposition opens with narrative.

Narrative is the most abstract discipline. Worldbuilding, emotion, tone — none of it reduces to numbers. Unlike art or systems, there are no clean units like "sprite count" or "damage coefficient." If a discipline this abstract decomposes cleanly into Layers, the more concrete disciplines naturally follow the same pattern. In effect, we test whether the approach works in the hardest place first.

Narrative also has the most interfaces. Characters touch art; quests touch content and level design; dialogue touches UX and localization; rewards touch systems. It crosses more discipline boundaries than anything else, so the value of unifying the Layers shows up immediately.

Finally, natural-language deliverables make up the largest share of its output — which also makes it the discipline where AI assistance does the most work. That said, not every game is narrative-driven. For a casual or arcade genre, this chapter's depth may be overkill. Even so, the skeleton itself — split the monolithic document into Layers and narrow the interfaces — carries over to any discipline as is.

---

## 5.1.2 The Five-Drawer Chest

Here is the shape of NarrativeDocs decomposed into five layers. Vision (L0) sits at the top, build/QA (L4) at the bottom, and the passages connecting the layers are deliberately narrow.

<svg viewBox="0 0 720 520" xmlns="http://www.w3.org/2000/svg" font-family="sans-serif">
  <style>
    .layerbox { rx: 8; }
    .ltitle { font-size: 15px; font-weight: bold; fill: #1a1a2e; }
    .ldesc { font-size: 11px; fill: #444; }
    .iface { font-size: 11px; fill: #b03030; font-style: italic; }
    .owner { font-size: 10px; fill: #2a6; }
  </style>

  <!-- L0 -->
  <rect x="120" y="20" width="480" height="60" rx="8" fill="#fdeaea" stroke="#c0392b" stroke-width="2"/>
  <text x="140" y="44" class="ltitle">L0 Vision — "What is this world?"</text>
  <text x="140" y="64" class="ldesc">world_premise · narrative_pillar · tone_manifesto  (immutable · ~4.5 pages)</text>
  <text x="560" y="44" class="owner" text-anchor="end">Director · Lead</text>

  <!-- arrow L0->L1 -->
  <line x1="360" y1="80" x2="360" y2="108" stroke="#888" stroke-width="2"/>
  <polygon points="360,116 355,106 365,106" fill="#888"/>
  <text x="372" y="100" class="iface">pillar · tone (change triggers L1 re-review)</text>

  <!-- L1 -->
  <rect x="120" y="116" width="480" height="60" rx="8" fill="#eef2fb" stroke="#2c5fa0" stroke-width="2"/>
  <text x="140" y="140" class="ltitle">L1 System — "How does it work?"</text>
  <text x="140" y="160" class="ldesc">faction_system · reputation_model · dialogue_branching_rule · lore_consistency_rule</text>
  <text x="560" y="140" class="owner" text-anchor="end">Senior narrative</text>

  <!-- arrow L1->L2 -->
  <line x1="360" y1="176" x2="360" y2="204" stroke="#888" stroke-width="2"/>
  <polygon points="360,212 355,202 365,202" fill="#888"/>
  <text x="372" y="196" class="iface">rulebooks · branching policy (affected quests auto-listed)</text>

  <!-- L2 -->
  <rect x="120" y="212" width="480" height="60" rx="8" fill="#eef9ee" stroke="#2e8b57" stroke-width="2"/>
  <text x="140" y="236" class="ltitle">L2 Content — "What happens?"</text>
  <text x="140" y="256" class="ldesc">main_quest · side_quest · character_bible · lore_codex  (the thickest layer)</text>
  <text x="560" y="236" class="owner" text-anchor="end">Multiple designers</text>

  <!-- arrow L2->L3 (narrow!) -->
  <line x1="360" y1="272" x2="360" y2="300" stroke="#b03030" stroke-width="3"/>
  <polygon points="360,308 354,297 366,297" fill="#b03030"/>
  <text x="372" y="292" class="iface">quest_id · npc_id · dialogue_id (a single column)</text>

  <!-- L3 -->
  <rect x="120" y="308" width="480" height="60" rx="8" fill="#fbf5e9" stroke="#c08a2c" stroke-width="2"/>
  <text x="140" y="332" class="ltitle">L3 Data — "The form machines read"</text>
  <text x="140" y="352" class="ldesc">quest_table · npc_table · dialogue_id_table · reward_table  (0 lines of natural language)</text>
  <text x="560" y="332" class="owner" text-anchor="end">Narrative + data</text>

  <!-- arrow L3->L4 -->
  <line x1="360" y1="368" x2="360" y2="396" stroke="#888" stroke-width="2"/>
  <polygon points="360,404 355,394 365,394" fill="#888"/>
  <text x="372" y="388" class="iface">sheet change auto-triggers lint</text>

  <!-- L4 -->
  <rect x="120" y="404" width="480" height="60" rx="8" fill="#f2eefb" stroke="#7a4fa0" stroke-width="2"/>
  <text x="140" y="428" class="ltitle">L4 Build·QA — "Can it ship?"</text>
  <text x="140" y="448" class="ldesc">narrative_qa_checklist · voice_review_log · localization_status</text>
  <text x="560" y="428" class="owner" text-anchor="end">Narrative + QA</text>

  <!-- side note: locked anchor -->
  <line x1="120" y1="50" x2="60" y2="50" stroke="#c0392b" stroke-width="1.5" stroke-dasharray="4 3"/>
  <line x1="60" y1="50" x2="60" y2="434" stroke="#c0392b" stroke-width="1.5" stroke-dasharray="4 3"/>
  <line x1="60" y1="434" x2="120" y2="434" stroke="#c0392b" stroke-width="1.5" stroke-dasharray="4 3"/>
  <text x="52" y="245" class="iface" text-anchor="middle" transform="rotate(-90 52 245)">L0 is injected as context every time (anchor for generation &amp; review)</text>
</svg>

Mapped to folders, it looks like this. Each filename below is not an abstraction invented for this book — it is a file that actually exists in that folder.

```
NarrativeDocs/
├── Layer0_Vision/
│   ├── world_premise.md          (worldbuilding premise — immutable)
│   ├── narrative_pillar.md       (3 emotional pillars)
│   └── tone_manifesto.md         (tone + forbidden-word list)
├── Layer1_System/
│   ├── faction_system.md
│   ├── reputation_model.md
│   ├── dialogue_branching_rule.md
│   └── lore_consistency_rule.md
├── Layer2_Content/
│   ├── main_quest/               (per chapter)
│   ├── side_quest/
│   ├── character_bible/
│   └── lore_codex/
├── Layer3_Data/
│   ├── quest_table.xlsx
│   ├── npc_table.xlsx
│   ├── dialogue_id_table.xlsx
│   └── reward_table.xlsx
└── Layer4_Build_QA/
    ├── narrative_qa_checklist.md
    ├── voice_review_log.md
    └── localization_status.md
```

What matters is that no single person owns all five layers. Each layer has a different primary owner, and only the passages between adjacent layers are standardized. The red arrows in the figure above are those passages. The passage between L2 and L3 is drawn deliberately as the narrowest of all (the thick red arrow) — the reason comes later.

As a chest of drawers, it is a five-drawer chest. The first drawer holds the one worldbuilding line that never moves, the second the rulebooks, the third the prose, the fourth the sheets, the fifth the review logs. The passages between the drawers are narrow, and each passage has a change-notification bell mounted on it.

---

## 5.1.3 L0 — Kept Small So It Never Changes

L0 does not change. If it changes, the game's identity changes. So it has to be small. Small is what keeps it from changing.

| Document | Length |
|---|---|
| world_premise.md | 1.5 A4 pages |
| narrative_pillar.md | 1 A4 page (3 emotions) |
| tone_manifesto.md | 2 A4 pages (tone + forbidden-word list) |

About 4.5 pages in total. That is the weight of L0. If it gets heavy, changing it becomes frightening; once changing it is frightening, the other layers start routing around L0. Once the routing-around begins, L0 is a dead document.

The actual skeleton of `narrative_pillar.md` looks like this (content abstracted).

```markdown
---
title: Narrative Emotional Pillars
layer: L0
status: locked
last_updated: 2026-05-18
---

## 1. Longing for What Was Lost
- The player loses one thing at the end of every chapter.
- What is lost does not come back (flashbacks only).

## 2. The Conflict Between Duty and Freedom
- Every major NPC carries two duties.
- The player's choice can honor only one of them.

## 3. The Weight of a Small Kindness
- A small kindness yields a greater outcome than a grand heroic act.
```

These three pillars — longing for what was lost, the conflict between duty and freedom, the weight of a small kindness — set the direction for the hundreds of pages beneath them. And `status: locked` is not a mere label. One of L4's automated checks reads it: if a locked document is modified in a PR, the merge is blocked without the lead narrative designer's approval.

---

## 5.1.4 L1 — The Narrative Closest to Code

L1 can change, but changes are expensive. It is the rulebook layer. When one line of a rulebook changes, every piece of content that follows that rule is affected.

Here is the skeleton of `faction_system.md`.

```markdown
---
title: Faction System
layer: L1
atoms:
  - faction_relation_matrix
  - faction_membership_rule
  - faction_quest_eligibility
---

## 1. Faction Concept
N factions. Each faction is defined by (ideology, resources, territory).

## 2. Inter-Faction Relations
- relation_matrix.json (-3 hostile ~ +3 allied)
- Relation change triggers: main quest decisions, reputation thresholds

## 3. Player Membership Rules
- Maximum 2 simultaneous memberships (no two hostile factions at once)
- Leaving penalty: reputation -2, allied factions -1
```

Look at the `atoms:` list in the frontmatter. These three atom names are not just memos — they are the identifiers tracked by the Part 7 ontology and the Part 11 relation map. When a quest references `faction_quest_eligibility`, that quest is automatically added to the impact list whenever this rule changes. L1 is the narrative deliverable closest to game code, so we write it paired with a systems designer.

---

## 5.1.5 L2 — The Thickest Layer, Where the Prose Lives

L2 is the thickest layer. Main quests, side quests, the character bible, and the lore codex all live here. The real lore for Kim — the "adoptive father vs. traitorous comrade" who collided in that meeting room — now exists only as one file inside `character_bible/`. That file is the single source of truth, and quests only reference it.

The main quest folder looks like this.

```
main_quest/
├── chapter_01_awakening/
│   ├── 00_chapter_overview.md
│   ├── 01_quest_a_call_to_arms.md
│   ├── 02_quest_b_first_choice.md
│   └── ...
├── chapter_02_road/
│   └── ...
└── _TEMPLATES/
    └── quest_template.md
```

Each quest file follows the standard atom format.

```markdown
---
title: When You Take Up Arms
layer: L2
type: main_quest
atoms:
  - quest_chapter_01_awakening_a
related:
  affects: [reputation_model, faction_relation_matrix]
  derives_from: [narrative_pillar, world_premise]
  requires: [character_kim, faction_alpha]
  part_of: chapter_01_awakening
---

## Progression Steps
1. ...

## Branches
- If option A is chosen: ...
- If option B is chosen: ...

## Rewards (see L3)
- reward_table.xlsx → quest_001 row
```

The heart of it is the `related:` block. The moment you write `requires: [character_kim]`, you have declared that this quest pulls Kim's lore from the character bible — it no longer defines Kim anew inside its own file. Narrative prose lives in L2; numeric rewards live in the L3 sheet. If both live in one file, every one-line sheet fix means touching the prose, and then the translation keys drift out of sync.

---

## 5.1.6 L3 — Not a Single Line of Natural Language

L3 is sheets and IDs. Not one natural-language sentence goes in.

```
quest_table.xlsx
| quest_id | chapter | type | unlock_level | reward_xp | reward_gold | dialogue_set_id |
|----------|---------|------|--------------|-----------|-------------|-----------------|
| q_001    | ch01    | main | 1            | 500       | 100         | ds_001          |
| q_002    | ch01    | main | 2            | 800       | 150         | ds_002          |
```

Even dialogue is referenced only by ID. The lines themselves live separately in `dialogue_id_table.xlsx`, mapped 1:1 to translation keys. The passage connecting L2 and L3 needs exactly one column: `quest_id`. That is why this passage alone was the thick red arrow in the earlier figure. Narrowing an interface down to a single column is the core of Layer separation. When the passage is wide, the two sides know too much about each other, and fixing one side breaks the other.

---

## 5.1.7 L4 — The Ship Gate

L4 is review and shipping. Every time new content lands, automated and manual checks run. The automated checks are scripts. The four below are the lints actually wired into CI.

| Check | Tool |
|---|---|
| Every dialogue_id has a mapping | `dialogue_lint.py` |
| Every quest_id belongs to a chapter | `quest_lint.py` |
| Reward totals stay within each chapter's curve range | `reward_curve_check.py` |
| Forbidden-word occurrences | `tone_lint.py` (based on the L0 tone_manifesto) |

Note that `tone_lint.py` reads L0's `tone_manifesto.md` directly. The topmost layer (the immutable vision) and the bottom layer (the ship gate) are wired together by automation. If a forbidden word written into the vision turns up in the prose right before release, the build is blocked.

What automation cannot catch, people review.

| Check | Owner |
|---|---|
| Fit with the L0 emotional pillars | Lead narrative designer |
| Character voice consistency | Character owner + narrative |
| Localizability | Localizer |

When the boundary between automated and manual is clear, review time becomes predictable. You can answer the question, "How many days will this chapter's review take?"

---

## 5.1.8 Putting AI to Work on the Layers — A Worked Transcript

That is the structure. But where the structure really earns its keep is in making new content. What follows is a real session, reproduced as is, in which I had Claude take a one-line side-quest idea — "a short side quest where Kim hands the player a keepsake from his late wife" — and scatter it across the Layers of a project where the five layers were already in place. Nothing is summarized; the parts that missed and the part I rejected are included. The prompt pinned the L0/L1 context as inviolable and asked for the L2 quest file (with the `related:` block and about 200 characters of prose), the L3 quest_table row and dialogue_id, and a tone self-check — with instructions to write only L2 first, then stop.

**The prompt I sent (in full):**

```
Do not violate the attached L0/L1 context in any way, and decompose this into our Layer structure.

[narrative_pillar.md in full — the 3 emotional pillars]
[tone_manifesto.md excerpt — forbidden words: "fate", "prophecy", "the chosen one"]
[character_bible/character_kim.md excerpt — Kim: the adoptive father who took
 the protagonist in, taciturn, trails off mid-sentence, never names his feelings]

Idea: "A short side quest where Kim hands the player a keepsake from his late wife."

The L2 quest file (with the related block, ~200 characters of prose), the L3 quest_table
row and dialogue_id, plus a tone-violation self-check. Write only L2 first, then stop.
```

**Claude's raw output (L2):**

```markdown
---
title: The Faded Handkerchief
layer: L2
type: side_quest
atoms:
  - quest_side_kim_keepsake
related:
  affects: [reputation_model]
  derives_from: [narrative_pillar]
  requires: [character_kim]
  part_of: chapter_02_road
---

## Progression Steps
1. Kim quietly calls the player over. As usual, he does not state his business right away.
2. He holds out an old handkerchief. "This… I thought it might be better off in your hands."
3. When the player asks what it is, Kim looks out the window instead of answering.
4. (Choice) Accept / Decline.

## Branches
- Accept: Kim's reputation +1. Unlocks 1 handkerchief-related flashback in a later chapter.
- Decline: Kim nods and puts the handkerchief away. No reputation change.
  The handkerchief never appears again.

## Rewards (see L3)
- reward_table.xlsx → quest_side_kim_keepsake row
```

Claude stopped there, as asked — a quest titled "The Faded Handkerchief," in which Kim quietly calls the player over, wordlessly offers an old handkerchief, looks out the window instead of answering when asked what it is, and an accept/decline branch where declining means the handkerchief never appears again — and appended one line.

> "Reflecting L0 pillar 1 (what is lost does not come back), I made the handkerchief disappear forever if the player chooses 'decline.' One thing needs confirmation, though: the dead-wife detail was not in the character_kim.md excerpt. I added it on my own, so it may conflict with the character bible."

**Human verification and rejection:**

The tone of this output was exactly right. Kim's reticence, his trailing-off sentences, his habit of never naming his feelings — all of it was alive. No forbidden words either. But that final self-check had put its finger on the real problem. Nothing anywhere established that Kim had a dead wife. The AI had made the natural inference from "keepsake" — the Korean word in the idea, 유품, specifically means something left behind by the dead — to a deceased spouse, and that amounts to quietly adding a new fact to the bible. This is precisely the seed of the accident from the meeting room at the start of this chapter.

So I rejected it and sent a follow-up: the tone could stay, but "dead wife" was new lore the bible does not have — Kim's past is confirmed only as far as "he left something behind when he left his hometown" — so do not specify what the keepsake is, blur it to "something left behind long ago," and since the handkerchief is not in the bible either, soften it to a scrap of cloth.

**The follow-up request:**

```
The tone is good. But "dead wife" is new lore the bible does not have, so we can't use it.
Kim's past is confirmed only as far as "he left something behind when he left his
hometown." Don't specify what the keepsake is — rewrite it blurred to no more than
"something left behind long ago." The handkerchief isn't in the bible either, so soften it to "a scrap of cloth."
```

**Output after the follow-up (excerpt):**

```
2. He holds out an old scrap of cloth. "There was something… I left behind,
   long ago. Let's just say this is a corner of it."
3. When the player asks what it is, Kim looks out the window instead of answering.
```

This time I accepted it. Kim now offers an old scrap of cloth — "There was something… I left behind, long ago. Let's just say this is a corner of it." — and again looks out the window when asked. It touches emotional pillar 1 (loss) without nailing down anything new about Kim's past. What was lost stays blurred to the very end.

This one session shows two things. First, when you inject the Layer context precisely (L0 pillars + L1 tone + L2 bible), the AI matches tone remarkably well. Second, when the AI meets a blank, it fills it in plausibly. When that filler is a new fact absent from the bible, catching it is still a human's job. Because the Layer structure's `requires: [character_kim]` said where to look, the verifier knew immediately which file to check against. Without the structure, it would have meant digging through 70 pages all over again.

---

## 5.1.9 Alarm Bells on the Narrow Passages

The real reason for splitting into five layers is to narrow the interfaces. And every narrow interface gets change-detection automation attached to it.

| Interface | What flows through it |
|---|---|
| L0 → L1 | pillar, tone (a change triggers an L1 rulebook re-review) |
| L1 → L2 | rulebooks and branching policy (a change auto-lists the affected quests) |
| L2 → L3 | quest_id, npc_id, dialogue_id (a single column) |
| L3 → L4 | a sheet change triggers lint automatically |

For example, when someone opens a PR on L1's `faction_system.md` changing "maximum 2 simultaneous memberships" to "maximum 1," the relation map sweeps the L2 quests that reference `faction_quest_eligibility`, builds the impact list, and attaches it to the PR as an automatic comment. Instead of "book two or three meetings to find out who is affected," the person who changed the rule reads the attached list and finishes with one 30-minute meeting.

The point is this: if you split the Layers but leave the interfaces vague, all you have added is partitions. The essence is not the Layer split itself but the interface automation.

---

## 5.1.10 Six Months of Operation: What Changed

These are the measurements after running the five layers for six months. The figures below are based on my team's operating records, but absolute values are carried over only as directions and ratios (author's estimate, unverified). One limitation: "before" comes from memory and "after" from actual measurement, so this is not the same data measured twice.

| Item | Before the Layer split | After the Layer split |
|---|---|---|
| New designer onboarding | 3 weeks | 1 week |
| Scoping the impact of a rulebook change | 2–3 meetings | Auto comment + a 30-minute meeting |
| Producing 1 new main quest chapter | 4 weeks | 2.5 weeks |
| Pre-release review of 1 chapter | 5 days | 2 days |
| Localization-miss incidents | 3–5 per quarter | 0–1 per quarter |

The clearest drop is localization misses. With dialogue_id tied 1:1 to translation keys in L3 and `dialogue_lint.py` blocking mapping gaps, the "untranslated line ships in the build" incident all but disappeared. The shorter onboarding mattered too. Instead of telling a new designer "read all 70 pages of the Word file," we could say "memorize the 4.5 pages of L0, and fill in the L2 template for your quest."

One honest addition: the effect did not arrive all at once. In the first quarter we attached only one piece of automation — the auto comment — and everything else was manual. We added interface automation one piece per quarter. Attaching the automation took longer than splitting the Layers.

---

## 5.1.11 The Deeper Reason — The Road to Procedural Generation

Everything so far is the surface reason: unify the language of collaboration across disciplines. But there is one more reason, a deeper one. Layer decomposition is the precondition that makes procedural generation possible. The general thesis — each of the five layers maps onto one role in procedural generation (L0 anchor → L1 rulebook → L2 prose → L3 numbers → L4 gate), and when they are mixed into one mass the generator collapses because it cannot decide where to read from and where to write to — was covered in full in §6.6. Here we look only at how that precondition actually operates on the five narrative layers.

On a monolithic 70-page document, a generation algorithm cannot decide where to start reading or where to write. The five narrative layers become, as they are, the five stages of a generation pipeline — L0 the anchor, L1 the input rules, L2 the place where the prose accumulates, L3 the simulation input, L4 the verification gate.

The worked transcript above was already a miniature of these five stages. The L0 pillars and L1 tone were injected as context (the anchor), prose was generated into L2, an L3 row followed, and the tone self-check stood in for the verification gate. Take what a human ran one quest at a time, move it onto the same structure with a generator mass-producing side quests, and you have procedural generation.

Taken further, the flow runs like this — accumulated player actions → state changes in world Behavior Tree (BT) nodes (above the Squad level) → NPC stat changes (reputation, interests, priorities) → NPC tags plus stats become the spawn conditions → matching quests manifest from the quest cloud. Instead of writing every quest in advance, you leave them tagged, "floating in the air like clouds"; when player actions shift an NPC's stats, the quests whose spawn conditions match those stats descend and appear. This progressive model is covered in detail in 5.3 (flow diagram included). The point to stress here is that this entire flow works only on top of the five-layer decomposition.

Conversely, a team whose Layers are mixed together cannot get to procedural generation. The moment they try, consistency accidents bring it down. Just imagine the Kim accident — adoptive father and traitor at once — mass-produced automatically by a generator.

That does not mean you need the full five-drawer chest from day one. In the first quarter, separating just the one L0 line and a single L1 rulebook is enough. Separate gradually; keep the interfaces narrow.

One last note, on timing. Layer decomposition itself is a separation that dates back to the era of deterministic PCG (procedural content generation). What is new is that an LLM now handles natural-language prose, personas, and narrative branching on top of that separation — the AI writing dialogue in Kim's voice in the transcript above was impossible with the rule tables of five years ago. This timing argument is developed further in 5.3.

The next chapter (5.2) shows how `lore_consistency_rule` automatically verifies worldbuilding→character→quest consistency on top of these five layers — that is, the checker that structurally prevents the Kim accident.

---

## Try It Yourself — Your First Layer Decomposition

Assume you are starting with an existing monolithic worldbuilding document.

**setup.** Create five empty folders under a NarrativeDocs folder, from `Layer0_Vision` to `Layer4_Build_QA`. Leave the existing monolithic document where it is; pick out only the "one line that never changes" and move it into `Layer0_Vision/narrative_pillar.md`. Put `status: locked` in the frontmatter. Keep this one file under 4.5 pages.

**prompt.** When you create new content, give the AI context in this order — the pillars in full, the forbidden words, the relevant bible excerpt, then your one-line idea — and tell it to write the L2 quest file first, fill in the `related:` block (requires/derives_from/affects), add no lore that is not in the bible, and stop and ask when in doubt.

```
[narrative_pillar.md in full]
[tone_manifesto.md forbidden words]
[relevant character_bible file excerpt]

Idea: "<one-line idea>"

Do not violate this context, and write the L2 quest file first. Fill in the
related block (requires/derives_from/affects), add no new lore that is not
in the bible, and stop and ask if in doubt.
```

**verify.** Check two things in the output. (1) Actually open the files listed in `requires:` and confirm the AI did not invent facts that are not there. (2) Search the prose for forbidden words (automatic if you have `tone_lint.py`, by eye if not). If either check fails, reject, state exactly what was wrong, and re-request. Rejecting the "dead wife" in the transcript above was check (1).

---

## 5.1.12 Solo Scale-Down

If you are an indie developer working alone, five layers can look like too much. Two drawers will do.

Keep one `vision.md` (L0 and L1 merged), one `content/` folder (L2), and one spreadsheet (L3). Instead of a separate QA layer, substitute one habit: a single `requires:` line in each content file's frontmatter. Each time you write a new quest, reopen only the files listed in `requires:` and check against them — you prevent the Kim accident without digging through 70 pages. What matters is not the number of layers but the habit of keeping "the one line that never changes" separate and showing it to the AI first, every time. That one line is your context anchor, and whether you are solo or a mid-size team, that is where you start.

---

### Key Takeaways

- If narrative — the most abstract discipline — decomposes into Layers, the other disciplines follow the same pattern.
- L0 stays unchanged only if it stays small, and only an unchanging L0 can serve as the context anchor for generation and review.
- The essence is not splitting the Layers but attaching automation to the narrow interfaces.
