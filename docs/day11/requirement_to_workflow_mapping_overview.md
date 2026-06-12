# Day 11 — Requirement-to-Workflow Mapping Overview

## Purpose

This document defines the overview for mapping raw API requirements into Workflow Schema v2.

Day 10 created the refined Workflow Schema v2 structure. Day 11 explains how raw requirement information should be interpreted and placed into that schema.

The goal is to create clear mapping rules so that future extraction agents, documentation generators, Excel generators, Node-RED generators, Go code generators, and test generators all work from the same structured workflow representation.

No coding is performed on Day 11.

---

## Background

The 50-Day AI Software Engineer Challenge is building an AI-assisted pipeline for enterprise API development.

The long-term goal is to convert requirement sources into structured artifacts such as:

* Node-RED flows
* Excel intake files
* Go service code
* API documentation
* Test cases
* Evaluation reports

Before generating these artifacts, the project needs an intermediate workflow representation.

Workflow Schema v2 provides that intermediate representation.

Day 11 focuses on the next question:

```text
How do we map raw requirement content into Workflow Schema v2?
```

---

## Why Mapping Rules Are Needed

Raw requirements are often written for humans, not machines.

They may contain:

* Paragraphs of business logic
* Tables of request fields
* API contract sections
* Downstream service notes
* Database lookup descriptions
* Validation rules
* Error examples
* Response examples
* Flow diagrams
* Node-RED prototype behavior
* Excel intake expectations

A future extraction agent cannot reliably generate artifacts directly from this raw content unless there are clear rules for interpreting it.

Mapping rules reduce ambiguity by defining exactly where each requirement detail should go in Schema v2.

---

## Problem with Direct Generation

Directly generating Node-RED flows, Excel files, or Go code from raw requirement text is risky.

Raw requirements may be:

* Incomplete
* Ambiguous
* Inconsistent
* Mixed with business and technical language
* Missing field types
* Missing error behavior
* Missing response examples
* Written in different styles by different teams
* Spread across multiple documents

If a generator reads the requirement directly, it may make assumptions.

Those assumptions can create incorrect artifacts.

Examples:

```text
A requirement says "validate account details" but does not clearly say which fields are required.

A requirement says "call account service" but does not clearly define the request mapping.

A requirement says "apply discount rules" but does not clearly separate employee discount, new account discount, exclusions, and calculations.

A requirement says "return error response" but does not define status code, error code, or message format.
```

Because of this, direct generation should be avoided in the early design.

Instead, raw requirements should first be converted into Workflow Schema v2.

---

## Role of Workflow Schema v2

Workflow Schema v2 acts as a structured contract between requirement extraction and artifact generation.

The pipeline should follow this pattern:

```text
Raw Requirements
        ↓
Requirement Extraction and Mapping
        ↓
Workflow Schema v2
        ↓
Artifact Generators
        ↓
Node-RED / Excel / Go / Documentation / Tests
```

Schema v2 helps separate two different responsibilities.

## Responsibility 1 — Requirement Understanding

This is where raw requirement text is analyzed and converted into structured schema fields.

This includes identifying:

* API contract
* Request fields
* Validation rules
* Workflow steps
* Downstream services
* Database references
* Business rules
* Error behavior
* Response mappings
* Test scenarios
* Source traceability

## Responsibility 2 — Artifact Generation

This is where structured schema data is used to generate artifacts.

This includes generating:

* Node-RED flows
* Excel workbook rows
* Go code layers
* Documentation
* Test cases
* Evaluation outputs

By separating these responsibilities, the project becomes easier to validate and improve.

---

## Mapping Rule Objective

The purpose of mapping rules is to define how each requirement detail should be placed into Schema v2.

The mapping rules should answer questions like:

```text
If a requirement mentions an endpoint, where does it go?

If a requirement lists headers, where do they go?

If a requirement says a field is required, where does that validation go?

If a requirement says call a downstream service, how is that represented?

If a requirement says check a database table, how is that captured?

If a requirement says apply a business rule, how is that converted into a workflow step?

If a requirement gives an error response, how is it mapped?

If a requirement gives a response field, how is it mapped from source to target?
```

---

## Requirement Sources

The mapping process should support multiple requirement sources.

## 1. Confluence Requirement Documents

Confluence documents may include:

* Functional requirements
* API endpoint details
* Request and response examples
* Business rules
* External service details
* Error handling rules
* Database table references
* Diagrams or screenshots

These documents are usually the primary source of truth.

## 2. API Contracts

API contracts may include:

* Endpoint path
* HTTP method
* Headers
* Request body schema
* Response schema
* Status codes
* Field descriptions
* OpenAPI-style definitions

These details should map mostly into the `api_contract`, `field_validations`, and `response_mappings` blocks.

## 3. Node-RED Prototype Flows

Node-RED flows may clarify:

* Step order
* Mapping logic
* Downstream service calls
* Branching behavior
* Error handling
* Response construction
* Runtime behavior

