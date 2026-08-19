# Multi-Agent Article Lab (n8n Workflow)

An automated multi-agent editorial workflow built in **n8n** that converts user briefs into fully researched, structured, and quality-checked articles.

---

## Overview

This workflow orchestrates a team of specialized AI agents powered by **Google Gemini** to handle article generation end-to-end. It features automated planning, content writing, and two iterative feedback loops (outline review and draft review) to ensure comprehensive coverage and high editorial standards.

---

## Architecture and Workflow Stages

1. **Intake and Brief Building**:
   - **`Article Intake`**: Collects the topic, article type, required points, target audience, tone, language, and target word count through an n8n Form trigger.
   - **`Brief Builder`**: Formats the input fields into a standardized creative brief.

2. **Orchestration**:
   - **`Lead Editor`**: Acts as the root supervisor determining execution routing based on current stage state (`PLAN`, `WRITE`, or `DELIVER`).
   - **`Task Dispatcher`**: Routes execution flow to the appropriate stage based on the supervisor's decision.

3. **Planning Lane (Outline Review Loop)**:
   - **`Outline Designer`**: Generates multi-headline options, editorial angles, hooks, section breakdowns, and point coverage mappings.
   - **`Outline Reviewer`**: Inspects the outline against input requirements using structured JSON validation. If points are missing, it sends actionable feedback back to the designer until approved.

4. **Writing Lane (Draft Review Loop)**:
   - **`Article Writer`**: Translates approved outlines into full Markdown articles adhering to section guidelines, word limits, and specified tone.
   - **`Quality Reviewer`**: Evaluates draft quality across structure, word count, coverage, and readability. Unapproved drafts return to the writer with revision notes.

5. **Final Delivery**:
   - **`Final Handover`**: Renders the fully approved and polished article to the end-user.

---

## Model Configuration

* **LLM Engine**: Google Gemini (`models/gemini-3.5-flash-lite`) configured across all agent roles.
* **Temperature**: Set to `0.4` across all nodes to maintain consistent, reliable outputs while preserving creativity.

---

## Prerequisites

1. **n8n Instance**: Deployment supporting `@n8n/n8n-nodes-langchain` and `n8n-nodes-base.form` nodes.
2. **Google Gemini API Credentials**: Valid PaLM/Gemini API key configured within n8n.

---

## Setup and Usage

1. Import the JSON workflow file into your n8n workspace via **Import from File / JSON**.
2. Link your **Google Gemini API Credentials** to each language model node (`Gemini · Editor`, `Gemini · Outline`, `Gemini · Outline Review`, `Gemini · Article Writer`, and `Gemini · Quality Review`).
3. Save and set the workflow status to **Active**.
4. Open the generated **Article Intake** form URL, submit your article brief, and wait for the multi-agent pipeline to process and deliver the finished piece.
