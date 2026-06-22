# Day 20 — Methodology Section Draft

## Purpose

This document drafts the methodology section for the research paper in the 50-Day AI Software Engineer Challenge.

The methodology explains how the proposed AI-assisted workflow synthesis framework works and how it will be evaluated.

The goal of this section is to clearly describe the process used to convert enterprise API requirements into structured, validated, artifact-ready workflow representations.

This methodology should be written in a research-oriented way, not as an implementation guide.

---

## Methodology Overview

This work proposes a validation-driven AI-assisted methodology for converting enterprise API requirements into structured workflow artifacts.

The methodology consists of the following stages:

```text
1. Source requirement intake
2. Requirement extraction
3. Structured requirement representation
4. Workflow synthesis
5. Workflow validation
6. Workflow quality scoring
7. Failure classification
8. Human review and correction
9. Ground truth comparison
10. Manual baseline comparison
11. Artifact-ready workflow preparation
```

The methodology is designed to evaluate not only whether AI can generate workflow drafts, but also whether those workflows can be validated, corrected, and compared against human-created baselines.

---

## Methodology Pipeline

The overall methodology pipeline is:

```text
Source Requirement
        ↓
Requirement Extraction
        ↓
Structured Requirement Output
        ↓
Workflow Synthesis
        ↓
Workflow Validation
        ↓
Quality Scoring
        ↓
Failure Classification
        ↓
Human Review and Correction
        ↓
Ground Truth Comparison
        ↓
Manual Baseline Comparison
        ↓
Artifact-Ready Workflow
```

This pipeline ensures that AI-generated workflows are not accepted blindly.

Instead, they are reviewed using structured validation methods before being considered artifact-ready.

---

## Stage 1 — Source Requirement Intake

### Description

The first stage is source requirement intake.

Enterprise API requirements may come from different sources, including:

* Confluence pages
* API design documents
* sample requests
* sample responses
* Excel specifications
* business rule notes
* downstream service notes
* database mapping notes
* legacy system documentation
* manually written requirement samples

The source requirement acts as the input to both the manual baseline process and the AI-assisted workflow synthesis process.

---

### Source Requirement Characteristics

The source requirement may include:

* endpoint description
* HTTP method
* endpoint path
* required headers
* optional headers
* request body fields
* validation rules
* business rules
* downstream service dependencies
* database interactions
* response structure
* error scenarios
* ambiguity or missing information

The methodology assumes that source requirements may be incomplete or semi-structured, which reflects common enterprise API development conditions.

---

### Requirement Intake Goal

The goal of intake is to preserve the original requirement as the source of truth.

The AI system should not invent missing behavior during this stage.

If a requirement is unclear, it should be marked as ambiguous instead of being guessed.

---

## Stage 2 — Requirement Extraction

### Description

In the second stage, the source requirement is analyzed to extract structured requirement elements.

This extraction stage identifies the major components needed to create a workflow.

---

### Extracted Elements

The requirement extraction stage captures:

| Element               | Description                                                        |
| --------------------- | ------------------------------------------------------------------ |
| Endpoint              | HTTP method, path, operation name, and API purpose                 |
| Headers               | Required and optional request headers                              |
| Request Body          | Required and optional request body fields                          |
| Validations           | Header, body, enum, format, and conditional validations            |
| Business Rules        | Decision logic, eligibility checks, calculations, and reject rules |
| Downstream Services   | External or internal service calls required by the API             |
| Database Interactions | Queries, inserts, updates, lookups, and no-record behavior         |
| Response Mapping      | Success and error response structures                              |
| Error Handling        | Validation, business, downstream, database, and unexpected errors  |
| Traceability          | Source references for extracted elements                           |
| Ambiguity             | Requirement areas that need review                                 |

---

### Extraction Principle

The extraction process should be requirement-first.

Only behavior supported by the source requirement should be extracted as confirmed logic.

Any unclear areas should be marked separately as ambiguity.

---

## Stage 3 — Structured Requirement Representation

### Description

After extracting requirement elements, the methodology converts them into a structured requirement representation.

This representation acts as an intermediate artifact between raw source text and workflow generation.

---

### Purpose of Structured Requirement Representation

The structured requirement representation helps separate two tasks:

```text
Understanding the requirement
        vs
Generating the workflow
```

This separation makes the process easier to validate.

