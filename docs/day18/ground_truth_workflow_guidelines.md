# Day 18 — Ground Truth Workflow Guidelines

## Purpose

This document defines the ground truth workflow guidelines for the workflow evaluation dataset in the 50-Day AI Software Engineer Challenge.

The purpose of a ground truth workflow is to provide the trusted expected workflow for each sample requirement in the evaluation dataset.

The AI-generated workflow will be compared against this ground truth workflow to measure correctness, completeness, traceability, and artifact readiness.

A ground truth workflow acts as the reference answer.

---

## Why Ground Truth Workflows Are Needed

The project is designed to evaluate whether an AI system can convert enterprise API requirements into structured workflow artifacts.

To evaluate the AI output, we need something reliable to compare it against.

The comparison is:

```text
Source Requirement
        ↓
AI-Generated Workflow
        ↓
Compare Against
        ↓
Ground Truth Workflow
```

Without a ground truth workflow, evaluation becomes subjective.

A reviewer may say a generated workflow “looks good,” but there would be no consistent way to measure whether it is correct.

Ground truth workflows solve this problem by defining the expected correct output for each dataset item.

---

## What a Ground Truth Workflow Means

A ground truth workflow is a manually reviewed and approved workflow that accurately represents the source requirement.

It should capture the correct:

* endpoint behavior
* header requirements
* request body requirements
* validation rules
* business rules
* downstream service calls
* database interactions
* workflow step order
* response mapping
* error handling
* traceability references

The ground truth workflow should be detailed enough to support validation, scoring, and future artifact generation.

---

## Role of Ground Truth in Evaluation

The ground truth workflow is used to evaluate the AI-generated workflow.

It helps answer:

* Did the AI capture the correct endpoint?
* Did the AI include all required validations?
* Did the AI preserve the business rules?
* Did the AI map downstream calls correctly?
* Did the AI capture database operations correctly?
* Did the AI generate the correct response mapping?
* Did the AI include required error scenarios?
* Did the AI avoid unsupported assumptions?
* Did the AI preserve correct workflow order?
* Is the AI-generated workflow ready for downstream artifact generation?

The ground truth workflow is the benchmark used to calculate quality metrics.

---

## Who Creates the Ground Truth Workflow

For this project, the ground truth workflow should be created manually by the project author or reviewer.

The person creating the ground truth workflow should review:

* the original source requirement
* sample request
* sample response
* business rule notes
* downstream service notes
* database notes
* expected error scenarios
* ambiguity notes

The ground truth workflow should not be generated blindly by AI without review.

AI can assist in drafting the workflow, but the final ground truth workflow must be reviewed and approved manually.

---

## Ground Truth Creation Principles

The ground truth workflow should follow these principles.

### 1. Source Requirement Is the Source of Truth

The ground truth workflow must be based on the source requirement.

Do not add behavior that is not supported by the requirement.

If something is unclear, mark it as ambiguous instead of guessing.

---

### 2. No Hallucinated Logic

The ground truth workflow should not include invented logic.

Avoid adding unsupported:

* headers
* request fields
* authorization rules
* downstream services
* database tables
* business rules
* response fields
* error behaviors

---

### 3. Workflow Must Be Complete

The workflow should include all required behavior from the source requirement.

A ground truth workflow should not capture only the happy path.

It should include:

* success path
* validation failure path
* business failure path
* downstream failure path, if applicable
* database failure path, if applicable

---

### 4. Workflow Must Be Ordered Correctly

The workflow steps must be arranged in the correct execution order.

