# Day 19 — AI-Assisted Process Baseline

## Purpose

This document defines the AI-assisted process baseline for the 50-Day AI Software Engineer Challenge.

The AI-assisted process baseline represents the proposed approach where an AI system helps convert enterprise API requirements into structured workflow artifacts.

This baseline is needed so the project can compare the AI-assisted approach against the traditional manual process.

The goal is not to remove human review. The goal is to measure whether AI can reduce initial workflow drafting effort, improve consistency, and produce reviewable workflow artifacts faster than a fully manual process.

---

## Why an AI-Assisted Baseline Is Needed

The project needs to evaluate whether AI provides measurable value in enterprise API workflow creation.

The AI-assisted baseline helps answer:

* How quickly can AI generate an initial workflow from a requirement?
* How complete is the AI-generated workflow?
* How many validation rules does the AI capture?
* How many business rules does the AI preserve?
* How accurate are downstream and database mappings?
* How much correction is needed after AI generation?
* Does AI produce more consistent workflow structure?
* Does AI introduce unsupported or hallucinated logic?
* Can validation improve AI output quality?

The AI-assisted baseline provides the proposed approach that will be compared with the manual baseline.

---

## Definition of AI-Assisted Process Baseline

The AI-assisted process baseline is the process where a source requirement is provided to an AI system and the AI generates a structured workflow artifact.

The AI-generated workflow is then reviewed using the validation framework, checklist, quality metrics, and failure categories defined earlier in the project.

The AI-assisted process includes both generation and validation.

The process should be measured in two stages:

```text
Initial AI Workflow
Corrected AI Workflow After Validation
```

This allows the project to measure both the raw AI output and the improved output after review.

---

## AI-Assisted Process Scope

The AI-assisted baseline includes:

* preparing the source requirement input
* providing the requirement to the AI system
* generating structured requirement output
* generating workflow representation
* applying the validation checklist
* calculating workflow quality metrics
* identifying missing or incorrect logic
* classifying validation failures
* correcting the workflow
* preparing artifact-ready workflow output

The AI-assisted baseline does not include final implementation or coding.

The project remains aligned with the no-coding-until-Day-25 rule.

---

## AI-Assisted Process Input

The AI-assisted process uses the same dataset item used by the manual baseline.

Input should include:

```text
Source Requirement
Sample Request
Sample Response
Business Rule Notes
Downstream Service Notes
Database Notes
Error Handling Notes
Ambiguity Notes
```

The same input should be used for both the manual and AI-assisted processes to keep comparison fair.

The AI should not be given the ground truth workflow during initial generation.

The ground truth workflow should only be used later for evaluation and scoring.

---

## AI-Assisted Process Output

The AI-assisted process should produce a structured workflow artifact.

The output should include:

* endpoint details
* request headers
* request body fields
* validation rules
* business rules
* downstream service calls
* database interactions
* ordered workflow steps
* response mapping
* error handling
* traceability references
* ambiguity notes
* assumptions
* artifact readiness notes

This output will be compared against the approved ground truth workflow.

---

## AI-Assisted Workflow Creation Steps

The AI-assisted baseline should follow the steps below.

---

## Step 1 — Prepare the Requirement Input

The source requirement should be prepared before being provided to the AI system.

The input should be clear enough for the AI to process but should not include the ground truth workflow.

### Activities

* Select an approved dataset item.
* Copy the source requirement text.
* Include sample request and response if available.
* Include known business rule notes.
* Include downstream or database notes if available.
* Include ambiguity notes if they are part of the dataset item.
* Do not include the approved ground truth workflow.

### Output

```text
Prepared AI input prompt context.
```

---

## Step 2 — Provide Requirement to AI

The prepared source requirement is provided to the AI system.

The AI should be asked to generate a structured workflow representation.

The prompt should request:

* endpoint extraction
* header extraction
* body extraction
* validation extraction
* business rule extraction
* downstream service identification
* database interaction identification
* workflow step generation
* response mapping
* error handling
* traceability
* ambiguity detection

### Prompt Goal

The goal is to produce an artifact-ready workflow, not implementation code.

---

## Step 3 — Generate Structured Requirement Output

Before generating the final workflow, the AI may first extract structured requirement information.

