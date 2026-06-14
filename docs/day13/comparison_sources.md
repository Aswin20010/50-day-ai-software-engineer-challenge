# Day 13 — Phase 3: Comparison Sources

## Purpose

This document defines the comparison sources used by the Gap Analysis Framework.

The goal of this phase is to clearly identify which artifacts will be compared, what information each artifact provides, and how each source should be treated during gap detection.

The Gap Analysis Framework compares three main sources:

1. Confluence requirement documents
2. Node-RED workflow implementations
3. Excel intake workbooks

Each source has a different role in the AI-assisted API development pipeline.

---

## Why Comparison Sources Are Important

Gap analysis requires clearly defined sources.

Without clearly defining the comparison sources, the system may not know:

* Which artifact is the source of truth
* Which artifact represents implementation logic
* Which artifact represents structured code generation input
* Which artifact should be corrected when a mismatch is found
* How to resolve conflicts between artifacts

For this project, the Confluence requirement is treated as the main source of truth.

Node-RED is treated as the workflow representation.

Excel is treated as the structured intake format for the Go code generation factory.

---

## Source 1: Confluence Requirement Document

## Role

The Confluence requirement document is the primary source of truth.

It represents the original business and technical requirement written by business analysts, architects, or engineering teams.

All other artifacts should be checked against the Confluence requirement.

---

## What Confluence Provides

The Confluence document may provide:

```text
API endpoint path
HTTP method
Operation name
Request headers
Request body fields
Response body fields
Required and optional fields
Validation rules
Enum values
Business rules
Downstream service details
Database table references
Query expectations
Error handling rules
Authentication requirements
Tracing and logging expectations
Non-functional requirements
```

---

## Example Confluence Information

A Confluence requirement may define an API such as:

```text
POST /api/v1/discount/interrogateAccount
```

It may also define required headers:

```text
requestorInfo-clientId
requestorInfo-subclientId
requestorInfo-traceId
x-macys-apikey
```

It may define body fields:

```text
account.encryptInfo.encryptedData
account.encryptInfo.keyType
account.encryptInfo.keyName
account.encryptInfo.keyVersion
account.encryptInfo.division
account.encryptInfo.storeNbr
account.entryMode
```

It may also define business rules such as:

```text
If keyType is RSA, entryMode is required.
If account is an employee account, follow employee discount logic.
If account is a new account, follow new account discount logic.
```

---

## How Confluence Is Used in Gap Analysis

Confluence is used to answer the question:

```text
What was originally required?
```

The gap analysis system should extract requirement elements from Confluence and compare them against the Node-RED flow and Excel workbook.

If an item exists in Confluence but is missing from Node-RED or Excel, it should be marked as a gap.

---

## Confluence Comparison Responsibility

Confluence should be used to validate:

* Endpoint completeness
* Header completeness
* Request body completeness
* Response body completeness
* Validation completeness
* Business rule completeness
* Service dependency completeness
* Database dependency completeness
* Error handling completeness
* Technology alignment

---

## Source 2: Node-RED Workflow

## Role

The Node-RED workflow represents the visual or prototype implementation of the API logic.

It shows how the requirement is executed step by step.

While Confluence explains what should happen, Node-RED helps show how the logic is currently modeled.

---

## What Node-RED Provides

The Node-RED workflow may provide:

```text
HTTP input node
Header validation node
Body validation node
Enum validation node
Transformation node
Business rule node
REST service call node
SOAP service wrapper node
Database lookup node
Response mapping node
Error handling node
Final HTTP response node
```

---

## Example Node-RED Workflow

A typical Node-RED API workflow may look like this:

```text
HTTP In
  ↓
Validate Headers
  ↓
Validate Body
  ↓
Validate Enum Fields
  ↓
Build Downstream Request
  ↓
Invoke REST Service
  ↓
Validate Downstream Response
  ↓
Apply Business Rules
  ↓
Map Final Response
  ↓
HTTP Response
```

---

## How Node-RED Is Used in Gap Analysis

Node-RED is used to answer the question:

```text
How was the requirement modeled as a workflow?
```

The gap analysis system should compare Node-RED against Confluence and Excel.

If a workflow step exists in Confluence but is missing in Node-RED, the workflow is incomplete.

