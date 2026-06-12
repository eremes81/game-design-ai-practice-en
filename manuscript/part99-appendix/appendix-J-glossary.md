---
title: "Appendix J. Abbreviations and Glossary"
appendix: J
status: v3
written: 2026-06-06
author: 이민수
version: v3
---

# Appendix J. Abbreviations and Glossary

This appendix gathers in one place the abbreviations used in the main text and the terms specific to this book. The main text spells out each abbreviation once at its first occurrence, but if you are not reading in order, or forget one along the way, you can look it up here directly. Where one abbreviation carries different meanings depending on context, I listed both.

The glossary is grouped in this order: Team Size Tiers → Game Design Documents → Game Domain → Data and Operations → AI and Tools → UI and Accessibility Standards → Files and Formats. Think first about what kind of abbreviation you are looking for, and you can narrow down which group it belongs to. For example, `DPS` and `TTK` are in "Game Domain," `KPI` and `DAU` in "Data and Operations," and `atom` and `JIT` in "AI and Tools."

There are three notation rules. ① For general abbreviations, I listed the full name together with its meaning. ② For terms used only in this book, such as `atom` and `Wrapper`, the full-name column reads "(a term specific to this book)." ③ When one abbreviation has two meanings, such as `PK` (Player Kill in war contexts ↔ Primary Key in data contexts), the main text notes which sense applies at first occurrence, and this table lists both.

## Team Size Tiers

This book does not pin team headcount to a specific number; it uses the following three tiers. The same technique calls for a different depth of adoption depending on team size.

| Tier | Headcount | Description |
|---|---|---|
| Small | up to \~10 | From solo and hobbyist developers to single-digit teams. Adoption steps 1–2 are usually enough |
| Mid-size | 10–50 | The range my team — the source of this book's operational cases — belongs to. The scale where the cumulative effects of standardization and consistency automation become unmistakable |
| Large | 100+ | Multiple disciplines, multiple teams. The scale that justifies dedicated infrastructure and dedicated operations staff |

Wherever the main text gives both the tier and the headcount range, as in "a mid-size (10–50 person) team," this table is the reference. Where the headcount itself carries the meaning — solo or one-person development — I write the exact number instead of the tier.

## Game Design Documents

| Abbreviation | Full Name | Meaning |
|---|---|---|
| GDD | Game Design Document | Game design document. The detailed spec that locks down systems, numbers, and behavior |
| CDD | Concept Design Document | Concept design document. The early-stage design doc (direction and concept) that precedes the GDD |
| TF | TaskForce | A dedicated team assembled temporarily for a short-term goal (e.g., a combat TF) |
| DD | Design Director | Design director. The lead role that oversees the game's overall design direction |
| RnD | Research and Development | Research and development. The phase or organization that explores prototypes and new techniques (e.g., procedural generation RnD) |

## Game Domain

| Abbreviation | Full Name | Meaning |
|---|---|---|
| NPC | Non-Player Character | A character the player does not control |
| HUD | Heads-Up Display | Status information overlaid on the game screen (health, minimap, etc.) |
| DPS | Damage Per Second | Damage dealt per second |
| GCD | Global Cooldown | Global cooldown. A shared wait time that briefly locks all skills together after you use one |
| TTK | Time To Kill | The time it takes to kill a target |
| PK | Player Kill | (In war/PvP contexts) combat and kills between players |
| BT | Behavior Tree | Behavior tree. A structure that defines an NPC AI's behavior branches as a tree |
| FSM | Finite State Machine | Finite state machine. A model that defines behavior through states and transitions |
| PCG | Procedural Content Generation | Procedural content generation. Generating content automatically from rules and algorithms |
| VFX | Visual Effects | Visual effects |
| SFX | Sound Effects | Sound effects |
| VA | Voice Actor | Voice actor |
| RPG / MMORPG | (Massively Multiplayer Online) Role-Playing Game | Role-playing game / massively multiplayer online RPG |
| P2W / P2E | Pay To Win / Play To Earn | A structure where paying makes you stronger / a structure where playing earns you money |
| RMT | Real Money Trading | Trading in-game goods for real-world money |

## Data and Operations

| Abbreviation | Full Name | Meaning |
|---|---|---|
| KPI | Key Performance Indicator | Key performance indicator |
| DAU | Daily Active Users | The number of daily active users |
| FK | Foreign Key | Foreign key. A column that points to another sheet's primary key |
| PK | Primary Key | (In data contexts) primary key. The column that uniquely identifies a row |
| ROI | Return on Investment | The return you get relative to what you invested |
| MECE | Mutually Exclusive, Collectively Exhaustive | Mutually exclusive, collectively exhaustive. A classification principle: no overlaps, no gaps |
| STT | Speech-to-Text | Converting speech to text |
| VBA | Visual Basic for Applications | The macro language built into Excel |
| SVN | Subversion | A file version control system |
| telemetry | (measurement data) | Play logs and metrics collected automatically from game builds and runs (inputs, combat, churn, etc.) |

## AI and Tools

| Abbreviation | Full Name | Meaning |
|---|---|---|
| AI | Artificial Intelligence | Artificial intelligence |
| LLM | Large Language Model | Large language model (the foundation of ChatGPT, Claude, and the like) |
| JIT | Just-In-Time | Inserting something only at the moment it is needed (in this book, automatic injection of memory matched to the input) |
| MCP | Model Context Protocol | A standard for connecting AI tools to external services |
| API | Application Programming Interface | A convention for calls between programs |
| UE | Unreal Engine | Unreal Engine |
| atom | (a term specific to this book) | A decision/rule card codified as one decision = one file |
| Wrapper / Cascade / Junction | (terms specific to this book) | An entry point for frequently used tools / a tool that bundles multiple checks into a single run / a symbolic link that connects to the main copy |
| rg | ripgrep | A fast text search command (a CLI tool that replaces grep). Used for exhaustive searches across code and documents |
| ClickUp | (a task/issue tracker) | A cloud collaboration tool for managing tasks and schedules. JIRA, Redmine, and Linear are in the same category. Connected via MCP so the AI can query and update it |

## UI and Accessibility Standards

| Abbreviation | Full Name | Meaning |
|---|---|---|
| UI / UX | User Interface / User Experience | User interface / user experience |
| WCAG | Web Content Accessibility Guidelines | Web accessibility guidelines (pass thresholds for contrast ratio, touch target size, and so on) |
| HIG | (Apple) Human Interface Guidelines | Apple's interface guidelines |
| SC | Success Criterion | The number of an individual WCAG pass criterion (e.g., SC 1.4.3) |
| pt / dp / px | point / density-independent pixel / pixel | Units of on-screen size |

## Files and Formats

| Abbreviation | Full Name | Meaning |
|---|---|---|
| YAML | YAML Ain't Markup Language | A human-readable format for configuration and data |
| JSON | JavaScript Object Notation | A data interchange format |
| HTML / SVG | HyperText Markup Language / Scalable Vector Graphics | Web document format / vector graphics format |
| GLB | GL Transmission Format (Binary) | A binary file format for 3D models |

---

*For abbreviations like PK whose meaning splits by context, the main text notes which sense applies at first occurrence. If you lose track, come back to this table.*
