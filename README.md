# Claude Code Plugins

A collection of plugins for [Claude Code](https://claude.ai/code) with tools for Git workflows and web security analysis.

## Plugins

| Plugin | Description |
|--------|-------------|
| [`git-tools`](./git-tools) | Git productivity tools — PR review and more |
| [`websec`](./websec) | Web security tools — dependency vulnerability analysis and more |

## Installation

### Option 1 — Permanent install (recommended)

Add the marketplace and install the plugins you want:

```bash
# add the marketplace (only needed once)
claude plugin marketplace add marcoscouto/claude-plugins-example

# install the plugins you want
claude plugin install git-tools@plugins-example
claude plugin install websec@plugins-example
```

### Option 2 — Session only (local clone)

If you already have the repo cloned, load plugins for a single session without installing:

```bash
claude --plugin-dir ~/path/to/claude-code-plugins/git-tools \
       --plugin-dir ~/path/to/claude-code-plugins/websec
```

### Managing plugins

```bash
claude plugin list                  # list installed plugins
claude plugin list --available --json  # list available plugins from marketplaces
claude plugin disable git-tools     # disable a plugin
claude plugin enable git-tools      # re-enable a plugin
claude plugin uninstall git-tools   # uninstall a plugin
```

## Usage

### git-tools

#### `/review-pr`

Reviews a GitHub Pull Request — analyzes code changes, flags issues, and gives actionable feedback.

```
/review-pr 123
/review-pr https://github.com/owner/repo/pull/123
```

### websec

#### `/dependencies-analysis`

Analyzes project dependencies for known security vulnerabilities using available audit tools (npm audit, pip-audit, cargo audit, etc.).

```
/dependencies-analysis
```

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for how to create new plugins, skills, and commands.
