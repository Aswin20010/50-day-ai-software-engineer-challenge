# Day 16 — Phase 5: Agent Workflow Sequence

## Purpose

This document defines the agent workflow sequence for the future AI-assisted enterprise API artifact generation and gap analysis system.

The purpose of the workflow sequence is to describe how specialized agents will work together to transform enterprise API requirements into structured artifacts, gap reports, evaluation outputs, and readiness decisions.

The future system should follow a staged pipeline instead of asking one AI prompt to complete all tasks at once.

---

## Why Agent Workflow Sequence Is Needed

A clear agent workflow sequence is needed because enterprise API artifact generation involves multiple connected steps.

Each step depends on the quality of the previous step.

For example:

```text
If requirement extraction is incomplete,
then workflow modeling may be incomplete.

If workflow modeling is incomplete,
then Excel intake generation may be incomplete.

If Excel intake generation is incomplete,
then gap analysis may detect blockers.

If gap analysis is inaccurate,
then evaluation and readiness scoring may be unreliable.
```

A defined sequence makes the system easier to control, evaluate, and improve.

---

## High-Level Workflow

The core agent workflow is:

```text
Requirement Extraction Agent
        ↓
Workflow Modeling Agent
        ↓
Excel Intake Generation Agent
        ↓
Gap Analysis Agent
        ↓
Evaluation Agent
        ↓
Reviewer Agent
```

Each agent has a specific responsibility and produces an output that becomes the input for the next agent.

---

# Stage 1 — Requirement Extraction Agent

## Objective

The Requirement Extraction Agent reads the Confluence-style requirement and extracts structured requirement information.

This is the first and most important stage because it defines the source-of-truth requirement elements.

---

## Input

```text
Confluence requirement document
API requirement notes
Sample request payloads
Sample response payloads
Business process description
Validation rules
Downstream service references
Database references
```

---

## Output

```text
Structured requirement extraction output
Open questions
Confidence ratings
Source evidence
```

---

## Main Tasks

The agent should extract:

```text
API name
Endpoint path
HTTP method
Operation name
Required headers
Optional headers
Request body fields
Response body fields
Validation rules
Enum values
Business rules
Downstream services
Database references
Error handling rules
Technology assumptions
Open questions
```

---

## Exit Criteria

This stage is complete when:

```text
All visible requirement elements are extracted.
Missing or unclear details are marked.
No unsupported details are invented.
Source evidence is included where possible.
The output follows the requirement extraction schema.
```

---

# Stage 2 — Workflow Modeling Agent

## Objective

The Workflow Modeling Agent converts the extracted requirement into an ordered workflow representation.

This stage defines how the API logic should execute step by step.

---

## Input

```text
Structured requirement extraction output
Business rules
Validation rules
Service references
Database references
Response mapping details
Open questions from extraction stage
```

---

## Output

```text
Workflow representation
Ordered workflow steps
Decision points
Service call steps
Database query steps
Error handling paths
Workflow open questions
```

---

## Main Tasks

The agent should model:

```text
HTTP input
Header validation
Body validation
Enum validation
Transformation steps
Business rule steps
Downstream service calls
Database queries
Decision branches
Response mapping
Error handling
HTTP response
```

---

## Exit Criteria

This stage is complete when:

```text
The workflow has a clear ordered sequence.
Each step has an input and output.
Decision points are documented.
Service and database steps are source-supported.
Ambiguous workflow steps are marked for manual review.
The output follows the workflow representation schema.
```

---

# Stage 3 — Excel Intake Generation Agent

## Objective

The Excel Intake Generation Agent converts the requirement extraction and workflow representation into Excel-ready sheet content.

This stage prepares structured input for the future Go code generation factory.

---

## Input

```text
Structured requirement extraction output
Workflow representation
Technical requirement notes
Business rules
Service mappings
Database references
Open questions
```

---

## Output

Excel-ready content for:

```text
technical_requirements
api_endpoints
entities
external_services
business_requirements
endpoint_service_mapping
queries
```

---

## Main Tasks

