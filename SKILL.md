---
name: stakeholder-radar
description: "Build evidence-based stakeholder profiles from real artifacts (meeting notes, Slack threads, emails, doc comments, review threads, presentations) and simulate how reviewers will react to a document before the real review. Works on any document: PRDs, proposals, briefs, budgets, strategy docs, design specs, architecture decisions, slide decks, policy changes, roadmaps. Use when the user wants to anticipate objections, prepare for a review or approval meeting, build a stakeholder profile, update a profile with new evidence, run a multi-stakeholder simulation, or audit profile coverage. Trigger on: stakeholder profile, anticipate feedback, review prep, what will [person] push back on, update my [name] profile, simulate a review, stakeholder radar, build a persona from artifacts."
argument-hint: /build-profile | /update-profile | /simulate | /synthesize | /audit-profile
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash(mkdir *)
  - Bash(ls *)
  - Bash(date *)
---

# Stakeholder Radar

Simulate how your reviewers will react to a document before you share it.

You know who the reviewers are. You have worked with them before. Stakeholder Radar turns that experience into structured profiles so you can simulate their reactions, address concerns preemptively, and walk into the review prepared.

Build profiles from real artifacts (meeting notes, Slack threads, emails, doc comments, review threads). Run simulations against any document type: PRDs, proposals, briefs, budgets, strategy docs, design specs, architecture decisions, slide decks, policy changes, or roadmaps.

Works standalone. Does not require any other tool or skill.

---

## How This Differs From Stakeholder Mapping

Stakeholder mapping frameworks (Mendelow, RACI, influence/interest grids) categorize people by power and interest. Stakeholder Radar does something different: it builds behavioral models of specific people from real interactions. RACI tells you who approves. Radar tells you what they will object to and why. Use both. They are complementary.

---

## Core Principles

**Evidence only.** Every claim in a profile must cite the artifact it came from using `[source: <label>]`. Never fabricate stakeholder reactions. Never use generic persona archetypes ("engineers always care about...") as evidence.

**Recurrence is signal.** Themes appearing in 2+ artifacts are marked recurring. Themes from a single artifact are marked tentative. Rank everything by frequency.

**Confidence is earned.** Confidence is based on signal density (number of distinct sourced themes extracted), not just artifact count:
- Low: fewer than 4 distinct sourced themes, regardless of artifact count
- Medium: 4-10 distinct sourced themes across 2+ artifact types
- High: 10+ distinct sourced themes across 3+ artifact types, with 3+ recurring themes

**Staleness decays.** Any signal older than 6 months gets marked `[stale - needs reconfirmation]` during updates.

**Honest gaps.** If a section has no evidence, write `[no evidence yet]`. If profile confidence is low, prepend simulation output with a reliability warning and flag the weakest sections.

---

## Responsible Use

Stakeholder profiles model real people's behavior from real communications. This creates an obligation to use them responsibly.

**Preparation, not manipulation.** Use profiles to understand concerns and address them substantively. Do not use profiles to engineer around someone's judgment or exploit their blind spots.

**Source respect.** Artifacts from private contexts (1:1 notes, DMs) carry higher trust obligations than artifacts from group settings (team meetings, public Slack channels). If you would not quote the artifact in the review meeting, reconsider using it in a profile.

**Transparency option.** Consider sharing profile summaries with the profiled stakeholder. "Here is what I think you will care about. Am I reading you right?" This builds trust and improves accuracy simultaneously.

**Retention.** When a stakeholder leaves the org or changes roles significantly, archive or delete their profile. Stale behavioral models from a different context are misleading, not useful.

---

## Commands

| Command | Purpose |
|---|---|
| `/build-profile` | Create a new profile from artifacts |
| `/update-profile` | Add new artifacts to an existing profile |
| `/simulate` | Run a multi-stakeholder review simulation on any document |
| `/synthesize` | Stack simulation output by recurrence, surface top N issues |
| `/audit-profile` | Show evidence gaps and confidence scores per section |

