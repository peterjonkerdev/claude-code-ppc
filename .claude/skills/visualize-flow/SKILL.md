---
name: visualize-flow
description: Generate Mermaid diagrams to visualize project data flows, pipeline architectures, and workflow systems. Use when the user asks for a visualization, diagram, or flowchart of a project or data flow.
---

# Visualize Flow

Generate a high-level Mermaid diagram that visualizes a project's data flow or system architecture.

## When to use this skill

Trigger when the user asks for:
- "visualize [project]"
- "show me the data flow for [project]"
- "make a diagram of [workflow/pipeline]"
- "how does [project] work visually"

## Steps

### Step 1: Load project context

Read the relevant project docs to understand the system:
- `projects/[project]/GOAL.md`
- `projects/[project]/docs/project-overview.md`

Do NOT read workflow JSON files unless strictly necessary — the overview doc is enough for a high-level diagram.

### Step 2: Generate the diagram

Create a Mermaid `flowchart TD` diagram following these rules:

**Keep it high-level:**
- Show major stages/workflows as subgraphs
- Show data stores (BigQuery tables) as `[(Table Name)]`
- Show decision points as `{Question?}`
- Show external systems (Google Ads, APIs) with emoji for clarity
- Do NOT show individual node logic, SQL details, or field mappings

**Good example structure:**
```
flowchart TD
    A[Input Source] --> B

    subgraph WF1["Workflow 1 — Name"]
        B[Step] --> C[AI Model\nWhat it does]
        C --> D{Decision?}
        D -- Yes --> E[Output]
        D -- No --> F[Fallback]
    end

    E --> G[(BigQuery\ntable_name)]
    G --> H[Next workflow...]
```

### Step 3: Save and present

1. Save the diagram as a `.mermaid` file to the relevant project directory (e.g., `projects/[project]/docs/[name]-flow.mermaid`)
2. Present the diagram to the user
3. Briefly explain what the diagram shows (2-3 sentences max)

## Output rules

- High-level only — no nitty gritty details
- Keep subgraph labels descriptive: `"Workflow 1 — Search Term Classification"` not just `"WF1"`
- Use emoji sparingly for input/output nodes to make them scannable