The agent should generate:

```text
Technical requirement rows
API endpoint rows
Header and body definitions
Validation rule definitions
External service definitions
Business requirement rows
Endpoint-service mapping sequence
Entity references
Database query references
Open questions for missing sheet values
```

---

## Exit Criteria

This stage is complete when:

```text
Each required Excel sheet has structured content.
Required fields are captured where source-supported.
Missing values are marked as TBD or Needs Manual Review.
Target technology remains Go + PostgreSQL unless source says otherwise.
No unsupported services, tables, or CRUD assumptions are added.
```

---

# Stage 4 — Gap Analysis Agent

## Objective

The Gap Analysis Agent compares the Confluence requirement, workflow representation, and Excel intake content to detect gaps.

This stage acts as the quality-control layer.

---

## Input

```text
Requirement extraction output
Workflow modeling output
Excel intake generation output
Gap category definitions
Severity rules
Gap output schema
```

---

## Output

```text
Gap analysis report
Structured gap list
Severity summary
Recommendations
Open questions
Blocking gap count
```

---

## Main Tasks

The agent should compare:

```text
Confluence to Workflow
Confluence to Excel
Workflow to Excel
Excel to Confluence
Workflow to Confluence
```

The agent should detect:

```text
Missing endpoints
Missing headers
Missing request fields
Missing response fields
Missing validations
Missing enum values
Missing business rules
Missing downstream services
Missing database queries
Incorrect mappings
Technology mismatches
Unsupported assumptions
Workflow step gaps
Error handling gaps
```

---

## Exit Criteria

This stage is complete when:

```text
All detected gaps follow the gap output schema.
Each confirmed gap includes evidence.
Each gap has severity and confidence.
Each gap has an actionable recommendation.
Ambiguous findings are marked as Informational or Needs Manual Review.
Critical and High gaps are clearly flagged.
```

---

# Stage 5 — Evaluation Agent

## Objective

The Evaluation Agent compares the AI-generated gap report against the manual review baseline.

This stage measures the quality of the AI-assisted review.

---

## Input

```text
Manual baseline expected gaps
AI-detected gaps
Manual review time
AI-assisted review time
Evaluation metrics
Matching rules
Scoring model
```

---

## Output

```text
evaluation_result.json
evaluation_summary.md
gap_comparison_table.md
readiness_score.json
```

---

## Main Tasks

The agent should calculate:

```text
True positives
Partial matches
False positives
False negatives
Severity matches
Severity mismatches
Gap detection accuracy
False positive rate
False negative rate
Severity assignment accuracy
Manual review reduction
Code generation readiness score
```

---

## Exit Criteria

This stage is complete when:

```text
Manual and AI gaps are compared using matching rules.
Metrics are calculated using fixed formulas.
False positives and false negatives are reported honestly.
Measured results are separated from planned targets.
Readiness scoring is generated.
Limitations are documented.
```

---

# Stage 6 — Reviewer Agent

## Objective

The Reviewer Agent reviews all previous outputs and determines whether the artifact set is ready for the next stage.

This agent supports human review and does not replace it.

---

## Input

```text
Requirement extraction output
Workflow modeling output
Excel intake output
Gap analysis report
Evaluation result
Open questions
Low-confidence findings
```

---

## Output

```text
review_summary.md
risk_items.md
open_questions.md
approval_recommendation.md
```

---

## Main Tasks

The agent should review:

```text
Critical gaps
High gaps
Low-confidence findings
Unsupported assumptions
Technology mismatches
Missing evidence
Open questions
Code generation blockers
```

The agent should classify the artifact status as:

```text
Ready
Ready with Minor Cleanup
Needs Review
Blocked
Not Ready
```

---

## Exit Criteria

This stage is complete when:

```text
The final review status is assigned.
Blocking gaps are clearly listed.
Human review actions are identified.
Open questions are summarized.
The approval recommendation is evidence-based.
Critical gaps are not ignored.
```

---

# Full Agent Sequence Diagram

