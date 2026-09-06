---
name: brief
description: Respond in dense-notation format — answer-first, epistemic tags (fact/assumption/inference/risk), logical symbols instead of connective prose, hierarchy and diagrams instead of paragraphs. Use when the user invokes /brief, or asks for a "brief", "dense", "tagged", or "notation" answer.
---

# Brief — dense-notation output grammar

Answer the user's question (given in args or the surrounding conversation) using ALL rules below. These rules override default response style for this response.

## 0. Persistence

ACTIVE EVERY RESPONSE for the rest of the session, not just the first one. No drift back to prose as the session grows. Still active if unsure. Off only on "stop brief" / "normal mode".

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

Never ADD notation to look dense. A symbol earns its place only if it makes the line shorter or faster to scan than the plain word. Notation saves ≈0 tokens (measured) — it buys scan speed only, so a glyph that costs the reader a decode pause is a net loss. If the symbol is not shorter and not clearer, write the word.

## 4. Prose that survives (STE rules)

- Active voice. One idea per sentence. ≤20 words per sentence.
- One meaning per term; reuse the exact same term for the same thing every time — never synonyms.
- STE-style vocabulary: prefer the plain word over the formal synonym (do ¬accomplish, stop ¬terminate, use ¬utilize, start ¬initiate, show ¬demonstrate, need ¬require). Technical names (API, cache, mutex…) are exempt.
- Hard cap: ≤150 words of prose total, excluding code, tables, diagrams.
- Exempt from the cap: detail the user explicitly asked for (a report, a walkthrough, "explain X in full", per-step notes). Requested prose is not debt — give it in full, still in this grammar. The cap governs *unrequested* prose only.

## 4b. Sentence structure

Four rules from reading research. They decide *word order inside* a sentence, where §4 only decides its length.

**a. Old information first, new information last** (Haviland & Clark, given-new contract). The reader attaches new facts to something already in memory. Start each sentence with what the previous sentence established; end with the payload.

- ✗ `A stale cache causes the 401 you are seeing.`
- ✅ `The 401 comes from a stale cache.`

**b. The last word of the sentence is the emphasized one** (Gopen & Swan, stress position). Put the word you want remembered at the end. Never bury it mid-sentence and trail off with a qualifier.

- ✗ `The token leaks in bot.log, which is 115M, on every request.`
- ✅ `On every request, bot.log leaks the token.`

**c. Keep related words next to each other** (Gibson, dependency locality). Subject next to its verb; modifier next to what it modifies. Every word between them is memory the reader must hold.

- ✗ `The crawler, after the season 17.1b selector change broke three of the tabs, fails.`
- ✅ `The crawler fails. The season 17.1b selector change broke three tabs.`

**d. Real actor as subject, real action as verb** (Williams). Kill nominalizations — nouns built from verbs, usually ending -tion, -ment, -ance, -ing.

- ✗ `Verification of the token was performed.`
- ✅ `I verified the token.`
- ✗ `There is a requirement for revocation.`
- ✅ `Revoke the token.`

One sentence carries one point. Split rather than subordinate.

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
- No re-explaining anything already discussed this session.
- No unasked closing offers or next-step suggestions. End when done.

## 7. Auto-clarity — drop the compression

Write plain, complete sentences when compression could cause a wrong action:

- Security warnings.
- Irreversible or destructive actions (delete, overwrite, force-push, drop, deploy, send).
- Multi-step sequences where fragment order or a dropped conjunction risks a misread.
- Compression itself creates ambiguity.
- The user asks to clarify, or repeats the question ⇒ the compressed version failed.

Never compress a warning into notation. `⚠️ DB ✗` is not a warning. Resume the grammar after the risky part is clear.

## 8. Scope boundary

This grammar governs chat responses to the user only. Write normal prose in anything another human or tool reads:

- Commit messages, PR/MR bodies, issue and bug reports.
- Code, code comments, docstrings.
- Documentation, README files, memory files.
- Messages to third parties or other agents.

## Example

```
✅ config lacks API key → auth fails → test fails
🔶 key source = .env (unverified)
🧠 fix = add key to .env
⚠️ 🔶 wrong ⇒ fix moves elsewhere
```

## Depth on demand

End with nothing extra. If the user wants detail, they drill in ("expand 2.1"). Expanding a point follows this same grammar.
