---
title: "Appendix M. Dimension Vectors and Embeddings — An Intuition for Game Designers"
author: 이민수
status: v3
ip_check: done
---

# Appendix M. Dimension Vectors and Embeddings — An Intuition for Game Designers

In five places in this book — §8.2.7 (economy), §5.4 (voice), §6.3 (personas), §7.3 (patterns), and §13.3 (data) — expressions like "compress into a dimension vector," "embedding," and "close in vector space" appear as **signposts**. Even without a machine learning background, the concept is fully graspable with a game designer's instincts alone. Lay the intuition down once, and all five of those places read as one and the same picture.

But let me nail one thing down first. **The intuition behind the concept is easy; the conditions for applying it are heavy.** This idea is not an entry point — it is the **application at the farthest end**, one that can stand only on the foundation this book has been stacking up all along: the verification gates of conservative application, data and telemetry infrastructure, per-domain checkers like `voice_lint` and consistency checks, and Layer unification. Draw the coordinates first without that foundation, and — as M.4 shows — the map becomes an illusion that has neatly compressed even the error by which it diverges from the game. Here is the real reason all five places say "still premature" — not because the idea is hard, but because the supporting foundation comes first. This appendix is the map a team that already has that foundation opens to gauge its next step, not a primer for taking the first one.

## M.1 One Line — Features Become Coordinates, Similar Things Become Nearby Points

Turning any object's features into a list of numbers and placing it as a single point on a "map" — that is an **embedding**. That list of numbers is the **dimension vector**. There is only one promise — you build the map so that **the more similar two objects are, the closer their points end up**.

For example, place an NPC at the coordinates of features like (formality of speech, emotional expressiveness, vocabulary difficulty, ...), and NPCs with similar voices gather close together on the map. For cooking recipes, use the ingredient composition as the coordinates and similar dishes gather close together — this is exactly what Epicure, cited as a lead in §8.2.7, did.

<svg viewBox="0 0 520 312" width="700" xmlns="http://www.w3.org/2000/svg" font-family="sans-serif" font-size="11">
  <defs><marker id="ax3" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 z" fill="#9aa3ad"/></marker></defs>
  <text x="260" y="20" text-anchor="middle" font-size="13" font-weight="bold">Features as coordinates — 3D is a metaphor; reality is hundreds of dimensions</text>
  <polygon points="210,120 305,170 210,220 115,170" fill="#f5f7f9" stroke="#cdd5dd" stroke-width="1"/>
  <line x1="257.5" y1="145" x2="162.5" y2="195" stroke="#dde3e8" stroke-width="0.8"/>
  <line x1="162.5" y1="145" x2="257.5" y2="195" stroke="#dde3e8" stroke-width="0.8"/>
  <line x1="210" y1="120" x2="305" y2="170" stroke="#9aa3ad" stroke-width="1.2" marker-end="url(#ax3)"/>
  <line x1="210" y1="120" x2="115" y2="170" stroke="#9aa3ad" stroke-width="1.2" marker-end="url(#ax3)"/>
  <line x1="210" y1="120" x2="210" y2="48" stroke="#9aa3ad" stroke-width="1.2" marker-end="url(#ax3)"/>
  <text x="312" y="178" font-size="9" fill="#888">Feature 1</text>
  <text x="84" y="178" font-size="9" fill="#888">Feature 2</text>
  <text x="210" y="42" text-anchor="middle" font-size="9" fill="#888">Feature 3 … (hundreds)</text>
  <g fill="#388e3c" opacity="0.5">
    <circle cx="167" cy="148" r="5"/><circle cx="178" cy="140" r="5"/><circle cx="158" cy="151" r="5"/><circle cx="186" cy="144" r="5"/><circle cx="170" cy="145" r="5"/>
  </g>
  <text x="150" y="172" text-anchor="middle" fill="#2e7d32" font-size="9">Back cluster (fainter when farther)</text>
  <ellipse cx="214" cy="134" rx="29" ry="18" fill="none" stroke="#e64a19" stroke-dasharray="5" stroke-width="1.4"/>
  <text x="214" y="132" text-anchor="middle" fill="#d84315" font-size="9">Empty region</text>
  <text x="214" y="144" text-anchor="middle" fill="#d84315" font-size="8">no such combination yet</text>
  <line x1="252" y1="136" x2="168" y2="148" stroke="#7b1fa2" stroke-dasharray="4" stroke-width="1.4"/>
  <circle cx="210" cy="142" r="4" fill="#7b1fa2"/>
  <g fill="#1976d2">
    <circle cx="256" cy="124" r="7"/><circle cx="252" cy="144" r="7"/><circle cx="244" cy="116" r="7"/><circle cx="253" cy="135" r="7"/><circle cx="259" cy="121" r="7"/>
  </g>
  <text x="274" y="106" text-anchor="middle" fill="#1565c0" font-size="9">Front cluster (darker when closer)</text>
  <text x="258" y="246" text-anchor="middle" fill="#6a1b9a" font-size="10">Join two points: what lies between = interpolation (a middle variation)</text>
  <text x="260" y="290" text-anchor="middle" font-size="9" fill="#888">Nearby points = similar · empty region = no such combination yet · between two points = interpolation</text>
