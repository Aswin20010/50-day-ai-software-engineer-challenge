# Day 17 — Workflow Validation Framework

## Purpose

This document defines the workflow validation framework for the 50-Day AI Software Engineer Challenge.

The purpose of workflow validation is to verify that a generated workflow correctly represents the original enterprise API requirement.

In this project, requirements may come from Confluence pages, business documents, sample requests, sample responses, API specifications, or manually prepared input documents. These requirements are later transformed into structured workflow steps that can be used by downstream systems such as:

- Node-RED flow generators
- Excel intake generators
- Go code generators
- documentation generators
- test case generators
- gap analysis tools

Workflow validation ensures that the generated workflow is complete, accurate, traceable, and safe to use as an intermediate artifact.

---

## What Workflow Validation Means

Workflow validation is the process of reviewing a generated workflow and checking whether it correctly captures the source requirement.

A workflow is considered valid when it satisfies the following conditions:

1. All required API behavior from the source requirement is represented.
2. All mandatory request headers are captured.
3. All required request body fields are captured.
4. All validation rules are represented as workflow steps.
5. All business rules are mapped into the correct execution order.
6. All downstream service calls are included when required.
7. All database interactions are captured when required.
8. All response mappings are represented.
9. All error scenarios are handled.
10. Each workflow step can be traced back to a source requirement.

The goal is not only to check whether a workflow exists, but whether the workflow is correct enough to support future artifact generation.

---

## Why Workflow Validation Is Needed

AI-generated workflows can look structurally correct while still being logically incomplete.

For example, a workflow may include an endpoint, request body, and response mapping, but miss an important business validation such as:

- checking whether a required header is present
- validating an enum value
- calling a downstream service before applying business logic
- checking a database table before calculating a response
- returning the correct error response
- applying rules in the correct order

Without validation, these mistakes may flow into later stages of the system and produce incorrect generated artifacts.

This is especially risky because the workflow representation acts as a bridge between requirement extraction and code generation.

If the workflow is wrong, every downstream artifact may also become wrong.

---

## Problems That Can Happen Without Validation

Without workflow validation, the system may generate artifacts with several types of errors.

### 1. Missing Requirement Coverage

The workflow may ignore part of the original requirement.

Example:

A requirement says the API must reject requests when `Client-Id` is missing, but the workflow does not include a header validation step.

---

### 2. Incorrect Step Ordering

The workflow may contain the right steps but place them in the wrong order.

Example:

A workflow performs discount calculation before checking whether the account is eligible.

---

### 3. Hallucinated Logic

The workflow may include logic that was not present in the original requirement.

Example:

The source requirement does not mention authorization, but the workflow adds an authorization validation step.

---

### 4. Missing Downstream Service Calls

The workflow may fail to include required external service calls.

Example:

A requirement says the API must call a PIID lookup service, but the workflow only validates the request and returns a response.

---

### 5. Incorrect Database Mapping

The workflow may refer to the wrong table, wrong column, or wrong query type.

Example:

The requirement says to check an active message overlap in `APP_MSGS`, but the workflow maps the check to an unrelated table.

---

### 6. Incorrect Response Mapping

The workflow may produce a response that does not match the source requirement.

Example:

The generated workflow maps downstream response fields directly without transforming them into the expected API response format.

---

### 7. Incomplete Error Handling

The workflow may handle only success scenarios and ignore failure scenarios.

Example:

The workflow handles a valid request but does not define what should happen when a downstream service fails.

---

## Position of Validation in the Full Pipeline

Workflow validation sits between requirement extraction and artifact generation.

The pipeline can be represented as:

```text
Source Requirement
        ↓
Requirement Extraction
        ↓
Structured Requirement Output
        ↓
Workflow Generation
        ↓
Workflow Validation
        ↓
Artifact Generation
        ↓
Generated Node-RED / Excel / Go / Tests / Documentation
```

Validation acts as a quality gate.

Only workflows that pass validation should move forward into artifact generation.

---

## Inputs to Workflow Validation

The validation framework compares two major inputs:

### 1. Source Requirement

This is the original business or technical requirement.

Examples:

- Confluence requirement page
- API specification
- sample request and response
- business rule document
- Excel input file
- manually written API requirement

### 2. Generated Workflow

This is the structured workflow produced after requirement extraction.

A generated workflow may contain:

- endpoint metadata
- headers
- request body fields
- validation steps
- business rule steps
- downstream service calls
- database query steps
- response mapping steps
- error handling steps
- traceability references

---

## Outputs of Workflow Validation

The output of validation should clearly show whether the workflow is acceptable.

A validation output may include:

- validation status
- passed checks
- failed checks
- missing workflow steps
- incorrect mappings
- hallucinated logic
- traceability gaps
- suggested corrections
- overall workflow quality score

Example validation result:

```json
{
  "workflow_id": "WF-001",
  "validation_status": "FAILED",
  "coverage_score": 78,
  "failed_checks": [
    "Missing required header validation for Store-Number",
    "Missing downstream service error handling",
    "Response mapping does not include error response structure"
  ],
  "recommendation": "Workflow should not proceed to artifact generation until missing validation and error handling steps are added."
}
```

---

## Validation Scope

The validation framework should inspect the following areas.