If the structured requirement output is incorrect, the workflow generated from it will also be incorrect.

---

### Recommended Sections

The structured requirement output should include:

```text
metadata
endpoint
headers
request_body
validations
business_rules
downstream_services
database_interactions
response_mapping
error_handling
traceability
ambiguity_notes
```

---

### Example Structure

```text
api_name:
endpoint:
  method:
  path:
  operation_name:

headers:
  required:
  optional:

request_body:
  fields:

validations:
  rules:

business_rules:
  rules:

downstream_services:
  services:

database_interactions:
  queries:

response_mapping:
  success:
  error:

ambiguity_notes:
```

---

## Stage 4 — Workflow Synthesis

### Description

Workflow synthesis converts the structured requirement representation into an ordered workflow.

This workflow is the central intermediate artifact in the proposed framework.

The workflow captures the operational behavior of the API without writing implementation code.

---

### Workflow Representation

A workflow contains ordered steps that describe how the API should behave.

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

---

### Workflow Step Types

Recommended workflow step types include:

```text
REQUEST_INTAKE
HEADER_VALIDATION
BODY_VALIDATION
ENUM_VALIDATION
PRECONDITION_CHECK
DOWNSTREAM_CALL
DATABASE_QUERY
DATABASE_INSERT
DATABASE_UPDATE
BUSINESS_RULE
CALCULATION
RESPONSE_MAPPING
ERROR_MAPPING
FINAL_RESPONSE
```

---

### Workflow Ordering

The workflow should follow a logical execution order.

A typical order is:

```text
Receive Request
        ↓
Validate Headers
        ↓
Validate Request Body
        ↓
Apply Preconditions
        ↓
Call Downstream Services
        ↓
Perform Database Interactions
        ↓
Apply Business Rules
        ↓
Perform Calculations
        ↓
Map Response
        ↓
Return Success or Error
```

The exact order may vary based on the API, but the workflow must preserve the dependency between steps.

---

## Stage 5 — Workflow Validation

### Description

After workflow synthesis, the generated workflow is validated.

Validation is a required quality gate in the methodology.

The purpose of validation is to check whether the generated workflow correctly represents the source requirement.

---

### Validation Scope

The validation framework checks:

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

---

### Validation Checklist

Each workflow is reviewed using a validation checklist.

Checklist items can be marked as:

```text
PASS
FAIL
WARNING
NOT APPLICABLE
```

This makes workflow review repeatable and consistent.

---

### Validation Purpose

Validation helps detect:

* missing workflow steps
* missing validation rules
* missing business rules
* wrong step order
* incorrect downstream mapping
* incorrect database mapping
* incorrect response mapping
* incomplete error handling
* unsupported assumptions
* hallucinated logic

---

## Stage 6 — Workflow Quality Scoring

### Description

After validation, each workflow is scored using workflow quality metrics.

The purpose of scoring is to convert workflow review into measurable evaluation.

---

### Quality Metrics

The workflow quality metrics include:

| Metric                            | Purpose                                              |
| --------------------------------- | ---------------------------------------------------- |
| Requirement Coverage Score        | Measures how much of the requirement is captured     |
| Step Completeness Score           | Measures whether required workflow steps are present |
| Validation Accuracy Score         | Measures correctness of validation rules             |
| Business Rule Coverage Score      | Measures business rule preservation                  |
| Step Ordering Accuracy Score      | Measures workflow sequencing correctness             |
| Downstream Mapping Accuracy Score | Measures downstream service mapping correctness      |
| Database Mapping Accuracy Score   | Measures database operation correctness              |
| Response Mapping Accuracy Score   | Measures response mapping correctness                |
| Error Handling Completeness Score | Measures error scenario coverage                     |
| Traceability Score                | Measures source reference coverage                   |
| Hallucination Control Score       | Measures unsupported logic avoidance                 |
| Artifact Readiness Score          | Measures downstream generation readiness             |

---

### Scoring Purpose

The scores allow comparison between:

```text
AI-generated workflows
Manual workflows
Ground truth workflows
Corrected workflows
```

This makes the methodology suitable for research evaluation.

---

## Stage 7 — Failure Classification

### Description

If a workflow fails validation, the failure is categorized.

Failure classification helps identify what type of issue occurred.

---

### Failure Categories

Failure categories include:

