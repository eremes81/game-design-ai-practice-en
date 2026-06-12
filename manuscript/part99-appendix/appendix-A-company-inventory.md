# Appendix A. Detailed Inventory of the Company PC System

> This appendix gathers the company PC environment I run at MMORPG studio A onto a single page — from hardware to tools, knowledge assets, and verification materials. Throughout the main text I mention "with this tool," "with this atom structure," "with this report"; this is where you can see, at a glance, the actual scale and combination in which those things exist. All real names and proper nouns have been anonymized, and since the figures shift with time, please read them as ratios and composition rather than absolute values.

There are two ways to read this appendix. One is to compare it with your own environment, item by item. Fill in the same cells — "Which engine do I use, which collaboration tools, in what form do my knowledge assets accumulate?" — and your blank cells reveal themselves. The other is to look at the balance of the composition. Having many tools does not make a good environment; what matters is whether the five axes — engine, design, art, collaboration, and AI — mesh together without blocking one another. Please look at the weave rather than at any single item.

---

## A.1 System Overview

First, the hardware and operating system that form the foundation. Once you start using AI tools in earnest, you end up running diffusion models or STT locally, so memory and GPU headroom translate directly into working speed. Treat the specs below as something close to a lower bound — the baseline at which "things run without stalling."

### A.1.1 Hardware

| Item | Spec |
|---|---|
| CPU | Desktop workstation class |
| RAM | 64GB or more |
| GPU | For UE5 development (CUDA compatible) |
| Storage | 2TB SSD + shared NAS |
| Monitors | Two 27-inch |

The RAM and GPU rows are the key ones. The moment when the engine editor, a local LLM helper, and image generation are all running at once comes around often.

### A.1.2 Operating System and Base Setup

| Item | Value |
|---|---|
| OS | Windows 11 Pro |
| Virtualization | WSL2 (Ubuntu), as needed |
| Backup | Daily, automated |

WSL2 is not kept running at all times; I pull it in only when a Linux-only tool needs to run. The single most important line in this table is that backups run automatically every day.

---

## A.2 Tool Inventory

I group the tools into five categories: engine and tools, design, art, collaboration and operations, and AI/LLM. No one person uses all five, but a game designer moves between three of them every day: design, collaboration and operations, and AI/LLM. Note that each category takes the shape of "one or two essentials plus supporting tools."

### A.2.1 Game Engine and Tools

| Tool | Purpose |
|---|---|
| Unreal Engine 5.7 or later | Main engine |
| Visual Studio | Code |
| Rider | C# IDE (secondary) |
| Perforce or SVN | Code and asset version control |

The engine and version control come as a pair. Even game designers must be able to handle a version control client, because the data sheets and specs all live in the same repository.

### A.2.2 Design

| Tool | Purpose |
|---|---|
| Excel | Data sheets + VBA macros |
| Markdown editor | Specs and meeting notes |
| Figma | UI and wireframes |
| Mermaid | Diagrams |

This is the game designer's everyday workbench. Excel is the home base for data, Markdown the home base for writing — and they are also the two points where AI tools attach most deeply. Mermaid earns its own row because, as the main text emphasizes, diagrams are the language of agreement.

### A.2.3 Art

| Tool | Purpose |
|---|---|
| Maya / Blender | 3D |
| Substance 3D | Textures and materials |
| Photoshop | 2D and illustration |
| Stable Diffusion (SDXL) / ComfyUI | Self-hosted production of concepts and textures (LoRA, ControlNet) |
| Midjourney | Early mood boards (secondary) |

These are not tools a game designer uses directly, but knowing which tools the art team has on their side changes the resolution of your requests when trading concepts back and forth. Production work runs primarily on self-hosted Stable Diffusion (SDXL) / ComfyUI — assets never go outside, which protects the IP, and character LoRAs plus ControlNet let us control the same character's consistency across repeated generations. Closed tools like Midjourney serve only as a supplement for early mood boards, when we are first feeling out the project's tone; they are not used for production work where consistency and repeat control are at stake.

### A.2.4 Collaboration and Operations

| Tool | Purpose |
|---|---|
| Collaboration tool (ClickUp) | Tasks |
| Team messenger | Real-time communication |
| Self-built wiki | Wiki and long-lived documents |
| In-house web portal | Unified interface (20.3) |

The time axis of communication is what divides the tools. Real-time communication that needs immediacy goes to the team messenger; tasks go to the collaboration tool (our team uses ClickUp); knowledge meant to last goes to the self-built wiki. Swap the tracker for JIRA or Redmine, the wiki for Confluence or Notion, use whatever messenger you have — the flow of this book stays the same. The in-house web portal is the unified gateway that connects these three with the AI tools on one screen, and 20.3 covers it in detail.

