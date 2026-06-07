# 50-Day AI Software Engineer Challenge — Daily Journal

## Project Title

**Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis**

## Challenge Goal

The goal of this 50-day challenge is to build a research-backed AI software engineering project that explores how enterprise API development can be accelerated using AI-driven requirement extraction, workflow synthesis, Excel specification generation, and gap analysis.

The project focuses on transforming Confluence-style enterprise API requirements and Node-RED prototype flows into structured engineering artifacts that can support downstream Go code-generation workflows.

---

# Day 1 — Research Foundation and Project Direction

## Date

June 1, 2026

## Focus

Research problem definition, project mission, architecture direction, and success metrics.

## What I Completed

Today I started the 50-Day AI Software Engineer Challenge and defined the core research direction.

I identified the main research problem: enterprise API development often involves manual interpretation of Confluence requirements, prototype Node-RED flows, Excel specification templates, and code-generation inputs. This creates delays, inconsistencies, and missing business logic during modernization projects.

I defined the project title:

**Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis**

I also outlined the two main projects that will support the research:

1. **Project 1 — AI Enterprise API Factory**

   * Converts Confluence-style requirements into structured requirement output and Node-RED workflow plans.

2. **Project 2 — AI Factory Excel Agent**

   * Converts Confluence requirements and Node-RED flows into structured Excel specifications and gap analysis reports.

## Key Deliverables

* Defined the research problem.
* Created the mission statement.
* Created the initial system architecture.
* Defined success metrics.
* Created project architecture notes for Project 1 and Project 2.
* Established the 50-day challenge direction.

## Why This Matters

Day 1 created the foundation for the entire research project.

Instead of building a random project, I defined a problem that is directly connected to real enterprise API modernization workflows.

## Reflection

Day 1 helped convert the idea into a structured research project.

The project now has a clear title, a real-world problem, and measurable goals.

## Next Step

Day 2 will focus on literature review and related work.

---

# Day 2 — Literature Review and Research Foundation

## Date

June 2, 2026

## Focus

Literature review, related work, research gap, and dataset strategy.

## What I Completed

Today I focused on understanding the research background behind AI-assisted software engineering.

I reviewed the broader areas connected to this project, including requirement engineering, software automation, workflow generation, low-code platforms, prompt-based extraction, and AI-assisted code generation.

I also started positioning the project within a research gap: existing tools can summarize requirements or generate code, but they often do not provide a complete enterprise pipeline from requirement documents to workflow logic, structured Excel specifications, and gap analysis.

## Key Deliverables

* Created literature review notes.
* Identified related work categories.
* Defined the research gap.
* Created a competitor or baseline analysis.
* Created an initial dataset strategy.
* Connected the project to enterprise API modernization.

## Research Gap

The key research gap is:

Current AI software engineering tools often focus on isolated tasks such as code generation, summarization, or documentation. They do not fully address the enterprise workflow where requirements, prototype flows, Excel intake templates, database mappings, downstream services, and business rules must stay aligned.

## Why This Matters

A strong research project needs evidence that the problem is meaningful and not already fully solved.

Day 2 helped justify why this project is valuable from both a practical and research perspective.

## Reflection

Day 2 helped me understand that the strongest contribution of this project is not just generating code.

The stronger contribution is building a traceable AI workflow that can move from requirements to implementation-ready artifacts while detecting gaps.

## Next Step

Day 3 will focus on methodology, baselines, benchmarks, and evaluation design.

---

# Day 3 — Methodology, Baselines, and Evaluation Design

## Date

June 3, 2026

## Focus

Experimental methodology, baseline comparison, benchmark definition, threats to validity, and paper outline.

## What I Completed

Today I focused on turning the project idea into a research-ready methodology.

I defined how the system will be evaluated and what baselines it should be compared against.

The project will evaluate AI-generated outputs against manually prepared ground truth artifacts.

The main evaluation areas are:

1. Requirement extraction accuracy
2. Node-RED workflow correctness
3. Excel specification completeness
4. Gap detection precision and recall
5. Manual effort reduction

## Key Deliverables

* Created experimental methodology documentation.
* Defined baseline comparison strategy.
* Defined benchmark tasks.
* Identified threats to validity.
* Created the initial paper outline.
* Connected evaluation metrics to the research question.

## Baseline Direction

The system can later be compared against:

1. Manual requirement interpretation
2. Generic LLM summarization
3. Rule-based extraction
4. AI-assisted extraction with structured schema
5. AI-assisted extraction plus workflow and Excel validation

## Why This Matters

Day 3 made the project measurable.

Without evaluation design, the project would only be a tool. With evaluation design, it becomes a research project that can support a paper, portfolio, and technical case study.

## Reflection

Day 3 helped clarify that every artifact should be evaluated against ground truth.

The goal is not only to generate outputs, but to prove that those outputs are accurate, complete, and useful.

## Next Step

Day 4 will focus on dataset structure and implementation foundation.

---

# Day 4 — Implementation Foundation

## Date

June 4, 2026

## Focus

Dataset structure, ground truth schema, and implementation foundation for the AI Enterprise API Factory research project.

## What I Completed

Today I transitioned from research planning into implementation preparation.

I created the initial dataset folder structure for the project, including folders for Confluence requirements, Node-RED flows, ground truth files, and generated outputs.

I also documented the dataset schema so that each enterprise API requirement can be paired with a Node-RED flow, a ground truth file, and AI-generated outputs.

## Key Deliverables

* Created dataset directory structure.
* Defined Confluence requirement dataset format.
* Defined Node-RED flow dataset format.
* Defined ground truth annotation format.
* Created initial FEED Discount requirement placeholder.
* Created initial FEED Discount ground truth placeholder.
* Created implementation foundation documentation.

## Files Created