This step helps separate requirement understanding from workflow generation.

### Expected Sections

* endpoint
* headers
* request body
* validations
* business rules
* downstream services
* database interactions
* response mapping
* error scenarios
* ambiguity notes

### Why This Step Matters

Structured requirement output makes the workflow generation more transparent.

It also helps reviewers identify whether the AI misunderstood the source requirement before workflow steps are created.

---

## Step 4 — Generate Workflow Representation

The AI converts the structured requirement output into ordered workflow steps.

Each workflow step should include:

* step ID
* step name
* step type
* description
* input
* output
* success path
* failure path
* source reference

### Recommended Step Types

```text
REQUEST_INTAKE
HEADER_VALIDATION
BODY_VALIDATION
ENUM_VALIDATION
PRECONDITION_CHECK
DOWNSTREAM_CALL
DATABASE_QUERY
DATABASE_INSERT
BUSINESS_RULE
CALCULATION
RESPONSE_MAPPING
ERROR_MAPPING
FINAL_RESPONSE
```

### Expected Output

A structured workflow that can later support:

* validation
* quality scoring
* Excel intake generation
* Node-RED flow generation
* Go code generation after Day 25
* test case generation
* documentation generation

---

## Step 5 — Apply Validation Checklist

The AI-generated workflow should be reviewed using the Day 17 validation checklist.

The checklist should inspect:

* source requirement coverage
* endpoint correctness
* header validation
* request body validation
* business rule coverage
* workflow step order
* downstream service mapping
* database mapping
* response mapping
* error handling
* traceability
* hallucination detection
* artifact readiness

Each checklist item should be marked as:

```text
PASS
FAIL
WARNING
NOT APPLICABLE
```

---

## Step 6 — Calculate Workflow Quality Metrics

The AI-generated workflow should be scored using the Day 17 workflow quality metrics.

Metrics include:

* requirement coverage score
* step completeness score
* validation accuracy score
* business rule coverage score
* step ordering accuracy score
* downstream mapping accuracy score
* database mapping accuracy score
* response mapping accuracy score
* error handling completeness score
* traceability score
* hallucination control score
* artifact readiness score

The first score should represent the initial AI output before correction.

---

## Step 7 — Classify Validation Failures

Any issues found in the AI-generated workflow should be categorized using the Day 17 validation failure categories.

Examples:

| AI Output Issue                      | Failure Category               |
| ------------------------------------ | ------------------------------ |
| Missing required header validation   | Header Validation Failure      |
| Missing business rule                | Business Rule Coverage Failure |
| Wrong downstream request mapping     | Incorrect Downstream Mapping   |
| Wrong database table                 | Incorrect Database Mapping     |
| Missing error scenario               | Error Handling Failure         |
| Added unsupported authorization rule | Hallucinated Logic             |

This makes AI failure analysis measurable and repeatable.

---

## Step 8 — Correct the AI Workflow

After validation, the AI-generated workflow can be corrected.

Corrections should be based on:

* source requirement
* validation checklist failures
* quality metric gaps
* failure category analysis
* ground truth comparison during evaluation
* human review

The corrected AI workflow should be stored separately from the initial AI workflow.

This allows the project to compare:

```text
Initial AI Workflow
Corrected AI Workflow
```

---

## Step 9 — Record Timing and Effort

The AI-assisted process should record time and effort consistently.

Recommended timing categories:

| Time Category           | Description                                     |
| ----------------------- | ----------------------------------------------- |
| Prompt Preparation Time | Time spent preparing the requirement and prompt |
| AI Generation Time      | Time spent generating the initial workflow      |
| Validation Time         | Time spent reviewing the workflow               |
| Correction Time         | Time spent fixing the workflow                  |
| Total Time              | Total AI-assisted process time                  |

This makes the AI-assisted process comparable to the manual baseline.

---

## Step 10 — Compare Against Ground Truth

The initial and corrected AI workflows should be compared against the approved ground truth workflow.

This comparison should produce:

* initial AI workflow score
* corrected AI workflow score
* missing requirement count
* hallucinated logic count
* correction count
* artifact readiness decision

The ground truth should not be used during initial generation.

It should be used only during evaluation.

---

## AI-Assisted Baseline Measurement Template