Node-RED should be used as a workflow clarification source, especially when the requirement document is ambiguous.

## 4. Excel Intake Expectations

Excel intake formats may clarify how requirements should be consumed by future Go Factory or code generation tools.

Excel expectations may include:

* API endpoint rows
* External service rows
* Entity rows
* Query rows
* Business requirement rows
* Endpoint service mapping rows

These details should map from Schema v2 into Excel later, but during Day 11 they help shape mapping consistency.

## 5. Existing Workflow Notes

Existing notes from previous analysis may include:

* Confirmed field behavior
* Corrected mappings
* Known downstream errors
* Business logic clarifications
* User-confirmed assumptions
* Final response examples

These notes can help resolve gaps in raw requirements.

---

## Mapping Priority Order

When multiple sources provide information, the mapping process should use a priority order.

Recommended priority:

```text
1. Official requirement document
2. API contract or confirmed request/response examples
3. User-confirmed clarification
4. Existing prototype behavior
5. Node-RED flow behavior
6. Excel intake expectation
7. Reasonable assumption marked with low confidence
```

The mapping process should avoid silent assumptions.

If something is inferred, it should be marked in `source_traceability` with lower confidence.

---

## Schema v2 Blocks Used in Mapping

Requirement details should map into the following Schema v2 blocks:

```text
metadata
api_contract
field_validations
workflow_steps
branching
external_services
authentication
database_references
business_rules
calculation_rules
request_mappings
response_mappings
error_mappings
test_scenarios
source_traceability
generator_readiness
```

Each block has a specific purpose.

The mapping rules should make sure that requirement information is placed in the correct block.

---

# High-Level Mapping Categories

## 1. API-Level Requirements

API-level requirements describe the public interface of the API.

They include:

* Endpoint path
* HTTP method
* Operation name
* API version
* Headers
* Request body
* Response format
* Status codes

These map primarily to:

```text
api_contract
field_validations
response_mappings
error_mappings
source_traceability
```

---

## 2. Validation Requirements

Validation requirements describe what the API must check before processing.

They include:

* Required headers
* Required body fields
* Data types
* Enum values
* Numeric rules
* String rules
* Array rules
* Nested object rules
* Invalid input behavior

These map primarily to:

```text
field_validations
workflow_steps
error_mappings
test_scenarios
source_traceability
```

---

## 3. Workflow Requirements

Workflow requirements describe what happens after the request is accepted.

They include:

* Step order
* Internal context creation
* Mapping
* External service calls
* Database lookups
* Business rules
* Branching
* Calculations
* Response creation

These map primarily to:

```text
workflow_steps
branching
request_mappings
external_services
database_references
business_rules
calculation_rules
response_mappings
error_mappings
source_traceability
```

---

## 4. External Service Requirements

External service requirements describe downstream dependencies.

They include:

* Service name
* Service endpoint
* HTTP method or SOAP operation
* Request mapping
* Response mapping
* Authentication
* Timeout behavior
* Retry behavior
* Error handling

These map primarily to:

```text
external_services
authentication
request_mappings
response_mappings
error_mappings
test_scenarios
source_traceability
```

---

## 5. Database Requirements

Database requirements describe persistence or lookup behavior.

They include:

* Database type
* Table names
* Query purpose
* Query inputs
* Query outputs
* No-record behavior
* Failure behavior

These map primarily to:

```text
database_references
business_rules
workflow_steps
error_mappings
test_scenarios
source_traceability
```

---

## 6. Business Rule Requirements

Business rules describe decisions or eligibility logic.

They include:

* Eligibility conditions
* Exclusion rules
* Priority rules
* Date-based rules
* Cap rules
* Discount rules
* Path selection logic

These map primarily to:

```text
business_rules
branching
calculation_rules
database_references
workflow_steps
test_scenarios
source_traceability
```

---

## 7. Response Requirements

Response requirements describe how the API output should be built.

They include:

* Success response structure
* Error response structure
* Source-to-target field mappings
* HTTP status codes
* Response transformation rules

These map primarily to:

```text
response_mappings
error_mappings
workflow_steps
test_scenarios
source_traceability
```

---

## 8. Testing Requirements

Testing requirements describe scenarios that must be verified.

They include:

* Positive scenarios
* Negative scenarios
* Boundary scenarios
* Downstream failure scenarios
* Database failure scenarios
* Business rule path scenarios
* Expected response body
* Expected status code

These map primarily to:

```text
test_scenarios
field_validations
branching
external_services
database_references
business_rules
error_mappings
```

---

# Mapping Confidence

Every important mapped item should include a confidence level when possible.

Recommended confidence values:

```text
high
medium
low
```

## High Confidence

Use `high` when the requirement explicitly states the detail.

Example:

```text
"The endpoint shall be POST /api/v1/fabrics."
```

## Medium Confidence

Use `medium` when the detail is supported by a prototype, example, or related document but is not clearly stated in the main requirement.

Example:

```text
A Node-RED flow shows that Trace-Id is passed downstream, but the Confluence document does not explicitly say this.
```