---

## File Layout

```
stakeholder-radar/
  profiles/
    {firstname-lastname}.md
  reviews/
    {doc-slug}/{timestamp}.md
  SKILL.md
```

All profiles and reviews live in the project root's `stakeholder-radar/` folder unless the user specifies otherwise. Create it if it does not exist.

Slug generation: lowercase the document filename, replace spaces and special characters with hyphens, truncate to 50 characters. Example: "Q3 Budget Proposal (revised).md" becomes "q3-budget-proposal-revised".

---

## Profile Schema

Every profile is a Markdown file with this structure:

```markdown
---
name: Full Name
role: Title / Team
last_updated: YYYY-MM-DD
artifact_count: N
theme_count: N
confidence: low | medium | high
bootstrap: false
---

## Quick Scan
**Will push back on:** [top 3 objection themes]
**Resonates with:** [decision lens summary, one sentence]
**Avoid:** [blind spots or non-negotiables to steer clear of]

## Identity
Role, team, org level, tenure signal, key relationships.

## Decision Lens
What framing do they consistently apply? (Cost? Risk? Speed? User impact? Org politics? Technical debt? Timeline? Headcount?)
Source every inference.

## Recurring Objections  [RANKED BY FREQUENCY]
<!-- Format: "theme (N occurrences) [sources]" -->
1. ...
2. ...

## Recurring Questions  [RANKED BY FREQUENCY]
1. ...
2. ...

## Priorities and Non-Negotiables
What they will not trade away. Backed by artifact evidence.

## Blind Spots / Weak Signal Zones
Topics where they defer, disengage, or show low signal.

## Communication Style
Preferred format, preferred depth, async vs sync, directness level.

## Evidence Log
| Date | Artifact | Key Signal |
|------|----------|------------|
```

### Profile Size Management

When a profile exceeds 2000 words, generate a condensed view for use in /simulate that keeps only: Quick Scan, top 5 recurring objections, top 5 questions, current priorities, and the last 10 Evidence Log entries. Use the condensed view for simulations. Keep the full profile for /audit-profile and direct reading.

---

## `/build-profile`

### Inputs
- Stakeholder name and role (required)
- Artifact paths or pasted text (at least one, unless bootstrap mode)
- Artifact type labels: `meeting-notes`, `slack`, `email`, `doc-comment`, `review-thread`, `presentation`, `post`, `1on1-notes`, `decision-doc`, `other`

### Bootstrap Mode

When fewer than 3 artifacts are provided, generate an initial profile using a role archetype as a weak prior. Common archetypes include Engineering Manager, Director of Product, VP Engineering, CFO, Design Lead, Staff Engineer, Legal Counsel, Head of Operations, or any other role. This provides a starting point instead of blocking on insufficient evidence.

Bootstrap rules:
- Mark every archetype-derived claim with `[prior - not evidenced]`
- Set `bootstrap: true` in profile frontmatter
- As real artifacts are added via `/update-profile`, replace prior-derived claims with evidence-backed ones and remove the prior tag
- Once artifact count reaches 5, suppress the archetype prior entirely and set `bootstrap: false`
- Bootstrap profiles produce simulations with a prominent warning: `[LOW CONFIDENCE - bootstrapped profile, results are speculative]`

### Process

1. **Ingest all artifacts.** Read each file or block of text. Tag each with its label and date if available.

2. **Extract signals.** For each artifact, identify:
   - Explicit objections raised
   - Questions asked
   - Topics they approved or praised
   - Framing language used (risk words, cost words, user words, org words, timeline words, headcount words)
   - What they asked to change vs. accepted as-is
   - Any stated or implied priorities
   - Decision patterns (what they approve fast, what they slow-walk)

3. **Cluster and count.** Group signals across artifacts by theme. Count occurrences. Themes in 2+ artifacts are recurring. Single-artifact themes are tentative.

4. **Score confidence.** Based on signal density (distinct sourced themes), not just artifact count.

