# Stakeholder Radar

Simulate how your reviewers will react to a document before you share it.

Build evidence-based stakeholder profiles from real artifacts (meeting notes, Slack threads, emails, doc comments, review threads, presentations). Run simulations against any document type: PRDs, proposals, briefs, budgets, strategy docs, design specs, architecture decisions, slide decks, policy changes, or roadmaps.

## What It Does

Traditional stakeholder mapping captures titles and influence levels. Stakeholder Radar captures how people actually behave in reviews: what they push back on, what they champion, what they miss, and how their positions shift with new evidence.

Five commands:

- **build-profile**: Create a stakeholder profile from artifacts you provide
- **update-profile**: Add new evidence to an existing profile
- **simulate**: Run a document through one or more stakeholder profiles and get predicted feedback
- **synthesize**: Combine predictions across stakeholders into a prioritized preparation plan
- **audit-profile**: Check profile quality and flag gaps in evidence coverage

## Install

Copy `SKILL.md` into your project's `.claude/skills/stakeholder-radar/` directory:

```bash
mkdir -p .claude/skills/stakeholder-radar
cp SKILL.md .claude/skills/stakeholder-radar/SKILL.md
```

Works with Claude Code. LLM-agnostic design: the skill file is a standalone system prompt usable in any LLM that supports structured instructions.

## Usage

```
/stakeholder-radar build-profile    # from meeting notes, emails, Slack threads
/stakeholder-radar simulate         # predict reactions to your document
/stakeholder-radar synthesize       # combine predictions into prep plan
```

## How It Works

Profiles are built from real artifacts, not assumptions. Each claim about a stakeholder's behavior is grounded in specific evidence with timestamps and context. Confidence levels (high, medium, low) reflect evidence density.

Simulations predict feedback per section of your document, flagging likely objections, questions, and areas of support. The synthesis step prioritizes across stakeholders so you know which concerns to address first.

## License

MIT
