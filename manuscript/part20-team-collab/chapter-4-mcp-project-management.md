---
title: "20.4 MCP Project Management — Connecting Collaboration Tools and Documents to the LLM"
part: 20
chapter: 4
status: v3
version: v3
written: 2026-05-24
author: 이민수
ip_check: done
---

# 20.4 MCP Project Management — Connecting Collaboration Tools and Documents to the LLM

Tuesday, just after getting to work — 9:12 a.m. Before I even open the collaboration tool board, I type one line into the Claude Code window.

```
Show me this week's incomplete P0 tasks, ordered by closest due date
```

It paused for about three seconds, then the answer appeared. I never opened the collaboration tool myself. I didn't dig through dashboard tabs, didn't message any assignees. And yet a task one day past due sat at the top of the list. Only then did I open the collaboration tool — to check that one card.

How those three seconds came to be is everything this chapter covers. The key point is that we didn't change tools. The Project A team still uses its collaboration tool (ClickUp on this project — a SaaS for managing tasks and schedules; JIRA, Redmine, and Linear fill the same slot). Whatever your collaboration tool is, this chapter's flow transfers as is — just swap the tool name. The data sheets are still in SVN, and the decision cards are still on the portal. Only one thing changed: the LLM can now **open those tools with its own hands**. The standard for that connection is MCP (Model Context Protocol).

By way of analogy, this is not seating one more new hire at the front desk. It is closer to unlocking the existing archive room for the LLM as well. The archive room stays the same. We just cut one more key.

---

## 20.4.1 What Exactly MCP Connects

MCP is a standard protocol for LLMs to access external tools and data. The word standard is the key. Instead of carving one adapter for the collaboration tool, another for documents, another for git, each tool exposes itself as a "server" on top of a single shared convention — JSON-RPC — and the LLM talks to those servers as a "client."

The structure has three pieces.

<svg viewBox="0 0 760 220" xmlns="http://www.w3.org/2000/svg" font-family="sans-serif" font-size="13">
  <rect x="20" y="70" width="180" height="80" rx="8" fill="#e8f0fe" stroke="#3367d6" stroke-width="1.5"/>
  <text x="110" y="100" text-anchor="middle" font-weight="bold">MCP Client</text>
  <text x="110" y="122" text-anchor="middle">LLM / User</text>
  <text x="110" y="140" text-anchor="middle" fill="#555">(Claude Code)</text>

  <rect x="300" y="70" width="160" height="80" rx="8" fill="#fef7e0" stroke="#f9a825" stroke-width="1.5"/>
  <text x="380" y="105" text-anchor="middle" font-weight="bold">Protocol</text>
  <text x="380" y="128" text-anchor="middle">JSON-RPC</text>

  <rect x="560" y="30" width="180" height="55" rx="8" fill="#e6f4ea" stroke="#1e8e3e" stroke-width="1.5"/>
  <text x="650" y="55" text-anchor="middle" font-weight="bold">MCP Server — Collab Tool</text>
  <text x="650" y="74" text-anchor="middle" fill="#555">Task search &amp; lookup</text>

  <rect x="560" y="100" width="180" height="55" rx="8" fill="#e6f4ea" stroke="#1e8e3e" stroke-width="1.5"/>
  <text x="650" y="125" text-anchor="middle" font-weight="bold">MCP Server — Documents</text>
  <text x="650" y="144" text-anchor="middle" fill="#555">Decision card &amp; GDD lookup</text>

  <line x1="200" y1="110" x2="300" y2="110" stroke="#666" stroke-width="1.5" marker-end="url(#a)"/>
  <line x1="460" y1="100" x2="558" y2="60" stroke="#666" stroke-width="1.5" marker-end="url(#a)"/>
  <line x1="460" y1="120" x2="558" y2="128" stroke="#666" stroke-width="1.5" marker-end="url(#a)"/>
  <defs><marker id="a" markerWidth="9" markerHeight="9" refX="7" refY="3" orient="auto"><path d="M0,0 L7,3 L0,6 z" fill="#666"/></marker></defs>