* missing requirement coverage
* incorrect endpoint mapping
* header validation failure
* request body validation failure
* business rule coverage failure
* incorrect workflow step order
* missing downstream service call
* incorrect downstream mapping
* incorrect database mapping
* response mapping failure
* error handling failure
* traceability failure
* hallucinated logic
* ambiguous requirement handling failure
* artifact readiness failure

---

### Failure Classification Purpose

Failure classification helps answer:

* What did the AI miss?
* What did the AI incorrectly add?
* Which workflow areas are most error-prone?
* Which prompt or schema sections need improvement?
* What types of human review are most important?

This supports continuous improvement of the workflow synthesis process.

---

## Stage 8 — Human Review and Correction

### Description

Human review remains part of the methodology.

The framework does not assume that AI-generated workflows are always correct.

A human reviewer checks the validation results, reviews failures, and corrects the workflow.

---

### Review Responsibilities

The reviewer should:

* confirm requirement coverage
* resolve ambiguity
* remove unsupported assumptions
* add missing workflow steps
* correct mappings
* verify business rules
* verify error handling
* approve artifact readiness

---

### Initial vs Corrected Workflow

The methodology separates the initial generated workflow from the corrected workflow.

This produces two scores:

```text
Initial AI Workflow Score
Corrected AI Workflow Score
```

This distinction is important because it shows how much validation and review improves output quality.

---

## Stage 9 — Ground Truth Workflow Comparison

### Description

Each AI-generated workflow is compared against an approved ground truth workflow.

The ground truth workflow is the trusted expected output for a dataset item.

---

### Ground Truth Purpose

Ground truth comparison helps determine:

* what the AI captured correctly
* what the AI missed
* what the AI mapped incorrectly
* what unsupported logic the AI introduced
* how close the AI output is to the expected workflow

---

### Comparison Model

```text
Source Requirement
        ↓
AI-Generated Workflow
        ↓
Compare Against
        ↓
Ground Truth Workflow
```

---

### Ground Truth Evaluation

The comparison should evaluate:

* endpoint match
* header match
* request body match
* validation match
* business rule match
* downstream mapping match
* database mapping match
* response mapping match
* error handling match
* traceability match
* artifact readiness match

---

## Stage 10 — Manual Baseline Comparison

### Description

The AI-assisted process is compared against a manual baseline.

In the manual baseline, a human reviewer reads the same source requirement and manually creates a workflow artifact.

Both outputs are compared against the same ground truth workflow.

---

### Comparison Model

```text
Approved Dataset Item
        ↓
Manual Workflow Creation
        ↓
Manual Workflow Score
        ↓
AI-Assisted Workflow Generation
        ↓
AI Workflow Score
        ↓
Manual vs AI Comparison
```

---

### Baseline Comparison Metrics

The comparison measures:

* total time taken
* initial drafting time
* time reduction percentage
* requirement coverage
* validation accuracy
* business rule coverage
* response mapping accuracy
* error handling completeness
* correction count
* hallucination count
* repeatability
* artifact readiness

---

### Purpose of Baseline Comparison

The purpose is to determine whether AI-assisted workflow synthesis provides measurable value compared to manual workflow creation.

The evaluation should not claim that AI replaces the manual process.

Instead, it should measure whether AI reduces early drafting effort and improves consistency when combined with validation.

---

## Stage 11 — Artifact-Ready Workflow Preparation

### Description

The final output of the methodology is an artifact-ready workflow.

This workflow has passed validation, has been reviewed, and is structured enough to support downstream artifact generation.

---

### Artifact-Ready Targets

The validated workflow can later support:

* Excel intake generation
* Node-RED flow generation
* test case generation
* documentation generation
* gap analysis reports
* Go code generation after Day 25

---

### Artifact Readiness Criteria

A workflow is artifact-ready when:

* endpoint details are complete
* headers are defined
* request body is defined
* validations are defined
* business rules are defined
* downstream services are mapped
* database interactions are mapped
* workflow steps are ordered
* response mapping is defined
* error handling is defined
* traceability is available
* ambiguity is documented
* unsupported logic is removed

---

## Evaluation Dataset Usage

The methodology uses a curated workflow evaluation dataset.

Each dataset item includes:

* source requirement
* sample request
* sample response
* expected requirement details
* ground truth workflow
* ambiguity notes
* review status

