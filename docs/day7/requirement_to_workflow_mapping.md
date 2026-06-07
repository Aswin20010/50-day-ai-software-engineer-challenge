# Day 7 — Requirement to Workflow Mapping

## Purpose

This document defines how extracted requirements should be mapped into structured workflow steps.

The purpose of this mapping layer is to convert natural language API requirements into an ordered workflow representation that can later support Node-RED flow generation, Excel intake preparation, and Go code generation.

No implementation or coding is performed on Day 7.

---

## Why Requirement to Workflow Mapping Is Needed

Requirement documents usually describe API behavior in paragraphs, tables, bullet points, and examples.

These requirements may include:

* API endpoint details
* Required headers
* Required request fields
* Validation rules
* Business rules
* Database operations
* Downstream service calls
* Response mapping rules
* Error handling rules

However, code generators and flow generators need this information in a structured format.

The requirement-to-workflow mapping process converts extracted requirement details into clear execution steps.

---

## Input to the Mapping Layer

The input to this layer is the structured output from the Requirement Extraction Agent.

Example extracted requirement fields:

```json
{
  "endpoint": "POST /api/v1/example",
  "headers": ["Division-Id", "Store-Number", "Trace-Id"],
  "request_fields": ["accountNumber", "transactionAmount"],
  "business_rules": ["Validate account eligibility before applying discount"],
  "external_services": ["Account Interrogation Service"],
  "database_operations": ["Query discount profile table"],
  "response_fields": ["discountAmount", "discountApplied"]
}
```

---

## Output of the Mapping Layer

The output is an ordered workflow representation.

Example workflow output:

```json
[
  {
    "step_id": "STEP-001",
    "node_type": "HTTP_INPUT",
    "step_name": "Receive API Request",
    "execution_order": 1
  },
  {
    "step_id": "STEP-002",
    "node_type": "HEADER_VALIDATION",
    "step_name": "Validate Required Headers",
    "execution_order": 2
  },
  {
    "step_id": "STEP-003",
    "node_type": "BODY_VALIDATION",
    "step_name": "Validate Request Body",
    "execution_order": 3
  }
]
```

---

## Mapping Principle

Each requirement should be mapped to one or more workflow steps.

A single sentence in a requirement document may produce multiple workflow nodes.

Example requirement:

```text
The API must validate Division-Id and Store-Number headers before calling the downstream service.
```

Mapped workflow steps:

| Requirement Fragment         | Workflow Node Type      |
| ---------------------------- | ----------------------- |
| Validate Division-Id header  | `HEADER_VALIDATION`     |
| Validate Store-Number header | `HEADER_VALIDATION`     |
| Call downstream service      | `EXTERNAL_SERVICE_CALL` |

---

## Standard Mapping Rules

### 1. Endpoint Requirement to HTTP_INPUT

When the requirement defines an HTTP method and path, it should map to an `HTTP_INPUT` node.

Example requirement:

```text
The service exposes POST /api/v1/discount/evaluateTransactionDiscount.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-001",
  "node_type": "HTTP_INPUT",
  "step_name": "Receive Evaluate Transaction Discount Request",
  "description": "Receives POST request for /api/v1/discount/evaluateTransactionDiscount.",
  "input_source": "HTTP client",
  "output_target": "Raw request context",
  "execution_order": 1,
  "requirement_reference": "API-001",
  "error_behavior": "Return HTTP 404 or 405 when route or method is unsupported",
  "mandatory": true
}
```

---

### 2. Header Requirements to HEADER_VALIDATION

When the requirement defines required headers, optional headers, header formats, or allowed header values, it should map to a `HEADER_VALIDATION` node.

Example requirement:

```text
The request must include Division-Id, Store-Number, and Trace-Id headers.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-002",
  "node_type": "HEADER_VALIDATION",
  "step_name": "Validate Request Headers",
  "description": "Validates required headers including Division-Id, Store-Number, and Trace-Id.",
  "input_source": "HTTP request headers",
  "output_target": "Validated request context",
  "execution_order": 2,
  "requirement_reference": "HDR-001",
  "error_behavior": "Return HTTP 400 when required headers are missing or invalid",
  "mandatory": true
}
```

