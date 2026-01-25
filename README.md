# n8n_backup

Comprehensive backup of exported n8n workflows and related workflow assets. This repository stores exported workflow JSON files that can be imported back into an n8n instance for restoration, reuse, or documentation purposes.
---

Table of contents
- About
- Repository structure and purpose
- Per-file summary (inferred)
- How to restore/import a workflow into n8n
- Automating backups (examples: script + GitHub Actions)
- Security and handling secrets
- Recommended naming and versioning conventions
- Maintenance and validation checks
- Troubleshooting
- Contributing
- License and notes

---

About

This repository (n8n_backup) is a collection of exported n8n workflow JSON files and workflow folders. Each JSON file is typically an export of a single n8n workflow that contains the workflow metadata, nodes, connections and (sometimes) credentials references. The goal of this README is to provide:
- A clear description of what lives in this repo;
- Per-file explanations (inferred from filenames) so you can quickly identify important workflows;
- Practical instructions for importing these workflows into an n8n instance;
- Recommendations for automating, securing and maintaining backups.

Repository structure and purpose

- Location: root of repository contains exported .json workflow files and a few directories.
- Purpose: human-readable, versioned backups of n8n workflows. These backups allow you to:
  - Restore workflows to a running n8n instance;
  - Inspect workflow logic offline;
  - Share or template automations between environments.

Note about API listing used to create this README
- The file list used to build the per-file summary was obtained via the GitHub Contents API call and may be incomplete due to API limitations. Verify the repository contents on GitHub: https://github.com/rajeetcodeN/n8n_backup/tree/main

Per-file summary (inferred from filenames)\n
Below are the files and directories found when listing the repository root. Descriptions are inferred from the file names and typical n8n use-cases. For exact details (nodes, triggers, credentials) open the JSON and inspect its top-level fields (name, nodes, connections, credentials).

Files (selected list with short descriptions)
- Brand profiler.json — Workflow likely used to gather and profile brand information (scrape, enrich, store).
- COST CALCULATION.json — Workflow to compute costs; could be invoice processing or aggregate cost calculations.
- Internal knowledge RAG and chat agent.json — Retrieval-augmented-generation pipeline integrated with a chat agent and knowledge storage.
- MISTRAL OCR.json — OCR processing workflow (image/PDF -> text) using Mistral or another OCR provider.
- agent.json — Generic orchestration agent workflow (API calls, multi-step automation).
- ai chat.json / chat.json / chat test.json — Chatbot or AI integration workflows (LLM interaction, message routing).
- analytics.json — Ingest or transform analytics events or metrics for a dashboard.
- backup.json — Possibly a workflow that exports workflows or triggers backups.
- banner.json — Automation for generating or posting banners (marketing).
- blogs agent.json — Blog publication, scraping, or drafting automation.
- carousel.json / carousel n8n.json — Posting image carousels to social platforms.
- community temp.json — Temporary or experimental community-related workflow.
- dashboard.json — Prepares and pushes data to dashboards or monitoring systems.
- data loader.json — ETL-style workflow to load data into databases or storage.
- dbt ai knowledge bot.json & other dbt-related files — dbt-related automations (documentation, model checks, notifications).
- emai send.json — Sends email messages (typo in filename 'emai' — verify content).
- facebook.json / fb.json — Facebook posting or ingestion flows.
- gemini.json / gemini test.json — Integrations/testing with 'Gemini' or an LLM API.
- git hub backup.json — Workflow that likely backs up repositories or pushes exports to GitHub.
- image.json — Image processing workflow (resize, OCR, thumbnailing).
- intsa.json — Instagram automation (posting, fetching).
- knowledge.json — Build or query a knowledge base.
- legal.json — Legal document ingestion/processing.
- linkedmulti call.json — Performs multiple linked API calls in sequence.
- linkprof.json / linkurl.json — URL/link profiling and metadata extraction.
- master.json / master linked in.json — Master orchestration workflows (possibly for LinkedIn).
- material alternative.json — Small template or alternative material workflow.
- posting.json — Social posting pipeline.
- product.json — Product data ingestion or updates.
- profile.json — Profile update/management workflow.
- ref.json — Reference or utility workflow.
- rss ref.json — RSS feed ingestion.
- salesforce agent.json — Salesforce integration (create/update records).
- scheduler.json — Cron or schedule-based triggers for periodic tasks.
- search.json — Indexing or search automation.
- sharepoint.json — SharePoint file/item automation.
- supbase trigger.json — Supabase-triggered workflow (note name typo 'supbase' — likely 'supabase').
- teams test.json — MS Teams testing automation.
- temp dumps.json — Large interim dump(s) / temp exports; possibly bulk exports.
- template.json — Template workflow(s) to copy/start from.
- testrag.json / testrag.json — Tests for RAG (retrieval-augmented generation).
- timesheetprocess.json — Timesheet ingestion and approval process automation.
- travel agent.json — Travel booking or itinerary automation.
- web.json — Webhook, scraping or web integration workflow.
- xlsx.json — Excel (.xlsx) read/write workflow.