5. **Write the profile.** Populate each schema section including Quick Scan. Rank Recurring Objections and Recurring Questions by frequency (highest first). Cite source for every claim. Never pad.

6. **Save** to `stakeholder-radar/profiles/{firstname-lastname}.md`.

### Output
- The saved profile file at `stakeholder-radar/profiles/{firstname-lastname}.md`
- A terminal report containing: confidence score, theme count, artifact count, top 3 objection themes, and what artifact types are missing that would increase confidence.

---

## `/update-profile`

### Inputs
- Profile name (required). The firstname-lastname slug of an existing profile.
- New artifact paths or pasted text (at least one required).
- Artifact type labels (same as /build-profile).

### Process

1. Load existing profile from `profiles/{name}.md`.
2. Ingest new artifacts. Extract signals (same process as build).
3. Merge with existing evidence:
   - Increment occurrence counts for recurring themes
   - Add new themes from new artifacts
   - Demote themes with no new evidence (do not remove, note staleness)
   - Re-rank Recurring Objections and Questions
   - Replace any `[prior - not evidenced]` claims that now have real evidence
   - Regenerate Quick Scan from updated data
4. Update `last_updated`, `artifact_count`, `theme_count`, and `confidence`.
5. Append new rows to the Evidence Log.
6. Save the updated profile.

### Output
- The updated profile file at `stakeholder-radar/profiles/{name}.md`
- A terminal delta report showing: what changed, what was confirmed, what is newly surfaced, and the new confidence score.

---

## `/simulate`

### Inputs
- Document path (required). Any document type: PRD, proposal, brief, budget, strategy doc, design spec, architecture decision, slide deck, policy, roadmap, or other.
- Profile names to include (required, 2-5 recommended)
- Review mode: `cold` (profile only) or `warm` (add context notes per stakeholder)

### Process

First, detect the document type from its content and structure. Use the detected type to select type-specific simulation probes (see below).

For profiles exceeding 2000 words, use the condensed view for the simulation prompt.

For each stakeholder, run a review pass using this framing:

```
You are simulating a review of this {document_type} by {name}, {role}.

Here is their evidence-based profile:
{profile contents or condensed view}

Here is the document:
{document contents}

Your task:
1. List every objection you would raise, ordered by how strongly the profile
   evidence predicts you would raise it. Mark each: HIGH / MEDIUM / LOW confidence.
2. List every question you would ask before approving.
3. List what you would approve without pushback.
4. Identify the single most likely blocker from this stakeholder.

Type-specific probes for this {document_type}:
{type_specific_probes}

Ground every point in the profile. Do not invent reactions not evidenced in the
profile. If the document covers topics with [no evidence yet] in the profile, flag
them as unpredictable.
```

### Document Type Simulation Lenses

Select the closest match. If ambiguous, use the general probes.

**Budget / resource proposal:**
- What ROI evidence is missing?
- What competing priorities would you fund instead?
- What cost assumptions would you challenge?

**Architecture / technical decision:**
- What scalability risks concern you?
- What alternative approaches were not considered?
- What operational burden does this create?

**Strategy / vision doc:**
- What market assumptions are unvalidated?
- What timeline is unrealistic?
- What organizational capability is assumed but not proven?

**Design spec / UX artifact:**
- What user scenarios are missing?
- What accessibility concerns apply?
- What existing patterns does this violate?

**Policy / process change:**
- Who is affected negatively by this change?
- What enforcement gaps exist?
- What transition plan is missing?

**Product spec / PRD:**
- What scope is missing or underspecified?
- What dependencies are not accounted for?
- What success metrics are measurable vs aspirational?

**General (fallback):**
- What assumptions does this document make that are not stated?
- What risks are acknowledged vs ignored?
- What would you need to see before approving?

After all individual reviews, run a synthesis pass:

```
Given these N stakeholder reviews, produce:
1. CONSENSUS BLOCKERS: objections raised by 2+ stakeholders
2. SOLO BLOCKERS: objections from only one stakeholder (note who)
3. OPEN QUESTIONS: questions raised by 2+ stakeholders
4. SAFE ZONES: items all stakeholders approved
5. UNPREDICTABLE ZONES: topics flagged as low-evidence across profiles
```

Save full output to `stakeholder-radar/reviews/{doc-slug}/{timestamp}.md`.

### Post-Review Update

After every /simulate output, append this template:

```
## Post-Review Update
After the real review, fill in and run /update-profile for each stakeholder:
- Predictions that matched: [list]
- Surprises (not predicted): [list]
- Predicted but did not come up: [list]

/stakeholder-radar /update-profile {names}
```

---

## `/synthesize`

### Inputs
- Review output file path (required). A simulation output from a prior /simulate run.

### Process

1. Read the simulation output file.
2. Parse each stakeholder's individual review for objections, questions, approvals, and blockers.
3. Cross-reference objections and questions across stakeholders. Count how many stakeholders flagged each issue.
4. Rank issues by stakeholder recurrence (highest first).
5. Produce the ranked action list.
6. Save alongside the review file as `{doc-slug}/{timestamp}-synthesis.md`.

### Output Format

```
RANKED ISSUES (by stakeholder recurrence):
[N/N stakeholders flagged] Issue description
  -> Recommended document change

RANKED QUESTIONS TO PRE-ANSWER IN DOCUMENT:
[N/N stakeholders asked] Question
  -> Suggested section or addition

CONFIDENCE FLAGS:
Sections where low-profile-confidence means the simulation may be unreliable.
```

Print to terminal and save alongside the review file.

---

## `/audit-profile`

### Inputs
- Profile name (required). The firstname-lastname slug of an existing profile.

### Process

1. Load the profile from `profiles/{name}.md`.
2. For each schema section (Identity, Decision Lens, Recurring Objections, Recurring Questions, Priorities, Blind Spots, Communication Style), count the number of evidence citations and which artifact types contributed.
3. Identify gaps: sections with fewer than 2 citations or missing artifact type coverage.
4. Generate recommendations for which artifact types to collect next, prioritized by which gaps would increase confidence the most.

### Output Format

| Section | Evidence Count | Artifact Types Used | Gap |
|---|---|---|---|
| Recurring Objections | N | meeting-notes, email | Missing: slack, doc-comments |

Recommendations: what artifact types to collect next to raise confidence.

---

## Example Usage

```
# PM preparing for a PRD review
/stakeholder-radar /build-profile
Name: Priya Sharma, Engineering Manager
Artifacts: notes/q1-planning-priya.md (meeting-notes), slack/priya-prd-thread.txt (slack)

# Engineer preparing an architecture decision for review
/stakeholder-radar /build-profile
Name: Marcus Chen, VP Engineering
Artifacts: docs/marcus-adr-comments.txt (review-thread), notes/arch-review-q4.md (meeting-notes)

# Bootstrap a profile with minimal artifacts
/stakeholder-radar /build-profile
Name: Amir Cohen, Director of Product
Artifacts: emails/amir-feedback-jan.txt (email)
(will use Director of Product archetype as weak prior)

# Designer preparing a design spec for critique
/stakeholder-radar /simulate
Document: docs/checkout-redesign-spec.md
Profiles: priya-sharma, marcus-chen, amir-cohen
Mode: cold

# Program manager simulating a budget review
/stakeholder-radar /simulate
Document: docs/q3-budget-proposal.md
Profiles: cfo-sarah-lee, vp-eng-marcus-chen
Mode: warm

# Update a profile after a real review
/stakeholder-radar /update-profile priya-sharma
New artifacts: docs/priya-design-review.txt (doc-comment)

# Stack simulation output into ranked action items
/stakeholder-radar /synthesize reviews/checkout-redesign-spec/2026-04-03.md

# Check evidence gaps
/stakeholder-radar /audit-profile priya-sharma
```