---

### 3. Body Field Requirements to BODY_VALIDATION

When the requirement defines required request body fields, nested fields, arrays, data types, or format constraints, it should map to a `BODY_VALIDATION` node.

Example requirement:

```text
The request body must contain account.encryptInfo and account.entryMode.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-003",
  "node_type": "BODY_VALIDATION",
  "step_name": "Validate Request Body",
  "description": "Validates required request body fields including account.encryptInfo and account.entryMode.",
  "input_source": "HTTP request body",
  "output_target": "Validated request body",
  "execution_order": 3,
  "requirement_reference": "BODY-001",
  "error_behavior": "Return HTTP 400 when required request body fields are missing",
  "mandatory": true
}
```

---

### 4. Enum Requirements to ENUM_VALIDATION

When a requirement defines allowed values for a field, it should map to an `ENUM_VALIDATION` node.

Example requirement:

```text
entryMode must be one of KEYED, SWIPED, BARCODE_SCAN, or TOKENIZED.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-004",
  "node_type": "ENUM_VALIDATION",
  "step_name": "Validate Entry Mode Enum",
  "description": "Validates that entryMode contains one of the supported values.",
  "input_source": "Validated request body",
  "output_target": "Validated request context",
  "execution_order": 4,
  "requirement_reference": "ENUM-001",
  "error_behavior": "Return HTTP 400 when entryMode contains an unsupported value",
  "mandatory": true
}
```

---

### 5. Transformation Requirements to REQUEST_TRANSFORMATION

When a requirement describes building, formatting, renaming, padding, converting, or restructuring data, it should map to a `REQUEST_TRANSFORMATION` node.

Example requirement:

```text
The service must build a PIIDLookupRequest using encryptedData and entryMode from the incoming request.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-005",
  "node_type": "REQUEST_TRANSFORMATION",
  "step_name": "Build PIID Lookup Request",
  "description": "Transforms the incoming request into the downstream PIIDLookupRequest format.",
  "input_source": "Validated request context",
  "output_target": "PIIDLookupRequest",
  "execution_order": 5,
  "requirement_reference": "MAP-001",
  "error_behavior": "Return HTTP 500 when downstream request construction fails",
  "mandatory": true
}
```

---

### 6. Business Rule Requirements to BUSINESS_RULE

When a requirement defines conditional logic, eligibility checks, caps, overlap checks, routing decisions, or domain rules, it should map to a `BUSINESS_RULE` node.

Example requirement:

```text
The service must reject active messages when another active message overlaps for the same division and time window.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-006",
  "node_type": "BUSINESS_RULE",
  "step_name": "Check Active Message Overlap",
  "description": "Checks whether another active message exists for the same division and overlapping time window.",
  "input_source": "Validated request context",
  "output_target": "Business rule decision",
  "execution_order": 6,
  "requirement_reference": "BR-001",
  "error_behavior": "Return HTTP 409 when an overlapping active message exists",
  "mandatory": true
}
```

---

### 7. Database Read Requirements to DATABASE_QUERY

When a requirement describes reading, checking, selecting, or fetching data from a database, it should map to a `DATABASE_QUERY` node.

Example requirement:

```text
Query the discount profile table to determine the applicable employee discount percentage.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-007",
  "node_type": "DATABASE_QUERY",
  "step_name": "Query Discount Profile",
  "description": "Queries the discount profile table to determine the applicable discount percentage.",
  "input_source": "Business rule context",
  "output_target": "Discount profile result",
  "execution_order": 7,
  "requirement_reference": "SQL-001",
  "error_behavior": "Return HTTP 500 when database query fails",
  "mandatory": true
}
```

---

### 8. Database Write Requirements to DATABASE_COMMAND

When a requirement describes inserting, updating, deleting, saving, or committing data to a database, it should map to a `DATABASE_COMMAND` node.

Example requirement:

```text
Insert the application message into APP_MSGS and return the generated message ID.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-008",
  "node_type": "DATABASE_COMMAND",
  "step_name": "Insert Application Message",
  "description": "Inserts a new application message into APP_MSGS and returns the generated ID.",
  "input_source": "Mapped database command payload",
  "output_target": "Generated message ID",
  "execution_order": 8,
  "requirement_reference": "SQL-APP-001",
  "error_behavior": "Return HTTP 500 when database insert fails",
  "mandatory": true
}
```

