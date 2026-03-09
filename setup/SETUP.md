# Setup Interview

> Run this on the first session. Ask one section at a time.
> When done, write the answers to `setup/context.md`.
> There are no right or wrong answers — the goal is to understand the user's stack so you can help them build.

---

## Section 1: Environment

- What OS?
- Python installed? Run: `python3 --version`
- Any CLI tools already set up? (gcloud, bq, aws, etc.)
- What API keys do you have? (LLM providers, web search, etc.)

If something is missing and they need it for what they want to build, help them install it before continuing.

## Section 2: Data

- Where does your ad platform data live? (BigQuery, Snowflake, Redshift, spreadsheets, API only, etc.)
- What platforms are you advertising on? (Google Ads, Meta, TikTok, Microsoft, etc.)
- Do you have any backend data beyond ad platform data? (revenue, LTV, orders, margins, inventory, etc.)
- If yes: how does it connect to your ad data?

## Section 3: Ad accounts

- Which platforms and account IDs are you working with?
- How are campaigns structured? (by product, region, funnel stage, audience, etc.)
- What bid strategies are active?
- What are your primary KPIs?

## Section 4: First project

What's the most valuable thing to build first? Some common starting points:

- Search term classification and routing
- Automated reporting (weekly, monthly)
- Budget optimization
- Ad copy generation and rotation
- LTV or margin-aware bidding
- A data chat / analytics agent

Pick one. Create a project folder under `projects/` using the template at `projects/_template/GOAL.md` and fill in what you know so far. You don't need all the answers yet — the GOAL.md will evolve as you build.

---

After setup, update `docs/index.md` with the real project name and any data sources that are now documented. Write a summary of the user's setup to `setup/context.md`.