The dataset should include different requirement categories such as:

* validation-only API
* simple create API
* database lookup API
* downstream REST wrapper API
* response transformation API
* business rule branching API
* multi-step orchestration API
* calculation-based API
* error-handling-heavy API
* ambiguous requirement API
* existing system modernization API
* mixed enterprise API

---

## Methodology Example

### Source Requirement

```text
The API creates an application message.
It must validate required headers and request body fields.
messageSeverity must be INFO or CRITICAL.
Before inserting the message, the API must check whether an active message already exists for the same division with an overlapping time window.
If overlap exists, reject the request.
If no overlap exists, insert the message and return the generated id.
```

---

### Extracted Requirement Elements

```text
Endpoint:
POST /api/v1/clienteling/messages

Headers:
Client-Id required
Subclient-Id required
Division-Id required
Store-Number required
Trace-Id required

Request Body:
title required
message required
messageSeverity required enum INFO, CRITICAL
startDateTime required
endDateTime optional

Business Rule:
Reject overlapping active message for same division and time window.

Database:
Check APP_MSGS for overlap.
Insert APP_MSGS record if no overlap exists.

Response:
Return generated id.
```

---

### Generated Workflow

```text
STEP-001: Receive request
STEP-002: Validate required headers
STEP-003: Validate required body fields
STEP-004: Validate messageSeverity enum
STEP-005: Check APP_MSGS for overlapping active message
STEP-006: If overlap exists, return business validation error
STEP-007: Insert message into APP_MSGS
STEP-008: Map generated id to response
STEP-009: Return success response
STEP-010: Handle validation and database errors
```

---

### Validation Result Example

```text
Requirement Coverage Score: 92%
Validation Accuracy Score: 90%
Business Rule Coverage Score: 100%
Database Mapping Accuracy Score: 95%
Response Mapping Accuracy Score: 90%
Error Handling Completeness Score: 80%
Hallucination Control Score: 100%
Artifact Readiness Score: 92%

Final Decision: PASS WITH WARNINGS
```

---

## Methodology Assumptions

The methodology makes the following assumptions:

* Source requirements are available in text or semi-structured format.
* Ground truth workflows can be created for evaluation samples.
* Human review is available for validation and correction.
* The AI system is used for workflow synthesis, not direct production deployment.
* The evaluation dataset is sanitized and safe for research use.
* Workflow quality can be scored using defined metrics.

---

## Methodology Constraints

The methodology has the following constraints:

* It does not perform production code generation during the current documentation phase.
* It does not assume AI output is always correct.
* It does not eliminate human review.
* It does not resolve business ambiguity automatically.
* It does not claim full autonomous software engineering.
* It depends on the quality of the source requirement and prompt design.

---

## Methodology Evaluation Plan

The methodology will be evaluated by applying it to approved dataset samples.

For each dataset item:

```text
1. Create or select source requirement.
2. Create approved ground truth workflow.
3. Generate AI-assisted workflow.
4. Create manual workflow baseline.
5. Validate both workflows.
6. Score both workflows.
7. Classify failures.
8. Correct workflows.
9. Compare manual and AI results.
10. Record findings.
```

---

## Expected Methodology Outcome

The expected outcome is that the AI-assisted process will:

* reduce initial workflow drafting time
* produce more consistent structure
* provide a strong starting point for review
* require validation to correct gaps
* improve after human review
* become comparable to manual output after correction
* support artifact-ready workflow preparation

---

## Research Method Summary

The research method can be summarized as:

```text
This study evaluates an AI-assisted framework for synthesizing enterprise API workflows from natural language requirements. The framework generates structured workflow representations, validates them using checklist-based and metric-based evaluation, compares them against ground truth workflows, and evaluates performance against a manual baseline using efficiency, correctness, repeatability, and artifact-readiness metrics.
```

---

## Summary

This methodology section defines how the proposed AI-assisted workflow synthesis framework operates and how it will be evaluated.

The methodology includes:

* source requirement intake
* requirement extraction
* structured requirement representation
* workflow synthesis
* workflow validation
* workflow quality scoring
* failure classification
* human review and correction
* ground truth comparison
* manual baseline comparison
* artifact-ready workflow preparation

Day 20 Phase 3 establishes the research methodology needed to explain how the system works and how its value will be measured.