## Low Confidence

Use `low` when the detail is inferred and should be reviewed later.

Example:

```text
The requirement says "return error" but does not define the status code. The mapper assumes 500 for unexpected system errors.
```

---

# Handling Ambiguity

When a requirement is unclear, the mapping process should not silently invent details.

Instead, it should:

1. Capture the best available interpretation.
2. Mark the mapped item with lower confidence.
3. Add a note in source traceability.
4. Add the issue to a future gap analysis or review list.

Example:

```yaml
source_traceability:
  step_sources:
    - step_id: STEP-009
      requirement_section: New Account Discount Logic
      source_text_reference: Inferred from prototype flow
      confidence: medium
      notes: Requirement document does not explicitly define all new account branch conditions.
```

---

# Mapping Output Expectation

The output of requirement-to-workflow mapping should not be code.

The output should be a structured workflow document using Schema v2.

Example output structure:

```text
workflow:
  metadata:
  api_contract:
  field_validations:
  workflow_steps:
  branching:
  external_services:
  authentication:
  database_references:
  business_rules:
  calculation_rules:
  request_mappings:
  response_mappings:
  error_mappings:
  test_scenarios:
  source_traceability:
  generator_readiness:
```

This structured output becomes the source of truth for future generation.

---

# Why This Improves the Pipeline

Mapping raw requirements into Schema v2 improves the project in several ways.

## 1. Reduces Ambiguity

The schema forces unclear requirements to be structured.

This makes missing information easier to detect.

## 2. Improves Generator Reliability

Generators work better when inputs are structured.

Schema v2 gives generators predictable fields instead of raw paragraphs.

## 3. Supports Human Review

Humans can review the schema before generation.

This is safer than reviewing generated code after incorrect assumptions have already been made.

## 4. Supports Reuse

The same schema can be used for multiple generators.

One mapped workflow can support:

* Node-RED
* Excel
* Go
* Documentation
* Tests

## 5. Supports Evaluation

The schema provides a measurable artifact.

It can be checked for:

* Completeness
* Accuracy
* Missing fields
* Traceability
* Generator readiness

---

# Example Mapping Flow

A raw requirement might say:

```text
The API shall validate that Division-Id is present and must be either 12 or 13.
```

This maps to Schema v2 as:

```yaml
field_validations:
  - validation_id: VAL-FABRIC-HDR-001
    field_path: request.headers.Division-Id
    location: header
    type: integer
    required: true
    allowed_values:
      - 12
      - 13
    failure_behavior:
      http_status: 400
      error_code: INVALID_DIVISION
      message: Division-Id must be 12 or 13.
```

It may also create or support a workflow step:

```yaml
workflow_steps:
  - step_id: STEP-002
    step_name: Validate Division-Id
    step_type: validation
    inputs:
      - request.headers.Division-Id
    outputs:
      - validatedDivisionId
```

And a test scenario:

```yaml
test_scenarios:
  - scenario_id: TC-FABRIC-003
    scenario_name: Unsupported Division-Id
    scenario_type: negative
    expected:
      http_status: 400
```

This shows how one requirement can map into multiple Schema v2 sections.

---

# Mapping Review Checklist

Each mapped workflow should be reviewed using the following questions:

```text
Did every endpoint detail map to api_contract?
Did every required field map to field_validations?
Did every validation failure map to error_mappings?
Did every workflow action map to workflow_steps?
Did every condition map to branching or business_rules?
Did every downstream call map to external_services?
Did every authentication rule map to authentication?
Did every database lookup map to database_references?
Did every business rule map to business_rules?
Did every calculation map to calculation_rules?
Did every source-to-target field mapping map to request_mappings or response_mappings?
Did every important requirement have source_traceability?
Did generator_readiness identify missing information?
```

---

# Day 11 Scope

Day 11 defines mapping rules only.

It does not:

* Implement an extraction agent
* Parse Confluence pages
* Generate Schema v2 automatically
* Generate Node-RED flows
* Generate Excel files
* Generate Go code
* Generate tests
* Connect to external systems

Day 11 is a documentation and design step.

---

# Day 11 Expected Files

Day 11 should produce the following files:

```text
docs/day11/
├── requirement_to_workflow_mapping_overview.md
├── api_contract_mapping_rules.md
├── validation_mapping_rules.md
├── workflow_step_mapping_rules.md
├── service_query_business_rule_mapping.md
└── day11_completion_report.md
```

This file completes the mapping overview.

The next files will define detailed mapping rules for specific Schema v2 areas.

---

# Final Summary

Requirement-to-workflow mapping is the bridge between raw enterprise requirements and Workflow Schema v2.

It prevents direct, assumption-heavy generation by first converting requirements into a structured, reviewable, and generator-ready format.

This mapping process is essential for the project because it supports:

* More accurate extraction
* Better workflow representation
* Safer artifact generation
* Better test planning
* Stronger documentation
* Clearer research evaluation

This completes Phase 1 of Day 11.
