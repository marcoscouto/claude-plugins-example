# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A marketplace of Claude Code plugins. Each plugin lives in its own directory and is listed in `.claude-plugin/marketplace.json`.

## Plugin structure

Every plugin follows this layout:

```
<plugin-name>/
├── .claude-plugin/
│   └── plugin.json          # name, description, version
├── commands/                # slash commands (user-invocable, argument-driven)
│   └── <command-name>.md
└── skills/                  # structured multi-step prompts
    └── <skill-name>/
        └── SKILL.md
```

**Commands** (`commands/*.md`) — simple slash commands, always user-invocable, accept `$ARGUMENTS`.
Frontmatter: `description` only.

**Skills** (`skills/<name>/SKILL.md`) — richer prompts with structured steps. Can be invoked by users (`user-invocable: true`, default) or by Claude automatically based on `description`.
Key frontmatter fields: `name`, `description`, `user-invocable`, `argument-hint`, `allowed-tools`, `model`, `effort`.

## Adding a plugin

1. Create `<plugin-name>/.claude-plugin/plugin.json`
2. Add commands and/or skills
3. Register the plugin in `.claude-plugin/marketplace.json`
4. Update the plugin table in `README.md`

## Marketplace registration

`.claude-plugin/marketplace.json` is the registry. Each entry needs: `name`, `description`, `category`, `source` (relative path), `homepage`.
