# claude-skills

Personal Claude Code skills.

## brief — dense-notation output grammar

Makes Claude answer in a compressed, visual-first, epistemically-tagged format:

- **Answer first** (BLUF) + Lamport-style numbered hierarchy — drill in with "expand 2.1"
- **Epistemic tags** on every claim: ✅ fact · 🔶 assumption · 🧠 inference · ⚠️ risk (ICD-203 style)
- **Notation over prose**: `A → B ∴ C` (→ causes, ∴ therefore, ∵ because, ¬ not, ? unverified)
- **STE prose rules**: active voice, one idea per sentence, ≤20 words per sentence, ≤150 words total
- **Visual defaults**: mermaid for flow/architecture, tables for comparisons

Example output:

```
✅ config lacks API key → auth fails → test fails
🔶 key source = .env (unverified)
🧠 fix = add key to .env
⚠️ 🔶 wrong ⇒ fix moves elsewhere
```

### Install

```sh
mkdir -p ~/.claude/skills
cp -r skills/brief ~/.claude/skills/brief
```

Then invoke with `/brief` in any Claude Code session.

### Sources

Built from: [ASD-STE100 Simplified Technical English](https://www.asd-ste100.org/about_STE.html) ·
[ICD 203 Analytic Standards](https://www.intelligence.gov/assets/documents/intelligence-community-directives/ICD_203.pdf) ·
[Iverson, *Notation as a Tool of Thought*](https://dl.acm.org/doi/10.1145/358896.358899) ·
Lamport's structured proofs · cognitive-load research on working memory.
