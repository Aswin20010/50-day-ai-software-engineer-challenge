# Day 10 — Workflow Schema v2 Overview

## Purpose

This document introduces Workflow Schema v2 for the 50-Day AI Software Engineer Challenge.

Workflow Schema v2 is a refined version of the workflow schema created on Day 8 and validated on Day 9.

The purpose of Schema v2 is to improve the structure, completeness, and generator-readiness of the workflow representation before future artifact generation begins.

No coding is performed on Day 10.

---

## Background

The project is building an AI-assisted software engineering pipeline that can eventually transform enterprise API requirements into structured artifacts.

The long-term goal is to support generation of:

* Node-RED flows
* Excel intake files
* Go service code
* API documentation
* Test cases
* Evaluation reports

To support this goal, a workflow schema is needed.

The workflow schema acts as a formal contract between requirement extraction and downstream artifact generation.

---

## What Day 8 Created

On Day 8, the first version of the workflow schema was defined.

Schema v1 focused on representing an API workflow as a structured sequence of workflow steps.

It introduced the idea that extracted requirements can be stored in a consistent format containing:

* Workflow metadata
* API endpoint details
* Request headers
* Request body fields
* Workflow steps
* Step inputs
* Step outputs
* Step dependencies
* External service references
* Database references
* Business rule references
* Response mapping
* Error behavior

Schema v1 was useful because it created the first formal structure for storing workflow information.

It moved the project from informal requirement notes toward a reusable workflow representation layer.

---

## Why Schema v1 Was Important

Schema v1 was important because it established the foundation for future artifact generation.

Before Day 8, requirements and workflows could be described in natural language, but there was no formal structure that future generators could reliably consume.

Schema v1 helped answer the question:

```text
How should an extracted API workflow be represented before it is converted into generated artifacts?
```

It introduced a common representation that could later be used by:

* A Node-RED flow generator
* An Excel intake generator
* A Go code generator
* A documentation generator
* A test case generator

However, Schema v1 was intentionally a first version. It was expected to be validated and improved.

---

## What Day 9 Validated

On Day 9, Schema v1 was validated against two realistic API workflows.

The selected APIs were:

1. FEED Discount API
2. Customize IT Fabric API

These APIs were chosen because they represent two different workflow types.

---

## Validation API 1 — FEED Discount API

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

This API was selected because it represents a complex enterprise workflow.

It includes:

* Required header validation
* Request body validation
* Enum validation
* External account interrogation service call
* HMAC authentication
* Database-backed business rules
* Employee discount path
* New account discount path
* Conditional branching
* Exclusion checks
* Discount calculation
* Final response mapping
* Centralized error handling

This API was useful for stress-testing whether Schema v1 could represent complex business orchestration.

---

## Validation API 2 — Customize IT Fabric API

```text
POST /api/v1/fabrics
```

This API was selected because it represents a simpler wrapper-style workflow.

It includes:

* Header validation
* Request body validation
* Array validation
* Optional trace header pass-through
* Downstream request mapping
* External REST service call
* Success response mapping
* Error response mapping

This API was useful for validating whether Schema v1 could represent simple service wrapper workflows without forcing unnecessary complexity.

---

## What Day 9 Proved

Day 9 proved that Schema v1 is directionally correct.

Schema v1 can represent the main structure of both complex and simple API workflows.

It can describe:

* API metadata
* Request validation
* Workflow steps
* Step sequencing
* External services
* Database references
* Business rules
* Response mapping
* Error behavior

This confirms that the overall approach is valid.

The idea of converting requirements into a workflow representation is practical and useful.

---

## Why Schema v2 Is Needed

Although Schema v1 worked as a foundation, Day 9 revealed that it is not detailed enough for reliable artifact generation.

Schema v1 can describe a workflow, but future generators need more precision.

A generator cannot depend only on high-level descriptions. It needs structured information such as:

* Exact branch conditions
* True and false paths
* External service authentication rules
* Query inputs and outputs
* Standard error mappings
* Calculation rules
* Field-level validations
* Test scenarios
* Source traceability
* Generator readiness status

Without these details, future generated artifacts may become incomplete, inconsistent, or difficult to evaluate.

Schema v2 is needed to make the workflow representation stronger, more explicit, and more useful for generation.

