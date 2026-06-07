# Day 7 — Sample Workflow Representation

## Purpose

This document provides a sample workflow representation for the 50-Day AI Software Engineer Challenge.

The goal is to show how extracted API requirements can be converted into a structured sequence of workflow steps before Node-RED flow generation, Excel intake generation, or Go code generation.

No implementation or coding is performed on Day 7.

---

## Sample API Scenario

This sample uses a simplified enterprise API scenario.

The API receives a request to create an application message.

The API must:

1. Receive an HTTP request.
2. Validate required headers.
3. Validate required request body fields.
4. Validate enum values.
5. Check whether an active message already exists for the same division and overlapping time window.
6. Insert the new message into the database.
7. Return the generated message ID.
8. Return a standardized error response when validation or processing fails.

---

## Sample Requirement Summary

```text
Endpoint:
POST /api/v1/clienteling/messages

Required Headers:
Client-Id
Subclient-Id
Division-Id
Store-Number
Trace-Id
Associateid
X-Acting-As-Id
X-Acting-As-Type

Required Body Fields:
title
message
messageSeverity
startDateTime

Optional Body Fields:
endDateTime
targetRoles

Validation Rules:
Division-Id must be numeric.
messageSeverity must be one of INFO or CRITICAL.
X-Acting-As-Type must be global.

Business Rule:
Reject the request if an active message already exists for the same division and overlapping time window.

Database Operations:
Check existing active messages.
Insert new application message.
Return generated message ID.

Success Response:
HTTP 201 with generated message ID.

Error Response:
Standardized error response with code, message, and details.
```

---

## Workflow Representation Overview

The workflow is represented as an ordered sequence of steps.

Each step contains:

* Step identifier
* Step name
* Node type
* Description
* Input source
* Output target
* Execution order
* Requirement reference
* Error behavior
* Mandatory flag

---

## Ordered Workflow Steps