Directories noted in the listing:
- ocr/ — folder for OCR workflows or assets (API listing showed directory, but not its files).
- real_estateworkflow/ — folder likely containing real estate related workflows.

How to inspect a workflow JSON
1. Open the JSON in a text editor or JSON viewer.
2. Look at top-level keys: name, id, nodes, connections, settings, and credentials.
3. Nodes: each entry has a type (e.g., "httpRequest", "cron", "webhook", "function"), parameters and name.
4. Triggers: identify which node is the starting trigger (cron, webhook, etc.).
5. Credentials: check the credentials array. Most public backups should avoid including secret values; however some exports reference credential IDs/names only.

How to restore/import a workflow into n8n (manual)
1. In n8n UI: Workflows → Import → Upload the JSON or paste the raw JSON content.
2. After import, review nodes and reattach credentials in the n8n credentials manager.
3. Save and activate the workflow when ready.

How to restore/import a workflow into n8n (API/automated)
- n8n exposes HTTP API endpoints for creating workflows (depending on n8n version).
- General approach: POST the JSON representation of the workflow to the appropriate endpoint (authenticate using your n8n credentials or API key).
- Always test imports against a staging n8n instance before importing into production.

Automating backups (recommended approaches)\nOption 1 — Scripted export + Git commit (recommended)
- Script responsibilities:
  1. Call n8n API to list all workflows.
  2. Export each workflow as JSON with a deterministic filename: e.g. <name>__id-<id>__YYYYMMDD-HHMM.json
  3. Optionally sanitize credentials (remove secrets) before writing to disk.
  4. Commit and push changes to this repository (or a private repo).

Option 2 — GitHub Actions (cron)
- A GitHub Actions workflow can run on a schedule, call your n8n instance, export workflows and push them back to this repo.
- Essential steps: Checkout repo → Run export script (with secrets stored in GitHub Actions secrets) → Commit and push.

Example shell script (conceptual)
#!/usr/bin/env bash
set -euo pipefail
N8N_BASE_URL=${N8N_BASE_URL:-"https://n8n.example.com"}
N8N_TOKEN=${N8N_TOKEN:-""}
OUT_DIR=exports
mkdir -p "$OUT_DIR"
# 1) list workflows (API endpoint depends on n8n version)
# 2) for each workflow: curl to get JSON and save to file with timestamp
# 3) git add/commit/push

(Provide your n8n API path and auth approach — I can generate a concrete script for your n8n version.)

Security and handling secrets
- Treat exported workflow JSON as potentially sensitive. Do not commit files containing secrets to a public repository.
- Recommended practices:
  - Use a private repository for backups containing credentials.
  - Sanitize or redact credentials before committing.
  - Use environment variables and secret stores (HashiCorp Vault, GitHub Secrets) for CI/CD scripts.
  - Consider encrypting files at rest (gpg) if you must store secrets in the repo.

Suggested .gitignore entries
- .env
- credentials.json
- private-*.json
- secret-*.json
- *.key

Naming and versioning conventions
- Filename format: <workflow-name>__id-<id>__YYYYMMDD-HHMM.json
- Keep each workflow in its own file; for major changes create a new file with a timestamp.
- Optionally maintain an index file (backups/index.json) that lists workflow metadata and latest backup timestamp.

Maintenance & validation checks
- Add CI checks to ensure: valid JSON, presence of top-level name and nodes fields, and no embedded secrets (basic heuristics).
- Periodically prune or archive very old backups.

Troubleshooting
- Import errors: check for missing credentials and reattach them after import.
- Webhook trigger problems: webhooks may generate new IDs on import, you might need to re-create webhook endpoints or update downstream integrations.
- Large files or embedded binary blobs: identify which nodes generate big data and consider externalizing output to storage.

Contributing
- Workflow export process:
  1. Export from n8n UI (Workflows → Export) or via API.
  2. Sanitize credentials.
  3. Create a descriptive commit and file name.
- If you want more structured docs, add a markdown file per workflow in a docs/ or workflows/ folder describing purpose, trigger, inputs, outputs, and credentials used.