---

## Main Gaps Found in Schema v1

Day 9 identified the following major gaps.

---

## Gap 1 — Branching Was Not Formal Enough

Schema v1 could describe step dependencies, but it did not provide a strong structure for conditional branching.

The Discount API requires branching between employee discount and new account discount paths.

The Fabric API requires branching between success response and error response paths.

Schema v2 must include a formal branching block with:

* Branch ID
* Branch condition
* True path
* False path
* Merge step
* Branch type

---

## Gap 2 — Error Mapping Was Not Standardized

Schema v1 allowed each step to define failure behavior.

However, this can lead to inconsistent error handling across workflows.

Schema v2 must define reusable error mapping structures for:

* Validation errors
* External service errors
* Database errors
* Business rule failures
* Calculation errors
* Mapping errors
* Unexpected system errors

Each error mapping should include:

* Error source
* Error condition
* Target HTTP status
* Target error code
* Target message
* Retryable flag
* Fail-fast behavior

---

## Gap 3 — External Service Authentication Needed More Detail

The Discount API uses HMAC authentication for the downstream account interrogation service.

Schema v1 could describe the external service, but not enough authentication metadata.

Schema v2 must support authentication details such as:

* Authentication type
* Required headers
* API key source
* Signature algorithm
* Signature encoding
* Signature payload source
* Secret source
* Authorization header format

This is important for future Node-RED and Go client generation.

---

## Gap 4 — Query References Needed Stronger Linking

The Discount API uses multiple database queries for eligibility, profile lookup, exclusions, and configuration.

Schema v1 allowed query references, but it did not strongly connect:

* Workflow step
* Business rule
* Query ID
* Query inputs
* Query outputs
* No-record behavior
* Failure behavior

Schema v2 must make these relationships explicit.

---

## Gap 5 — Calculation Rules Needed Structure

The Discount API includes discount calculations.

Schema v1 could mark a step as a calculation step, but it did not structure the actual calculation rules.

Schema v2 must support:

* Calculation rule ID
* Calculation name
* Applies-when condition
* Inputs
* Outputs
* Calculation behavior
* Cap behavior
* Item-level behavior
* Failure behavior

---

## Gap 6 — Field Validation Needed More Precision

The sample APIs showed the need for stronger field validation.

Examples include:

* Nested body fields
* Required objects
* Required arrays
* Non-empty arrays
* Array item validation
* Enum validation
* Numeric validation
* Optional field handling

Schema v2 must include field-level validation metadata using clear field paths.

---

## Gap 7 — Optional Pass-Through Mapping Needed Explicit Support

The Fabric API includes an optional `Trace-Id` header.

The behavior is:

```text
If Trace-Id is present, pass it downstream.
If Trace-Id is missing, continue without failure.
```

Schema v1 could list optional headers, but it did not clearly describe optional pass-through mapping.

Schema v2 must support:

* Optional source fields
* Target mapping
* Pass-through when present
* Omit when missing

---

## Gap 8 — Test Scenario Metadata Was Missing

Future test generation requires structured test scenario data.

Schema v1 did not include a strong test scenario block.

Schema v2 must support:

* Scenario ID
* Scenario name
* Scenario type
* Input request
* Mock external service responses
* Mock database responses
* Expected HTTP status
* Expected response body
* Expected executed workflow path

This will be important for future evaluation scripts and research metrics.

---

## Gap 9 — Source Traceability Needed Improvement

The project is intended to support research and measurable evaluation.

For that, every generated artifact should trace back to source requirements.

Schema v1 did not strongly require traceability.

Schema v2 must support source traceability at multiple levels:

* Workflow level
* Step level
* Field level
* Business rule level
* Query level
* External service level
* Error mapping level
* Response mapping level

---

## Gap 10 — Generator Readiness Needed to Be Explicit

The workflow schema is intended to support multiple generators.

Each generator needs different information.

Schema v1 did not clearly show whether a workflow was ready for:

* Node-RED generation
* Excel intake generation
* Go code generation
* Documentation generation
* Test case generation
* Evaluation scripts

Schema v2 must include a generator readiness section to identify readiness status and missing fields.

---

## Objective of Schema v2

