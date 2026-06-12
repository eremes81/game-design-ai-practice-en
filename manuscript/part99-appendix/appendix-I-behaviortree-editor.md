# Appendix I. Behavior Tree Editor Case Study (Advanced)

> An advanced case study of the Behavior Tree editor covered in 7.2. The in-house development decision, implementation, and operating experience.

---

## I.1 The Decision to Build In-House

Details behind the four decision rationales covered in 7.2.8.

| Rationale | Detail |
|---|---|
| Diff and git tracking are a must | UE BTs are .uasset binaries, which makes change tracking hard. JSON allows text diffs |
| subtree references + impact tracking | When running BTs for 100–300 NPCs, subtree-level impact analysis is decisive |
| Simulation verification | BTs can be executed in isolation without a build |
| AI-assisted authoring | LLMs generate and interpret JSON BTs naturally |

---

## I.2 Implementation Phases

```
[1. Design and Requirements Definition (1–2 weeks)]
   - Spell out the four requirements in writing
   - JSON schema design
   
[2. Runtime Implementation (3–4 weeks)]
   - JSON parser
   - BT execution engine
   - subtree reference resolution
   
[3. Editor Implementation (4–6 weeks)]
   - JSON editor (graphical)
   - subtree library UI
   - Impact analysis tool
   
[4. Simulator (2–3 weeks)]
   - Isolated BT execution
   - Statistics extraction
   
[5. AI Integration (2–3 weeks)]
   - LLM-assisted BT authoring
   - Context injection
   
[6. UE Integration (2–4 weeks)]
   - Conversion to and from UE BT
   - Build integration
```

About 4–6 months in total, with 1–2 developers.

---

## I.3 Operational Design and Incident Preparedness

### I.3.1 On the Operational Numbers

This editor is an in-house tool at the R&D stage; it never ran long enough or at a large enough scale to call the result "a year of measured operation." So, following this book's principle, I am not publishing made-up operational statistics here. The 100–300 NPC scale covered in I.1 is the **design target** that justified building in-house — not a measured result.

Of the limits the design nailed down, the one that actually went into code is the **subtree reference depth limit of 5** (to prevent infinite recursion). Values like the number of BTs in operation or the number of simulation runs vary with project scale, so instead of writing made-up numbers, I recommend measuring them in your own environment.

### I.3.2 Incidents We Prepared For and What We Learned

| Incident | Lesson |
|---|---|
| Infinite subtree references (recursion) | Reference depth limit of 5 |
| Simulation vs. live behavior divergence | Monthly calibration of the simulation environment |
| Hallucinations in LLM-generated BTs | Verification + stronger designer review |
| BT count explosion (unplanned) | Quarterly cleanup cycle |

---

## I.4 Cost and ROI

The development and operating costs are estimates based on the schedule we set when building this tool, and the "effects" are not measurements but the directions the adoption was aiming for. Instead of made-up savings figures, I record only the direction.

| Item | Value | Nature |
|---|---|---|
| Development cost | Developers for 4–6 months | Planned schedule (estimate) |
| Operating cost | 1–2 developer-weeks per quarter (maintenance) | Planned schedule (estimate) |
| Intended effect — operations staffing | Compress the headcount dedicated to BTs when running enemy NPCs at scale | Direction (unmeasured) |
| Intended effect — incidents | Structurally reduce BT incidents such as subtree recursion and LLM hallucination | Direction (unmeasured) |

The payback period for the adoption cost depends on your project's NPC scale and labor costs, so I recommend measuring the items above in your own environment before judging. I make no claim like "it pays for itself within a year" — we do not have that number.

---

## I.5 LLM Assistance Example

### I.5.1 Writing a New Enemy NPC BT

Uses the prompt from 7.2.6. Below is an example that shows the output structure (a format example, not actual operational data).

```json
{
  "bt_id": "bt_new_mage_v1",
  "category": "ranged_combatant",
  "tags": ["scholar_faction", "ranged", "magic"],
  "root": {
    "type": "selector",
    "children": [
      {
        "type": "sequence",
        "name": "low_hp_retreat",
        "children": [
          {"type": "condition", "fn": "hp_below", "param": 0.3},
          {"type": "subtree_ref", "id": "subtree_retreat_to_ally"}
        ]
      },
      {
        "type": "sequence",
        "name": "magic_attack",
        "children": [
          {"type": "condition", "fn": "enemy_in_range", "param": 15},
          {"type": "subtree_ref", "id": "subtree_magic_attack_pattern"}
        ]
      }
    ]
  }
}
```

After designer review: simulation → pass → applied to the build.

---

## I.6 Notes for the Reader

Building your own Behavior Tree tooling tends to be justified at an operating scale of 100+ NPCs (a design judgment). Below that, UE's built-in BT is enough.

Alternative options:
- BehaviorTree.CPP (open source, standard)
- Behavior Designer (third-party commercial)
- In-house development (maximum freedom, operational burden)

For selection criteria, see 7.2.8.