```json
[
  {
    "step_id": "STEP-001",
    "step_name": "Receive Create Message Request",
    "node_type": "HTTP_INPUT",
    "description": "Receives POST request for /api/v1/clienteling/messages.",
    "input_source": "HTTP client",
    "output_target": "Raw request context",
    "execution_order": 1,
    "requirement_reference": "EP-001",
    "error_behavior": "Return HTTP 404 or 405 when route or method is unsupported",
    "mandatory": true
  },
  {
    "step_id": "STEP-002",
    "step_name": "Validate Required Headers",
    "node_type": "HEADER_VALIDATION",
    "description": "Validates required headers including Client-Id, Subclient-Id, Division-Id, Store-Number, Trace-Id, Associateid, X-Acting-As-Id, and X-Acting-As-Type.",
    "input_source": "HTTP request headers",
    "output_target": "Validated header context",
    "execution_order": 2,
    "requirement_reference": "HDR-001",
    "error_behavior": "Return HTTP 400 when required headers are missing",
    "mandatory": true
  },
  {
    "step_id": "STEP-003",
    "step_name": "Validate Header Formats",
    "node_type": "HEADER_VALIDATION",
    "description": "Validates that Division-Id is numeric and X-Acting-As-Type equals global.",
    "input_source": "Validated header context",
    "output_target": "Normalized header context",
    "execution_order": 3,
    "requirement_reference": "HDR-002",
    "error_behavior": "Return HTTP 400 when header values are invalid",
    "mandatory": true
  },
  {
    "step_id": "STEP-004",
    "step_name": "Validate Request Body",
    "node_type": "BODY_VALIDATION",
    "description": "Validates required request body fields including title, message, messageSeverity, and startDateTime.",
    "input_source": "HTTP request body",
    "output_target": "Validated request body",
    "execution_order": 4,
    "requirement_reference": "BODY-001",
    "error_behavior": "Return HTTP 400 when required body fields are missing",
    "mandatory": true
  },
  {
    "step_id": "STEP-005",
    "step_name": "Validate Message Severity Enum",
    "node_type": "ENUM_VALIDATION",
    "description": "Validates that messageSeverity contains one of the supported values: INFO or CRITICAL.",
    "input_source": "Validated request body",
    "output_target": "Validated request context",
    "execution_order": 5,
    "requirement_reference": "ENUM-001",
    "error_behavior": "Return HTTP 400 when messageSeverity contains an unsupported value",
    "mandatory": true
  },
  {
    "step_id": "STEP-006",
    "step_name": "Build Active Message Lookup Context",
    "node_type": "REQUEST_TRANSFORMATION",
    "description": "Builds the database lookup context using division, startDateTime, and endDateTime.",
    "input_source": "Validated request context",
    "output_target": "Active message lookup payload",
    "execution_order": 6,
    "requirement_reference": "MAP-001",
    "error_behavior": "Return HTTP 500 when lookup context cannot be built",
    "mandatory": true
  },
  {
    "step_id": "STEP-007",
    "step_name": "Check Existing Active Messages",
    "node_type": "DATABASE_QUERY",
    "description": "Queries the database to find active messages for the same division and overlapping time window.",
    "input_source": "Active message lookup payload",
    "output_target": "Existing active message count",
    "execution_order": 7,
    "requirement_reference": "SQL-APP-002",
    "error_behavior": "Return HTTP 500 when active message query fails",
    "mandatory": true
  },
  {
    "step_id": "STEP-008",
    "step_name": "Evaluate Message Overlap Rule",
    "node_type": "BUSINESS_RULE",
    "description": "Rejects the request when an overlapping active message already exists for the same division.",
    "input_source": "Existing active message count",
    "output_target": "Business rule decision",
    "execution_order": 8,
    "requirement_reference": "BR-001",
    "error_behavior": "Return HTTP 409 when overlapping active message exists",
    "mandatory": true
  },
  {
    "step_id": "STEP-009",
    "step_name": "Build Application Message Insert Payload",
    "node_type": "REQUEST_TRANSFORMATION",
    "description": "Builds the database insert payload using request body fields and validated header context.",
    "input_source": "Validated request context",
    "output_target": "Application message insert payload",
    "execution_order": 9,
    "requirement_reference": "MAP-002",
    "error_behavior": "Return HTTP 500 when insert payload cannot be built",
    "mandatory": true
  },
  {
    "step_id": "STEP-010",
    "step_name": "Insert Application Message",
    "node_type": "DATABASE_COMMAND",
    "description": "Inserts the new application message into APP_MSGS and returns the generated message ID.",
    "input_source": "Application message insert payload",
    "output_target": "Generated message ID",
    "execution_order": 10,
    "requirement_reference": "SQL-APP-001",
    "error_behavior": "Return HTTP 500 when database insert fails",
    "mandatory": true
  },
  {
    "step_id": "STEP-011",
    "step_name": "Map Create Message Success Response",
    "node_type": "RESPONSE_MAPPING",
    "description": "Maps the generated message ID into the final API success response.",
    "input_source": "Generated message ID",
    "output_target": "Final success response body",
    "execution_order": 11,
    "requirement_reference": "RESP-001",
    "error_behavior": "Return HTTP 500 when response mapping fails",
    "mandatory": true
  },
  {
    "step_id": "STEP-012",
    "step_name": "Return API Response",
    "node_type": "HTTP_RESPONSE",
    "description": "Returns HTTP 201 with the generated message ID when processing succeeds.",
    "input_source": "Final success response body",
    "output_target": "HTTP client",
    "execution_order": 12,
    "requirement_reference": "RESP-002",
    "error_behavior": "End request processing",
    "mandatory": true
  }
]
```

---

## Error Handling Representation

Error handling can be represented either as a dedicated node or as behavior attached to each workflow step.

For this sample, each step includes an `error_behavior` field.

A separate centralized error handling workflow can also be represented as follows:

```json
[
  {
    "step_id": "ERR-001",
    "step_name": "Handle Validation Error",
    "node_type": "ERROR_HANDLING",
    "description": "Builds standardized HTTP 400 error response for validation failures.",
    "input_source": "Validation error context",
    "output_target": "Standardized error response",
    "requirement_reference": "ERR-001",
    "mandatory": true
  },
  {
    "step_id": "ERR-002",
    "step_name": "Handle Conflict Error",
    "node_type": "ERROR_HANDLING",
    "description": "Builds standardized HTTP 409 error response when overlapping active message exists.",
    "input_source": "Business rule failure context",
    "output_target": "Standardized error response",
    "requirement_reference": "ERR-002",
    "mandatory": true
  },
  {
    "step_id": "ERR-003",
    "step_name": "Handle Database Error",
    "node_type": "ERROR_HANDLING",
    "description": "Builds standardized HTTP 500 error response when database operations fail.",
    "input_source": "Database error context",
    "output_target": "Standardized error response",
    "requirement_reference": "ERR-003",
    "mandatory": true
  }
]
```

---

## Visual Workflow Order

```text
HTTP_INPUT
    ↓
HEADER_VALIDATION
    ↓
HEADER_VALIDATION
    ↓
BODY_VALIDATION
    ↓
ENUM_VALIDATION
    ↓
REQUEST_TRANSFORMATION
    ↓
DATABASE_QUERY
    ↓
BUSINESS_RULE
    ↓
REQUEST_TRANSFORMATION
    ↓
DATABASE_COMMAND
    ↓
RESPONSE_MAPPING
    ↓
HTTP_RESPONSE
```

