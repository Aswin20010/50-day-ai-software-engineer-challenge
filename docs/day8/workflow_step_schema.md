# Day 8 — Workflow Step Schema

## Purpose

This document defines the schema for individual workflow steps in the 50-Day AI Software Engineer Challenge.

A workflow step represents one logical action inside an API workflow.

This schema ensures that every step has enough structure to support future Node-RED flow generation, Excel intake generation, Go code generation, documentation generation, and testing.

No implementation or coding is performed on Day 8.

---

## Relationship to Workflow Schema

The workflow schema defined in Phase 1 contains a `workflow_steps` array.

Each item inside that array must follow the workflow step schema defined in this document.

```text
Workflow Schema
        ↓
workflow_steps[]
        ↓
Individual Workflow Step Schema
```

---

## Workflow Step Definition

A workflow step is a structured object that describes one unit of execution.

A step may represent:

* Receiving an API request
* Validating headers
* Validating body fields
* Applying enum checks
* Building request payloads
* Applying business rules
* Querying a database
* Writing to a database
* Calling an external service
* Mapping responses
* Handling errors
* Returning the final response

---

## Required Step Fields

Each workflow step should include the following required fields:

| Field                   | Type    | Required | Description                                                    |
| ----------------------- | ------- | -------: | -------------------------------------------------------------- |
| `step_id`               | string  |      Yes | Unique identifier for the workflow step                        |
| `step_name`             | string  |      Yes | Human-readable name of the step                                |
| `node_type`             | string  |      Yes | Type of workflow node                                          |
| `description`           | string  |      Yes | Explanation of what the step does                              |
| `execution_order`       | integer |      Yes | Position of the step in the workflow                           |
| `input_source`          | string  |      Yes | Source of input data for the step                              |
| `output_target`         | string  |      Yes | Destination of output data from the step                       |
| `requirement_reference` | string  |      Yes | Source requirement, business rule, query, or mapping reference |
| `error_behavior`        | string  |      Yes | Expected behavior when the step fails                          |
| `mandatory`             | boolean |      Yes | Indicates whether the step is required                         |

---

## Optional Step Fields

Optional fields provide additional detail for generation, review, and traceability.

| Field               | Type    | Description                                                |
| ------------------- | ------- | ---------------------------------------------------------- |
| `depends_on`        | array   | List of prior step IDs that must complete before this step |
| `produces`          | array   | Named outputs produced by this step                        |
| `consumes`          | array   | Named inputs consumed by this step                         |
| `condition`         | string  | Conditional rule that determines whether this step runs    |
| `success_next_step` | string  | Step ID to execute when this step succeeds                 |
| `failure_next_step` | string  | Step ID or error handler to execute when this step fails   |
| `related_query`     | string  | Query ID associated with this step                         |
| `related_service`   | string  | External service ID associated with this step              |
| `related_entity`    | string  | Entity or table associated with this step                  |
| `review_required`   | boolean | Indicates whether the step requires human review           |
| `ambiguity_reason`  | string  | Explains why review is required                            |
| `generation_notes`  | string  | Notes for future artifact generation                       |
| `test_expectations` | array   | Expected test scenarios for this step                      |

---

## Allowed Node Types

The `node_type` field should use one of the approved workflow node types from Day 7.

Allowed values:

```text
HTTP_INPUT
HEADER_VALIDATION
BODY_VALIDATION
ENUM_VALIDATION
REQUEST_TRANSFORMATION
BUSINESS_RULE
DATABASE_QUERY
DATABASE_COMMAND
EXTERNAL_SERVICE_CALL
RESPONSE_MAPPING
ERROR_HANDLING
HTTP_RESPONSE
```

---

## Example Basic Workflow Step

```json
{
  "step_id": "STEP-002",
  "step_name": "Validate Required Headers",
  "node_type": "HEADER_VALIDATION",
  "description": "Validates that all mandatory request headers are present before processing the request.",
  "execution_order": 2,
  "input_source": "HTTP request headers",
  "output_target": "Validated header context",
  "requirement_reference": "HDR-001",
  "error_behavior": "Return HTTP 400 when mandatory headers are missing",
  "mandatory": true
}
```

---

## Example Detailed Workflow Step

```json
{
  "step_id": "STEP-007",
  "step_name": "Check Existing Active Messages",
  "node_type": "DATABASE_QUERY",
  "description": "Queries the database to find active messages for the same division and overlapping time window.",
  "execution_order": 7,
  "input_source": "Active message lookup payload",
  "output_target": "Existing active message count",
  "requirement_reference": "SQL-APP-002",
  "error_behavior": "Return HTTP 500 when active message query fails",
  "mandatory": true,
  "depends_on": ["STEP-006"],
  "consumes": ["divisionId", "startDateTime", "endDateTime"],
  "produces": ["activeMessageCount"],
  "related_query": "SQL-APP-002",
  "related_entity": "APP_MSGS",
  "success_next_step": "STEP-008",
  "failure_next_step": "ERR-003",
  "review_required": false,
  "generation_notes": "Generate repository query or Node-RED database node for active message overlap check.",
  "test_expectations": [
    "Returns zero when no overlapping message exists",
    "Returns count greater than zero when overlapping active message exists",
    "Returns database error when query execution fails"
  ]
}
```

