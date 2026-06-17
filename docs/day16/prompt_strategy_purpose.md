# Day 16 — Phase 1: Prompt Strategy Purpose

## Purpose

This document defines the purpose of the prompt strategy for the 50-Day AI Software Engineer Challenge.

The goal of the prompt strategy is to make the AI-assisted enterprise API artifact generation pipeline more reliable, repeatable, and measurable.

The project is designed to support the following pipeline:

```text
Confluence Requirement
        ↓
Requirement Extraction
        ↓
Workflow Modeling
        ↓
Node-RED Flow Representation
        ↓
Excel Intake Workbook
        ↓
Gap Analysis Report
        ↓
Evaluation and Code Generation Readiness
```

Prompt strategy is needed because the quality of AI-generated artifacts depends heavily on how the AI is instructed.

A clear prompt strategy helps reduce inconsistent outputs, unsupported assumptions, hallucinated requirements, and incomplete artifact generation.

---

## Why Prompt Strategy Is Needed

AI can generate useful artifacts, but without structured prompts, the output may vary from one run to another.

For enterprise API development, this is risky because requirements must be precise.

A small mistake can cause:

* Missing request headers
* Incorrect request body fields
* Wrong validation rules
* Missing downstream service mappings
* Incorrect database query definitions
* Unsupported business logic
* Wrong technology assumptions
* Incorrect Excel intake rows
* Inaccurate gap analysis results

The prompt strategy is designed to reduce these risks by defining how the AI should behave for each major task.

---

## Main Objective

The main objective of the prompt strategy is to create reusable prompting patterns for the future system.

These prompts should guide the AI to:

```text
Extract requirements accurately
Preserve source-of-truth details
Avoid inventing missing information
Represent workflow logic step by step
Generate structured Excel intake content
Detect gaps across artifacts
Assign severity consistently
Provide evidence-based recommendations
Support evaluation metrics
```

The prompt strategy should make AI behavior predictable and easier to evaluate.

---

## Relationship to Previous Days

Day 16 builds directly on the previous design work.

Earlier days defined:

```text
Day 6 — Requirement Extraction Specification
Day 7 — Workflow Representation Layer
Day 8 — Workflow Schema Specification
Day 13 — Gap Analysis Framework
Day 14 — Gap Analysis Evaluation Methodology
Day 15 — Research Dataset and Experiment Design
```

Day 16 now defines how the AI should be prompted to perform these tasks consistently.

This connects the project architecture to actual AI behavior.

---

## Prompt Strategy in the Overall System

The future system may include multiple prompt-driven stages.

Each stage should have a clear prompt purpose.

```text
Requirement Extraction Prompt
        ↓
Workflow Modeling Prompt
        ↓
Excel Intake Generation Prompt
        ↓
Gap Analysis Prompt
        ↓
Evaluation Scoring Prompt
        ↓
Reviewer Prompt
```

Each prompt should produce a structured output that can be reviewed, measured, and used by the next stage.

---

## What the Prompt Strategy Should Control

The prompt strategy should control the following areas.

---

## 1. Input Interpretation

The AI must correctly understand the input artifact.

For example, when given a Confluence requirement, the AI should identify:

```text
Endpoint path
HTTP method
Headers
Request body fields
Response body fields
Validation rules
Business rules
Downstream services
Database references
Error handling expectations
Technology assumptions
```

The prompt must tell the AI to extract only what is supported by the source document.

---

## 2. Output Structure

The AI must produce outputs in a predictable format.

For example, requirement extraction output should follow a structured schema.

Workflow output should follow the workflow representation schema.

Gap analysis output should follow the gap output schema.

This helps make the system measurable and easier to integrate later.

---

## 3. Assumption Handling

The AI must not silently invent missing details.

If something is unclear, the AI should mark it as:

```text
Missing
Ambiguous
Needs Manual Review
Assumption
Not Found in Source
```

This is important because enterprise development requires traceability.

Unsupported assumptions can lead to incorrect generated code.

---

## 4. Evidence Tracking

The AI should include evidence whenever possible.

For example, a gap analysis output should explain:

```text
What Confluence says
What Node-RED contains
What Excel contains
Why the difference is a gap
```

Evidence makes the output reviewable and helps human engineers trust the result.

---

## 5. Severity Consistency

The AI should assign severity using predefined rules.

Severity levels include:

```text
Critical
High
Medium
Low
Informational
```

The prompt should instruct the AI to use the severity rules defined in the gap severity document.

This prevents random severity assignment.

---

## 6. Recommendation Quality

The AI should provide recommendations that are specific and actionable.

Weak recommendation:

```text
Fix Excel.
```

Strong recommendation:

```text
Add x-macys-apikey to the api_endpoints required_headers field and include it in validation_rules as a required header.
```

Prompt strategy should push the AI toward the stronger recommendation style.