### A.2.5 AI and LLM

| Tool | Purpose |
|---|---|
| Claude (Opus + Sonnet) | Main LLM |
| GPT-4 | Alternative |
| Whisper (self-hosted) | Speech recognition (STT) |
| Stable Diffusion | Image generation (self-hosted) |
| MCP servers | Tool integration (20.4) |

Claude is the main model, with GPT-4 kept for cross-checking and as an alternative. The Whisper and Stable Diffusion rows carry a principle: sensitive audio and images are processed self-hosted, never sent outside. The MCP servers are the glue that fits these tools into the workflow; 20.4 explains the structure.

---

## A.3 atom Inventory

An atom is a fragment of knowledge — the "minimum unit of decision" covered in the main text, dropped into a file. The table below shows how those atoms are distributed across domains, a snapshot as of May 2026. Look at where the decisions cluster rather than at the absolute counts. Where decisions cluster is where the project is thinking hardest.

| Category | atom Count | Notes |
|---|---|---|
| combat | 47 | Combat system decisions |
| narrative | 38 | Five narrative layers |
| ui | 31 | UI and HUD |
| balance | 28 | Balance values |
| level | 22 | Level design |
| character | 19 | Characters and voice_profile |
| meta·governance | 18 | Procedures and rules |
| qa·integrity | 16 | Verification |
| content | 14 | Content production |
| operations | 14 | Operations workflows |
| external_reference | 12 | External references |
| economy | 11 | Economy and resources |
| Other | 34 | Classification in progress |

The distribution — combat thickest, narrative right behind — directly reflects the character of this project: a combat-centered MMORPG that nonetheless refuses to give up its narrative weight. The "Other: 34" row holds new decisions whose categories are not yet settled; when that cell grows too large, it is a signal that the classification scheme is due for an overhaul. The total as of May 2026 is 304.

---

## A.4 Verification and Operations Materials

Even with tools and knowledge in place, quality slides downhill without a mechanism that confirms they are actually working. This section shows that mechanism in two forms: reports produced on a regular cadence, and decision cards that leave decisions traceable after the fact.

### A.4.1 Reports

| Report | Frequency |
|---|---|
| Daily build report | Daily |
| Alpha gap report | Weekly (10.3) |
| Sprint quality report | Biweekly |
| Milestone QA report | Every milestone |
| Quarterly retrospective | Quarterly |

The frequency is the report's character. What runs daily is a state check; weekly and biweekly are trend checks; milestone and quarterly are direction checks. Where AI contributes most is drafting the reports that repeat — the daily and weekly ones — and 10.3 covers that case.

### A.4.2 Decision Cards

| Quarter | Decisions |
|---|---|
| Q4 2025 | 132 |
| Q1 2026 | 156 |
| Q2 2026 (in progress) | 89 |
| Cumulative | 547 |

The bare fact that around 100 decisions per quarter end up as cards shows the operating principle: decisions are handled as records, not memories. The Q2 figure of 89 is a running count at mid-quarter, so it is a value in progress; by quarter's end it reaches the level of the previous quarter. The flow in which these cards accumulate and get promoted into the atoms of A.3 is this system's learning axis.

---

## A.5 Meeting Distribution (for Reference)

Meetings are where the most time leaks away — and where the effect of AI tools is felt fastest. Below is the average distribution of meetings per quarter, grouped by category, offered as a reference for gauging the input volume of the meeting-notes system covered in 17.3. The numbers swing from quarter to quarter, so I give them as ranges.

| Category | Quarterly Average (Meetings) |
|---|---|
| Daily standups (daily) | 65–70 |
| Combat (battle) | 35–45 |
| Art (art) | 25–30 |
| Issues (issue) | 8–15 |
| Reviews (review) | 6–10 |
| Other (1:1 and external) | 40–50 |

Daily standups are the most frequent, with combat-related meetings right behind. It is telling that this is the same shape as the atom distribution in A.3: meetings cluster in the same domains where decisions cluster. The more meeting-heavy the environment, the greater the payoff from automated meeting-notes processing, and 17.3 explains the concrete operation.

---

## A.6 Notes for the Reader

Every table above is a single photograph of my environment. It is not a list to copy verbatim; please use it as a template for laying out your own environment in the same frame. With a different team size, genre, or platform, the tools, the atom distribution, and the meeting mix will all differ. What matters is not matching the items but whether the four layers — foundation → tools → knowledge → verification — run unbroken in your environment too. If one of those four layers has an empty cell, that cell is the next place to work on.