Example order:

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
Perform Database Checks
↓
Apply Business Rules
↓
Map Final Response
↓
Return Success or Error
```

The exact order may change depending on the API, but the order must make logical sense.

---

### 5. Workflow Must Be Traceable

Each workflow step should be traceable to the source requirement.

Traceability helps prove that the workflow was derived from the requirement and not invented.

Each step should reference one or more of the following:

* requirement ID
* business rule ID
* endpoint ID
* sample request field
* sample response field
* downstream service reference
* database query reference

---

### 6. Workflow Must Be Artifact-Ready

The ground truth workflow should be structured enough to support future artifact generation.

It should be usable for:

* Node-RED flow generation
* Excel intake generation
* Go code generation later
* test case generation
* documentation generation
* validation scoring

A workflow written only as a paragraph is not enough.

It should be broken into structured workflow steps.

---

## Recommended Ground Truth Workflow Structure

Each ground truth workflow should follow a consistent structure.

Recommended file format:

```text
ground_truth_workflow_id:
dataset_item_id:
api_name:
source_requirement_reference:
workflow_status:
complexity_level:
endpoint:
headers:
request_body:
validations:
business_rules:
downstream_services:
database_interactions:
workflow_steps:
response_mapping:
error_handling:
traceability_matrix:
ambiguity_notes:
review_status:
```

---

## Ground Truth Field Definitions

### ground_truth_workflow_id

A unique ID for the ground truth workflow.

Example:

```text
WF-GT-001
```

---

### dataset_item_id

The dataset item connected to the ground truth workflow.

Example:

```text
REQ-WF-001
```

---

### api_name

The name of the API.

Example:

```text
Create Application Message API
```

---

### source_requirement_reference

The source requirement file or document used to create the ground truth workflow.

Example:

```text
datasets/workflow_evaluation/requirements/REQ-WF-001.md
```

---

### workflow_status

The status of the ground truth workflow.

Recommended values:

```text
DRAFT
REVIEWED
APPROVED
NEEDS_UPDATE
```

Only approved ground truth workflows should be used for formal evaluation.

---

### complexity_level

The complexity level of the workflow.

Recommended values:

```text
LOW
MEDIUM
HIGH
```

---

### endpoint

The endpoint details represented by the workflow.

This should include:

* HTTP method
* endpoint path
* operation name
* request type
* response type

---

### headers

The request headers expected by the workflow.

This should include:

* header name
* required or optional status
* data type
* validation rule
* downstream propagation rule, if applicable

---

### request_body

The request body fields expected by the workflow.

This should include:

* field name
* required or optional status
* data type
* enum values, if applicable
* nested path, if applicable
* validation rule

---

### validations

The validation rules required by the workflow.

Examples:

* required header validation
* required field validation
* enum validation
* numeric validation
* timestamp format validation
* conditional validation

---

### business_rules

The business rules represented in the workflow.

Examples:

* overlap check
* eligibility check
* discount calculation rule
* reject condition
* branching condition
* cap or threshold rule

---

### downstream_services

The downstream service calls required by the workflow.

This should include:

* service ID
* service name
* service type
* endpoint path
* HTTP method
* request mapping
* header mapping
* response mapping
* error behavior

---

### database_interactions

The database interactions required by the workflow.

This should include:

* query ID
* table name
* query purpose
* query type
* input parameters
* output fields
* no-record behavior
* database error behavior

---

### workflow_steps

The ordered steps of the workflow.

Each workflow step should have:

* step ID
* step name
* step type
* description
* input
* output
* success path
* failure path
* source reference

Recommended step types:

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

---

### response_mapping

The final response mapping expected from the workflow.

This should include:

* success response structure
* error response structure
* field-level mappings
* transformation rules
* source field
* target field
* data type

---

### error_handling

The error handling behavior expected from the workflow.

This should include:

* validation errors
* business rule failures
* downstream failures
* database failures
* no-record scenarios
* unexpected errors
* HTTP status mapping
* error response format

---

### traceability_matrix

The traceability matrix links workflow steps back to source requirement references.

Example:

| Workflow Step | Source Reference          | Requirement Element          |
| ------------- | ------------------------- | ---------------------------- |
| STEP-001      | REQ-WF-001 Header Section | Required headers             |
| STEP-002      | REQ-WF-001 Body Section   | Required request body fields |
| STEP-003      | BR-001                    | Overlap validation rule      |
| STEP-004      | DB-001                    | APP_MSGS overlap query       |

---

### ambiguity_notes

This section records unclear or incomplete requirement details.

Ambiguous areas should be clearly documented instead of guessed.

Example:

```text
The requirement says "standard headers are required" but does not define the exact header list.
This should be marked as ambiguous and should not be expanded without confirmation.
```

---

### review_status

The final review status.

Recommended values:

```text
DRAFT
REVIEWED
APPROVED
NEEDS_UPDATE
```

---

## Recommended Ground Truth Workflow Template

````text
# Ground Truth Workflow — WF-GT-001

## Metadata

| Field | Value |
|---|---|
| Ground Truth Workflow ID | WF-GT-001 |
| Dataset Item ID | REQ-WF-001 |
| API Name |  |
| Source Requirement Reference |  |
| Complexity Level | LOW / MEDIUM / HIGH |
| Workflow Status | DRAFT / REVIEWED / APPROVED / NEEDS_UPDATE |

---

## Endpoint

| Field | Value |
|---|---|
| HTTP Method |  |
| Endpoint Path |  |
| Operation Name |  |
| Request Type |  |
| Response Type |  |

---

## Headers

| Header | Required | Data Type | Validation Rule | Downstream Propagation |
|---|---|---|---|---|

---

## Request Body

| Field | Required | Data Type | Enum / Format | Validation Rule |
|---|---|---|---|---|

---

## Validations

| Validation ID | Validation Rule | Source Reference |
|---|---|---|

---

## Business Rules

| Business Rule ID | Rule Description | Source Reference |
|---|---|---|

---

## Downstream Services

| Service ID | Service Name | Method | Endpoint | Purpose |
|---|---|---|---|---|

---

## Database Interactions

| Query ID | Table | Query Type | Purpose | Input | Output |
|---|---|---|---|---|---|

---

## Workflow Steps

| Step ID | Step Name | Step Type | Description | Success Path | Failure Path | Source Reference |
|---|---|---|---|---|---|---|

---

## Response Mapping

| Source | Target | Data Type | Transformation Rule |
|---|---|---|---|

---

## Error Handling

| Error Scenario | Expected Behavior | HTTP Status | Error Response |
|---|---|---|---|

---

## Traceability Matrix

| Workflow Element | Source Requirement Reference | Notes |
|---|---|---|

---

## Ambiguity Notes

List unclear or incomplete requirement areas here.

---

## Review Status

```text
DRAFT
````