---

## Core Prompting Principles

The project should follow these prompting principles.

---

## Principle 1: Source-First Reasoning

The AI should treat the source artifact as the authority.

For most requirement tasks, Confluence is the source of truth.

The AI should not override Confluence with assumptions from Node-RED, Excel, or general knowledge.

---

## Principle 2: No Unsupported Invention

The AI should not invent endpoints, fields, headers, database tables, downstream services, or business rules.

If a detail is not present, it should be marked as missing or unknown.

---

## Principle 3: Structured Output

Every major prompt should request a structured output.

Examples:

```text
Markdown tables
JSON records
Workflow step lists
Excel sheet summaries
Gap report schema
Evaluation result schema
```

Structured output makes later automation easier.

---

## Principle 4: Traceability

Every important output should be traceable back to an input artifact.

For example:

```text
Header extracted from Confluence section
Validation rule extracted from request description
Business rule derived from process section
Service call extracted from workflow node
```

Traceability supports review and evaluation.

---

## Principle 5: Separate Facts from Assumptions

The AI should clearly separate confirmed facts from inferred assumptions.

Example:

```text
Confirmed: POST /api/v1/discount/interrogateAccount is defined in Confluence.
Assumption: requestorInfo-traceId may be optional because it is not listed under required headers.
Needs Review: Confluence does not clearly define timeout behavior.
```

This prevents hidden ambiguity.

---

## Principle 6: Review Before Generation

The AI should not directly jump from requirement text to final generation.

The pipeline should follow a staged approach:

```text
Extract
Validate
Represent
Generate
Compare
Evaluate
Review
```

This staged approach reduces errors and improves quality.

---

## Prompt Strategy Goals

The prompt strategy should support the following goals.

| Goal          | Description                                      |
| ------------- | ------------------------------------------------ |
| Accuracy      | Extract and represent requirements correctly     |
| Completeness  | Capture all required API elements                |
| Consistency   | Produce stable outputs across runs               |
| Traceability  | Link outputs back to source artifacts            |
| Reviewability | Make outputs easy for humans to inspect          |
| Measurability | Support evaluation metrics                       |
| Safety        | Avoid unsupported assumptions and hallucinations |
| Reusability   | Support repeatable prompt templates              |

---

## Example Prompt Behavior

A good prompt should instruct the AI like this:

```text
You are analyzing a Confluence API requirement.
Extract only details explicitly present in the source.
Do not invent missing headers, fields, services, or database tables.
If a detail is unclear, mark it as Needs Manual Review.
Return the output using the required schema.
For every major extracted item, include evidence from the source text.
```

This kind of instruction improves reliability.

---

## Prompt Strategy and Gap Analysis

Prompt strategy is especially important for gap analysis.

The Gap Analysis Agent should be instructed to:

```text
Compare Confluence, Node-RED, and Excel
Treat Confluence as the primary source of truth
Identify missing, incorrect, inconsistent, or unsupported details
Use predefined gap categories
Use predefined severity levels
Include evidence from each artifact
Provide actionable recommendations
Mark ambiguous cases as informational or low-confidence
```

This prevents the gap analysis output from becoming vague or subjective.

---

## Prompt Strategy and Evaluation

Prompt strategy also supports evaluation.

The Evaluation Agent should be instructed to:

```text
Compare detected gaps against manual baseline gaps
Identify true positives
Identify false positives
Identify false negatives
Identify severity matches
Identify severity mismatches
Calculate metrics using predefined formulas
Avoid changing metric formulas during evaluation
Report limitations honestly
```

This helps make the experiment repeatable and research-ready.

---

## Expected Benefits

A strong prompt strategy should provide the following benefits:

```text
More consistent requirement extraction
More complete workflow representation
More accurate Excel intake generation
Better gap detection quality
Fewer hallucinated fields or services
More useful recommendations
Easier manual review
Better research evaluation
```

These benefits support the overall goal of building a reliable AI-assisted enterprise API development pipeline.

---

## What This Phase Does Not Include

This phase does not include:

* Writing final production prompts
* Implementing agents
* Running prompts against real artifacts
* Creating benchmark outputs
* Parsing Confluence, Node-RED, or Excel files
* Generating Go code
* Measuring live prompt performance

Those tasks will come later.

This phase only defines why prompt strategy is needed and what it should accomplish.

---

## Summary

The purpose of the prompt strategy is to make AI-assisted artifact generation reliable, structured, and reviewable.

It ensures that the AI follows source-of-truth rules, avoids unsupported assumptions, produces structured outputs, and supports measurable evaluation.

This prompt strategy will guide future agents for requirement extraction, workflow modeling, Excel generation, gap analysis, evaluation, and review.

By defining this strategy, the project becomes more repeatable, research-ready, and suitable for future implementation.
