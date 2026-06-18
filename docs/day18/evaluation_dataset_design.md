# Day 18 — Evaluation Dataset Design

## Purpose

This document defines the evaluation dataset design for the 50-Day AI Software Engineer Challenge.

The purpose of the evaluation dataset is to provide a structured set of enterprise API requirement samples that can be used to test whether the AI system can correctly convert natural language requirements into structured workflow artifacts.

The dataset will act as the benchmark for evaluating the quality of AI-generated workflows.

This is important because the project is not only about generating workflows, but also about proving that the generated workflows are accurate, complete, traceable, and useful for downstream artifact generation.

---

## Background

In the earlier phases of the project, we defined how enterprise requirements can be extracted and represented.

We also defined:

* requirement extraction strategy
* workflow representation
* workflow schema
* workflow validation framework
* validation checklist
* workflow quality metrics
* validation failure categories

Day 18 builds on that foundation by defining the dataset that will be used to evaluate the system.

The evaluation dataset will contain sample API requirements and their expected workflow outputs.

This allows the project to compare:

```text
Source Requirement
        vs
AI-Generated Workflow
        vs
Ground Truth Workflow
```

This comparison will help measure whether the AI-generated workflow is correct.

---

## What the Evaluation Dataset Is

The evaluation dataset is a collection of curated enterprise API requirement examples.

Each dataset item represents one API requirement scenario.

A dataset item should include:

* source requirement document
* requirement metadata
* endpoint information
* request header details
* request body details
* validation rules
* business rules
* downstream service details
* database interaction details
* response mapping details
* error handling expectations
* expected ground truth workflow
* review notes
* ambiguity markers, if applicable

The dataset should be designed so that each requirement can be used to test a specific capability of the AI system.

---

## Why the Evaluation Dataset Is Needed

The evaluation dataset is needed because the project requires measurable validation.

Without a dataset, we cannot fairly answer questions such as:

* Did the AI capture all required fields?
* Did the AI preserve business logic?
* Did the AI generate the correct workflow sequence?
* Did the AI avoid hallucinated logic?
* Did the AI correctly map downstream services?
* Did the AI correctly represent database interactions?
* Did the AI correctly map the final response?
* Did the AI handle failure scenarios?
* Is the workflow ready for Excel, Node-RED, Go, test, and documentation generation?

The dataset provides the controlled input needed to measure these outcomes.

---

## Evaluation Dataset Goals

The evaluation dataset has the following goals:

1. Provide realistic enterprise API requirement examples.
2. Cover different levels of workflow complexity.
3. Include both simple and complex API scenarios.
4. Support measurement of workflow generation quality.
5. Support comparison between AI-generated workflows and ground truth workflows.
6. Help identify common workflow generation failures.
7. Support the research paper with measurable evaluation results.
8. Help improve prompts, schemas, and validation methods over time.

---

## Dataset Item Structure

Each dataset item should follow a consistent structure.

Recommended structure:

```text
dataset_item_id:
api_name:
requirement_source:
requirement_type:
complexity_level:
source_requirement_text:
expected_endpoint:
expected_headers:
expected_request_body:
expected_validations:
expected_business_rules:
expected_downstream_services:
expected_database_interactions:
expected_response_mapping:
expected_error_handling:
ground_truth_workflow:
ambiguity_notes:
review_status:
```

---

## Dataset Item Field Definitions

### dataset_item_id

A unique identifier for the dataset item.

Example:

```text
REQ-WF-001
```

---

### api_name

The name of the API represented by the requirement.

Example:

```text
Create Application Message API
```

---

### requirement_source

The source from which the requirement was derived.

Examples:

* Confluence page
* API design document
* sample request/response
* Excel intake file
* business rule document
* manually curated sample

---

### requirement_type

The type of API requirement represented by the dataset item.

Examples:

* validation-only API
* downstream REST wrapper API
* database lookup API
* business rule API
* response transformation API
* mixed orchestration API

---

### complexity_level

The complexity level of the requirement.

Recommended values:

```text
LOW
MEDIUM
HIGH
```

---

### source_requirement_text

The full source requirement text used as input to the AI system.

This should contain the natural language requirement exactly as the AI would receive it.

---

### expected_endpoint

The expected endpoint information.

This should include:

* HTTP method
* endpoint path
* operation name
* API version
* request type
* response type

---

### expected_headers

The expected request headers.

This should include:

* required headers
* optional headers
* data types
* validation rules
* downstream propagation rules

---

### expected_request_body

The expected request body fields.

This should include:

* required fields
* optional fields
* nested objects
* field data types
* enum values
* format rules

---

### expected_validations

The expected validation rules.

This should include:

* required field validations
* enum validations
* data type validations
* conditional validations
* missing or blank value behavior

---

### expected_business_rules

The expected business rules extracted from the requirement.

This should include:

* eligibility checks
* conditional logic
* reject conditions
* calculation logic
* ordering rules
* special cases

---

### expected_downstream_services

The downstream services that should be called by the workflow.

This should include:

* service name
* service type
* endpoint path
* HTTP method
* request mapping
* response mapping
* error behavior

---

### expected_database_interactions

The database operations required by the workflow.

This should include:

* database type
* table name
* query purpose
* query type
* input parameters
* output fields
* no-record behavior
* database error behavior

---

### expected_response_mapping

The expected final API response mapping.

This should include:

* success response structure
* error response structure
* field names
* field data types
* transformation rules
* downstream-to-final response mapping

---

### expected_error_handling

The expected error handling behavior.

This should include:

* validation errors
* business rule failures
* downstream failures
* database failures
* no-record scenarios
* unexpected failures
* HTTP status mapping
* error response format

---

### ground_truth_workflow

The trusted correct workflow created manually or through reviewed expert input.

This workflow is used as the reference output for evaluation.

---

### ambiguity_notes

Any unclear or incomplete requirement areas should be recorded here.

Ambiguous areas should not be guessed.

They should be marked for review.

---

### review_status

The review status of the dataset item.

Recommended values:

```text
DRAFT
REVIEWED
APPROVED
NEEDS_UPDATE
```

Only approved dataset items should be used for final evaluation.

---

## Dataset Folder Structure

The recommended dataset folder structure is:

```text
datasets/
└── workflow_evaluation/
    ├── README.md
    ├── requirements/
    │   ├── REQ-WF-001.md
    │   ├── REQ-WF-002.md
    │   └── REQ-WF-003.md
    ├── ground_truth_workflows/
    │   ├── WF-GT-001.md
    │   ├── WF-GT-002.md
    │   └── WF-GT-003.md
    ├── evaluation_results/
    │   ├── evaluation_result_001.md
    │   └── evaluation_result_002.md
    └── review_notes/
        ├── review_REQ-WF-001.md
        └── review_REQ-WF-002.md
```

---

## Recommended Dataset Size

For the first research version, the dataset does not need to be very large.

A practical starting dataset can include:

| Dataset Size | Purpose                      |
| ------------ | ---------------------------- |
| 5 samples    | Initial prototype validation |
| 10 samples   | Small research evaluation    |
| 20 samples   | Stronger evaluation set      |
| 30+ samples  | Expanded benchmark dataset   |

For this 50-day challenge, the recommended target is:

```text
10 to 15 curated API requirement samples
```

This is realistic within the project timeline and large enough to show meaningful evaluation results.

---

## Dataset Complexity Levels

The dataset should include requirements with different complexity levels.

### Low Complexity

Low-complexity requirements are simple APIs with limited logic.

Examples:

* required header validation
* required body validation
* simple success response
* no downstream call
* no database interaction

Purpose:

These samples test basic extraction and workflow generation.

---

### Medium Complexity

Medium-complexity requirements include some orchestration.

Examples:

* downstream REST call
* database lookup
* enum validation
* response transformation
* basic error handling

Purpose:

These samples test whether the AI can handle real API flow structure.

---

### High Complexity

High-complexity requirements include multiple decision points.

Examples:

* multiple business rules
* branching workflow
* downstream service call
* database checks
* calculations
* fallback behavior
* complex response mapping
* multiple error scenarios

Purpose:

These samples test whether the AI can handle enterprise-level workflow generation.

---

## Recommended Dataset Distribution

The dataset should be balanced across different requirement types.

Example distribution for 10 samples:

| Requirement Type                        | Count |
| --------------------------------------- | ----: |
| Validation-only API                     |     2 |
| Downstream REST wrapper API             |     2 |
| Database lookup API                     |     2 |
| Business rule branching API             |     2 |
| Response transformation API             |     1 |
| Ambiguous or incomplete requirement API |     1 |

This helps test the AI system across different workflow generation challenges.

---

## Example Dataset Item

```text
dataset_item_id: REQ-WF-001
api_name: Create Application Message API
requirement_source: Curated enterprise API sample
requirement_type: Database insert with overlap validation
complexity_level: MEDIUM

source_requirement_text:
The API should create an application message. It must validate required headers including Client-Id, Subclient-Id, Division-Id, Store-Number, User-Id, and Trace-Id. The request body must include title, message, messageSeverity, startDateTime, and optional endDateTime. messageSeverity must be INFO or CRITICAL. Before inserting the message, the API must check whether an active message already exists for the same division with an overlapping time window. If overlap exists, reject the request. If no overlap exists, insert the message and return the generated id.

expected_endpoint:
POST /api/v1/clienteling/messages

expected_headers:
Client-Id required
Subclient-Id required
Division-Id required numeric
Store-Number required
User-Id required
Trace-Id required

expected_request_body:
title required string
message required string
messageSeverity required enum INFO, CRITICAL
startDateTime required timestamp
endDateTime optional timestamp

expected_business_rules:
BR-001: Reject overlapping active message for same division and time window.

expected_database_interactions:
DB-001: Check APP_MSGS for overlapping active messages.
DB-002: Insert new message into APP_MSGS.

expected_response_mapping:
Return id from inserted APP_MSGS record.

expected_error_handling:
Missing required headers return validation error.
Invalid messageSeverity returns validation error.
Overlap detected returns business validation error.
Database failure returns internal error.

review_status: DRAFT
```

