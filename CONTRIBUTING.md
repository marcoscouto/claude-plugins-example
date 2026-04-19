# Contributing

## Plugin structure

Each plugin lives in its own directory with a mandatory manifest:

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json       # required — plugin manifest
├── commands/             # slash commands (user-invocable prompts)
│   └── my-command.md
└── skills/               # skills (agent-invocable or user-invocable)
    └── my-skill/
        └── SKILL.md
```

### `plugin.json`

```json
{
  "name": "my-plugin",
  "description": "Short description of what this plugin does",
  "version": "1.0.0"
}
```

---

## Creating a command

Commands are slash commands that users can invoke directly (e.g. `/review-pr 123`).

Create a Markdown file inside `commands/`:

```
my-plugin/commands/my-command.md
```

### Frontmatter

```markdown
---
description: One-line description shown in the command list
---
```

### Body

Write the prompt that Claude will execute. Use `$ARGUMENTS` to capture whatever the user types after the command name.

### Example — `git-tools/commands/review-pr.md`

```markdown
---
description: Review a GitHub Pull Request — analyzes code changes, flags issues, and gives actionable feedback
---

Review the Pull Request: $ARGUMENTS

Use the `gh` CLI to fetch the PR details and diff. Follow these steps:

1. Run `gh pr view $ARGUMENTS --json title,body,author,baseRefName,headRefName` to get PR metadata.
2. Run `gh pr diff $ARGUMENTS` to get the full diff.
3. Run `gh pr view $ARGUMENTS --json comments,reviews` to get existing review comments.

Then perform a thorough code review covering bugs, edge cases, security issues,
code quality, and test coverage.

Format the output in clear Markdown sections.
```

**Invoked as:** `/review-pr 123`

---

## Creating a skill

Skills are reusable prompt modules. They can be user-invocable (appear as slash commands) or agent-invocable (called internally by Claude).

Create a directory inside `skills/` with a `SKILL.md` file:

```
my-plugin/skills/my-skill/
└── SKILL.md
```

### Frontmatter

```markdown
---
name: my-skill
description: One-line description used to match when the skill is relevant
user-invocable: true   # set to false to hide from the slash command list
---
```

### Body

Write the detailed instructions Claude will follow when the skill is triggered.

### Example — `websec/skills/dependencies-analysis/SKILL.md`

```markdown
---
name: dependencies-analysis
description: Analyze project dependencies for known security vulnerabilities using available audit tools
user-invocable: true
---

Analyze the dependencies of the current project for security vulnerabilities.

**Step 1 — Detect the project type**

Check which package manifests exist:
- `package.json` → Node.js
- `requirements.txt` / `pyproject.toml` → Python
- `Cargo.toml` → Rust

**Step 2 — Run the appropriate audit tool**

- npm: `npm audit --json`
- Python: `pip-audit --format json`
- Rust: `cargo audit`

**Step 3 — Report findings grouped by severity**

🔴 Critical → 🟠 High → 🟡 Medium → 🟢 Low
```

**Invoked as:** `/dependencies-analysis`

---

## Commands vs Skills

| | Command | Skill |
|---|---|---|
| File location | `commands/*.md` | `skills/<name>/SKILL.md` |
| Frontmatter `name` | Not required (filename is the name) | Required |
| Accepts arguments | Yes — via `$ARGUMENTS` | Yes — via `$ARGUMENTS` if `user-invocable: true` |
| User-invocable | Always | Only if `user-invocable: true` |
| Agent-invocable | No | Yes |

Use a **command** when the interaction is simple and argument-driven (e.g. a PR number).
Use a **skill** when Claude should be able to trigger it automatically based on context, or when the prompt is complex enough to warrant structured multi-step instructions.

Skills can also accept arguments and flags when `user-invocable: true`. Parse `$ARGUMENTS` manually inside the skill body (see the `arch-analyze` example below).

### Example — `arch-tools/skills/arch-analyze/SKILL.md` (skill with arguments and flags)

```markdown
---
name: arch-analyze
description: Analyze the architecture of one or more projects — generates ubiquitous language docs, technical overview, and optionally an Excalidraw diagram
user-invocable: true
---

Analyze the architecture of the project(s) specified in: $ARGUMENTS

**Parsing arguments**

Parse `$ARGUMENTS` as follows:
- Extract all path arguments (everything that is not a flag). Default to current directory if none.
- Check if `--draw` flag is present. If yes, also generate an Excalidraw diagram.

**Step 1 — Explore each project**
...

**Step 2 — Generate `ubiquitous-language.md`**
...

**Step 3 — Generate `technical-overview.md`**
...

**Step 4 — Generate Excalidraw diagram (only if `--draw` was given)**
...
```

**Invoked as:** `/arch-analyze ./serviceA ./serviceB --draw`

---

## Adding a new plugin to this repo

1. Create a directory for your plugin:
   ```bash
   mkdir -p my-plugin/.claude-plugin
   ```

2. Add the manifest:
   ```bash
   cat > my-plugin/.claude-plugin/plugin.json << 'EOF'
   {
     "name": "my-plugin",
     "description": "What this plugin does",
     "version": "1.0.0"
   }
   EOF
   ```

3. Add commands and/or skills following the examples above.

4. Validate the plugin:
   ```bash
   claude plugin validate ./my-plugin
   ```

5. Test it locally before opening a PR:
   ```bash
   claude --plugin-dir ./my-plugin
   ```

6. Update the plugin table in [README.md](./README.md).

7. Open a pull request.
