# n8n Automation Examples

Examples of n8n workflows. To use any of them:

1. Open your n8n instance
2. Go to **Workflows → Import from file** (or paste JSON via Ctrl+C / Ctrl+V)
3. Add your credentials
4. Run and test

---

## [Doc Editor](./doc-editor/Doc%20Editor.json)

Automated QA agent for Markdown documentation in a GitHub repository.

The workflow reads a Markdown file from GitHub, runs it through two Mistral Large agents that find and fix issues (grammar, formatting, code quality), then commits the corrected file back. A run log is saved in `qa-report.md` so the agent remembers what was already fixed.

**Required credentials:** GitHub account, Mistral Cloud account

---

## [Issue Tracker](./issue-tracker/Issue%20Tracker.json)

AI-powered GitHub issue tracker with semantic search via Telegram.

The workflow has two main parts:

**Notification + Autosync** — listens for GitHub issue events (opened, closed, edited, reopened, deleted) via webhook. New and updated issues are embedded using the Gemini Embedding API (`gemini-embedding-001`, 768 dimensions) and upserted into a Supabase vector store (PostgreSQL + pgvector). Closed issues trigger a Telegram notification. Deleted issues are removed from Supabase to keep the vector store in sync. A manual trigger lets you do a one-time bulk import of all existing issues.

**Semantic Search** — a Telegram bot that accepts `/search <query>` commands, embeds the query with Gemini, runs a cosine similarity search against Supabase via an RPC function, and returns the top matching tickets with similarity scores. Results below the relevance threshold (0.60 best / 0.55 secondary) are suppressed.

**Required credentials:** GitHub OAuth2, Telegram Bot API, Google Gemini API key, Supabase API key
