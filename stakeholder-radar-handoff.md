# Stakeholder Persona — Handoff

Skill file: `stakeholder-persona.skill`
Context: Follow-up design questions after initial skill build. Answer these in order — each one adds a layer of capability.

---

## Q1: Shareable Team Export

Should the profile support a `persona-export` command that outputs a version safe to share with your team?

Strips raw artifact excerpts but keeps inferred themes, recurrence counts, and confidence scores. Other PMs on your team could run their own PRD reviews against your stakeholder graph without accessing the underlying source artifacts.

**Prompt:**
> "Add a `/persona-export` command to the stakeholder-radar skill. It should output a sanitized version of a profile that removes raw artifact excerpts and source citations but preserves all inferred themes, recurrence counts, ranked objections, ranked questions, and confidence scores. Output as a standalone Markdown file suitable for sharing with teammates who don't have access to the original artifacts."

---

## Q2: Bootstrap Mode for Thin Coverage

How should the skill handle stakeholders you have very little artifact coverage on?

Two options to choose from before prompting:

- **Option A — Hard floor:** Keep current behavior. Flag low confidence loudly, refuse to simulate until artifact count meets a minimum threshold (e.g., 3 artifacts).
- **Option B — Prior + update:** Use role archetypes (EM, Director of Product, Senior PM) as a weak prior to bootstrap an initial profile, clearly marked as inferred-not-evidenced. Updates push the profile toward real evidence over time, and archetype assumptions are retired as evidence accumulates.

**Prompt:**
> "Add a bootstrap mode to `/build-profile` in stakeholder-radar. When fewer than 3 artifacts are provided, instead of blocking, generate an initial profile using a role archetype as a weak prior (Engineering Manager, Director of Product, or Senior PM — inferred from the role field). Mark every archetype-derived claim with `[prior — not evidenced]`. As real artifacts are added via `/update-profile`, replace prior-derived claims with evidence-backed ones and remove the prior tag. Once artifact count reaches 5, suppress the archetype prior entirely."

---

## Q3: PRD Iteration Diff

Do you want a `/compare-runs` command to diff two review outputs on the same PRD across iterations?

Shows whether your PRD revisions actually resolved the predicted blockers — closes the loop between simulation and real review prep.

**Prompt:**
> "Add a `/compare-runs` command to stakeholder-radar. It takes two review output files for the same PRD (e.g., v1 and v2 after revisions) and produces a diff showing: (1) blockers that were present in run 1 and are gone in run 2, (2) blockers that persist across both runs, (3) new blockers that appeared in run 2 that were not in run 1, and (4) a net resolution score (resolved / total from run 1). Save the diff to the same reviews folder alongside the source files."

---

## Install reminder

Drop `stakeholder-persona.skill` into your skills directory before starting.
Verify with: `ls ~/.claude/skills/` or wherever your skill path is configured.
