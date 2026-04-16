# prompt-architect

A Claude Code skill that turns plain-English task descriptions into structured, production-ready prompts — and then executes them after you approve.

Instead of writing a detailed prompt yourself (or using a separate AI to draft one for Claude), you describe what you want in casual English. The skill drafts a precise prompt with role, context, requirements, I/O spec, and constraints; you review it; you approve; it runs.

## Install

This skill lives in `~/.claude/skills/` so Claude Code can discover it.

### Option 1: Symlink (recommended — updates follow the repo)

```bash
git clone https://github.com/mewz/Claude-prompt-architect.git
cd Claude-prompt-architect
ln -s "$(pwd)/prompt-architect" ~/.claude/skills/prompt-architect
```

### Option 2: Copy

```bash
git clone https://github.com/mewz/Claude-prompt-architect.git
cp -r Claude-prompt-architect/prompt-architect ~/.claude/skills/prompt-architect
```

Then restart Claude Code so it re-scans skills.

### Verify installation

In Claude Code, run `/help` or start typing `/prompt-architect` — the skill should appear in the list.

## Usage

Just describe what you want in plain English. Trigger phrases the skill listens for:

- "Write a prompt for…"
- "Build me a prompt that…"
- "I need Claude Code to…"
- "Make a prompt that…"
- "Design a prompt for…"
- "Prompt architect: …"

### The four-phase loop

1. **Draft** — You describe the task. The skill produces a structured prompt in a fenced code block and asks for approval.
2. **Revise** — Ask for changes in plain English ("make it more formal", "also handle blank rows"). The skill regenerates the *full* prompt and asks again.
3. **Execute** — Say "run it", "approved", "go ahead", or "looks good". The skill follows the prompt's instructions: creates files, runs scripts, produces artifacts.
4. **Iterate** — If the output isn't right, say what's wrong. The skill decides whether to revise the prompt or fix the output directly.

### Example session

> **You:** Write a prompt for Claude Code that reads `students.csv` and generates a personalized email for each student, saved as `.txt` files named by student ID.

> **Skill:** *(drafts a structured prompt with role, input spec, processing rules, output format, and edge-case handling, then asks:)* Ready to execute this, or would you like any changes?

> **You:** Add a rule to skip rows where the email column is blank.

> **Skill:** *(regenerates the full prompt with the new rule and asks for approval again.)*

> **You:** Looks good, run it.

> **Skill:** *(creates the script, runs it against `students.csv`, and shows the output files.)*

## Repo layout

```
prompt-architect/
├── README.md
├── CLAUDE.md               # Guidance for agents editing this repo
└── prompt-architect/
    └── SKILL.md            # The skill definition (frontmatter + runtime instructions)
```

The whole project is the skill file — there's no build system or dependencies.

## Editing the skill

Everything lives in `prompt-architect/SKILL.md`:

- The `description` in frontmatter is what the Claude Code harness uses to decide when to invoke the skill. Keep trigger phrases concrete and exhaustive.
- The body is the runtime instruction set Claude follows. If you change one phase, keep the intent-recognition table and "Important Behaviors" section in sync.

After editing, restart Claude Code (or reload skills) to pick up changes.

## License

MIT