</svg>

## M.2 Once the Map Is Drawn, Three Things Become Visible as Distance and Position

1. **Nearby points = similar things.** When the points clump into a single spot, that is the signal that "diversity has died" (you read §5.4's voice convergence and §6.3's persona staleness not as an impression but as point density).
2. **An empty region = a combination that doesn't exist yet.** A spot with no points at all is a design blank that says "this design doesn't exist yet" (§7.3's empty pattern regions, §13.3's emergence of new topics).
3. **Between two points = a middle variation.** Connect two points and take what lies between them, and you get a "middle" — this is **interpolation**. Join "rice" and "curry leaf" and you get the flavor in between; join two NPCs and you get the persona in between.

## M.3 Three Terms That Come Up Often

- **Embedding**: turning features into coordinates (a vector) and putting them on the map.
- **Cosine similarity**: 1 when two points point in the same direction from the origin, 0 when they are at right angles — a common ruler (from 0 to 1) for measuring closeness by "how much the directions agree." The 0.83 in §6.3 means "quite similar."
- **Cluster**: a clump of points massed together on the map. A cluster that surfaces on its own, without anyone naming it in advance, is what §13.3 calls an "emergent cohort."

> **Similar-looking but opposite — do not confuse this with AHP.** AHP (Analytic Hierarchy Process, Saaty), used for multi-criteria decision making, can look like a cousin because it too turns qualitative judgments into vectors — it derives priority weights (the principal eigenvector) from pairwise comparisons that weigh criteria two at a time. But the direction is reversed. AHP is top-down decision making in which **a person predefines the criteria and the hierarchy** and assigns weights within them; the embedding here is bottom-up discovery in which **clusters surface from the data on their own, with no human definitions**. The limitation §13.3 tries to break through — "segments a person defined in advance" — is exactly where AHP starts. The two do not sit in the same spot; they sit on opposite sides.

## M.4 Why This Book Writes "Still Premature" — The Price of Compression

The map is powerful, but it is not free.

- The real number of dimensions is not two but hundreds. The 2D map in the figure above is only a metaphor; hundreds of dimensions cannot be seen with the eye, so you handle them only through "numbers" like distance and clusters.
- **Compression means throwing information away.** Which features to take as dimensions (what is independent and what is dependent) is a hard domain problem, and the accidents happen in the dimensions you threw away.
- Above all, if the data and the model are out of line with the game before you ever turn them into coordinates, compression merely compresses that error neatly along with everything else.

That is why this book keeps dimension vectors as a **signpost, not a prescription** — territory for a team with verification (telemetry and simulation) laid down solid to look into a few years from now. The concrete per-domain leads are scattered across §8.2.7 (economy), §5.4 (voice), §6.3 (personas), §7.3 (patterns), and §13.3 (data), and you can read them all on this appendix's single map.
