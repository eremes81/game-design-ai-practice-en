# Appendix C. Permissions and Settings Reference

This appendix is a reference that gathers in one place the permissions and settings for the tools and systems cited in the main text. The main text explains why I run things this way, but when you actually apply it to your own environment, what you need is "so which value, concretely, goes where?" This appendix fills that gap.

The reason behind each value matters more than the value itself. Rather than copying the numbers in the tables verbatim, read the short note under each item and adjust to your own team size and risk level. If you work alone, there is no need to split permission tiers; if you have no contractors, drop the contractor rows entirely.

There are two ways to use this appendix. When setting up an environment for the first time, walk through it in order from C.1 and use it as a checklist for anything missing. During operations, when an incident occurs, open C.7 (Incident Response) first, find the matching incident row, and trace back up to the prevention items above it.

---

## C.1 LLM API Permissions

LLM API keys tie directly to cost, and an exposed key turns into a financial incident immediately. That is why key management and permission tiers come first.

### C.1.1 Key Management

| Key | Storage |
|---|---|
| Anthropic API | environment variable + 1Password |
| OpenAI API | environment variable + 1Password |
| Self-hosted | internal to the company |

Inject keys through environment variables, not in code, and keep the originals in a secrets manager (e.g., 1Password). The most common incident is pushing a key to git embedded in code, so putting keys in git is prohibited without exception.

### C.1.2 Permission Tiers

| User | Permission |
|---|---|
| Directors and seniors | full (responsible for running the cost cap) |
| Regular members | per-task cap |
| Contractors | one-time, per task only |

Permissions are divided not by trust but by the size of the responsibility. Whoever holds full permission also carries the responsibility of managing the cost cap. Contractors get access opened once per task, revoked when the task ends.

---

## C.2 Tool Settings Standards

Recommended settings differ by tool, but the core is separating analysis work from creative work. Analysis must be reproducible; creative work needs variety.

### C.2.1 Claude Code

Below is an example base configuration for Claude Code. Line by line: pin the model, turn on extended thinking, set a token ceiling, enable auto-update, and hook a memory-injection script to prompt submission.

```json
{
  "model": "claude-opus-4-8",
  "extended_thinking": true,
  "max_tokens": 100000,
  "auto_update": true,
  "hooks": {
    "UserPromptSubmit": ["~/.claude/hooks/inject_memory.py"]
  }
}
```

The `model` value is only an example. Model names change with each generation (this example is as of the time of writing), so do not copy it verbatim — check the latest available name with `/model` and use that. Even when the names change, the workflow skeleton in this book keeps working (see Appendix K).

The script hooked to `hooks.UserPromptSubmit` automatically inserts the relevant memory fragments every time a prompt is submitted. This memory-injection mechanism is covered in detail in Part 24.

**C.2.1.1 Tool Permission Schema (allow / deny)**

Inside the same `settings.json`, permissions live in their own `permissions` block. Tools the AI may run automatically without human approval go in `allow`; tools where a single incident would be fatal, so automatic execution must be blocked, go in `deny`. The notation is `Tool(command pattern)`, and `:*` means "every call that starts with that command."

```json
{
  "permissions": {
    "allow": [
      "Bash(ls:*)",
      "Bash(git status:*)",
      "Bash(git diff:*)",
      "Read(*)",
      "Grep(*)"
    ],
    "deny": [
      "Bash(rm -rf:*)",
      "Bash(git push --force:*)"
    ]
  }
}
```

Commands with nothing to undo — reads and searches (`Read`, `Grep`) and status queries (`git status`, `git diff`) — are auto-approved via `allow` to cut down approval-popup fatigue. Conversely, commands where one mistake is unrecoverable, like `rm -rf` and `git push --force`, go into `deny` no matter how wide you make the auto-approval range.

There are four operating principles.

