# Day 8 — Workflow Schema Specification

## Purpose

This document defines the workflow schema for the 50-Day AI Software Engineer Challenge.

The workflow schema describes the structure used to store an API workflow after requirements have been extracted and mapped into workflow steps.

This schema acts as a formal contract between the requirement extraction phase and future artifact generation phases.

No implementation or coding is performed on Day 8.

---

## Background

On Day 7, the project defined the workflow representation layer.

That layer described how extracted requirements can be converted into a structured sequence of workflow steps.

Day 8 extends that work by defining the schema for storing those workflow steps in a consistent format.

A schema is needed so that the same workflow representation can later be consumed by:

* Node-RED flow generators
* Excel intake generators
* Go code generators
* Documentation generators
* Test case generators
* Evaluation scripts

---

## Why a Workflow Schema Is Needed

A workflow representation without a schema may become inconsistent.

Different APIs may use different field names, missing metadata, or unclear step definitions.

A workflow schema solves this problem by defining:

* Required workflow-level fields
* Required step-level fields
* Optional metadata fields
* Allowed node types
* Execution order rules
* Traceability fields
* Error handling fields
* Review and ambiguity markers

The schema ensures that every generated workflow can be reviewed, validated, and consumed by later tools.

---

## Position in the Overall AI Pipeline

The workflow schema sits after requirement extraction and requirement-to-workflow mapping.

```text
Requirement Document
        ↓
Requirement Extraction Agent
        ↓
Structured Requirement Schema
        ↓
Requirement-to-Workflow Mapping
        ↓
Workflow Schema
        ↓
Node-RED Flow / Excel Intake / Go Code Generation
```

---

## Workflow Schema Definition

A workflow schema is a structured object that describes one API workflow.

Each workflow should include:

1. Workflow metadata
2. API endpoint metadata
3. Input contract information
4. Ordered workflow steps
5. Error handling information
6. Traceability metadata
7. Review status
8. Generation readiness indicators

---

## Top-Level Workflow Schema Fields

| Field                | Required | Description                                                 |
| -------------------- | -------: | ----------------------------------------------------------- |
| `workflow_id`        |      Yes | Unique identifier for the workflow                          |
| `workflow_name`      |      Yes | Human-readable workflow name                                |
| `description`        |      Yes | Summary of what the workflow does                           |
| `api_endpoint`       |      Yes | API method and path metadata                                |
| `input_contract`     |      Yes | Headers, query parameters, path parameters, and body fields |
| `workflow_steps`     |      Yes | Ordered list of workflow execution steps                    |
| `error_handling`     |      Yes | Standardized error handling definitions                     |
| `traceability`       |      Yes | Links back to source requirements                           |
| `review_status`      |      Yes | Indicates whether the workflow is ready for generation      |
| `generation_targets` |      Yes | Lists future artifacts supported by the workflow            |
| `metadata`           |       No | Additional versioning or ownership information              |

---

## Example Top-Level Structure

```json
{
  "workflow_id": "WF-001",
  "workflow_name": "Create Application Message Workflow",
  "description": "Workflow for creating application messages after validation and overlap checks.",
  "api_endpoint": {},
  "input_contract": {},
  "workflow_steps": [],
  "error_handling": [],
  "traceability": {},
  "review_status": {},
  "generation_targets": [],
  "metadata": {}
}
```

---

## API Endpoint Schema

The `api_endpoint` object defines the API entry point.

| Field            | Required | Description                |
| ---------------- | -------: | -------------------------- |
| `endpoint_id`    |      Yes | Unique endpoint identifier |
| `method`         |      Yes | HTTP method                |
| `path`           |      Yes | API route path             |
| `operation_name` |      Yes | Logical operation name     |
| `description`    |       No | Endpoint description       |

Example:

```json
{
  "endpoint_id": "EP-001",
  "method": "POST",
  "path": "/api/v1/clienteling/messages",
  "operation_name": "createApplicationMessage",
  "description": "Creates a new application message."
}
```

---

## Input Contract Schema

The `input_contract` object defines the expected API inputs.

It can include:

* Required headers
* Optional headers
* Path parameters
* Query parameters
* Request body fields

Example:

```json
{
  "required_headers": [
    "Client-Id",
    "Subclient-Id",
    "Division-Id",
    "Store-Number",
    "Trace-Id"
  ],
  "optional_headers": [],
  "path_parameters": [],
  "query_parameters": [],
  "body_fields": [
    {
      "name": "title",
      "required": true,
      "type": "string"
    },
    {
      "name": "messageSeverity",
      "required": true,
      "type": "string",
      "allowed_values": ["INFO", "CRITICAL"]
    }
  ]
}
```

---

## Workflow Steps Schema

The `workflow_steps` array contains the ordered execution steps.

Each step should represent one logical action.

Each step should include:

* Step identifier
* Step name
* Node type
* Execution order
* Input source
* Output target
* Requirement reference
* Error behavior
* Mandatory flag

Detailed step-level schema is defined separately in:

```text
docs/day8/workflow_step_schema.md
```

---

## Error Handling Schema

The `error_handling` array defines reusable error responses or failure behaviors.

Example:

```json
[
  {
    "error_id": "ERR-001",
    "error_type": "VALIDATION_ERROR",
    "http_status": 400,
    "description": "Returned when required headers or body fields are missing.",
    "response_shape": {
      "code": "VALIDATION_ERROR",
      "message": "Invalid request"
    }
  }
]
```

---

## Traceability Schema

The `traceability` object links workflow elements back to source requirements.

Example:

```json
{
  "source_document": "clienteling_message_requirements.md",
  "requirement_ids": ["EP-001", "HDR-001", "BODY-001", "BR-001"],
  "business_rule_ids": ["BR-001"],
  "query_ids": ["SQL-APP-001", "SQL-APP-002"],
  "service_ids": []
}
```

---

## Review Status Schema

The `review_status` object indicates whether the workflow is ready for later generation.

Example:

```json
{
  "status": "READY_FOR_REVIEW",
  "review_required": true,
  "ambiguities": [],
  "missing_information": [],
  "generation_ready": false
}
```

Common statuses:

| Status                | Meaning                                                    |
| --------------------- | ---------------------------------------------------------- |
| `DRAFT`               | Initial workflow schema created                            |
| `READY_FOR_REVIEW`    | Workflow is complete enough for human review               |
| `APPROVED`            | Workflow has been reviewed and accepted                    |
| `NEEDS_CLARIFICATION` | Missing or ambiguous requirement details exist             |
| `GENERATION_READY`    | Workflow can be used for Node-RED, Excel, or Go generation |

---

## Generation Targets

The `generation_targets` field lists which future artifacts this workflow can support.

Example:

```json
[
  "NODE_RED_FLOW",
  "EXCEL_INTAKE",
  "GO_SERVICE",
  "DOCUMENTATION",
  "TEST_CASES"
]
```

---

## Metadata Schema

The optional `metadata` object provides project-level information.

Example:

```json
{
  "created_on": "2026-06-10",
  "challenge_day": 8,
  "project": "50-Day AI Software Engineer Challenge",
  "version": "0.1",
  "author": "Aswin Tony Kanikairaj"
}
```

---

## Design Principles

The workflow schema should follow these principles.

### 1. Consistent

Every API workflow should follow the same structure.

### 2. Ordered

Workflow steps should have clear execution order.

### 3. Traceable

Each step should link back to requirement references.

### 4. Reviewable

The schema should be readable by humans before generation.

### 5. Technology-Neutral

The schema should not be tied only to Node-RED or Go.

### 6. Extensible

The schema should allow future fields for testing, evaluation, deployment, and observability.

---

## Relationship to Day 7

Day 7 defined what a workflow representation is.

Day 8 defines how that workflow representation should be stored.

| Day   | Focus                                                |
| ----- | ---------------------------------------------------- |
| Day 7 | Conceptual workflow representation and node taxonomy |
| Day 8 | Formal workflow schema and validation structure      |

---

## What Day 8 Does Not Include

Day 8 does not include:

* Writing parser code
* Creating Node-RED JSON
* Creating Excel files
* Creating Go source code
* Running tests
* Calling an LLM
* Deploying any service

Those implementation activities will begin later in the challenge.

---

## Summary

The workflow schema provides a consistent structure for storing API workflow representations.

It creates a formal bridge between requirement extraction and future artifact generation.

This schema will help ensure that Node-RED flows, Excel intake files, Go services, documentation, and tests can be generated from a common intermediate representation.
