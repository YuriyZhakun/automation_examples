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

---

## Other Automations

Smaller workflows covering a variety of use cases.

---

### [CV Submission](./other-automations/CV%20Submission.json)

Automated CV screening and candidate routing triggered by a Tally form submission.

When a candidate submits the form, the workflow downloads the attached PDF, extracts the text via OCR.space, and uses Mistral Medium to parse structured data (name, skills, years of experience). It then queries Airtable for open job positions and runs an AI Agent to check if the candidate meets the criteria (≥75% skills match and sufficient experience). Matching candidates are added to Airtable and HR is notified by email; non-matching candidates receive an automated rejection email.

**Required credentials:** Mistral Cloud account, Airtable Personal Access Token, Gmail OAuth2

---

### [Email Support Agent](./other-automations/Email%20Support%20Agent.json)

Automated email support agent for a real estate platform.

The workflow polls Gmail INBOX every minute and uses Mistral Medium to classify incoming emails as either a `report` request (office analytics) or a `location` request (listing info). For report requests, it runs two PostgreSQL queries — overall office engagement stats and the top 3 listings by pageviews — and sends an HTML summary back to the sender. For location requests, it fetches listing data from the database, reverse-geocodes the coordinates via BigDataCloud, optionally converts the price to a target currency via the Frankfurter API, and replies with the location and price details. All processed emails are logged to Google Sheets with a success or error outcome.

**Required credentials:** Gmail OAuth2, Mistral Cloud account, PostgreSQL, Google Sheets OAuth2

---

### [Holidays Request](./other-automations/Holidays%20Request.json)

Webhook API that returns upcoming public holidays for a given country and date.

Accepts `country_name`, `after_date` (YYYY-MM-DD), and an optional `formatter` parameter via a basic-auth protected webhook. The workflow validates the inputs, resolves the country name to an ISO code via the Nager.at API, fetches public holidays for that year, and filters them to only include dates after the specified cutoff. An optional formatter can be applied to holiday names: `alternating_case_formatter`, `uppercase_formatter`, or `lowercase_formatter`. The result is returned as a JSON response.

**Required credentials:** HTTP Basic Auth

---

### CSV Script

Node.js script (not an n8n workflow) that fetches users, posts, and comments from the JSONPlaceholder API and writes the results to a CSV file.

Filters users with even IDs, fetches the latest 5 posts per user and the latest 3 comments per post. Uses concurrent requests with `Promise.all`, automatic retries with exponential backoff, data validation, and structured logging. Output is written to `output.csv`.

Source: [CSV script (GitHub)](https://github.com/YuriyZhakun/automation_examples)