| Principle | Details |
|---|---|
| Start with a whitelist | Start auto-approval at the minimum and add to `allow` only when needed |
| Explicitly block dangerous commands | `rm -rf` and `git push --force` go in `deny`, no exceptions |
| Regular cleanup | Re-review `allow` every quarter and trim unused permissions |
| Separate by domain | Split global permissions from project permissions so the home PC and the work PC carry different policies |

The `allow` list is not a static setting but a trace of accumulated work. It grows as your repeated tasks grow, so it pays to pair it with a quarterly cleanup cycle. The background of this permission practice is covered in detail in Part 1, Chapter 3.

### C.2.2 In-House Tools

| Setting | Recommended value |
|---|---|
| LLM temperature (analysis) | 0 |
| LLM temperature (creative) | 0.7 |
| Cache TTL | 1 hour |
| Cost cap (daily) | defined per tool |
| Backup cycle | daily |

Analysis calls run at temperature 0 so that the same input produces the same output. Verification, lint, and classification — work whose results must not wobble — belong here. Idea divergence and draft generation, by contrast, get a variety of around 0.7. Do not set a single standard for the cost cap; define it per tool, because call frequency and token consumption differ from tool to tool.

---

## C.3 Git Permissions

| Branch | Permission |
|---|---|
| main | only directors and seniors push |
| feature/* | all members |
| protected branches | mandatory code review |

The main branch blocks direct pushes; every change comes in from a feature branch through code review. Force-push overwrites collaboration history, so it is banned; an exception is made only when unavoidable incident recovery requires it, and only by agreement between the director and the code lead.

---

## C.4 File System Permissions

Document folders divide permissions along the Layer structure (L0–L4). The higher up you go, the wider the blast radius, so write permission narrows; the lower you go, the more distributed the work, so write permission widens.

| Folder | Permission |
|---|---|
| docs/L0_vision/ | director write, everyone read |
| docs/L1_systems/ | domain director write, everyone read |
| docs/L2_content/ | owner read/write |
| docs/L4_meta/ | everyone write |
| team_memory/<per-user>/ | owner-only read/write |

Vision (L0): only the director writes, everyone reads. Systems (L1): the domain director writes. Content (L2): the owner writes; meta and scratch (L4): anyone writes. Personal memory is accessible only to its owner. The Layer structure itself is covered in Part 6.

---

## C.5 Backup and Recovery

| Asset | Backup |
|---|---|
| git repo | git itself + remote backup |
| sheets (Excel) | git + daily backup |
| user data | DB backup (server standard) |
| meeting notes and decisions | git |
| memory | daily automatic sync |

Backup paths differ by asset type, but the principle is one: the harder an asset is to recover once lost, the more redundantly you keep it. For text assets (meeting notes, decisions, code), git is the backup; binaries and server data get separate backups. Set the recovery time objective (RTO) at four hours or less, and adjust that value to the downtime your team can tolerate.

---

## C.6 Security

| Area | Rule |
|---|---|
| Sensitive data to external LLMs | placeholders or self-hosting |
| Payment and personal information | never send to an LLM |
| Quoting external material | source attribution + legal review |
| User data protection | anonymization + GDPR compliance |

The rule that is easiest to keep and broken most often is "do not send sensitive data to an external LLM." When work is urgent, the temptation to paste real data as-is is strong. Payment and personal information are no-send without exception; when analysis is needed, substitute placeholders or use a self-hosted model.

---

## C.7 Incident Response

| Incident | Response |
|---|---|
| Wrong information sent out due to LLM hallucination | immediate retraction + report |
| Cost cap exceeded | automatic block + review |
| Copyright incident | stop use within 1 hour + legal |
| Security incident (key exposure) | rotate the key immediately + review usage history |
| Data loss | restore from backup + incident analysis |

With incidents, stopping fast often matters more than preventing. Every response in the table follows the same order: stop first, analyze second. If a key is exposed, rotate the key before asking why, then review the usage history. If cost exceeds the cap, block automatically, then review. Write these response procedures down as a document and drill them regularly, so they run without hesitation when a real incident hits.
