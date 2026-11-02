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

It inspects the session (your messages, repo, branch, diff), gathers your installed skills, then **searches GitHub** (repos tagged `claude-skills`, `SKILL.md` files, "awesome" lists, with a web-search fallback) for community skills that fit. It reads each candidate's `SKILL.md`, vets third-party ones for red flags (remote script execution, secret access, etc.), and returns 5 (minimum) to 10 ranked picks. Each pick shows its origin (Anthropic / plugin / yours / community), install status, risk, source link, and a one-line install command.

Preferences you can add to your question: `official only`, `installed only`, or `include community` (default).

It never installs anything without your explicit yes. Requires the `gh` CLI for the best results; falls back to web search, then to installed skills only.