If a workflow step exists in Node-RED but is missing in Excel, the Excel intake workbook is incomplete.

If Node-RED contains logic that does not exist in Confluence, it may be an extra artifact or unsupported assumption.

---

## Node-RED Comparison Responsibility

Node-RED should be used to validate:

* Workflow step coverage
* Validation step coverage
* Mapping step coverage
* Service invocation sequence
* Database lookup sequence
* Response construction logic
* Error handling flow
* Business rule execution order

---

## Source 3: Excel Intake Workbook

## Role

The Excel intake workbook is the structured input used by the Go code generation factory.

It converts the requirements and workflow into machine-readable rows and columns.

The workbook must be accurate because it directly influences the generated Go code.

---

## What Excel Provides

The Excel workbook may contain sheets such as:

```text
technical_requirements
api_endpoints
entities
external_services
business_requirements
endpoint_service_mapping
queries
```

Each sheet captures a different part of the API generation input.

---

## Excel Sheet Responsibilities

## technical_requirements

This sheet defines the target technology and implementation environment.

It may include:

```text
Programming language
Framework
Database technology
Authentication approach
Logging requirements
Deployment assumptions
```

Example:

```text
Go + PostgreSQL
```

---

## api_endpoints

This sheet defines API contract information.

It may include:

```text
Endpoint path
HTTP method
Operation name
Required headers
Optional headers
Request body schema
Response body schema
Validation rules
Error responses
```

---

## entities

This sheet defines database entities or domain objects.

It may include:

```text
Entity name
Table name
Primary key
Important columns
Relationships
Business use
```

---

## external_services

This sheet defines downstream service dependencies.

It may include:

```text
Service ID
Service name
Service type
Endpoint URL
HTTP method
Request headers
Request body
Response fields
Timeout behavior
Error handling
```

---

## business_requirements

This sheet defines business rules in structured format.

It may include:

```text
Business rule ID
Rule description
Condition
Expected action
Acceptance criteria
Severity
```

---

## endpoint_service_mapping

This sheet defines endpoint orchestration.

It may include:

```text
Endpoint ID
Service or query reference
Workflow sequence
Input mapping
Output mapping
Transformation logic
Failure behavior
```

---

## queries

This sheet defines database queries required by the generated code.

It may include:

```text
Query ID
Query purpose
SQL statement
Input parameters
Output fields
Associated business rule
Associated endpoint
```

---

## How Excel Is Used in Gap Analysis

Excel is used to answer the question:

```text
Is the structured code generation input complete and accurate?
```

The gap analysis system should compare Excel against Confluence and Node-RED.

If Excel is missing any requirement, workflow step, validation, service call, entity, or query, it should be reported as a gap.

If Excel contains unsupported assumptions, they should also be reported.

---

## Excel Comparison Responsibility

Excel should be used to validate:

* API contract completeness
* Structured validation completeness
* Business requirement completeness
* Service dependency completeness
* Entity coverage
* Query coverage
* Endpoint mapping completeness
* Technology correctness
* Generated-code readiness

---

# Comparison Direction

The Gap Analysis Framework should compare the sources in multiple directions.

---

## 1. Confluence to Node-RED

This comparison checks whether the Node-RED workflow correctly represents the original requirement.

### Main Question

```text
Did the workflow capture the requirement correctly?
```

### Example Gaps

```text
Confluence defines a required header, but Node-RED has no header validation node.
Confluence defines a downstream service call, but Node-RED does not invoke that service.
Confluence defines an error condition, but Node-RED does not handle it.
```

---

## 2. Confluence to Excel

This comparison checks whether the Excel workbook correctly captures the original requirement.

### Main Question

```text
Did the Excel intake workbook capture the requirement correctly?
```

### Example Gaps

```text
Confluence defines a request field, but Excel does not include it.
Confluence defines enum values, but Excel only marks the field as required.
Confluence defines PostgreSQL, but Excel mentions SQLite.
```

---

## 3. Node-RED to Excel

This comparison checks whether the Excel workbook captures the workflow logic.

### Main Question

```text
Did the Excel workbook capture the workflow sequence correctly?
```

### Example Gaps

