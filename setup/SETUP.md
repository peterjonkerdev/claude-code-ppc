# Setup Interview

> Run this on the first session. Ask one section at a time, conversationally.
> When done, write the answers to `setup/context.md`.
> Keep it natural. The goal is to understand the user's world so you can start helping immediately.

---

## Section 1: About You

Get to know who you're working with:

- What's your role? (PPC specialist, marketing manager, data analyst, agency, freelancer)
- Team structure: solo or part of a team? Who reviews your work?
- Python experience: comfortable writing scripts, or more of a SQL/spreadsheet person?
- What does a typical work week look like for you?

## Section 2: Accounts & Channels

Understand the advertising landscape:

- Google Ads: single account or MCC? What are the customer IDs? (format: XXX-XXX-XXXX)
- Other channels? (Meta Ads, Microsoft Ads, TikTok, LinkedIn, Pinterest, Amazon, DV360)
- For each channel: how do you currently access the data? (platform UI, API, data warehouse, exports)
- Roughly how much spend are you managing across all channels?

## Section 3: Data & BigQuery

Where does the data live:

- Data warehouse: BigQuery, Snowflake, Redshift, or none yet?
- If BigQuery: GCP project ID, dataset names, is Google Ads Data Transfer set up?
- If other: connection details, how data gets there (ETL tool, manual exports, API)
- Existing documentation: dbt docs, Confluence pages, data dictionaries, or starting fresh?

## Section 4: Backend & Business Data

Data beyond ad platforms:

- Do you have revenue, LTV, orders, margins, or inventory data somewhere?
- If yes: where does it live? (warehouse, CRM, ERP, spreadsheets)
- How does it connect to your ad data? (shared customer ID, order ID, nothing yet)
- Conversion tracking setup: offline conversions, enhanced conversions, or just platform pixels?

## Section 5: Goals & First Project

What's the most valuable thing to start with? Common starting points:

- **Exploratory data analysis**: "show me what's in my data"
- **Search term classification**: categorize and route search queries
- **Automated reporting**: weekly or monthly performance summaries
- **Budget optimization**: allocation across campaigns or channels
- **Ad copy generation**: RSA variations, headline testing
- **LTV or margin-aware bidding**: feed business data back into bid strategies
- **Product feed optimization**: titles, descriptions, custom labels
- **Anomaly detection**: alert when metrics move outside normal ranges
- **tROAS optimization**: analyze and adjust target ROAS settings
- **Data chat / analytics agent**: conversational interface to your data
- Something else entirely

Pick one. What does success look like for this first project? Is there urgency?

## Section 6: Tech Stack

Check what's installed:

```bash
bq version && gcloud --version && python3 --version && uv --version && gh --version
```

If something is missing and they need it, help them install it before continuing.

Also ask:
- What API keys do you have? (LLM providers, web search, etc.)
- Editor: VS Code, Cursor, terminal-only?
- Comfortable with git, or prefer the agent handles version control?

## Section 7: Prototyping vs Production

Explain the two-tier architecture:

> This repo is your prototyping workspace: fast iteration, exploration, building things to see if they work. When something proves valuable and needs scheduling, team access, or CI/CD, it graduates to its own production repository with stricter code quality and deployment pipelines.

Ask:
- Does anything you're building right now need to run on a schedule?
- Will other people need to access or contribute to your work?
- Do you need CI/CD or deployment pipelines for anything today?
- Or are you starting in pure exploration mode?

## Section 8: Wrapping Up

Now set everything up:

1. Create the first project folder under `projects/` using the template at `projects/_template/`
2. Fill in `GOAL.md` with what you've learned about their first project
3. Update `docs/index.md` with the project name and any data sources
4. If BigQuery: replace `your-project` and `your-dataset` placeholders in `bigquery/README.md` with real IDs
5. Fill in `.env` with any API keys they have
6. Write the full context summary to `setup/context.md`, filling in every section

Tell the user: "Setup complete. You can start working on your first project. Just tell me what you want to do."

---

After setup, the agent should never need to re-run this interview. Everything learned is in `setup/context.md`.
