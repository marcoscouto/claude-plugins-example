---
description: Review a GitHub Pull Request — analyzes code changes, flags issues, and gives actionable feedback
---

Review the Pull Request: $ARGUMENTS

Use the `gh` CLI to fetch the PR details and diff. Follow these steps:

1. Run `gh pr view $ARGUMENTS --json title,body,author,baseRefName,headRefName,additions,deletions,changedFiles` to get PR metadata.
2. Run `gh pr diff $ARGUMENTS` to get the full diff.
3. Run `gh pr view $ARGUMENTS --json comments,reviews` to get existing review comments.

Then perform a thorough code review covering:

**Summary**
- What this PR does (based on title, description, and diff)
- Author and target branch

**Code Analysis**
- Logic errors or bugs
- Edge cases not handled
- Performance concerns
- Security issues (injection, auth bypass, secrets in code, etc.)
- Breaking changes

**Code Quality**
- Naming clarity
- Duplication or missed abstractions
- Dead code or unnecessary complexity

**Tests**
- Are the changes covered by tests?
- Are existing tests likely to break?

**Final Verdict**
Give one of: ✅ Approve / 🔄 Request Changes / 💬 Comment
Summarize the top 3 action items if requesting changes.

Format the output in clear Markdown sections.