---

### 9. Downstream Service Requirements to EXTERNAL_SERVICE_CALL

When a requirement describes calling another API, REST service, SOAP service, GraphQL service, or reusable wrapper, it should map to an `EXTERNAL_SERVICE_CALL` node.

Example requirement:

```text
The service must call the Account Interrogation Service before evaluating discount rules.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-009",
  "node_type": "EXTERNAL_SERVICE_CALL",
  "step_name": "Call Account Interrogation Service",
  "description": "Calls the downstream Account Interrogation Service to retrieve account details.",
  "input_source": "PIIDLookupRequest",
  "output_target": "Account interrogation response",
  "execution_order": 9,
  "requirement_reference": "SRV-001",
  "error_behavior": "Return mapped downstream error when service call fails",
  "mandatory": true
}
```

---

### 10. Response Mapping Requirements to RESPONSE_MAPPING

When a requirement defines how internal, database, or downstream output fields should appear in the final API response, it should map to a `RESPONSE_MAPPING` node.

Example requirement:

```text
The response must include discountApplied, discountAmount, and discountPercentApplied.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-010",
  "node_type": "RESPONSE_MAPPING",
  "step_name": "Map Final Discount Response",
  "description": "Maps discount calculation results into the final API response format.",
  "input_source": "Service execution result",
  "output_target": "Final API response body",
  "execution_order": 10,
  "requirement_reference": "RESP-001",
  "error_behavior": "Return HTTP 500 when response mapping fails",
  "mandatory": true
}
```

---

### 11. Error Handling Requirements to ERROR_HANDLING

When a requirement describes validation errors, downstream errors, database errors, status codes, error response shape, or failure behavior, it should map to an `ERROR_HANDLING` node.

Example requirement:

```text
If the downstream service returns an error, the API must return a standardized error response.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-011",
  "node_type": "ERROR_HANDLING",
  "step_name": "Normalize Downstream Error",
  "description": "Maps downstream failures into the API standard error response format.",
  "input_source": "Downstream error response",
  "output_target": "Standardized API error response",
  "execution_order": 11,
  "requirement_reference": "ERR-001",
  "error_behavior": "Return mapped HTTP status and error body",
  "mandatory": true
}
```

---

### 12. Final Response Requirement to HTTP_RESPONSE

When a requirement defines the final response returned to the client, it should map to an `HTTP_RESPONSE` node.

Example requirement:

```text
The API returns HTTP 200 with the mapped discount response when processing succeeds.
```

Mapped workflow step:

```json
{
  "step_id": "STEP-012",
  "node_type": "HTTP_RESPONSE",
  "step_name": "Return Final API Response",
  "description": "Returns the final success or error response to the API client.",
  "input_source": "Final response body",
  "output_target": "HTTP client",
  "execution_order": 12,
  "requirement_reference": "RESP-002",
  "error_behavior": "End request processing",
  "mandatory": true
}
```

---

## Mapping Examples

## Example 1 — Header Validation Requirement

Requirement:

```text
The request must include Client-Id, Subclient-Id, Division-Id, and Store-Number headers.
```

Workflow mapping:

| Step     | Node Type           | Step Name                 |
| -------- | ------------------- | ------------------------- |
| STEP-001 | `HTTP_INPUT`        | Receive API Request       |
| STEP-002 | `HEADER_VALIDATION` | Validate Required Headers |

---

## Example 2 — Business Rule Requirement

Requirement:

```text
Reject the request when an active application message already exists for the same division and overlapping start and end date time.
```

Workflow mapping:

| Step     | Node Type        | Step Name                                 |
| -------- | ---------------- | ----------------------------------------- |
| STEP-001 | `DATABASE_QUERY` | Check Existing Active Messages            |
| STEP-002 | `BUSINESS_RULE`  | Evaluate Message Overlap                  |
| STEP-003 | `ERROR_HANDLING` | Return Conflict Error When Overlap Exists |

---

## Example 3 — Downstream Service Requirement

Requirement:

```text
Build the downstream request and call the Account Interrogation Service.
```

Workflow mapping:

| Step     | Node Type                | Step Name                          |
| -------- | ------------------------ | ---------------------------------- |
| STEP-001 | `REQUEST_TRANSFORMATION` | Build Downstream Request           |
| STEP-002 | `EXTERNAL_SERVICE_CALL`  | Call Account Interrogation Service |
| STEP-003 | `ERROR_HANDLING`         | Handle Downstream Failure          |
| STEP-004 | `RESPONSE_MAPPING`       | Map Downstream Response            |

---

## Example 4 — Database Write Requirement

Requirement:

```text
Insert the new application message and return the generated ID.
```

Workflow mapping:

| Step     | Node Type                | Step Name                  |
| -------- | ------------------------ | -------------------------- |
| STEP-001 | `REQUEST_TRANSFORMATION` | Build Insert Payload       |
| STEP-002 | `DATABASE_COMMAND`       | Insert Application Message |
| STEP-003 | `RESPONSE_MAPPING`       | Map Generated ID Response  |

---

## Execution Ordering Guidelines

The mapping layer should preserve the logical execution order of the API.

Recommended default ordering:

```text
HTTP_INPUT
HEADER_VALIDATION
BODY_VALIDATION
ENUM_VALIDATION
REQUEST_TRANSFORMATION
BUSINESS_RULE
DATABASE_QUERY
EXTERNAL_SERVICE_CALL
DATABASE_COMMAND
RESPONSE_MAPPING
ERROR_HANDLING
HTTP_RESPONSE
```

This order can change depending on the requirement.

For example:

* A database query may happen before a business rule when the rule depends on database data.
* A downstream service call may happen before database checks when the database logic depends on downstream response data.
* Error handling may be attached to multiple steps rather than appearing only once at the end.

---

## Traceability Requirements

Each workflow step should link back to at least one source requirement.

Traceability can be maintained using:

* `requirement_reference`
* `business_rule_id`
* `api_endpoint_id`
* `query_id`
* `service_id`
* `mapping_id`

Example:

```json
{
  "step_id": "STEP-006",
  "node_type": "BUSINESS_RULE",
  "step_name": "Check Active Message Overlap",
  "requirement_reference": "BR-001",
  "related_query": "SQL-APP-002",
  "related_endpoint": "EP-001"
}
```

---

## Ambiguity Handling

Some requirement statements may be incomplete or unclear.

When ambiguity exists, the workflow representation should mark the step as needing review.

Example:

```json
{
  "step_id": "STEP-005",
  "node_type": "REQUEST_TRANSFORMATION",
  "step_name": "Build Downstream Request",
  "description": "Requirement indicates a downstream request must be built, but exact field mappings are not fully defined.",
  "review_required": true,
  "ambiguity_reason": "Missing downstream request field mapping"
}
```

---

## Mapping Quality Checklist

Before accepting the workflow mapping, verify the following:

* Every endpoint has an `HTTP_INPUT` step.
* Required headers map to `HEADER_VALIDATION`.
* Required body fields map to `BODY_VALIDATION`.
* Enum constraints map to `ENUM_VALIDATION`.
* Business rules map to `BUSINESS_RULE`.
* Downstream calls map to `EXTERNAL_SERVICE_CALL`.
* Database reads map to `DATABASE_QUERY`.
* Database writes map to `DATABASE_COMMAND`.
* Final response fields map to `RESPONSE_MAPPING`.
* Every flow ends with `HTTP_RESPONSE`.
* Error behavior is captured for validation, database, downstream, and mapping failures.
* Each step has a requirement reference.
* Ambiguous steps are clearly marked for review.

---

## What This Document Does Not Include

This document does not include:

* Actual code implementation
* Actual Node-RED flow JSON
* Actual Excel workbook generation
* Test execution
* LLM prompt execution
* Runtime configuration

Those activities will be handled later in the challenge.

---

## Summary

The requirement-to-workflow mapping process converts extracted API requirements into ordered workflow steps.

This mapping creates a reviewable bridge between requirement extraction and future generation tasks.

It ensures that natural language requirements can be transformed into structured logic before generating Node-RED flows, Excel intake files, or Go code.
