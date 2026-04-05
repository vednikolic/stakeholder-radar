# Stakeholder Radar

## Overview
LLM skill for evidence-based stakeholder profiles and document review simulation. Public repo vednikolic/stakeholder-radar. Installed locally at .claude/skills/stakeholder-radar/SKILL.md.

## Current State
- SKILL.md v1 complete. 5 commands: build-profile, update-profile, simulate, synthesize, audit-profile
- Red-teamed (13 findings, 6 agents) and steelmanned (9 recommendations). All findings resolved in SKILL.md
- Key features from adversarial analysis: Responsible Use section, signal-density confidence, document-type simulation lenses (7 types), Quick Scan profile summary, bootstrap mode, profile size management, post-review update template
- Hardened evals: 20 evals. v1 structural checks hit 100% baseline immediately, demonstrating need for eval inversion
- Autoresearch run complete (2026-04-03): 31.82% baseline to 88.64-93.18% (19/20 on best run). 21 rounds, 10 kept, 11 reverted. Sole persistent failure: behavioral_consistency_build (would two LLMs converge on top 3 objections)
- Key improvements from autoresearch: parseable citation format, bootstrap contradiction rule, update-profile contradiction handling, document-type severity weighting and approval gates, semantic dedup in synthesize, Quick Scan derivation rules, condensed view deterministic rules, staleness-confidence interaction, prediction accuracy tracking, multi-stakeholder artifact handling, competitive positioning scenario, vague directive elimination
- Published to GitHub, added to vednikolic profile README and vednikolic.com projects

## Decision Register
- [2026-04-03] Name: stakeholder-radar (not stakeholder-persona or stakeholder-profile). Radar implies continuous scanning, most distinctive brand [settled]
- [2026-04-03] Renamed /review-prd to /simulate for document-type-agnostic framing [settled]
- [2026-04-03] Signal-density confidence over artifact-count: prevents gaming by adding shallow artifacts [settled]
- [2026-04-03] Responsible Use as differentiator, not compliance checkbox. "Transparency option" (share profiles with stakeholder) is the key insight [settled]
- [2026-04-03] Eval inversion applied after 100% v1 baseline. Hardened suite tests behavioral consistency, contradiction handling, edge cases, feature interactions. 31.82% new baseline [settled]

## Friction Log
- [2026-04-03] .gitignore `stakeholder-radar/` pattern matched `.claude/skills/stakeholder-radar/` in repo. Fix: use `/stakeholder-radar/` (anchored) for runtime output only
- [2026-04-03] Subagent Bash permission denials in /tmp repos. Workaround: use `git -C /path` syntax instead of `cd /path && git`

## Next
- Sync improved SKILL.md to GitHub repo vednikolic/stakeholder-radar
- behavioral_consistency_build eval may need a worked example or the eval itself may need decomposition (it tests both extraction convergence and clustering convergence in one check)
- Handoff file has Q1 (persona-export) and Q3 (compare-runs) stretch features for future versions