```text
Confluence Requirement
        ↓
[1] Requirement Extraction Agent
        ↓
Requirement Extraction Output
        ↓
[2] Workflow Modeling Agent
        ↓
Workflow Representation
        ↓
[3] Excel Intake Generation Agent
        ↓
Excel Intake Content
        ↓
[4] Gap Analysis Agent
        ↓
Gap Analysis Report
        ↓
[5] Evaluation Agent
        ↓
Evaluation Result and Readiness Score
        ↓
[6] Reviewer Agent
        ↓
Review Summary and Approval Recommendation
```

---

# Optional Future Extended Workflow

After the no-code planning phase and after implementation begins, the workflow can be extended.

Future sequence:

```text
Reviewer Agent
        ↓
Node-RED Flow Generation Agent
        ↓
Go Code Readiness Agent
        ↓
Go Factory Execution
        ↓
Generated Go Code Review
        ↓
Documentation Agent
        ↓
Research Reporting Agent
```

These later steps should only be used after the planning and validation layers are stable.

---

# Input and Output Handoff Rules

## Rule 1: Each Agent Must Consume the Previous Agent Output

Each stage should use the structured output from the previous stage.

Example:

```text
Workflow Modeling Agent should use Requirement Extraction Output.
Excel Intake Generation Agent should use Workflow Modeling Output.
Gap Analysis Agent should use Requirement, Workflow, and Excel outputs.
```

---

## Rule 2: Agents Must Not Skip Stages

The system should not jump directly from Confluence to Excel or Go code without intermediate validation.

Correct flow:

```text
Extract → Model → Generate → Compare → Evaluate → Review
```

Incorrect flow:

```text
Confluence → Go Code
```

Skipping stages increases the risk of missing requirements and unsupported assumptions.

---

## Rule 3: Each Agent Must Preserve Open Questions

Open questions should not disappear between stages.

If the Requirement Extraction Agent marks a field as unclear, the Workflow Modeling Agent and Excel Intake Generation Agent should carry that uncertainty forward.

---

## Rule 4: Each Agent Must Preserve Evidence

Evidence should be preserved when possible.

If a header was extracted from a Confluence section, that evidence should remain available in later stages.

---

## Rule 5: Each Agent Must Mark Unsupported Details

If an agent needs a value that is not available, it should mark it as:

```text
TBD
Needs Manual Review
Not Found
Ambiguous
Low Confidence
```

It should not invent the value.

---

# Quality Gates Between Agents

Quality gates are checkpoints between stages.

They prevent poor-quality outputs from moving forward.

---

## Quality Gate 1 — Requirement Extraction Review

Before workflow modeling, verify:

```text
Endpoint is identified.
Headers are extracted.
Request and response fields are captured.
Business rules are listed.
Downstream services are identified.
Missing details are marked.
No unsupported fields are added.
```

---

## Quality Gate 2 — Workflow Modeling Review

Before Excel intake generation, verify:

```text
Workflow steps are ordered.
Validation steps are included.
Service calls are source-supported.
Database calls are source-supported.
Decision points are documented.
Error paths are captured if available.
```

---

## Quality Gate 3 — Excel Intake Review

Before gap analysis, verify:

```text
All required sheets are represented.
Endpoint information is present.
Headers and request fields are included.
Validations are included.
Service mappings are included.
Business rules are included.
Queries are included when required.
Missing values are marked clearly.
```

---

## Quality Gate 4 — Gap Analysis Review

Before evaluation, verify:

```text
Each gap has a type.
Each gap has severity.
Each gap has confidence.
Each gap has evidence.
Each gap has a recommendation.
Critical and High gaps are flagged.
```

---

## Quality Gate 5 — Evaluation Review

Before final reviewer approval, verify:

```text
Metrics use fixed formulas.
Manual baseline is used.
False positives are reported.
False negatives are reported.
Review time is measured.
Readiness score is calculated.
Limitations are included.
```

---

# Failure Handling

If a stage fails or produces incomplete output, the system should not continue blindly.

---

## Failure Case 1: Missing Requirement Detail

If the requirement does not define a necessary field:

