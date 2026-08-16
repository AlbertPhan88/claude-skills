---
name: brief
description: Respond in dense-notation format — answer-first, epistemic tags (fact/assumption/inference/risk), logical symbols instead of connective prose, hierarchy and diagrams instead of paragraphs. Use when the user invokes /brief, or asks for a "brief", "dense", "tagged", or "notation" answer.
---

# Brief — dense-notation output grammar

Answer the user's question (given in args or the surrounding conversation) using ALL rules below. These rules override default response style for this response.

## 1. Structure (Lamport hierarchy + BLUF)

- Line 1 = the conclusion. One sentence, no lead-in.
- More than 3 points → numbered hierarchy, max 2 levels deep. Level 1 readable alone; level 2 is drill-down detail only.
- Never write a paragraph where a hierarchy, table, or diagram can carry the content.

## 2. Epistemic tags (ICD-203 style)

Prefix every substantive claim:

| Tag | Meaning |
|---|---|
| ✅ | Fact — verified in code, output, or docs this session |
| 🔶 | Assumption — unverified, the reasoning relies on it |
| 🧠 | Inference — my conclusion derived from ✅/🔶 above |
| ⚠️ | Risk — what breaks if a 🔶 is wrong or an action fails |

Every ⚠️ must reference which 🔶 or action it depends on. Never present a 🧠 or 🔶 with the confidence of a ✅.

## 3. Notation (replaces connective prose)

| Symbol | Replaces |
|---|---|
| → | causes, leads to, then |
| ⇒ | implies |
| ∴ | therefore |
| ∵ | because |
| ¬ | not, absence of |
| Δ | change in, diff |
| ? | unverified, open question |
| = / ≠ | is, is not |

Quantity and state:

| Symbol | Replaces |
|---|---|
| ↑ / ↓ | increase / decrease |
| ↑↑ / ↓↓ | explode (sharp rise) / collapse (sharp drop) |
| ≈ | approximately |
| ≫ / ≪ | much greater / much smaller than |
| ∅ | none, empty, missing |
| ✗ | fails, broken |
| ⇄ | trade-off, two-way dependency |
| # | count of |
| ! | important |

Chains read left→right: `✅ missing key → auth fails → test fails`. One meaning per symbol — never overload or invent new ones mid-response.

## 4. Prose that survives (STE rules)

- Active voice. One idea per sentence. ≤20 words per sentence.
- One meaning per term; reuse the exact same term for the same thing every time — never synonyms.
- Hard cap: ≤150 words of prose total, excluding code, tables, diagrams.

## 5. Visual defaults

- Flow, dependency, architecture, sequence → diagram. Pick by medium:
  - Terminal / chat response → ASCII box-drawing (mermaid source does not render there).
  - `.md` file, artifact, GitHub → mermaid.
- Comparison of ≥2 things on ≥2 attributes → table.
- Prose only where neither fits.

## 6. Ban list

- No preamble, no restating the question, no "In summary" re-summaries.
- No hedging filler ("it's worth noting", "generally speaking", "may or may not").
- No repetition — reference an earlier point by its number (e.g. "per 2.1"), never restate it.

## Example

```
✅ config lacks API key → auth fails → test fails
🔶 key source = .env (unverified)
🧠 fix = add key to .env
⚠️ 🔶 wrong ⇒ fix moves elsewhere
```

## Depth on demand

End with nothing extra. If the user wants detail, they drill in ("expand 2.1"). Expanding a point follows this same grammar.
