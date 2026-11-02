# skill-recommender

A Claude Code skill that answers **"what skills do you prefer for this session?"** with a ranked list of 5–10 skills tailored to the current work.

## Install

Project-level (already in this repo):

```
.claude/skills/skill-recommender/SKILL.md
```

Or personal / all projects:

```bash
mkdir -p ~/.claude/skills && cp -r .claude/skills/skill-recommender ~/.claude/skills/
```

## Use

Ask Claude any of:

- "What skills do you prefer for this session?"
- "Which skills should I use here?"
- `/skill-recommender`

It inspects the session (your messages, repo, branch, diff), gathers real installed skills, ranks them, and returns 5 (minimum) to 10 with a reason and timing for each.