---

## Node-RED Mapping View

| Workflow Step                            | Node Type                | Possible Node-RED Equivalent |
| ---------------------------------------- | ------------------------ | ---------------------------- |
| Receive Create Message Request           | `HTTP_INPUT`             | `http in`                    |
| Validate Required Headers                | `HEADER_VALIDATION`      | `function`                   |
| Validate Header Formats                  | `HEADER_VALIDATION`      | `function`                   |
| Validate Request Body                    | `BODY_VALIDATION`        | `function`                   |
| Validate Message Severity Enum           | `ENUM_VALIDATION`        | `function`                   |
| Build Active Message Lookup Context      | `REQUEST_TRANSFORMATION` | `function`                   |
| Check Existing Active Messages           | `DATABASE_QUERY`         | PostgreSQL node / function   |
| Evaluate Message Overlap Rule            | `BUSINESS_RULE`          | `function`                   |
| Build Application Message Insert Payload | `REQUEST_TRANSFORMATION` | `function`                   |
| Insert Application Message               | `DATABASE_COMMAND`       | PostgreSQL node / function   |
| Map Create Message Success Response      | `RESPONSE_MAPPING`       | `function`                   |
| Return API Response                      | `HTTP_RESPONSE`          | `http response`              |

---

## Go Code Mapping View

| Workflow Step                            | Node Type                | Possible Go Layer       |
| ---------------------------------------- | ------------------------ | ----------------------- |
| Receive Create Message Request           | `HTTP_INPUT`             | Handler route           |
| Validate Required Headers                | `HEADER_VALIDATION`      | Handler / middleware    |
| Validate Header Formats                  | `HEADER_VALIDATION`      | Validation layer        |
| Validate Request Body                    | `BODY_VALIDATION`        | DTO validation          |
| Validate Message Severity Enum           | `ENUM_VALIDATION`        | Validation package      |
| Build Active Message Lookup Context      | `REQUEST_TRANSFORMATION` | Mapper / service layer  |
| Check Existing Active Messages           | `DATABASE_QUERY`         | Repository layer        |
| Evaluate Message Overlap Rule            | `BUSINESS_RULE`          | Service layer           |
| Build Application Message Insert Payload | `REQUEST_TRANSFORMATION` | Mapper / service layer  |
| Insert Application Message               | `DATABASE_COMMAND`       | Repository layer        |
| Map Create Message Success Response      | `RESPONSE_MAPPING`       | Mapper / service layer  |
| Return API Response                      | `HTTP_RESPONSE`          | Handler response writer |

---

## Excel Intake Mapping View

The workflow representation can later support Excel intake generation.

| Workflow Field        | Possible Excel Sheet                         |
| --------------------- | -------------------------------------------- |
| Endpoint details      | `api_endpoints`                              |
| Header validation     | `api_endpoints` / `business_requirements`    |
| Body validation       | `api_endpoints`                              |
| Business rule         | `business_requirements`                      |
| Database query        | `queries`                                    |
| Database command      | `queries`                                    |
| External service call | `external_services`                          |
| Step ordering         | `endpoint_service_mapping`                   |
| Response mapping      | `api_endpoints` / `endpoint_service_mapping` |
| Entity reference      | `entities`                                   |

---

## Review Checklist

Before accepting a workflow representation, confirm that:

* The endpoint is represented as an `HTTP_INPUT` node.
* All required headers are validated.
* Header format and allowed-value rules are captured.
* Required body fields are validated.
* Enum fields are validated.
* Business rules are represented as separate steps.
* Required database queries are represented.
* Required database commands are represented.
* Response mapping is included.
* The workflow ends with an `HTTP_RESPONSE` node.
* Error behavior is defined for each major failure path.
* Each step has a traceable requirement reference.
* The workflow is understandable by both humans and future generation tools.

---

## What This Sample Does Not Include

This sample does not include:

* Actual Node-RED JSON
* Actual Go code
* Actual Excel workbook
* Runtime configuration
* Test cases
* LLM prompt execution
* Production deployment details

Those activities will be handled in later days of the challenge.

---

## Summary

This sample workflow representation demonstrates how extracted API requirements can be converted into a structured execution plan.

The representation is technology-neutral and can later support Node-RED flow generation, Excel intake creation, and Go code generation.

This sample also shows how business rules, database operations, validation logic, response mapping, and error behavior can be captured before implementation begins.