````

---

## Example Ground Truth Workflow

```text
# Ground Truth Workflow — WF-GT-001

## Metadata

Ground Truth Workflow ID: WF-GT-001
Dataset Item ID: REQ-WF-001
API Name: Create Application Message API
Complexity Level: MEDIUM
Workflow Status: DRAFT

## Endpoint

HTTP Method: POST
Endpoint Path: /api/v1/clienteling/messages
Operation Name: createApplicationMessage

## Headers

Client-Id: required string
Subclient-Id: required string
Division-Id: required numeric
Store-Number: required string
User-Id: required string
Trace-Id: required string

## Request Body

title: required string
message: required string
messageSeverity: required enum INFO, CRITICAL
startDateTime: required timestamp
endDateTime: optional timestamp

## Business Rules

BR-001:
Before inserting a new message, check whether an active message already exists for the same division with an overlapping time window.
If overlap exists, reject the request.

## Database Interactions

DB-001:
Check APP_MSGS for active overlapping messages using division and time window.

DB-002:
Insert new application message into APP_MSGS.

## Workflow Steps

STEP-001:
Receive request.

STEP-002:
Validate required headers.

STEP-003:
Validate request body required fields.

STEP-004:
Validate messageSeverity enum.

STEP-005:
Check APP_MSGS for overlapping active message.

STEP-006:
If overlap exists, return business validation error.

STEP-007:
If no overlap exists, insert new message into APP_MSGS.

STEP-008:
Map generated message id to response.

STEP-009:
Return success response.

STEP-010:
Handle validation and database errors.

## Response Mapping

APP_MSGS.id maps to response.id.

## Error Handling

Missing required headers return validation error.
Invalid messageSeverity returns validation error.
Overlap detected returns business validation error.
Database failure returns internal error.

## Ambiguity Notes

No ambiguity identified in this sample.

## Review Status

DRAFT
````

---

## Level of Detail Required

The ground truth workflow should be detailed enough to support comparison, but not so detailed that it becomes implementation code.

The workflow should define what must happen, not how the final code must be written.

Good level of detail:

```text
Validate messageSeverity against INFO and CRITICAL.
```

Too vague:

```text
Validate request.
```

Too implementation-specific before Day 25:

```text
Use Go validator package to check messageSeverity and return gin.Context JSON response.
```

Until Day 25, ground truth workflows should stay at the design and workflow level.

---

## Ground Truth Workflow Review Process

A ground truth workflow should go through the following review process:

```text
Draft ground truth workflow
        ↓
Check against source requirement
        ↓
Check required fields and validations
        ↓
Check business rules
        ↓
Check downstream and database mappings
        ↓
Check response and error mapping
        ↓
Check traceability
        ↓
Mark ambiguity
        ↓
