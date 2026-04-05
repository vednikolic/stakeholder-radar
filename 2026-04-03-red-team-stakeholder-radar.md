# Red Team: Stakeholder Radar SKILL.md

**Date:** 2026-04-03
**Input type:** Product spec / LLM skill definition
**Active agents:** Engineering, UXR, PMM, Privacy, Data/Analytics, Ethics/Trust and Safety

---

## Findings

### Engineering

**[HIGH | ADDRESSABLE]** No mechanism to handle artifact volume at scale. Profiles grow indefinitely via Evidence Log and /simulate loads full profile contents. Users who build rich profiles will hit context window limits.

**[HIGH | ADDRESSABLE]** Document type detection is cosmetic. Claims "a budget review triggers different concerns than a design spec review" but simulation prompt template is identical regardless of type. No conditional logic or type-specific instructions.

**[MEDIUM | ADDRESSABLE]** Slug generation for review output paths undefined. No rules for spaces, special characters, long filenames, or duplicates.

### UXR

**[HIGH | ADDRESSABLE]** No artifact discovery or collection workflow. Assumes users arrive with organized, labeled files. High onboarding friction excludes the users who need this most.

**[HIGH | ADDRESSABLE]** Profile schema conflates what stakeholders care about with how they behave in reviews. Not optimized for quick-scan use in the flow of work.

**[MEDIUM | ADDRESSABLE]** Iteration loop requires post-mortem discipline that most users will not sustain. No structural forcing function.

### PMM

**[BLOCKING | ADDRESSABLE]** "Works for anyone" positioning does not match document-review-only functionality. Non-document alignment scenarios (coalition building, navigating competing priorities, 1:1 negotiations) are unaddressed.

**[HIGH | ADDRESSABLE]** No competitive positioning against established frameworks (Mendelow's matrix, influence/interest grids, RACI, pre-mortem workshops).

### Privacy

**[HIGH | ADDRESSABLE]** Stores behavioral profiles of named individuals from private communications with no privacy guardrails. No guidance on consent, appropriate use, data retention, access control, or handling when someone leaves.

**[MEDIUM | NOTE]** Bootstrap mode stereotypes by job title. Archetype priors anchor user expectations even when labeled as speculative.

### Data / Analytics

**[HIGH | ADDRESSABLE]** Confidence scoring based on artifact count, not signal quality. Five shallow meeting notes = Medium; two rich review threads = Low.

**[MEDIUM | ADDRESSABLE]** No simulation accuracy validation mechanism. No way to track prediction hit rate over time.

### Ethics / Trust and Safety

**[HIGH | ADDRESSABLE]** No use-boundary guidance. Profiles map stakeholder blind spots and communication preferences without distinguishing preparation from manipulation.

---

## Severity Summary

**BLOCKING (1)**
- [PMM] Positioning scope exceeds functional scope

**HIGH (8)**
- [Engineering] Context window / profile size management absent
- [Engineering] Document type detection cosmetic
- [UXR] No artifact discovery workflow
- [UXR] Profile schema not optimized for flow-of-work use
- [PMM] No competitive positioning
- [Privacy] No privacy guardrails for behavioral profiles
- [Data] Confidence scoring ignores signal quality
- [Ethics] No use-boundary guidance

**MEDIUM (4)**
- [Engineering] Slug generation undefined
- [UXR] Iteration loop requires unsustained behavior
- [Privacy] Bootstrap stereotyping
- [Data] No accuracy validation

## Overall Confidence: HIGH

All findings grounded in content visible in the document.

## Steelman Signal

ADDRESSABLE findings tagged. Privacy and ethics concerns are strongest steelman candidates. Resolve BLOCKING positioning gap first.
