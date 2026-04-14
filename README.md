# n8n Automation Examples

Examples of n8n workflows. To use any of them:

1. Open your n8n instance
2. Go to **Workflows → Import from file** (or paste JSON via Ctrl+C / Ctrl+V)
3. Add your credentials
4. Run and test

---

## [Longship QA](./longship-qa/Longship%20QA.json)

Automated QA agent for Markdown documentation in a GitHub repository.

The workflow reads a Markdown file from GitHub, runs it through two Mistral Large agents that find and fix issues (grammar, formatting, code quality), then commits the corrected file back. A run log is saved in `qa-report.md` so the agent remembers what was already fixed.

**Required credentials:** GitHub account, Mistral Cloud account