</svg>

A server exposes a list of "things I can do." For a collaboration tool server, those are functions like `search_tasks`, `get_task`, and `update_task`. The client (the LLM) takes the user's natural language, picks the matching function, calls it, and renders the returned JSON back into natural language. When I said "show me the P0 tasks," all the LLM did was call `search_tasks(priority=P0, status=open)`.

Let me make one thing clear here. This book does not cover writing MCP servers by hand. Most tools — ClickUp, JIRA, GitHub — already have official or community MCP servers, so a game designer's job is "install and connect the server," not "implement the protocol." Anthropic released the protocol itself as an open standard in November 2024, and the spec is published at modelcontextprotocol.io.

---

## 20.4.2 Worked Transcript — What "Show Me This Week's P0s" Actually Goes Through

Just saying "it happens automatically" sounds like magic. Let's follow one real run all the way through. Below is an almost verbatim copy of the first session I ran on Project A right after connecting the collaboration tool MCP. The LLM's raw output is left unsummarized, awkward parts included.

**① The prompt I typed (in full)**

```
From ClickUp, list the incomplete P0 tasks due this week, ordered by closest due date.
For each item, add the assignee and the days left until the deadline, and put anything past due at the top.
```

**② Claude's raw output (as is)**

```
I queried the ClickUp workspace for P0, incomplete, due this week.
3 tasks matched. (Past-due items are moved to the top.)

1. TASK_017  Global cooldown code change
   Assignee: teammate_a / Due: yesterday (−1 day, past due)
2. TASK_023  Apply voice_lint v2
   Assignee: teammate_b / Due: tomorrow (+1 day)
3. TASK_031  Update combat log schema
   Assignee: teammate_c / Due: Friday (+3 days)

Note: TASK_009 (balance table review) also looks like P0,
but its status is 'in review' and I wasn't sure whether to count it as 'incomplete'.
I left it out — let me know the criteria if needed.
```

Note where the LLM stopped on its own. `TASK_009` was in the 'in review' state, and rather than deciding whether that counts as "incomplete," the LLM asked the human. MCP fetches data accurately, but "does in review count as incomplete" is a matter of the team's definition. If the LLM had judged that on its own, that would have been the real incident.

**③ My verification and rejection**

I opened the collaboration tool and checked just one card, TASK_017. The deadline really had passed. But by our team's standard, 'in review' counts as incomplete too, so TASK_009 belonged on the list. The LLM's classification differed from our rule. So I rejected it and gave it the criterion again.

**④ The follow-up request**

```
Our team counts 'in review' as incomplete. Redo the list with that criterion.
From now on, treat 'in review' = incomplete.
```

In the next output, TASK_009 came in at the number 2 slot. The last sentence ("From now on, treat...") applies to this session only. If you don't want to repeat the same rule every time, enter the definition as an atom in the `shared` slot of `team_memory`, and the LLM applies it on its own from the next session onward (see §20.1 and §20.2).

What this one round trip shows is clear. MCP is **a tool that fetches information accurately**, not **a tool that makes judgments for you.** Data is automatic; definitions come from people. The moment that boundary blurs, automation turns into an incident.

---

## 20.4.3 Five Usage Patterns

Collaboration tool lookups alone get you no further than "search got a bit more convenient." MCP becomes a collaboration system when you weave multiple tools into a single flow. Here are the five patterns Project A actually runs, shown as flows.