Approve for evaluation
```

Only approved workflows should be used as evaluation references.

---

## How Ground Truth Supports Quality Metrics

Ground truth workflows directly support the quality metrics defined on Day 17.

Each metric compares the AI-generated workflow against the ground truth workflow.

Examples:

### Requirement Coverage Score

Checks how much of the ground truth workflow is present in the AI-generated workflow.

### Validation Accuracy Score

Checks whether validation steps match the ground truth validations.

### Business Rule Coverage Score

Checks whether the AI captured the same business rules as the ground truth.

### Step Ordering Accuracy Score

Checks whether the AI-generated step order matches the ground truth logic.

### Downstream Mapping Accuracy Score

Checks whether downstream service details match the ground truth.

### Database Mapping Accuracy Score

Checks whether database interactions match the ground truth.

### Response Mapping Accuracy Score

Checks whether final response mapping matches the ground truth.

### Error Handling Completeness Score

Checks whether required error paths match the ground truth.

### Hallucination Control Score

Checks whether the AI added anything not present in the ground truth or source requirement.

---

## How Ground Truth Supports Failure Classification

If an AI-generated workflow does not match the ground truth, the issue can be categorized using Day 17 failure categories.

Examples:

| AI Output Issue                       | Failure Category               |
| ------------------------------------- | ------------------------------ |
| Missing required header validation    | Header Validation Failure      |
| Missing overlap check                 | Business Rule Coverage Failure |
| Wrong database table                  | Incorrect Database Mapping     |
| Incorrect response field name         | Response Mapping Failure       |
| Added unsupported authorization logic | Hallucinated Logic             |
| Missing downstream failure handling   | Error Handling Failure         |

This makes failure analysis consistent and measurable.

---

## Ground Truth Approval Criteria

A ground truth workflow can be marked as approved only when:

* endpoint details are correct
* required headers are captured
* request body fields are captured
* validations are complete
* business rules are complete
* downstream services are mapped, if applicable
* database interactions are mapped, if applicable
* workflow steps are ordered correctly
* response mapping is defined
* error handling is defined
* ambiguity is documented
* no unsupported assumptions are included
* workflow is traceable to the source requirement
* workflow is artifact-ready

---

## Ground Truth Versioning

Ground truth workflows should be version-controlled.

If the source requirement changes, the ground truth workflow should also be updated.

Recommended version fields:

```text
version:
created_date:
updated_date:
reviewer:
change_summary:
```

Example:

```text
version: 1.0
created_date: 2026-06-18
updated_date: 2026-06-18
reviewer: Aswin
change_summary: Initial ground truth workflow created for evaluation dataset.
```

---

## Ground Truth File Naming Convention

Ground truth workflow files should use a consistent naming convention.

Recommended format:

```text
WF-GT-001.md
WF-GT-002.md
WF-GT-003.md
```

Recommended folder:

```text
datasets/workflow_evaluation/ground_truth_workflows/
```

Each ground truth workflow should map to one dataset requirement file.

Example:

```text
REQ-WF-001.md  →  WF-GT-001.md
REQ-WF-002.md  →  WF-GT-002.md
REQ-WF-003.md  →  WF-GT-003.md
```

---

## Ground Truth vs AI-Generated Workflow

The ground truth workflow and AI-generated workflow should use the same structure where possible.

This makes comparison easier.

Recommended comparison format:

| Section          | Ground Truth | AI-Generated    | Match Status |
| ---------------- | ------------ | --------------- | ------------ |
| Endpoint         | Present      | Present         | PASS         |
| Headers          | 6 required   | 5 required      | FAIL         |
| Body Fields      | 5 fields     | 5 fields        | PASS         |
| Business Rules   | 1 rule       | 0 rules         | FAIL         |
| Database Steps   | 2 steps      | 1 step          | FAIL         |
| Response Mapping | id field     | messageId field | FAIL         |

This comparison helps calculate workflow quality scores.

---

## What Not to Do in Ground Truth Workflows

Avoid the following:

* Do not invent logic not present in the source requirement.
* Do not include confidential internal data.
* Do not include implementation code before Day 25.
* Do not mark ambiguous behavior as confirmed.
* Do not skip error handling.
* Do not create ground truth from AI output without human review.
* Do not include vague steps that cannot be evaluated.
* Do not use inconsistent field names across dataset items.

---

## How This Supports the Research Project

Ground truth workflows are critical for the research value of this project.

They allow the system to report measurable results instead of subjective claims.

With ground truth workflows, the project can measure:

* how much of the requirement the AI captured
* how many workflow steps were correct
* how many business rules were preserved
* how many mappings were wrong
* how many hallucinated steps were added
* how much validation improves workflow quality

Example research result:

```text
Across 12 dataset samples, AI-generated workflows achieved an average ground-truth match score of 82%.
After applying the validation framework, corrected workflows improved to 93%.
```

This supports the research paper’s claim that AI-assisted workflow synthesis can reduce enterprise API development effort when paired with structured validation.

---

## Summary

Ground truth workflows are the trusted expected outputs for the workflow evaluation dataset.

They allow AI-generated workflows to be evaluated in a structured and measurable way.

A good ground truth workflow should be:

* complete
* accurate
* traceable
* logically ordered
* artifact-ready
* free from unsupported assumptions
* reviewed and approved

Day 18 Phase 3 defines how ground truth workflows should be created, structured, reviewed, and used for evaluation.
