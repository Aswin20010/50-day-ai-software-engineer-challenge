# Day 8 — Workflow Validation Rules

## Purpose

This document defines validation rules for workflow schemas in the 50-Day AI Software Engineer Challenge.

The goal of workflow validation is to ensure that a workflow representation is complete, consistent, traceable, and ready for future artifact generation.

No implementation or coding is performed on Day 8.

---

## Why Workflow Validation Rules Are Needed

A workflow schema may be generated from extracted requirements, manually edited, or refined through review.

Without validation rules, the workflow may contain problems such as:

* Missing required fields
* Invalid node types
* Duplicate step identifiers
* Incorrect execution order
* Missing requirement references
* Missing error behavior
* Incomplete input or output definitions
* Ambiguous steps not marked for review
* Missing endpoint or input contract details
* Workflow steps that cannot be mapped to Node-RED, Excel, or Go generation later

Validation rules help detect these issues early before implementation begins.

---

## Validation Scope

Workflow validation should apply at three levels:

1. Top-level workflow schema
2. Individual workflow steps
3. End-to-end workflow consistency

---

## Top-Level Workflow Validation Rules

### Rule WF-001 — Workflow ID Must Exist

The workflow must contain a non-empty `workflow_id`.

Example valid value:

```text
WF-001
```

Failure behavior:

```text
Mark workflow as invalid and require review.
```

---

### Rule WF-002 — Workflow Name Must Exist

The workflow must contain a non-empty `workflow_name`.

Failure behavior:

```text
Mark workflow as invalid and require review.
```

---

### Rule WF-003 — API Endpoint Must Be Defined

The workflow must include an `api_endpoint` object.

The `api_endpoint` object must include:

* `endpoint_id`
* `method`
* `path`
* `operation_name`

Failure behavior:

```text
Mark workflow as invalid because artifact generation cannot start without an endpoint.
```

---

### Rule WF-004 — Input Contract Must Be Defined

The workflow must include an `input_contract` object.

At minimum, it should define:

* `required_headers`
* `optional_headers`
* `path_parameters`
* `query_parameters`
* `body_fields`

Failure behavior:

```text
Mark workflow as incomplete and require review.
```

---

### Rule WF-005 — Workflow Steps Must Exist

The workflow must contain a non-empty `workflow_steps` array.

Failure behavior:

```text
Mark workflow as invalid because there is no execution flow.
```

---

### Rule WF-006 — Error Handling Must Be Defined

The workflow must include an `error_handling` section.

The section should define standard error behavior for:

* Validation errors
* Business rule failures
* Database errors
* Downstream service errors
* Internal mapping errors

Failure behavior:

```text
Mark workflow as incomplete if major failure categories are missing.
```

---

### Rule WF-007 — Traceability Must Be Defined

The workflow must include a `traceability` object that links workflow elements back to source requirements.

Failure behavior:

```text
Mark workflow as incomplete when requirement source references are missing.
```

---

### Rule WF-008 — Review Status Must Be Defined

The workflow must include a `review_status` object.

Valid statuses include:

```text
DRAFT
READY_FOR_REVIEW
APPROVED
NEEDS_CLARIFICATION
GENERATION_READY
```

Failure behavior:

```text
Mark workflow as draft until review status is added.
```

---

### Rule WF-009 — Generation Targets Must Be Defined

The workflow must include a `generation_targets` array.

Valid targets include:

```text
NODE_RED_FLOW
EXCEL_INTAKE
GO_SERVICE
DOCUMENTATION
TEST_CASES
```

Failure behavior:

```text
Mark workflow as not generation-ready.
```

---

## Workflow Step Validation Rules

### Rule STEP-001 — Step ID Must Be Unique

Each workflow step must have a unique `step_id`.

Failure behavior:

```text
Reject duplicate step IDs and require correction.
```

---

### Rule STEP-002 — Step Name Must Exist

Each workflow step must contain a non-empty `step_name`.

Failure behavior:

```text
Mark step as invalid.
```

---

### Rule STEP-003 — Node Type Must Be Valid

Each step must use an approved `node_type`.

Allowed node types:

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

Failure behavior:

```text
Mark step as invalid and require node type correction.
```

---

### Rule STEP-004 — Description Must Be Specific

Each step must include a meaningful `description`.

Weak descriptions such as the following should be avoided:

```text
Process request.
Do validation.
Call service.
Handle data.
```

Failure behavior:

```text
Mark step for review when description is too generic.
```

---

### Rule STEP-005 — Execution Order Must Be Present

Each step must include an integer `execution_order`.

Failure behavior:

```text
Mark step as invalid.
```

---

### Rule STEP-006 — Execution Order Must Be Sequential

Execution order should start at `1` and increase logically.

Example:

```text
1, 2, 3, 4, 5
```

Failure behavior:

```text
Mark workflow for review when execution order has duplicates, gaps, or conflicts.
```

---

### Rule STEP-007 — Input Source Must Be Defined

Each step must define where it receives data from using `input_source`.

Failure behavior:

```text
Mark step as incomplete.
```

---

### Rule STEP-008 — Output Target Must Be Defined

Each step must define where its output goes using `output_target`.

Failure behavior:

```text
Mark step as incomplete.
```

---

### Rule STEP-009 — Requirement Reference Must Exist

Each step must include a `requirement_reference`.

Failure behavior:

```text
Mark workflow as not traceable.
```

---

### Rule STEP-010 — Error Behavior Must Be Defined

Each step must define an `error_behavior`.

Failure behavior:

```text
Mark step as incomplete because failure behavior is unknown.
```

---

### Rule STEP-011 — Mandatory Flag Must Be Defined

Each step must include a boolean `mandatory` field.

Failure behavior:

```text
Mark step as incomplete.
```

---

