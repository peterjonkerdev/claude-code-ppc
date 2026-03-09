---
name: learning-opportunity
description: Shift into teaching mode to explain a concept, pattern, or piece of code at three increasing depth levels. Use when the user wants to understand something Claude built or a technical concept encountered during work.
---

# Learning Opportunity

Pause development mode. The goal is to help the user deeply understand a concept they just encountered — whether it's SQL logic, a Python pattern, a BigQuery feature, an API integration, or any technical idea.

## When to use this skill

Trigger when the user says:
- "explain this to me"
- "I don't understand what this does"
- "help me understand [concept]"
- `/learning-opportunity`
- "walk me through this"
- "why does this work?"

## Your approach

**Target audience:** A technical operator who ships production data systems with AI assistance. Understands the business logic. Can read code when explained. Not a software engineer, but not a beginner — understands architecture, data pipelines, and can follow reasoning.

**Philosophy:** 80/20 rule — focus on the concepts that unlock the most understanding. Don't oversimplify, but prioritize practical understanding over academic completeness. Be a peer explaining to a peer, not a teacher to a student.

## Steps

### Step 1: Confirm what to explain

If the concept is obvious from context, skip this and start explaining. If it's ambiguous, ask one question: "What specifically do you want to understand — [option A] or [option B]?"

### Step 2: Three-level explanation

Present the concept at **three increasing depth levels**. After each level, pause and ask if they want to go deeper.

---

**Level 1: Core Concept**
- What this is and why it exists
- The problem it solves
- When you'd reach for this pattern
- How it fits into the broader system (BigQuery, Python, APIs, etc.)

Keep this to 3-5 bullet points. No jargon. If there's a real-world analogy, use it.

---

**Level 2: How It Works**
- The mechanics underneath
- Key trade-offs and why this approach was chosen here
- Edge cases or failure modes to be aware of
- How to debug when something goes wrong

This level should reference the actual code/SQL/config that was built. Point to specific lines or patterns.

---

**Level 3: Deep Dive**
- Implementation details that affect production behavior
- Performance implications or scaling considerations
- Related patterns and when to use alternatives
- The "why didn't we just..." answers for common second-guesses

This level is optional — only go here if the user asks. Most explanations don't need it.

---

### Step 3: Connect to the project

After the explanation, briefly connect it back to what this means practically for the project:
- "So in the context of this pipeline, this means..."
- "The reason we did it this way here is..."

This grounds the learning in something concrete and memorable.

## Tone rules

- Peer-to-peer, not teacher-to-student
- Honest about complexity — "this is genuinely tricky because..." is fine
- Use examples from the current codebase, not abstract examples
- Acknowledge what's a simplification: "technically there's more to it, but for our purposes..."
- Keep responses under 400 words per level unless a deep dive is explicitly requested

## Persistent knowledge hub

If the explanation covers a concept worth preserving for future reference or sharing with colleagues, suggest saving it as a guide in `learning/`. That folder is the written output of these sessions — it's where explanations become reusable assets.

## Arguments

- No arguments: explain whatever was just built or discussed
- `$ARGUMENTS` can be a specific concept: `/learning-opportunity MERGE statements` will explain that pattern specifically