<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg" font-family="sans-serif" font-size="11">
  <defs><marker id="mcpar" markerWidth="9" markerHeight="9" refX="7" refY="3" orient="auto"><path d="M0,0 L7,3 L0,6 z" fill="#888"/></marker></defs>
  <text x="400" y="20" text-anchor="middle" font-size="14" font-weight="bold">Five MCP Usage Patterns — Each Pattern Is One Independent Flow</text>
  <text x="400" y="38" text-anchor="middle" font-size="10" fill="#666">Colored block on the left = pattern; white boxes on the right = the steps it takes (left to right)</text>

  <!-- Pattern 1: automated reports -->
  <rect x="8" y="52" width="118" height="42" rx="6" fill="#e3f2fd" stroke="#1976d2" stroke-width="1.8"/>
  <text x="67" y="70" text-anchor="middle" font-weight="bold" fill="#1565c0">Pattern 1</text>
  <text x="67" y="85" text-anchor="middle" font-size="10" fill="#1565c0">Automated reports</text>
  <rect x="138" y="53" width="118" height="40" rx="4" fill="#fff" stroke="#1976d2"/>
  <text x="197" y="69" text-anchor="middle" font-size="10">Daily at 09:00</text>
  <text x="197" y="83" text-anchor="middle" font-size="10">Scheduler trigger</text>
  <line x1="256" y1="73" x2="266" y2="73" stroke="#888" stroke-width="1.3" marker-end="url(#mcpar)"/>
  <rect x="268" y="53" width="118" height="40" rx="4" fill="#fff" stroke="#1976d2"/>
  <text x="327" y="69" text-anchor="middle" font-size="10">Query collab tool,</text>
  <text x="327" y="83" text-anchor="middle" font-size="10">git &amp; dashboard</text>
  <line x1="386" y1="73" x2="396" y2="73" stroke="#888" stroke-width="1.3" marker-end="url(#mcpar)"/>
  <rect x="398" y="53" width="118" height="40" rx="4" fill="#fff" stroke="#1976d2"/>
  <text x="457" y="76" text-anchor="middle" font-size="10">LLM report synthesis</text>
  <line x1="516" y1="73" x2="526" y2="73" stroke="#888" stroke-width="1.3" marker-end="url(#mcpar)"/>
  <rect x="528" y="53" width="118" height="40" rx="4" fill="#fff" stroke="#1976d2"/>
  <text x="587" y="76" text-anchor="middle" font-size="10">Send: portal/messenger</text>

  <!-- Pattern 2: decision → task -->
  <rect x="8" y="120" width="118" height="42" rx="6" fill="#e8f5e9" stroke="#388e3c" stroke-width="1.8"/>
  <text x="67" y="138" text-anchor="middle" font-weight="bold" fill="#2e7d32">Pattern 2</text>
  <text x="67" y="153" text-anchor="middle" font-size="10" fill="#2e7d32">Decision → task</text>
  <rect x="138" y="121" width="118" height="40" rx="4" fill="#fff" stroke="#388e3c"/>
  <text x="197" y="137" text-anchor="middle" font-size="10">Register decision card</text>
  <text x="197" y="151" text-anchor="middle" font-size="9">proposal P####</text>
  <line x1="256" y1="141" x2="266" y2="141" stroke="#888" stroke-width="1.3" marker-end="url(#mcpar)"/>
  <rect x="268" y="121" width="118" height="40" rx="4" fill="#fff" stroke="#388e3c"/>
  <text x="327" y="137" text-anchor="middle" font-size="10">Create task in</text>
  <text x="327" y="151" text-anchor="middle" font-size="10">collab tool</text>
  <line x1="386" y1="141" x2="396" y2="141" stroke="#888" stroke-width="1.3" marker-end="url(#mcpar)"/>
  <rect x="398" y="121" width="118" height="40" rx="4" fill="#fff" stroke="#388e3c"/>
  <text x="457" y="137" text-anchor="middle" font-size="10">Assignee &amp; due date</text>
  <text x="457" y="151" text-anchor="middle" font-size="10">set automatically</text>

  <!-- Pattern 3: task → card -->
  <rect x="8" y="188" width="118" height="42" rx="6" fill="#fff8e1" stroke="#f9a825" stroke-width="1.8"/>
  <text x="67" y="206" text-anchor="middle" font-weight="bold" fill="#e65100">Pattern 3</text>
  <text x="67" y="221" text-anchor="middle" font-size="9" fill="#e65100">Task→card (reverse)</text>
  <rect x="138" y="189" width="118" height="40" rx="4" fill="#fff" stroke="#f9a825"/>
  <text x="197" y="205" text-anchor="middle" font-size="10">Collab tool</text>
  <text x="197" y="219" text-anchor="middle" font-size="10">task completed</text>
  <line x1="256" y1="209" x2="266" y2="209" stroke="#888" stroke-width="1.3" marker-end="url(#mcpar)"/>
  <rect x="268" y="189" width="118" height="40" rx="4" fill="#fff" stroke="#f9a825"/>
  <text x="327" y="205" text-anchor="middle" font-size="10">Decision card</text>
  <text x="327" y="219" text-anchor="middle" font-size="9">execution_log updated</text>

  <!-- Pattern 4: progress analysis -->
  <rect x="8" y="256" width="118" height="42" rx="6" fill="#f3e5f5" stroke="#7b1fa2" stroke-width="1.8"/>
  <text x="67" y="274" text-anchor="middle" font-weight="bold" fill="#6a1b9a">Pattern 4</text>
  <text x="67" y="289" text-anchor="middle" font-size="10" fill="#6a1b9a">Progress analysis</text>
  <rect x="138" y="257" width="118" height="40" rx="4" fill="#fff" stroke="#7b1fa2"/>
  <text x="197" y="273" text-anchor="middle" font-size="10">Query all tasks</text>
  <text x="197" y="287" text-anchor="middle" font-size="10">for the quarter</text>
  <line x1="256" y1="277" x2="266" y2="277" stroke="#888" stroke-width="1.3" marker-end="url(#mcpar)"/>
  <rect x="268" y="257" width="118" height="40" rx="4" fill="#fff" stroke="#7b1fa2"/>
  <text x="327" y="273" text-anchor="middle" font-size="10">LLM delay</text>
  <text x="327" y="287" text-anchor="middle" font-size="10">pattern analysis</text>
  <line x1="386" y1="277" x2="396" y2="277" stroke="#888" stroke-width="1.3" marker-end="url(#mcpar)"/>
  <rect x="398" y="257" width="118" height="40" rx="4" fill="#fff" stroke="#7b1fa2"/>
  <text x="457" y="280" text-anchor="middle" font-size="10">Quarterly retro input</text>

  <!-- Pattern 5: 1:1 pre-read -->
  <rect x="8" y="324" width="118" height="42" rx="6" fill="#fff3e0" stroke="#e64a19" stroke-width="1.8"/>
  <text x="67" y="342" text-anchor="middle" font-weight="bold" fill="#d84315">Pattern 5</text>
  <text x="67" y="357" text-anchor="middle" font-size="10" fill="#d84315">1:1 pre-read</text>
  <rect x="138" y="325" width="118" height="40" rx="4" fill="#fff" stroke="#e64a19"/>
  <text x="197" y="348" text-anchor="middle" font-size="10">5 min before the 1:1</text>
  <line x1="256" y1="345" x2="266" y2="345" stroke="#888" stroke-width="1.3" marker-end="url(#mcpar)"/>
  <rect x="268" y="325" width="118" height="40" rx="4" fill="#fff" stroke="#e64a19"/>
  <text x="327" y="341" text-anchor="middle" font-size="9">Member tasks +</text>
  <text x="327" y="355" text-anchor="middle" font-size="9">team_memory &amp; activity</text>
  <line x1="386" y1="345" x2="396" y2="345" stroke="#888" stroke-width="1.3" marker-end="url(#mcpar)"/>
  <rect x="398" y="325" width="118" height="40" rx="4" fill="#fff" stroke="#e64a19"/>
  <text x="457" y="348" text-anchor="middle" font-size="10">Auto-generate summary</text>