```text
docs/day4/dataset_schema.md
docs/day4/implementation_foundation.md
datasets/confluence_requirements/REQ-001-feed-discount.md
datasets/ground_truth/GT-001-feed-discount.json
```

## Why This Matters

A research project needs a strong evaluation dataset.

Without ground truth, it is difficult to prove whether an AI system is actually generating correct enterprise workflows or simply producing plausible-looking outputs.

Day 4 created the structure needed to evaluate the system scientifically.

## Research Connection

This work directly supports the paper:

**Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis**

The dataset will help evaluate:

* Requirement extraction accuracy
* Workflow generation correctness
* Excel generation completeness
* Gap analysis precision and recall
* Manual effort reduction

## Reflection

Day 4 helped convert the project from an idea into an implementation-ready research system.

The project now has a clear structure for collecting examples, annotating expected outputs, and measuring AI-generated results against ground truth.

## Next Step

Day 5 will focus on planning the Requirement Extraction Agent before any coding work begins.

---

# Day 5 — Requirement Extraction Agent Planning

## Date

June 5, 2026

## Focus

Planning the Requirement Extraction Agent and defining its role in the overall AI workflow.

## What I Completed

Today I focused on planning the first major agent in the system: the Requirement Extraction Agent.

The purpose of this agent is to read Confluence-style enterprise API requirements and convert them into structured implementation-ready information.

Since coding is now postponed until Day 25, Day 5 remains a planning and design day.

## Agent Purpose

The Requirement Extraction Agent will eventually extract:

1. API metadata
2. Endpoint details
3. Request headers
4. Request body fields
5. Response body fields
6. Business rules
7. External services
8. Database references
9. Error handling rules
10. Mapping logic
11. Open questions

## Key Deliverables

* Defined the role of the Requirement Extraction Agent.
* Clarified how the agent connects to downstream Node-RED and Excel generation.
* Identified the types of fields the agent must extract.
* Confirmed that the agent should avoid hallucinating missing details.
* Established that ambiguous requirements should become open questions.

## Why This Matters

The Requirement Extraction Agent is the entry point of the entire system.

If the extraction is poor, every downstream artifact will also be weak.

Day 5 helped clarify that the future implementation should focus on conservative, traceable extraction rather than creative generation.

## Research Connection

This day supports the methodology and system design parts of the paper.

It defines the first transformation step:

```text
Confluence Requirement
        ↓
Requirement Extraction Agent
        ↓
Structured Requirement JSON
```

## Reflection

Day 5 helped make the project more disciplined.

The agent should not behave like a general summarizer. It should behave like a structured extraction system that preserves evidence and supports evaluation.

## Next Step

Day 6 will define the extraction specification, output schema, and prompt design strategy.

---

# Day 6 — Requirement Extraction Specification and Prompt Design

## Date

June 6, 2026

## Focus

Non-coding planning for the future Requirement Extraction Agent.

## What I Completed

Today I created detailed planning documents for the future Requirement Extraction Agent.

The focus was on defining what the agent should extract, what JSON schema it should produce, and how it should be prompted later when implementation begins from Day 25.

No coding was performed.

## Constraint Followed

The updated project rule was followed:

```text
All coding and implementation work will begin from Day 25.
```

Day 6 was limited to:

1. Documentation
2. Specification design
3. Output schema planning
4. Prompt strategy
5. Research preparation

## Key Deliverables

* Created the Requirement Extraction Specification.
* Created the Extraction Output Schema.
* Created the Prompt Design Strategy.
* Created the Day 6 Completion Report.
* Confirmed that coding work is postponed until Day 25.

## Files Created

```text
docs/day6/requirement_extraction_spec.md
docs/day6/extraction_output_schema.md
docs/day6/prompt_design_strategy.md
docs/day6/day6_completion_report.md
```

## Requirement Extraction Specification

The Requirement Extraction Specification defines what the future agent must extract from Confluence-style enterprise API requirements.

It covers:

1. API metadata
2. Endpoint details
3. Headers
4. Request body
5. Response body
6. Business rules
7. External services
8. Database references
9. Error handling
10. Mapping logic
11. Security requirements
12. Non-functional notes
13. Open questions

The main principle is:

```text
Extract only what is present. Do not invent missing details.
```

## Extraction Output Schema

The Extraction Output Schema defines the future JSON structure that the agent should produce.

The schema includes:

```text
requirement_id
source_document
api_metadata
endpoint
headers
request_body
response_body
business_rules
external_services
database_references
error_handling
mapping_logic
security_requirements
non_functional_notes
open_questions
extraction_metadata
```

This schema will later support:

1. Node-RED flow planning
2. Excel generation planning
3. Gap analysis
4. Ground truth comparison
5. Research evaluation

## Prompt Design Strategy

The Prompt Design Strategy defines how the future AI model should be instructed.

The prompt strategy emphasizes:

1. Extracting only supported facts
2. Preserving exact technical names
3. Avoiding unsupported assumptions
4. Producing consistent JSON
5. Adding ambiguity to open questions
6. Supporting downstream Node-RED and Excel agents

## Why This Matters

Day 6 strengthens the foundation before implementation.

Instead of jumping into code, the project now has a clear extraction specification, schema, and prompt strategy. This will make the future implementation more reliable and easier to evaluate.

## Research Connection

Day 6 supports the research paper:

**Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis**

It contributes to:

1. System design
2. Agent behavior definition
3. Prompting methodology
4. Evaluation readiness
5. Reproducibility

## Reflection

Day 6 made the future implementation more structured.

The most important lesson is that a good AI engineering system needs strong specifications before code. The agent must be conservative, traceable, and evaluation-friendly.

## Next Step

Day 7 will focus on:

**Ground Truth Annotation Guidelines**