## End-to-End Workflow Validation Rules

### Rule FLOW-001 — Workflow Must Start with HTTP_INPUT

Every API workflow should begin with an `HTTP_INPUT` node.

Failure behavior:

```text
Mark workflow as invalid for API generation.
```

---

### Rule FLOW-002 — Workflow Must End with HTTP_RESPONSE

Every API workflow should end with an `HTTP_RESPONSE` node or a clearly defined error response path.

Failure behavior:

```text
Mark workflow as incomplete.
```

---

### Rule FLOW-003 — Header Requirements Must Map to HEADER_VALIDATION

If the `input_contract.required_headers` array is not empty, the workflow should contain at least one `HEADER_VALIDATION` step.

Failure behavior:

```text
Mark workflow as incomplete.
```

---

### Rule FLOW-004 — Body Requirements Must Map to BODY_VALIDATION

If required body fields exist, the workflow should contain at least one `BODY_VALIDATION` step.

Failure behavior:

```text
Mark workflow as incomplete.
```

---

### Rule FLOW-005 — Enum Fields Must Map to ENUM_VALIDATION

If any input field contains `allowed_values`, the workflow should contain an `ENUM_VALIDATION` step.

Failure behavior:

```text
Mark workflow as incomplete.
```

---

### Rule FLOW-006 — Database Reads Must Map to DATABASE_QUERY

If the traceability object includes query IDs for read operations, the workflow should contain a `DATABASE_QUERY` step.

Failure behavior:

```text
Mark workflow as incomplete.
```

---

### Rule FLOW-007 — Database Writes Must Map to DATABASE_COMMAND

If the traceability object includes insert, update, delete, or save operations, the workflow should contain a `DATABASE_COMMAND` step.

Failure behavior:

```text
Mark workflow as incomplete.
```

---

### Rule FLOW-008 — External Services Must Map to EXTERNAL_SERVICE_CALL

If the traceability object includes service IDs, the workflow should contain an `EXTERNAL_SERVICE_CALL` step.

Failure behavior:

```text
Mark workflow as incomplete.
```

---

### Rule FLOW-009 — Response Fields Must Map to RESPONSE_MAPPING

If success response fields are defined, the workflow should contain a `RESPONSE_MAPPING` step.

Failure behavior:

```text
Mark workflow as incomplete.
```

---

### Rule FLOW-010 — Business Rules Must Map to BUSINESS_RULE

If business rule IDs exist in traceability, the workflow should contain at least one `BUSINESS_RULE` step.

Failure behavior:

```text
Mark workflow as incomplete.
```

---

## Ambiguity and Review Rules

### Rule REVIEW-001 — Ambiguous Steps Must Be Marked

If a workflow step is based on incomplete requirement information, it must include:

```json
{
  "review_required": true,
  "ambiguity_reason": "Explanation of missing or unclear information"
}
```

Failure behavior:

```text
Mark workflow as unsafe for generation.
```

---

### Rule REVIEW-002 — Missing Information Must Be Captured

If required information is missing, it should be listed in `review_status.missing_information`.

Examples:

```text
Missing downstream URL
Missing request field mapping
Missing database table name
Missing error response format
Missing enum values
```

Failure behavior:

```text
Set review status to NEEDS_CLARIFICATION.
```

---

### Rule REVIEW-003 — Generation Ready Requires No Blocking Ambiguities

A workflow cannot be marked as `GENERATION_READY` if it has unresolved ambiguities or missing required information.

Failure behavior:

```text
Keep workflow in READY_FOR_REVIEW or NEEDS_CLARIFICATION status.
```

---

## Generation Readiness Rules

### Rule GEN-001 — Node-RED Readiness

A workflow is ready for Node-RED generation when:

* Each step has a valid `node_type`
* Execution order is clear
* Inputs and outputs are defined
* Error paths are defined
* Review blockers are resolved

---

### Rule GEN-002 — Excel Intake Readiness

A workflow is ready for Excel intake generation when:

* Endpoint metadata is complete
* Input contract is complete
* Business rules are traceable
* Query IDs are defined
* Service IDs are defined where needed
* Response mappings are defined
* Step ordering is complete

---

### Rule GEN-003 — Go Service Readiness

A workflow is ready for Go service generation when:

* Handler-level inputs are defined
* DTO validation rules are defined
* Service orchestration steps are defined
* Repository operations are traceable
* Client operations are traceable
* Mapper behavior is defined
* Error handling behavior is defined

---

## Validation Checklist

Before accepting a workflow schema, confirm:

* `workflow_id` exists.
* `workflow_name` exists.
* `api_endpoint` is complete.
* `input_contract` is complete.
* `workflow_steps` is not empty.
* Every step has a unique `step_id`.
* Every step has a valid `node_type`.
* Execution order is logical.
* The workflow starts with `HTTP_INPUT`.
* The workflow ends with `HTTP_RESPONSE`.
* Required headers map to header validation.
* Required body fields map to body validation.
* Enum fields map to enum validation.
* Business rules map to business rule steps.
* Database reads map to query steps.
* Database writes map to command steps.
* External service calls map to service call steps.
* Response mapping is present.
* Error handling is defined.
* Traceability references are present.
* Ambiguities are marked.
* Generation readiness is not marked true unless blockers are resolved.

---

## What This Document Does Not Include

This document does not include:

* Automated validation script implementation
* JSON Schema implementation
* Node-RED generation
* Excel generation
* Go code generation
* Runtime testing

Those implementation tasks will be handled later in the challenge.

---

## Summary

Workflow validation rules ensure that a workflow schema is complete, consistent, traceable, and safe for future generation.

These rules help catch design problems before generating Node-RED flows, Excel intake files, Go services, documentation, or tests.

This validation layer improves the reliability and research value of the AI Software Engineer pipeline.