Use the following template for each dataset item.

```text
Dataset Item ID:
API Name:
Requirement Category:
Complexity Level:

Prompt Preparation Time:
AI Generation Time:
Validation Time:
Correction Time:
Total Time:

AI Initial Workflow Score:
AI Corrected Workflow Score:

Missed Requirement Count:
Incorrect Mapping Count:
Hallucinated Logic Count:
Error Handling Gaps:
Business Rule Gaps:
Traceability Gaps:
Final AI Decision:
```

---

## AI-Assisted Workflow Output Template

The AI-assisted workflow output should follow this structure.

```text
# AI-Assisted Workflow Output — REQ-WF-001

## Metadata

Dataset Item ID:
API Name:
Requirement Category:
Complexity Level:
AI Model:
Prompt Version:
Generation Date:
Workflow Status:

---

## Endpoint

HTTP Method:
Endpoint Path:
Operation Name:

---

## Headers

List required and optional headers.

---

## Request Body

List required and optional body fields.

---

## Validations

List validation rules.

---

## Business Rules

List business rules.

---

## Downstream Services

List downstream services, if applicable.

---

## Database Interactions

List database interactions, if applicable.

---

## Workflow Steps

List ordered workflow steps.

---

## Response Mapping

Define response mapping.

---

## Error Handling

Define error scenarios.

---

## Traceability Notes

Link workflow elements to source requirement.

---

## Assumptions and Ambiguity

List any assumptions or unclear areas.

---

## Validation Result

Initial Score:
Corrected Score:
Failure Categories:
Final Decision:
```

---

## Example AI-Assisted Baseline

### Dataset Item

```text
Dataset Item ID: REQ-WF-001
API Name: Create Application Message API
Category: Simple Create API
Complexity: MEDIUM
```

### AI-Assisted Process Timing

```text
Prompt Preparation Time: 5 minutes
AI Generation Time: 2 minutes
Validation Time: 12 minutes
Correction Time: 10 minutes
Total Time: 29 minutes
```

### AI Workflow Score

```text
AI Initial Workflow Score: 80%
AI Corrected Workflow Score: 93%
```

### AI Gaps Found

```text
- Missing database error handling
- Response mapping used messageId instead of id
- Traceability references were incomplete
- Error response structure was too generic
```

### Final Decision

```text
AI Initial Workflow Decision: PASS WITH WARNINGS
AI Corrected Workflow Decision: PASS
```

---

## Strengths of AI-Assisted Process

The AI-assisted process has several strengths.

### 1. Faster Initial Drafting

AI can generate the first workflow draft quickly from a source requirement.

### 2. Consistent Structure

AI can follow a predefined schema and produce consistently organized artifacts.

### 3. Reduced Repetitive Documentation Effort

AI can reduce manual work involved in extracting headers, body fields, validations, and workflow sections.

### 4. Better Starting Point for Review

Even if the initial output is not perfect, it can provide a structured draft for human review.

### 5. Scalable Across Multiple APIs

AI-assisted workflow generation can be repeated across many dataset items faster than fully manual drafting.

---

## Weaknesses of AI-Assisted Process

The AI-assisted process also has limitations.

### 1. Risk of Hallucinated Logic

AI may add unsupported headers, services, rules, or response fields.

### 2. Missing Business Rules

AI may miss subtle or complex business rules.

### 3. Weak Ambiguity Handling

AI may guess unclear behavior instead of marking it as ambiguous.

### 4. Generic Error Handling

AI may produce generic error handling instead of requirement-specific error behavior.

### 5. Need for Human Validation

AI output should not be accepted blindly.

A validation and review layer is required.

---

## AI-Assisted Baseline Risks

The AI-assisted baseline has the following risks:

| Risk                        | Description                                  | Mitigation                                    |
| --------------------------- | -------------------------------------------- | --------------------------------------------- |
| Prompt Bias                 | Prompt quality may strongly affect AI output | Use consistent prompt templates               |
| Hallucination               | AI may add unsupported logic                 | Apply hallucination detection checklist       |
| Missing Details             | AI may miss small but important fields       | Compare against ground truth workflow         |
| Overconfidence              | AI output may look correct even when wrong   | Use validation metrics and failure categories |
| Inconsistent Model Behavior | AI output may vary between runs              | Record prompt version and model used          |
| Ambiguity Guessing          | AI may infer unclear requirements            | Require ambiguity notes                       |