---

## Field Descriptions

### step_id

The `step_id` uniquely identifies the step within a workflow.

Recommended format:

```text
STEP-001
STEP-002
STEP-003
```

Error-specific steps may use:

```text
ERR-001
ERR-002
```

---

### step_name

The `step_name` should be short and human-readable.

Examples:

```text
Validate Required Headers
Build Downstream Request
Call Account Interrogation Service
Map Final Response
```

---

### node_type

The `node_type` classifies the behavior of the step.

Examples:

```text
HEADER_VALIDATION
DATABASE_QUERY
EXTERNAL_SERVICE_CALL
RESPONSE_MAPPING
```

The node type is important because it helps future generators decide how to convert the step into Node-RED nodes, Excel rows, or Go layers.

---

### description

The `description` explains what the step does in business and technical terms.

It should be specific enough for human review.

Good example:

```text
Validates that Division-Id is numeric and X-Acting-As-Type equals global.
```

Weak example:

```text
Validates request.
```

---

### execution_order

The `execution_order` defines where the step appears in the workflow.

Rules:

* Execution order should start at `1`.
* Execution order should increase sequentially.
* No two steps should share the same execution order unless parallel execution is explicitly supported.
* The execution order should match the logical API processing flow.

---

### input_source

The `input_source` describes where the step gets its data.

Examples:

```text
HTTP request headers
Validated request body
Downstream response
Database query result
Business rule context
```

---

### output_target

The `output_target` describes where the step output is stored or sent.

Examples:

```text
Validated header context
PIIDLookupRequest
Discount calculation context
Final API response body
HTTP client
```

---

### requirement_reference

The `requirement_reference` links the step back to its source requirement.

Examples:

```text
EP-001
HDR-001
BODY-001
BR-001
SQL-APP-002
MAP-001
RESP-001
ERR-001
```

This field is required for traceability.

---

### error_behavior

The `error_behavior` defines what should happen when the step fails.

Examples:

```text
Return HTTP 400 when required headers are missing
Return HTTP 409 when overlapping active message exists
Return mapped downstream error when service call fails
Return HTTP 500 when database query fails
```

---

### mandatory

The `mandatory` field indicates whether the step is required.

Most workflow steps should be mandatory.

Optional steps may be used for conditional logic, optional downstream calls, or optional enrichments.

---

## Dependency Fields

The optional `depends_on`, `success_next_step`, and `failure_next_step` fields help express control flow.

Example:

```json
{
  "depends_on": ["STEP-006"],
  "success_next_step": "STEP-008",
  "failure_next_step": "ERR-003"
}
```

These fields are useful for:

* Branching workflows
* Error paths
* Conditional execution
* Later Node-RED wiring
* Later Go orchestration logic

---

## Data Flow Fields

The optional `consumes` and `produces` fields help describe data movement.

Example:

```json
{
  "consumes": ["divisionId", "startDateTime", "endDateTime"],
  "produces": ["activeMessageCount"]
}
```

These fields are useful for:

* Validating missing mappings
* Generating Excel endpoint-service mappings
* Building mapper logic
* Designing test cases

---

## Review and Ambiguity Fields

Some steps may not have enough requirement detail to generate artifacts safely.

In those cases, the step should be marked for review.

Example:

```json
{
  "review_required": true,
  "ambiguity_reason": "Requirement says to call downstream service but does not define request field mapping."
}
```

Review markers help avoid generating incorrect flows or code from incomplete requirements.

---

## Generation Support Fields

The `generation_notes` field can describe how this step may map into future artifacts.

Example:

```text
Generate a Node-RED function node for this validation and a Go validation helper in the validation package.
```

This helps preserve design intent before implementation begins.

---

## Test Expectation Fields

The `test_expectations` field can list future test scenarios.

Example:

```json
[
  "Returns HTTP 400 when Division-Id is missing",
  "Returns HTTP 400 when Division-Id is non-numeric",
  "Continues processing when Division-Id is valid"
]
```

These expectations can later support test generation.

---

## Step Schema Quality Checklist

Before accepting a workflow step, verify that:

* The step has a unique `step_id`.
* The step has a clear `step_name`.
* The `node_type` is one of the approved values.
* The `description` is specific.
* The `execution_order` is present and logical.
* The `input_source` and `output_target` are defined.
* The `requirement_reference` is present.
* The `error_behavior` is clear.
* The `mandatory` field is defined.
* Dependencies are listed when ordering is not enough.
* Ambiguous steps are marked with `review_required`.
* Important data inputs and outputs are captured through `consumes` and `produces`.

---

## What This Document Does Not Include

This document does not include:

* Code implementation
* Runtime execution
* Node-RED JSON generation
* Go service generation
* Excel workbook creation
* Automated validation scripts

Those implementation tasks will be handled later in the challenge.

---

## Summary

The workflow step schema defines the structure of each execution step inside an API workflow.

It ensures that each step is traceable, reviewable, ordered, and ready to support future artifact generation.

This schema will help the project move from extracted requirements to consistent workflow models before implementation begins.
