# Claude Code Plugins

A collection of plugins for [Claude Code](https://claude.ai/code) with tools for Git workflows, web security analysis, and architecture documentation.

## Plugins

| Plugin | Description |
|--------|-------------|
| [`git-tools`](./git-tools) | Git productivity tools — PR review and more |
| [`websec`](./websec) | Web security tools — dependency vulnerability analysis and more |
| [`arch-tools`](./arch-tools) | Architecture analysis — ubiquitous language, technical overview, and Excalidraw diagrams |

## Installation

### Option 1 — Permanent install (recommended)

Add the marketplace and install the plugins you want:

```bash
# add the marketplace (only needed once)
claude plugin marketplace add marcoscouto/claude-plugins-example

# install the plugins you want
# note: claude-plugins-example becomes "plugins-example" after the "claude-" prefix is stripped
claude plugin install git-tools@plugins-example
claude plugin install websec@plugins-example
claude plugin install arch-tools@plugins-example
```

### Option 2 — Session only (local clone)

If you already have the repo cloned, load plugins for a single session without installing:

```bash
claude --plugin-dir ~/path/to/claude-code-plugins/git-tools \
       --plugin-dir ~/path/to/claude-code-plugins/websec \
       --plugin-dir ~/path/to/claude-code-plugins/arch-tools
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

### arch-tools

#### `/arch-analyze`

Analyzes the architecture of one or more projects — generates a ubiquitous language document, a technical overview, and optionally an Excalidraw diagram.

```
# analyze current directory
/arch-analyze

# analyze a specific project
/arch-analyze ./my-project

# analyze with diagram
/arch-analyze ./my-project --draw

# analyze multiple projects with diagram
/arch-analyze ./serviceA ./serviceB --draw
```

**Generated files:**

| File | Description |
|------|-------------|
| `ubiquitous-language.md` | Bounded contexts, domain terms, events, and business rules |
| `technical-overview.md` | Stack, architecture style, components, data flow, and inter-project relations |
| `arch-diagram.excalidraw` | Visual flow diagram (only with `--draw`) — open in [excalidraw.com](https://excalidraw.com) or the VS Code extension |

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for how to create new plugins, skills, and commands.
