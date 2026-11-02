---
name: skill-recommender
description: Recommends the best skills for the current session. Use when the user asks "what skills do you prefer for this session", "which skills should I use", "what skills would help here", "best skills for this task", or any similar request to pick or rank skills for the current work. Always returns at least 5 and, when enough fit, up to 10 ranked skills.
---

# Skill Recommender

When the user asks which skills you prefer, recommend, or would use for this session, answer with a **ranked list of 5–10 skills** (minimum 5, aim for 10), tailored to what the session is actually about.

## Step 1 — Understand the session

Before recommending anything, work out what the session is for. Use, in order:

1. The user's messages so far and any stated goal.
2. The repo: look at the working directory (`ls`, README, manifest files such as `package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`), the current git branch, and recent commits or diff (`git status`, `git log --oneline -10`).
3. Tools and files already in play (tests, CI config, docs, notebooks, spreadsheets, PDFs, slides).

Summarize the session in one sentence (e.g. "TypeScript web app, fixing a bug on a feature branch"). If the session is empty and there is no repo context, say so and recommend a general-purpose set.

## Step 2 — Collect candidate skills

Build the candidate pool from skills that actually exist, in this order:

1. The skills listed in your system context (the "available skills" list shown with the Skill tool).
2. If a `ListSkills` tool is available (it may be deferred; load it with `ToolSearch` using `select:ListSkills`), call it for the full, current list.
3. Project and user skills on disk: `.claude/skills/*/SKILL.md` and `~/.claude/skills/*/SKILL.md` (read the `name` and `description` frontmatter only).
4. If fewer than 5 installed skills fit, call `SearchSkills`/`SuggestSkills` (load via `ToolSearch` if deferred) to find installable ones, and mark them clearly as **not installed yet**.

Never invent skill names. Only recommend skills that exist in one of the sources above.

## Step 3 — Score and rank

Score each candidate on:

- **Fit** — does its description match the session's goal and file types? (most important)
- **Leverage** — does it save substantial time or prevent costly mistakes (review, security, testing)?
- **Timing** — is it useful now, or only later (e.g. a release skill during early prototyping)?
- **Cost of misuse** — skip skills that are risky or noisy for this context.

Pick the top 10. If fewer than 10 are genuinely useful, return fewer — but never fewer than 5. To reach 5, widen to broadly useful skills (code review, simplify, init/CLAUDE.md, security review, loop, deep research, docs) and say honestly that they are general-purpose picks.

## Step 4 — Respond

Use exactly this shape:

```
**Session read:** <one sentence on what this session is about>

**My preferred skills for this session**

1. `/skill-name` — <why it fits this session, one line> · *when:* <the moment to use it>
2. ...
...
10. ...

**Start with:** `/<best first skill>` — <one-line reason>
**Skipping:** <skills you considered and left out, and why — optional, one line>
```

Rules:

- Between 5 and 10 entries, ranked best first. Number them.
- Each entry names the skill exactly as listed (with its plugin prefix if it has one, e.g. `anthropic-skills:pdf`).
- The reason must reference the *session*, not just restate the skill's description.
- Flag any entry that is not installed and offer to install it.
- Keep it scannable: no preamble, no long paragraphs.
- Offer to invoke the top skill, but do not run any of them unless the user asks.

## Example

User: "what skills do you prefer for this session?"
Context: Python repo with failing pytest, branch `fix/parser-bug`.

```
**Session read:** Debugging a failing parser test in a Python project on a fix branch.

**My preferred skills for this session**

1. `/code-review` — verifies the parser fix for correctness bugs before you push · *when:* after the fix passes tests
2. `/simplify` — trims the patch and nearby code once it works · *when:* after review
3. `/security-review` — parser input handling is a common injection/DoS surface · *when:* before merging
4. `/init` — no CLAUDE.md yet; capturing test commands saves time on later sessions · *when:* now, quickly
5. `/fewer-permission-prompts` — repeated pytest/git calls will keep prompting · *when:* early
6. `/loop` — re-run the test suite on an interval while iterating · *when:* during the fix
7. `/run` — drive the real app to confirm the fix end to end · *when:* after tests pass

**Start with:** `/init` — cheap, and every later step benefits.
```