The objective of Workflow Schema v2 is to transform Schema v1 from a descriptive workflow format into a stronger generation-ready workflow contract.

Schema v2 should make the workflow representation:

* More structured
* More explicit
* More traceable
* More testable
* More generator-friendly
* More reusable across API types

Schema v2 does not need to generate code yet.

Its purpose is to define the improved structure that future generators can consume.

---

## What Schema v2 Will Improve

Schema v2 will improve the workflow representation by adding formal sections for:

* Field validations
* Conditional branching
* External service authentication
* Database query references
* Business rules
* Calculation rules
* Request mappings
* Response mappings
* Error mappings
* Test scenarios
* Source traceability
* Generator readiness

These improvements directly address the gaps found during Day 9 validation.

---

## Schema v2 Design Principles

Schema v2 should follow these principles.

---

## Principle 1 — Generator-Ready Structure

The schema should be structured enough for future tools to consume.

Natural language descriptions are still useful, but important execution details should be represented in fields.

Example:

Instead of only writing:

```text
Call downstream service with HMAC authentication.
```

Schema v2 should represent:

```yaml
authentication:
  type: HMAC
  algorithm: HMAC-SHA256
  encoding: base64
  header_name: Authorization
  format: MAC <signature>
```

---

## Principle 2 — Human-Readable and Machine-Readable

The schema should be understandable by humans and usable by generators.

It should include:

* Clear IDs
* Clear names
* Descriptions
* Structured fields
* Explicit references
* Consistent naming

This makes the schema useful for both documentation and automation.

---

## Principle 3 — Support Complex and Simple APIs

Schema v2 should support both:

* Complex business orchestration APIs
* Simple wrapper APIs

The Discount API should not be too complex for the schema.

The Fabric API should not be forced to include irrelevant database or calculation sections.

Schema v2 should allow optional sections when they are not applicable.

---

## Principle 4 — Strong Cross-References

Schema v2 should make relationships explicit.

For example:

* A workflow step should reference business rule IDs.
* A business rule should reference query IDs when applicable.
* A response mapping should reference workflow outputs.
* A test scenario should reference expected executed steps.
* A schema field should trace back to a source requirement.

This improves consistency and evaluation.

---

## Principle 5 — Support Evaluation and Research Metrics

This project has a research goal.

Schema v2 should support future evaluation of:

* Requirement extraction accuracy
* Workflow generation accuracy
* Excel generation completeness
* Gap detection accuracy
* Time reduction
* Artifact consistency

To support this, Schema v2 should include source traceability and generator readiness metadata.

---

## Expected Schema v2 Sections

Schema v2 should contain the following major sections:

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
  request_mapping:
  response_mapping:
  error_mappings:
  test_scenarios:
  source_traceability:
  generator_readiness:
```

These sections will be defined in detail in the next Day 10 file:

```text
docs/day10/workflow_schema_v2_blocks.md
```

---

## In Scope for Day 10

The following activities are in scope:

* Refining the schema design
* Documenting Schema v2 sections
* Comparing Schema v1 and Schema v2
* Creating a sample Schema v2 structure
* Explaining generator readiness
* Preparing a Day 10 completion report

---

## Out of Scope for Day 10

The following activities are not part of Day 10:

* Writing implementation code
* Creating a JSON schema file
* Creating a YAML parser
* Generating Node-RED flows
* Generating Excel files
* Generating Go code
* Running automated tests
* Calling external services
* Connecting to databases

Day 10 remains a documentation and design day.

---

## Success Criteria for Schema v2 Overview

This overview is complete when it clearly explains:

* What Schema v1 was
* Why Schema v1 mattered
* What Day 9 validated
* What gaps were found
* Why Schema v2 is needed
* What Schema v2 will improve
* What principles Schema v2 should follow
* What sections Schema v2 should contain

---

## Final Summary

Workflow Schema v2 is the next evolution of the requirement-to-workflow representation layer.

Schema v1 proved that API workflows can be represented as structured steps.

Day 9 proved that the structure is useful, but not yet detailed enough for reliable artifact generation.

Schema v2 will improve the schema by adding formal structures for branching, errors, authentication, query references, calculations, field validations, test scenarios, traceability, and generator readiness.

This creates a stronger foundation for future days of the 50-Day AI Software Engineer Challenge.