```text
Node-RED has a Validate Headers step, but Excel endpoint_service_mapping does not include validation.
Node-RED calls a SOAP wrapper, but Excel external_services does not define that wrapper.
Node-RED maps a downstream response field, but Excel response mapping is missing.
```

---

## 4. Excel to Confluence

This comparison checks whether Excel contains extra or unsupported details.

### Main Question

```text
Did the Excel workbook add anything that was not required?
```

### Example Gaps

```text
Excel includes Kafka publishing, but Confluence does not mention Kafka.
Excel includes a database query for a table not referenced in the requirement.
Excel marks a field as required even though Confluence says it is optional.
```

---

## 5. Node-RED to Confluence

This comparison checks whether the workflow contains unsupported assumptions.

### Main Question

```text
Did the workflow add logic that was not present in the original requirement?
```

### Example Gaps

```text
Node-RED adds a default value that is not mentioned in Confluence.
Node-RED calls an additional service not documented in the requirement.
Node-RED maps a response field that is not part of the required contract.
```

---

# Source Priority Rules

When two sources disagree, the framework should apply source priority rules.

---

## Priority 1: Confluence Requirement

Confluence has the highest priority because it is the source of truth.

If Confluence and Excel disagree, Confluence should usually win.

Example:

```text
Confluence says requestorInfo-traceId is optional.
Excel says requestorInfo-traceId is required.
```

The gap should be reported against Excel.

---

## Priority 2: Node-RED Workflow

Node-RED has second priority.

It shows how the requirement was modeled.

If Node-RED includes a step that is not in Excel but is consistent with Confluence, Excel should be updated.

Example:

```text
Confluence requires enum validation.
Node-RED contains enum validation.
Excel does not contain enum validation.
```

The gap should be reported against Excel.

---

## Priority 3: Excel Intake Workbook

Excel has third priority.

It is the final structured input to code generation, but it should not override the original requirement.

If Excel contains details not found in Confluence or Node-RED, the framework should mark them as unsupported assumptions.

Example:

```text
Excel includes a Kafka publishing step.
Confluence does not mention Kafka.
Node-RED does not contain Kafka logic.
```

This should be reported as an extra artifact gap.

---

# Comparison Matrix

The following matrix summarizes the responsibility of each source.

| Source     | Role                   | Main Use                             | Priority |
| ---------- | ---------------------- | ------------------------------------ | -------- |
| Confluence | Source of truth        | Defines original requirement         | Highest  |
| Node-RED   | Workflow model         | Shows step-by-step logic             | Medium   |
| Excel      | Code generation intake | Provides structured generation input | Lowest   |

---

# Gap Detection Examples

## Example 1: Missing Header

| Source     | Observation                                 |
| ---------- | ------------------------------------------- |
| Confluence | `x-macys-apikey` is required                |
| Node-RED   | Header validation includes `x-macys-apikey` |
| Excel      | Header is missing                           |

### Gap Result

```text
Gap Type: Missing Header Gap
Missing From: Excel
Severity: High
Recommendation: Add x-macys-apikey to api_endpoints required headers.
```

---

## Example 2: Technology Mismatch

| Source     | Observation                   |
| ---------- | ----------------------------- |
| Confluence | Target database is PostgreSQL |
| Node-RED   | Prototype uses SQLite         |
| Excel      | Mentions SQLite               |

### Gap Result

```text
Gap Type: Technology Mismatch Gap
Missing From: Excel
Severity: Critical
Recommendation: Update Excel technical requirements to PostgreSQL.
```

---

## Example 3: Missing Workflow Step

| Source     | Observation                                          |
| ---------- | ---------------------------------------------------- |
| Confluence | Request body must be validated                       |
| Node-RED   | Contains Validate Body node                          |
| Excel      | endpoint_service_mapping does not include validation |

### Gap Result

```text
Gap Type: Workflow Step Gap
Missing From: Excel
Severity: High
Recommendation: Add body validation step to endpoint_service_mapping.
```

---

## Summary

The Gap Analysis Framework compares Confluence, Node-RED, and Excel to determine whether the generated artifacts are complete and requirement-aligned.

Confluence is the source of truth.

Node-RED is the workflow implementation model.

Excel is the structured code generation intake.

By comparing these sources in multiple directions, the framework can detect missing requirements, incorrect mappings, unsupported assumptions, and technology mismatches before code generation begins.
