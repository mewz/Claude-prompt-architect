# prompt-architect

A single Claude Code skill that turns plain-English task descriptions into structured, production-ready prompts and then executes them after user approval.

## Repo layout

```
prompt-architect/
└── prompt-architect/
    └── SKILL.md        # The skill definition (frontmatter + body)
```

There is no source code, build system, or test suite. The whole project is the skill file.

## What the skill does

`prompt-architect/SKILL.md` defines a four-phase loop:

1. **Draft** — turn the user's natural-language ask into a structured prompt (role, context, requirements, I/O spec, constraints) shown in a fenced code block.
2. **Revise** — when the user requests changes, regenerate the *full* prompt (never a fragment) and ask for approval again.
3. **Execute** — once the user approves ("run it", "go ahead", "approved", etc.), follow the prompt's instructions directly: create files, run scripts, produce the requested artifacts.
4. **Iterate** — diagnose post-execution feedback as either a prompt problem (revise + re-run) or an execution bug (fix the output directly).

The skill is triggered by phrases like "write a prompt for…", "build me a prompt that…", "I need Claude Code to…", or follow-ups like "execute the prompt", "approve", "change the prompt to…".

## Working on the skill

When editing `SKILL.md`:

- The `description` in frontmatter is what the harness uses to decide when to invoke the skill — keep trigger phrases concrete and exhaustive.
- The body is the runtime instruction set. Keep phases, intent-recognition table, and "Important Behaviors" in sync if you change one.
- Skill files are not validated by any tooling here; correctness is judged by behavior when the skill is invoked.

## Installing the skill locally

To make the skill available to Claude Code on this machine, symlink or copy the `prompt-architect/` directory into `~/.claude/skills/`:

```
ln -s "$(pwd)/prompt-architect" ~/.claude/skills/prompt-architect
```

Then restart Claude Code so it re-scans skills.
