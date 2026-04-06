# Stakeholder Radar

LLM skill for evidence-based stakeholder profiles and document review simulation. Builds profiles from real artifacts (meeting notes, Slack threads, emails, review threads), then simulates how reviewers will react to a document before the real review.

## Usage

Invoke with `/stakeholder-radar`. The skill is auto-discovered from `.claude/skills/stakeholder-radar/SKILL.md`.

```
/stakeholder-radar build-profile "Priya Sharma, Engineering Manager" artifacts/
/stakeholder-radar simulate spec.md --profiles profiles/
/stakeholder-radar synthesize spec.md --profiles profiles/
```

## Commands

| Command | What it does |
|---|---|
| `build-profile` | Create a stakeholder profile from artifacts |
| `update-profile` | Add new evidence to an existing profile |
| `simulate` | Simulate a document review from a stakeholder's perspective |
| `synthesize` | Combine multiple simulated reviews into prioritized action items |
| `audit-profile` | Check a profile for staleness, gaps, or contradictions |

## Syncing with Workspace

If you also use this skill inside a larger claude-workspace monorepo:

```bash
./sync.sh pull   # Copy FROM workspace INTO this repo
./sync.sh push   # Copy FROM this repo INTO workspace
```

Set `WORKSPACE_ROOT` if your workspace is not at `~/claude`.
