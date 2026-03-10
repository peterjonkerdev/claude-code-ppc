# Setup Interview

> Run this on the first session. Ask one section at a time.
> When done, write the answers to `setup/context.md`.
> Keep it quick. The goal is to understand the user's stack so you can start helping.

---

## Section 1: Tools

What's already installed? Run a quick check:

```bash
bq version && gcloud --version && python3 --version && uv --version && gh --version
```

If something is missing and they need it, help them install it before continuing.

Also ask:
- Any other CLI tools set up? (aws, az, etc.)
- What API keys do you have? (LLM providers, web search, etc.)

## Section 2: Data

- Where does your data live? (BigQuery, Snowflake, Redshift, spreadsheets, API only)
- What platforms are you working with? (Google Ads, Meta, TikTok, Microsoft, other)
- Do you have backend data beyond ad platforms? (revenue, LTV, orders, margins, inventory)
- If yes: how does it connect to your ad data?

## Section 3: What to explore first

What's the most valuable thing to start with? Some common starting points:

- Exploratory data analysis: "show me what's in my data"
- Search term classification and routing
- Automated reporting (weekly, monthly)
- Budget optimization or forecasting
- Ad copy generation
- LTV or margin-aware bidding
- A data chat / analytics agent
- Something else entirely

Pick one. Create a project folder under `projects/` using the template at `projects/_template/GOAL.md` and fill in what you know so far. The GOAL.md will evolve as you build.

---

After setup, update `docs/index.md` with the real project name and any data sources that are now documented. Write a summary of the user's setup to `setup/context.md`.
