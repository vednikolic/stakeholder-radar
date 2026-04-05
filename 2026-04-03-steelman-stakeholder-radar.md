# Steelman: Stakeholder Radar SKILL.md

**Date:** 2026-04-03
**Input:** 1-projects/stakeholder-radar/SKILL.md
**Mode:** --from-red-team (13 findings across 6 agents)

---

## Summary

Stakeholder Radar's core insight is strong: real artifacts beat intuition for predicting reviewer behavior. The red-team surfaced 13 findings. Several convert from weaknesses into differentiators. Highest-leverage improvements: (1) reposition honestly as document review simulation, (2) convert privacy/ethics surface into explicit responsible-use framework, (3) fix confidence scoring to weight signal quality.

## Top Recommendations

1. **[High]** Reposition as "document review simulation" -- drop "works for anyone" general alignment claim
2. **[High]** Add Responsible Use section converting privacy/ethics concerns into differentiator
3. **[High]** Redesign confidence scoring: signal density (distinct sourced themes) over artifact count
4. **[High]** Add document-type-specific simulation lenses (budget, architecture, strategy, design, policy)
5. **[Medium]** Add Quick Scan summary view at top of every profile for pre-meeting use
6. **[Medium]** Position against established frameworks (Mendelow, RACI) as complementary, not competitive
7. **[Medium]** Add profile size management: condensed view at 2000 words for /simulate
8. **[Medium]** Embed post-review update template in /simulate output to lower iteration friction
9. **[Medium]** Articulate compounding advantages: institutional memory, cross-stakeholder pattern detection

## Red-Team Finding Resolution

| Finding | Resolution |
|---|---|
| [BLOCKING] Positioning exceeds scope | Reposition as document review simulation |
| [HIGH] Context window management | Profile condensed view at 2000 words |
| [HIGH] Cosmetic document type detection | Type-specific simulation lenses |
| [HIGH] No artifact discovery workflow | Acknowledge in README; out of scope for v1 |
| [HIGH] Schema not flow-of-work optimized | Quick Scan summary view |
| [HIGH] No competitive positioning | "How this differs" section |
| [HIGH] No privacy guardrails | Responsible Use section |
| [HIGH] Confidence ignores signal quality | Signal-density-based formula |
| [HIGH] No use-boundary guidance | Responsible Use section |
| [MEDIUM] Slug generation undefined | Minor; not addressed |
| [MEDIUM] Iteration loop unsustained | Post-review update template |
| [MEDIUM] Bootstrap stereotyping | Acceptable with existing labels |
| [MEDIUM] No accuracy validation | Partial via iteration template |
