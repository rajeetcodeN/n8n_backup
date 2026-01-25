# n8n_backup

Versioned backups of exported n8n workflows and related workflow assets.

This repository contains exported n8n workflow JSON files that can be imported back into an n8n instance for restoration, reuse, inspection, or documentation. The primary goal is to keep human-readable, auditable backups of real production and experimental workflows.

---

## Table of contents

* About this repository
* What lives here and why
* Repository structure
* Workflow summaries (inferred from filenames)
* Inspecting a workflow JSON
* Restoring / importing workflows into n8n
* Automating backups
* Security and secrets handling
* Naming and versioning conventions
* Maintenance and validation
* Troubleshooting
* Contributing
* Notes

---

## About this repository

`n8n_backup` is a structured backup of exported n8n workflows. Each `.json` file is typically a single workflow export containing:

* Workflow metadata
* Nodes and connections
* Settings
* References to credentials (not secret values)

This repo is meant to be:

* A restore point if workflows are deleted or broken
* A reference library for reusable automation patterns
* A lightweight documentation layer for complex automations
* A transport mechanism between environments (local → staging → prod)

This is not intended to be a runnable n8n instance by itself.

---

## What lives here and why

* **Exported workflows (`.json`)**
  Each file represents one workflow as exported from n8n (UI or API).

* **Folders**
  Used to group workflows by domain (e.g. OCR, real estate, etc.).

* **No credentials or secrets**
  Any workflow that depends on credentials must be re-wired after import.

The emphasis is on clarity and traceability rather than minimal file count.

---

## Repository structure

* Root directory contains exported workflow JSON files
* Some workflows are grouped into subfolders
* Filenames are descriptive and usually reflect the business purpose

---

## Workflow summaries (inferred from filenames)

Descriptions below are inferred from filenames and common n8n usage patterns. For exact behavior, inspect the JSON directly (nodes, triggers, credentials).

### Selected workflows

* **Brand profiler.json**
  Brand data extraction and enrichment workflow.

* **COST CALCULATION.json**
  Cost computation logic (likely RFQ, pricing, or invoice-related).

* **Internal knowledge RAG and chat agent.json**
  RAG-based internal knowledge system with a chat interface.

* **MISTRAL OCR.json**
  OCR pipeline for PDFs/images using Mistral or a similar provider.

* **agent.json**
  Generic agent/orchestration workflow.

* **ai chat.json / chat.json / chat test.json**
  LLM chat integrations and test variants.

* **analytics.json**
  Analytics ingestion or transformation workflow.

* **backup.json**
  Likely exports or synchronizes workflows for backup purposes.

* **blogs agent.json**
  Blog drafting, scraping, or publishing automation.

* **carousel.json / carousel n8n.json**
  Carousel-based social media posting workflows.

* **dashboard.json**
  Data preparation for dashboards or reporting tools.

* **data loader.json**
  ETL-style ingestion into databases or storage layers.

* **dbt ai knowledge bot.json** (and other dbt-related files)
  dbt-related automation: docs, checks, alerts, or AI summaries.

* **emai send.json**
  Email sending workflow (filename typo noted).

* **facebook.json / fb.json**
  Facebook posting or data ingestion.

* **gemini.json / gemini test.json**
  Gemini or LLM API integrations and tests.

* **git hub backup.json**
  Automation related to GitHub backups or sync.

* **image.json**
  Image processing (resize, OCR, generation, etc.).

* **intsa.json**
  Instagram automation.

* **knowledge.json**
  Knowledge base creation or querying.

* **legal.json**
  Legal document ingestion or processing.

* **linkedmulti call.json**
  Chained or dependent API call workflow.

* **master.json / master linked in.json**
  High-level orchestration workflows, likely LinkedIn-related.

* **rss ref.json**
  RSS feed ingestion.

* **salesforce agent.json**
  Salesforce integration (create/update/query records).

* **scheduler.json**
  Cron or scheduled triggers.

* **sharepoint.json**
  SharePoint file or item automation.

* **supbase trigger.json**
  Supabase-triggered workflow (filename typo noted).

* **timesheetprocess.json**
  Timesheet ingestion and processing.

* **travel agent.json**
  Travel booking or itinerary automation.

* **xlsx.json**
  Excel read/write automation.

### Directories

* **ocr/**
  OCR-related workflows or assets.

* **real_estateworkflow/**
  Real estate domain workflows.

---

## Inspecting a workflow JSON

1. Open the file in a text editor or JSON viewer.
2. Check top-level fields: `name`, `nodes`, `connections`, `settings`.
3. Identify trigger nodes (`cron`, `webhook`, etc.).
4. Review node parameters for external dependencies.
5. Inspect the `credentials` section to see what needs reattachment.

---

## Restoring / importing workflows into n8n

### Manual import (UI)

1. n8n UI → **Workflows → Import**
2. Upload the JSON file or paste its contents
3. Reattach credentials in the credentials manager
4. Save and activate

### API-based import

* Use n8n’s workflow API (endpoint varies by version)
* POST the workflow JSON with proper authentication
* Always test imports in a non-production environment first

---

## Automating backups

### Option 1 — Scripted export + Git (recommended)

Typical flow:

1. Call n8n API to list workflows
2. Export each workflow as JSON
3. Sanitize credentials
4. Save with deterministic filenames
5. Commit and push to this repo

Example filename:

```
<workflow-name>__id-<id>__YYYYMMDD-HHMM.json
```

### Option 2 — GitHub Actions (scheduled)

* Run on cron
* Export workflows via API
* Commit and push automatically
* Store secrets in GitHub Actions secrets

---

## Security and secrets

* Treat workflow exports as sensitive artifacts
* Never commit secret values to a public repo
* Prefer:

  * Private repositories
  * Credential sanitization
  * Environment variables
  * Secret managers
* Consider encrypting exports if secrets must exist at rest

### Suggested `.gitignore`

```
.env
credentials.json
private-*.json
secret-*.json
*.key
```

---

## Naming and versioning conventions

* One workflow per file
* Descriptive filenames
* Timestamped backups for major changes
* Optionally maintain an index file with latest versions

---

## Maintenance and validation

* Periodically validate:

  * JSON integrity
  * Presence of `name` and `nodes`
  * No embedded secrets
* Archive or prune outdated backups
* Prefer adding docs for complex workflows

---

## Troubleshooting

* **Import failures** → usually missing credentials
* **Webhook issues** → webhook URLs/IDs may change after import
* **Large files** → consider external storage instead of embedding data

---

## Contributing

1. Export workflow from n8n (UI or API)
2. Remove or sanitize credentials
3. Use clear filenames and commit messages
4. For complex workflows, add a short markdown doc describing:

   * Purpose
   * Trigger
   * Inputs / outputs
   * External systems used

---

## Notes

This repository reflects real-world workflows: some experimental, some production-tested and all workflows are clean templates. 