```text
Mark as Needs Manual Review.
Carry the issue into open questions.
Do not invent the missing value.
```

---

## Failure Case 2: Ambiguous Business Rule

If a business rule is unclear:

```text
Mark as Ambiguous.
Assign Low or Medium confidence.
Ask for human review.
Do not generate final logic as confirmed.
```

---

## Failure Case 3: Missing Excel Sheet Data

If Excel intake content cannot be generated for a sheet:

```text
Mark affected fields as TBD.
Create an open question.
Let Gap Analysis Agent report the missing information.
```

---

## Failure Case 4: Critical Gap Found

If a Critical gap is found:

```text
Pipeline status should become Blocked.
Reviewer Agent should require human action.
Do not mark the artifact as ready.
```

---

# Agent Workflow Status Values

Each stage can produce a status.

Allowed status values:

```text
Complete
Complete with Open Questions
Needs Review
Blocked
Not Ready
```

---

## Status Meaning

| Status                       | Meaning                                 |
| ---------------------------- | --------------------------------------- |
| Complete                     | Stage output is ready for next stage    |
| Complete with Open Questions | Mostly ready, but some questions remain |
| Needs Review                 | Human review needed before proceeding   |
| Blocked                      | Critical issue prevents continuation    |
| Not Ready                    | Output is incomplete or unusable        |

---

# Example End-to-End Sequence

## Input

A Confluence requirement defines:

```text
POST /api/v1/clienteling/messages
Required headers: Client-id, Division-id, X-Acting-As-Type
Rule: X-Acting-As-Type must equal global
Database: Insert into APP_MSGS
```

---

## Stage 1 Output

Requirement Extraction Agent extracts:

```text
Endpoint: POST /api/v1/clienteling/messages
Required Headers: Client-id, Division-id, X-Acting-As-Type
Business Rule: X-Acting-As-Type must equal global
Database Reference: APP_MSGS
```

---

## Stage 2 Output

Workflow Modeling Agent creates:

```text
HTTP Input
Validate Headers
Validate X-Acting-As-Type equals global
Validate Body
Check Active Message Overlap
Insert APP_MSGS
Map Response ID
HTTP Response
```

---

## Stage 3 Output

Excel Intake Generation Agent creates rows for:

```text
api_endpoints
business_requirements
endpoint_service_mapping
queries
```

---

## Stage 4 Output

Gap Analysis Agent detects:

```text
GAP-001: Missing Business Rule Gap
Excel missing X-Acting-As-Type equals global validation
Severity: High
```

---

## Stage 5 Output

Evaluation Agent compares the gap to manual baseline and marks it as:

```text
True Positive
Severity Match
```

---

## Stage 6 Output

Reviewer Agent recommends:

```text
Status: Needs Review
Action: Add X-Acting-As-Type validation to Excel api_endpoints and endpoint_service_mapping.
```

---

# Research Value of Agent Workflow Sequence

This workflow sequence supports the research direction:

```text
Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis
```

It shows that the project is not using AI randomly.

Instead, it defines a structured multi-agent pipeline with:

```text
Clear responsibilities
Source-of-truth behavior
Guardrails
Quality gates
Evaluation metrics
Human review checkpoints
```

This makes the project stronger for a research paper, resume, Medium article, and LinkedIn post.

---

## What This Phase Does Not Include

This phase does not include:

* Implementing agents
* Running actual prompts
* Creating executable workflows
* Building orchestration logic
* Parsing real files
* Generating Go code
* Running evaluation metrics

This phase only defines the future agent workflow sequence.

---

## Summary

The agent workflow sequence defines how specialized agents will work together in the future system.

The core sequence is:

```text
Requirement Extraction Agent
        ↓
Workflow Modeling Agent
        ↓
Excel Intake Generation Agent
        ↓
Gap Analysis Agent
        ↓
Evaluation Agent
        ↓
Reviewer Agent
```

Each agent has a clear input, output, task, and quality gate.

This sequence makes the system reliable, repeatable, reviewable, and research-ready.
