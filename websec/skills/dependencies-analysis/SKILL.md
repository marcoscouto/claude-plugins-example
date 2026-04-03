---
name: dependencies-analysis
description: Analyze project dependencies for known security vulnerabilities using available audit tools
user-invocable: true
---

Analyze the dependencies of the current project for security vulnerabilities.

**Step 1 — Detect the project type**

Check which package manifests exist:
- `package.json` → Node.js (npm/yarn/pnpm)
- `requirements.txt` / `pyproject.toml` / `Pipfile` → Python
- `Gemfile` → Ruby
- `go.mod` → Go
- `Cargo.toml` → Rust
- `pom.xml` / `build.gradle` → Java/Kotlin

A project may have multiple ecosystems — analyze all of them.

**Step 2 — Run the appropriate audit tool for each ecosystem**

- **npm**: `npm audit --json`
- **yarn**: `yarn audit --json`
- **pnpm**: `pnpm audit --json`
- **Python (pip)**: `pip-audit --format json` (install with `pip install pip-audit` if missing)
- **Python (poetry)**: `poetry audit`
- **Ruby**: `bundle audit check --update`
- **Go**: `govulncheck ./...` (install with `go install golang.org/x/vuln/cmd/govulncheck@latest` if missing)
- **Rust**: `cargo audit`
- **Java/Maven**: `mvn dependency-check:check`
- **Java/Gradle**: `./gradlew dependencyCheckAnalyze`

If a tool is not installed and cannot be installed automatically, note it and proceed with the others.

**Step 3 — Compile and present results**

For each vulnerability found, report:

| Field | Detail |
|---|---|
| Package | Name and version |
| Severity | Critical / High / Medium / Low |
| CVE / Advisory | ID and link if available |
| Description | What the vulnerability is |
| Fix | Upgrade path or recommended action |

**Step 4 — Prioritization**

Group findings by severity:
1. 🔴 **Critical** — must fix immediately
2. 🟠 **High** — fix before next release
3. 🟡 **Medium** — schedule fix
4. 🟢 **Low / Informational** — track and review

**Step 5 — Recommended actions**

Provide a concrete fix list, e.g.:
- `npm update <package>` or `npm audit fix`
- Specific version pins for packages without auto-fix
- Packages with no fix available (with suggested alternatives if applicable)

If no vulnerabilities are found, confirm the project is clean and state which tools and versions were used.
