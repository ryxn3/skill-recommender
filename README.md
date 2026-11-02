# skill-recommender

A Claude Code skill that answers **"what skills do you prefer for this session?"** with a ranked list of 5–10 skills tailored to the current work.

## Install with a prompt

Paste this into Claude Code and it installs the skill for you:

```text
Install the "skill-recommender" skill from https://github.com/ryxn3/claude-whats-needed-skill.
Download .claude/skills/skill-recommender/SKILL.md from that repo and save it to
~/.claude/skills/skill-recommender/SKILL.md (create the folder if needed).
Then read the file back to confirm it has a name and description in its frontmatter,
and tell me how to trigger it.
```

To install it for just one project, replace `~/.claude/skills/` with `.claude/skills/` in the prompt.

Prefer a shell one-liner?

```bash
mkdir -p ~/.claude/skills/skill-recommender && curl -fsSL \
  https://raw.githubusercontent.com/ryxn3/claude-whats-needed-skill/main/.claude/skills/skill-recommender/SKILL.md \
  -o ~/.claude/skills/skill-recommender/SKILL.md
```

(This fetches from the `main` branch, so it works once this change is merged there.)

## Install manually

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