---

## How the Dataset Supports Workflow Validation

The evaluation dataset supports workflow validation by providing a known expected result.

For each dataset item, the system can compare:

```text
AI-generated workflow
        vs
Ground truth workflow
```

The comparison can check:

* missing workflow steps
* incorrect workflow steps
* wrong step order
* missing validation logic
* missing business rules
* wrong downstream mapping
* wrong database mapping
* wrong response mapping
* unsupported assumptions
* missing traceability

This directly connects to the validation checklist and workflow quality metrics from Day 17.

---

## How the Dataset Supports Quality Metrics

Each dataset item should support scoring across the workflow quality metrics.

The dataset should allow calculation of:

* requirement coverage score
* validation accuracy score
* business rule coverage score
* step completeness score
* step ordering accuracy score
* downstream mapping accuracy score
* database mapping accuracy score
* response mapping accuracy score
* error handling completeness score
* traceability score
* hallucination control score
* artifact readiness score

This makes the evaluation repeatable.

---

## How the Dataset Supports Failure Analysis

The dataset also supports validation failure analysis.

If an AI-generated workflow fails, the issue can be classified using the Day 17 failure categories.

Examples:

| Failure                               | Category                       |
| ------------------------------------- | ------------------------------ |
| Missing required header validation    | Header Validation Failure      |
| Missing overlap check                 | Business Rule Coverage Failure |
| Wrong database table                  | Incorrect Database Mapping     |
| Added unsupported authorization logic | Hallucinated Logic             |
| Missing downstream error handling     | Error Handling Failure         |

This makes the evaluation more useful because it identifies the reason behind each failure.

---

## How the Dataset Supports the Research Paper

The evaluation dataset is essential for the research paper.

The paper needs evidence to support the claim that AI can help automate enterprise API development.

The dataset allows the project to report measurable results such as:

```text
Average workflow quality score
Average requirement coverage score
Average business rule coverage score
Average response mapping accuracy
Average hallucination control score
Number of critical validation failures
Improvement after validation
```

This turns the project from a demo into a measurable research system.

---

## Dataset Creation Principles

The dataset should follow these principles.

### 1. Realistic Requirements

Dataset samples should resemble real enterprise API requirements.

They should include practical details such as headers, validation rules, service calls, database checks, and response mappings.

---

### 2. Controlled Scope

Each dataset item should focus on a clear requirement scenario.

The goal is not to create huge documents, but to create useful benchmark cases.

---

### 3. Ground Truth First

Each dataset item must have a trusted expected workflow.

Without a ground truth workflow, the sample cannot be used for reliable evaluation.

---

### 4. Traceability

Each expected workflow step should be traceable to the source requirement.

This helps prevent unsupported assumptions.

---

### 5. Ambiguity Marking

If a requirement is unclear, the ambiguity should be documented instead of silently resolved.

This is important because enterprise requirements are often incomplete.

---

### 6. Version Control

Dataset items should be versioned and stored in Git.

This helps track improvements over time.

---

## Dataset Usage Flow

The evaluation dataset will be used as follows:

```text
Select dataset item
        ↓
Provide source requirement to AI system
        ↓
Generate workflow
        ↓
Compare generated workflow with ground truth workflow
        ↓
Apply validation checklist
        ↓
Calculate quality metrics
        ↓
Classify failures
        ↓
Record evaluation result
```

---

## What Should Not Be Included in the Dataset

The evaluation dataset should avoid:

* random examples with no enterprise relevance
* vague requirements with no review notes
* samples without ground truth workflows
* unsupported assumptions
* duplicate scenarios that do not test new behavior
* code implementation files before Day 25
* confidential or sensitive company data

If real enterprise patterns are used, they should be converted into sanitized examples.

---

## Dataset Safety and Sanitization

Because enterprise API requirements may contain internal details, the dataset should avoid exposing confidential information.

Dataset samples should be sanitized before being committed to GitHub.

Sanitization includes:

* replacing real service names with generic names when needed
* removing internal URLs
* removing secrets or API keys
* removing employee or customer data
* replacing production table names if sensitive
* removing proprietary business values if required

The dataset should preserve the structure and logic of the requirement while avoiding confidential details.

---

## Expected Outcome of Day 18 Phase 1

After this document, the project has a clear plan for designing the workflow evaluation dataset.

This dataset will later allow the project to evaluate whether the AI-generated workflows are correct enough to support downstream artifact generation.

---

## Summary

The evaluation dataset is the benchmark foundation for the AI Software Engineer system.

It defines the source requirements and expected workflow outputs needed to evaluate the quality of generated workflows.

The dataset supports:

* workflow validation
* quality scoring
* failure classification
* research evaluation
* prompt improvement
* artifact readiness assessment

Day 18 Phase 1 establishes how the project will organize and use sample enterprise API requirements to measure the reliability of AI-generated workflow artifacts.