### 1. Endpoint Validation

Checks whether the workflow includes the correct endpoint information.

This includes:

- HTTP method
- endpoint path
- operation name
- request type
- response type

---

### 2. Header Validation

Checks whether all required and optional headers are correctly captured.

This includes:

- required headers
- optional headers
- header data type
- header validation rules
- downstream header propagation

---

### 3. Request Body Validation

Checks whether request body fields are correctly represented.

This includes:

- required fields
- optional fields
- nested fields
- field data types
- enum values
- format rules
- body-level validation rules

---

### 4. Business Rule Validation

Checks whether business rules from the source requirement are converted into workflow steps.

This includes:

- eligibility checks
- conditional rules
- calculation rules
- ordering rules
- branching logic
- reject conditions

---

### 5. Downstream Service Validation

Checks whether required downstream service calls are included.

This includes:

- service name
- service endpoint
- request mapping
- header mapping
- response mapping
- timeout behavior
- downstream error behavior

---

### 6. Database Validation

Checks whether database interactions are correctly represented.

This includes:

- table name
- query purpose
- input parameters
- output fields
- query type
- result handling
- no-record handling

---

### 7. Response Mapping Validation

Checks whether the final API response is correctly mapped.

This includes:

- success response structure
- error response structure
- field-level mapping
- transformation logic
- default value handling
- downstream-to-API response conversion

---

### 8. Error Handling Validation

Checks whether the workflow handles failure scenarios.

This includes:

- validation errors
- business rule failures
- downstream failures
- database failures
- unexpected failures
- proper HTTP status mapping
- proper error response format

---

### 9. Traceability Validation

Checks whether each workflow step can be traced back to the source requirement.

This includes:

- source requirement ID
- business rule ID
- endpoint reference
- sample request reference
- sample response reference
- mapping reference

Traceability is important because it prevents unsupported logic from entering the workflow.

---

## Validation Principles

The workflow validation framework follows these principles.

### 1. Requirement-First Validation

The source requirement is the source of truth.

The workflow should not introduce assumptions that are not present in the requirement.

---

### 2. No Hallucinated Logic

Any workflow step that cannot be traced back to a requirement should be flagged.

This includes invented:

- headers
- authorization rules
- database queries
- downstream calls
- response fields
- business logic

---

### 3. Complete Coverage

All important requirement elements should be represented in the workflow.

A workflow that captures only the happy path is not complete.

---

### 4. Correct Execution Order

Workflow steps must appear in a logical order.

Typical order:

```text
Receive Request
↓
Validate Headers
↓
Validate Request Body
↓
Apply Precondition Rules
↓
Call Downstream Services
↓
Perform Database Checks
↓
Apply Business Logic
↓
Map Response
↓
Return Success or Error
```

---

### 5. Artifact Readiness

A validated workflow should be good enough to support downstream artifact generation.

If the workflow cannot guide generation of Node-RED, Excel, tests, or Go code, it is not sufficiently valid.

---

## Validation Status Levels

Each generated workflow can be assigned one of the following validation statuses.

### PASS

The workflow fully matches the source requirement and is ready for downstream artifact generation.

### PASS WITH WARNINGS

The workflow is mostly correct but has minor gaps that should be reviewed.

### FAIL

The workflow has major missing or incorrect logic and should not move forward.

### NEEDS REVIEW

The workflow contains ambiguous areas that require human review before approval.

---

## Example Validation Decision

### Source Requirement

The requirement states:

```text
The API must validate Client-Id, Subclient-Id, Division-Id, and User-Id.
The API must reject the request if any required header is missing.
The API must call the customer profile service after validation.
The API must return the mapped customer profile response.
```

### Generated Workflow

```text
1. Receive request
2. Validate Client-Id
3. Validate Subclient-Id
4. Call customer profile service
5. Return customer profile response
```

### Validation Result

```text
Status: FAIL
Reason:
- Missing Division-Id validation
- Missing User-Id validation
- Missing error response handling for missing headers
```

This workflow should not proceed to artifact generation.

---

## Role of Human Review

The validation framework does not remove human review.

Instead, it makes review easier by identifying:

- what is covered
- what is missing
- what is incorrect
- what needs clarification
- what is unsupported by the source requirement

For this project, human review remains important because enterprise requirements can be incomplete, ambiguous, or inconsistent.

The validation framework helps make that review more structured and repeatable.

---

## How This Supports the Research Project

This validation framework directly supports the research goal of the 50-Day AI Software Engineer Challenge.

The project is not only about generating workflows from requirements. It is also about proving that the generated workflows are reliable.

This framework supports the paper by providing a way to evaluate:

- requirement coverage
- workflow correctness
- traceability
- hallucination reduction
- artifact readiness

It also supports the larger claim that AI can assist enterprise API development when there is a structured validation layer between requirement understanding and code generation.

---

## Summary

Workflow validation is a critical quality gate in the AI-driven requirement-to-artifact pipeline.

It ensures that generated workflows are:

- complete
- accurate
- traceable
- logically ordered
- free from unsupported assumptions
- ready for downstream artifact generation

Day 17 establishes the foundation for evaluating generated workflows before they are used to create Node-RED flows, Excel intake files, Go code, tests, or documentation.