</svg>

**Pattern 1 — Automated reports.** Every morning at 9, a scheduler fires a trigger, and the LLM queries collaboration tool task states, git commits, and dashboard metrics all at once and writes the daily report. It goes out to the portal or the team messenger. "Portal" here means the in-house portal web covered in §20.3. `server.py` (FastAPI) is always running, and the `View_*.html` pages Claude wrote run on top of it, so the report slots in naturally as one more portal page.

**Pattern 2 — Decision → automatic task creation.** When a decision card (`proposal P####`) is registered, the card's implementation and verification items become collaboration tool tasks as is. Assignee and due date get filled in automatically. What the meeting settled with a "so that's what we'll do" lands on the board as cards without passing through anyone's hands.

**Pattern 3 — Task → decision card reverse reference.** This is Pattern 2 in the opposite direction. When a collaboration tool task is completed, MCP updates the original decision card's `execution_log`. "Was this decision actually executed?" is tracked automatically. Decision and execution are tied together in both directions (the dotted line in the figure).

**Pattern 4 — Progress analysis.** At quarter's end, pull every task from that quarter at once and analyze the delay patterns. "What kinds of tasks keep getting pushed back?" becomes input for the retrospective.

**Pattern 5 — 1:1 pre-reads.** Five minutes before a 1:1 meeting, synthesize that member's collaboration tool tasks, `team_memory` slot, and recent activity into a pre-read summary. Instead of burning five minutes on "what were you working on again?", the 1:1 starts with the substance.