---

## Fair AI-Assisted Baseline Rules

To keep the AI-assisted baseline fair:

1. Use the same source requirement as the manual process.
2. Do not provide the ground truth workflow during initial generation.
3. Use the same prompt template across dataset items.
4. Record prompt preparation time.
5. Record AI generation time separately.
6. Score the initial AI workflow before correction.
7. Score the corrected AI workflow separately.
8. Use the same validation checklist as the manual workflow.
9. Use the same workflow quality metrics as the manual workflow.
10. Use the same failure categories as the manual workflow.
11. Document hallucinated logic separately.
12. Document assumptions and ambiguity.

---

## Prompt Template for AI-Assisted Workflow Generation

A consistent prompt template should be used for evaluation.

Recommended prompt:

```text
You are assisting with enterprise API workflow extraction.

Given the source requirement below, generate a structured workflow representation.

Do not invent unsupported logic.
If any requirement is unclear, mark it as ambiguous instead of guessing.
Do not write implementation code.
Focus only on workflow-level design.

Return the output with these sections:
1. Endpoint
2. Headers
3. Request Body
4. Validations
5. Business Rules
6. Downstream Services
7. Database Interactions
8. Workflow Steps
9. Response Mapping
10. Error Handling
11. Traceability Notes
12. Ambiguity Notes
13. Artifact Readiness Notes

Source Requirement:
[PASTE REQUIREMENT HERE]
```

---

## AI Output Versioning

Each AI-assisted workflow should record version information.

Recommended fields:

```text
ai_model:
prompt_version:
generation_date:
output_version:
review_status:
change_summary:
```

Example:

```text
ai_model: GPT-based assistant
prompt_version: workflow_generation_prompt_v1
generation_date: 2026-06-18
output_version: 1.0
review_status: DRAFT
change_summary: Initial AI-generated workflow draft.
```

Versioning helps track improvements over time.

---

## Initial AI Score vs Corrected AI Score

The AI-assisted baseline should distinguish between initial and corrected scores.

This is important because the first AI output may be incomplete, but validation can improve it.

Example:

| Output Type           | Score |
| --------------------- | ----: |
| Initial AI Workflow   |   78% |
| Corrected AI Workflow |   93% |

This helps show the value of the validation framework.

The research claim should focus on the AI-assisted process, not raw AI output alone.

---

## Relationship to Manual Baseline

The AI-assisted baseline will be compared against the manual baseline.

Both processes use the same dataset item and are evaluated against the same ground truth workflow.

The comparison will measure:

```text
Manual Workflow
        vs
AI-Assisted Workflow
        vs
Ground Truth Workflow
```

The goal is to evaluate:

* time reduction
* quality difference
* correction effort
* repeatability
* artifact readiness
* failure patterns

---

## Expected Role in Research Paper

The AI-assisted baseline supports the research paper by providing the proposed method for comparison.

The paper can use this baseline to report:

* average AI workflow generation time
* average AI workflow quality score
* average AI correction count
* common AI workflow errors
* reduction in initial drafting effort
* improvement after validation
* comparison against manual workflow creation

Example research statement:

```text
The AI-assisted process reduced initial workflow drafting time while producing structured outputs that became comparable to manual workflows after validation and correction.
```

---

## Practical Value

The AI-assisted process has practical value for enterprise software teams.

It can help with:

* faster requirement analysis
* faster workflow drafting
* consistent documentation structure
* early gap identification
* repeatable artifact preparation
* easier review of complex API requirements
* preparing inputs for Excel, Node-RED, tests, and future code generation

This makes the project useful for enterprise API modernization and automation.

---

## Summary

The AI-assisted process baseline defines how the proposed AI workflow generation process will operate and be measured.

It includes prompt preparation, AI workflow generation, validation, scoring, failure classification, correction, and comparison against ground truth.

This baseline is essential for comparing AI-assisted workflow synthesis against the traditional manual process.

Day 19 Phase 3 establishes the AI-assisted baseline needed for future evaluation and research comparison.
