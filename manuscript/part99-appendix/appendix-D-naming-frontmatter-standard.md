# Appendix D. R&D Document Naming and Frontmatter Standard

> A generalized version of the R&D document naming and frontmatter standard (`_NAMING_FRONTMATTER_STANDARD`) used on Project A at my company.

---

## D.1 Naming Standard

### D.1.1 atom

```
<category>_<topic>_<subtopic>.md

Examples:
combat_global_cooldown_constant.md
narrative_voice_profile_K_007.md
ui_button_primary_style.md
```

snake_case, with a category prefix.

### D.1.2 Decision Cards

```
D<YEAR>_Q<QUARTER>_<NUMBER>.md

Example:
D2026_Q2_017.md
```

Year, quarter, number.

### D.1.3 Meeting Notes

```
<category>_<YYYY-MM-DD>[_<seq>].md

Examples:
95_BattleTF_2026-05-18.md
art_review_2026-05-18_1.md
art_review_2026-05-18_2.md
```

### D.1.4 Specs

```
spec_<topic>.md

Examples:
spec_combat_global_cooldown.md
spec_guild_attendance.md
```

### D.1.5 Reports

```
report_<period>_<type>.md

Examples:
report_W21_alpha_gap.md
report_Q2_user_voice.md
```

---

## D.2 Frontmatter Standard

### D.2.1 atom

```yaml
---
name: combat_global_cooldown_constant
description: Defines the standard global cooldown value for the combat system
type: atom
category: combat
status: active
priority: P0
related_atoms:
  - combat_skill_cooldown_rule
  - combat_healing_skill_cooldown_exception
created: 2026-05-18
last_modified: 2026-05-18
related:
  derives_from: [combat_design_principle]
  affects: [combat_skill_cooldown_rule, ui_skill_cooldown_indicator]
---
```

### D.2.2 Decision Cards

```yaml
---
decision_id: D2026_Q2_017
title: Unify the combat global cooldown at 0.5 seconds
type: system_change
status: active
created: 2026-05-18
created_by: Team member A
approved_by: Minsoo Lee
scope:
  - combat_system
affected_atoms: [...]
implementation:
  target_build: 2026-05-18
verification:
  layer_1: passed
  layer_2: passed
  layer_3: pending
---
```

### D.2.3 Meeting Notes

```yaml
---
type: meeting_note
category: battle
date: 2026-05-18
attendees: [Team member A, Team member B, Minsoo Lee]
related_atoms: [...]
---
```

### D.2.4 Specs

```yaml
---
title: Guild attendance feature spec
type: spec
priority: P1
target_milestone: MS2
---
```

---

## D.3 Required vs. Optional Fields

### D.3.1 Required Fields

| Document type | Required |
|---|---|
| atom | name, description, type, category, status |
| Decision card | decision_id, title, type, status, created, scope |
| Meeting notes | type, category, date, attendees |
| Spec | title, type, priority |

### D.3.2 Optional Fields (Nice to Have)

| Document type | Optional |
|---|---|
| atom | related, last_modified, priority |
| Decision card | rationale, related_decisions, verification |
| Meeting notes | related_atoms, sub_topic |
| Spec | target_milestone, related_atoms |

---

## D.4 Automated Lint Checks

```bash
# frontmatter_lint.py

for file in glob("**/*.md"):
    fm = parse_frontmatter(file)
    if not fm:
        warn(f"{file}: missing frontmatter")
    
    doc_type = infer_type_from_filename(file)
    required = REQUIRED_FIELDS[doc_type]
    
    for field in required:
        if field not in fm:
            warn(f"{file}: missing required field {field}")
```

Runs automatically at build time. Violations trigger an alert.

---

## D.5 Preventing Naming Collisions

| Area | Prevention |
|---|---|
| atom name | globally unique |
| Decision ID | unique within a quarter |
| Meeting ID | date + seq |
| Filename | unique within a folder |

Naming collisions are blocked automatically.

---

## D.6 Change Procedures

### D.6.1 Renaming an atom

```
1. Create an atom under the new name
2. Update every wikilink to the old atom with the new name (automated)
3. Mark the old atom deprecated + redirect
4. Move it to _archive after one month
```

Rushed renames risk damaging your records.

### D.6.2 Changing the Frontmatter Standard

```
1. Propose the reason for the change (decision process)
2. Migration script for all existing documents
3. Update the build lint
4. Notify the team
```

---

## D.7 Notes for the Reader

This standard reflects my environment. Adjust it to fit your own. The essentials:

| Essential | Why |
|---|---|
| Naming consistency | Search and automation |
| Frontmatter standard | Tool-friendly |
| Required/optional split | Writing burden ↓ |
| Automated lint | Enforces the standard |
| Change procedures | Protects your records |