The five patterns share one thing. **Value is recovered only where a pattern reduces repetitive work that would otherwise stay in human hands.** A pattern added to look impressive only adds operational burden.

---

## 20.4.4 Phased Adoption — Don't Connect Everything at Once

Connecting all five MCP servers on day one is the most common failure. There is an order.

| Stage | What | Rough Duration |
|---|---|---|
| 1 | Install one MCP server (ClickUp or JIRA) | 1–2 days |
| 2 | Pilot one pattern (automated reports) | About 1 week |
| 3 | Run all 5 patterns | 1–2 months |
| 4 | Develop your own MCP server (only if a niche tool requires it) | 1–3 months |

The durations above are the author's estimate (unverified), based on Project A's mid-sized (10–50 person) team. They vary with team size and tool familiarity. What is clear: **stages 1 through 3 are enough for most teams.** Go to stage 4 (developing your own server) only when you must connect a niche in-house tool that has no MCP server on the market. Most of the benefit is recovered without ever going that far.

There is a reason to mention JIRA separately. Where ClickUp is the team's internal board, JIRA is often the tool you **share with external organizations** — publishers, outsourcing partners. Connect a JIRA MCP and you can automatically pull the outsourcing board's progress before each weekly meeting and shortlist the tasks suspected of slipping. The meeting starts at "decisions" rather than "status sharing." But the more externally shared the tool, the heavier the permission and leak pitfalls of the next section weigh.

---

## 20.4.5 Four Pitfalls — Why MCP Cuts Both Ways

MCP is the act of putting tools in the LLM's hands. If what's in the hand is a knife, someone can get cut.

**Pitfall 1 — Permission incidents.** If the LLM also holds write permission, unintended changes happen — one casual "clean this up" bulk-flips task statuses. The prescription is clear. **Start MCP servers read-only.** Open up write only where it is truly needed, as in Patterns 2 and 3, and even then behind a confirmation gate before execution. The LLM asking the human about the 'in review' classification in the earlier transcript is the same spirit — when in doubt, stop.

**Pitfall 2 — Data leaks.** Company data pulled in via MCP gets transmitted to an external LLM API. Balance values, unannounced content, and revenue metrics can flow out as is. The prescription: for sensitive data, use a self-hosted LLM, or substitute placeholders at the MCP server layer before anything leaves (the same IP protection principle that runs through this book).

**Pitfall 3 — Dependency explosion.** Chain five MCP servers into the critical path, and when one goes down, the entire morning report stops. The prescription: keep only one or two core servers as critical and split the rest off as auxiliary. When an auxiliary server dies, only its section comes up empty — the report itself still ships.

**Pitfall 4 — API cost explosion.** A single MCP call burns LLM tokens and an external API call at the same time. Run the automated report every five minutes and the cost quietly balloons. The prescription: put a cap on call frequency, and cache query results that rarely change.

Tie the four pitfalls into one line, and MCP's safe position is: **start read-only, keep only the core critical, filter sensitive data, and cap the calls.**

---

## 20.4.6 Results — Where the Time Is Recovered

These are the changes I felt on Project A before and after running MCP. The time figures below are the author's empirical estimates (unverified) — read them as **direction and ratio**, not absolute values.

| Item | Without MCP | With MCP | Direction |
|---|---|---|---|
| Compiling info for the daily report | Manual, 30–60 min | Automated, \~5 min | Large reduction |
| Syncing info across tools | Manual human work | Automatic | Manual work eliminated |
| 1:1 prep | 10–15 min | Auto summary, \~3 min | Reduced |
| Tracking outsourcing progress | Meetings only | Real-time lookup | Always available |
| Decision ↔ task linkage | Manual | Two-way automatic | No more omissions |

The biggest recovery comes from "time spent gathering information." Decisions and judgment are still human work. What MCP cuts is the stage before that — the grunt work of digging through scattered tools and pulling everything into one place. The three seconds at the top of this chapter are exactly that spot.

---

## 20.4.7 Closing Part 20

Part 20 built the team collaboration system in four layers.

| Chapter | Core |
|---|---|
| 20.1 | atom operations — category classification, quarterly cleanup |
| 20.2 | Team member memory — `team_memory` 5-person slots (leeminsoo, teammates A/b/c, shared), shared vs. personal separation |
| 20.3 | Portal web — always on via `server.py` (FastAPI), `build_index.py`, nginx, and nssm; runs `View_*.html` |
| 20.4 | MCP — 5 patterns, phased adoption, permissions first |

The four chapters are not standalone tools; they are one body. The portal (20.3) is where the reports land; atoms and memory (20.1, 20.2) are where the LLM remembers team definitions like 'in review = incomplete'; and MCP (20.4) is the wiring that connects all of it to the collaboration tool and the documents. Detach any one of them and the value of the rest drops by half.

The next part covers governance and operations. The questions of safety, cost, copyright, and ethics that follow once you have connected this many tools — we take this chapter's pitfalls and elevate them into team-level rules.

---

### Key Takeaways

- MCP is a standard that cuts the LLM a key to your existing tools without changing the tools themselves.
- MCP fetches the data; people supply the definitions and judgment — that boundary is the safety line.
- Start read-only and keep only the core critical, and automation won't escalate into an incident.

---

## 20.4.8 Try It Yourself — Your First ClickUp MCP Connection

**setup**
1. Install a ClickUp MCP server (official or community) and issue a ClickUp API token.
2. Register the MCP server in your Claude Code settings, but start with a **read-only scope only**.
3. Check your workspace ID and limit the query scope to your own board.

**prompt**
```
From ClickUp, list the incomplete P0 tasks due this week, ordered by closest due date.
Add the assignee and the days left until the deadline, and put anything past due at the top.
```

**verify**
1. Open one of the returned tasks directly in ClickUp and cross-check the due date and assignee.
2. See whether the LLM asked back about ambiguous items on its own — if it asserted without asking, state the classification criteria explicitly and run it again.
3. Enter frequently used definitions ('in review = incomplete', etc.) as atoms in `team_memory`'s `shared` slot so they apply automatically in future sessions.

### Solo Scale-Down

If you are a solo developer with no team and no collaboration tool, MCP's first targets are **GitHub and your local documents**. Connect a GitHub MCP read-only and start with queries like "issues still open this week, oldest first," and instead of decision cards, query the markdown in your local `decisions/` folder through a document MCP. Automated reports (Pattern 1) apply as is — just change the destination from a team messenger to your own notes file. The permission and cost pitfalls apply just the same to a team of one, so keep the read-only start and the call cap from day one.
