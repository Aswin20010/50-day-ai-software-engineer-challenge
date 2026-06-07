# Day 7 — Workflow Representation Specification

## Purpose

This document defines the workflow representation layer for the 50-Day AI Software Engineer Challenge.

The goal of this layer is to describe how extracted requirements can be converted into a structured sequence of workflow steps before actual code generation or Node-RED flow generation begins.

This work is part of the planning and design phase of the project. No implementation or coding is performed on Day 7.

---

## Background

In enterprise API development, requirement documents often describe behavior in natural language.

Examples include:

* Validate required headers
* Validate request body fields
* Check business rules
* Call downstream services
* Query databases
* Transform request payloads
* Map downstream responses
* Return success or error responses

Before these requirements can be converted into code or Node-RED flows, they must first be represented in a structured intermediate format.

This intermediate format acts as a bridge between requirement extraction and implementation.

---

## Role of Workflow Representation

The workflow representation defines the logical execution flow of an API.

It answers the following questions:

1. What is the API entry point?
2. What validations must happen first?
3. What business rules must be checked?
4. What data transformations are required?
5. What external services must be called?
6. What database operations are required?
7. How should success responses be mapped?
8. How should error responses be handled?
9. What is the final response returned to the client?

---

## Why Workflow Representation Is Needed

Directly generating code from requirement documents can be difficult because natural language is often incomplete, inconsistent, or ambiguous.

A workflow representation provides a structured planning layer.

It helps the system:

* Separate requirement understanding from code generation
* Identify missing or unclear steps
* Preserve execution order
* Make API logic easier to inspect
* Support both Node-RED flow generation and Go code generation
* Improve traceability from requirement text to generated artifacts

---

## Position in the Overall AI Pipeline

The workflow representation sits between requirement extraction and artifact generation.

```text
Requirement Document
        ↓
Requirement Extraction Agent
        ↓
Structured Requirement Schema
        ↓
Workflow Representation Layer
        ↓
Node-RED Flow / Excel Intake / Go Code Generation
```

---

## Workflow Representation Definition

A workflow representation is a structured list of ordered execution steps.

Each step represents one logical action in the API flow.

A step may represent:

* Receiving an HTTP request
* Validating headers
* Validating request body fields
* Applying enum checks
* Applying business rules
* Building a downstream request
* Calling an external service
* Executing a database query
* Mapping a response
* Handling errors
* Returning the final HTTP response

---

## Core Workflow Step Fields

Each workflow step should contain the following fields:

| Field                   | Description                                      |
| ----------------------- | ------------------------------------------------ |
| `step_id`               | Unique identifier for the workflow step          |
| `step_name`             | Human-readable name of the step                  |
| `node_type`             | Type of workflow node                            |
| `description`           | Explanation of what the step does                |
| `input_source`          | Source of input data                             |
| `output_target`         | Destination of output data                       |
| `execution_order`       | Position of the step in the flow                 |
| `requirement_reference` | Requirement or business rule linked to this step |
| `error_behavior`        | How errors should be handled                     |
| `mandatory`             | Whether the step is required                     |

---

## Example Workflow Step

```json
{
  "step_id": "STEP-002",
  "step_name": "Validate Required Headers",
  "node_type": "HEADER_VALIDATION",
  "description": "Validates that all mandatory request headers are present before processing the request.",
  "input_source": "HTTP request headers",
  "output_target": "Validated request context",
  "execution_order": 2,
  "requirement_reference": "BR-001",
  "error_behavior": "Return HTTP 400 when mandatory headers are missing",
  "mandatory": true
}
```

---

## Workflow Execution Principle

The workflow should follow a clear and predictable execution order.

A standard API workflow should generally follow this pattern:

```text
HTTP Input
    ↓
Header Validation
    ↓
Body Validation
    ↓
Enum Validation
    ↓
Request Transformation
    ↓
Business Rule Processing
    ↓
External Service Call / Database Query
    ↓
Response Mapping
    ↓
Error Handling
    ↓
HTTP Response
```

---

## Design Principles

The workflow representation should follow these principles.

### 1. Ordered

Each step should have a clear execution order.

The system should know which step comes before and after each node.

### 2. Traceable

Each workflow step should be linked back to a requirement, business rule, API endpoint, or service mapping.

### 3. Modular

Each step should perform one logical function.

For example, header validation and body validation should be separate workflow steps.

### 4. Technology-Neutral

The representation should not depend only on Node-RED.

It should be usable for:

* Node-RED flow generation
* Excel generation
* Go service generation
* Documentation generation

### 5. Human-Reviewable

The workflow representation should be easy for a developer, architect, or reviewer to inspect.

---

## Workflow Representation and Node-RED

Although the workflow representation is technology-neutral, it can later be mapped to Node-RED nodes.

Example mapping:

| Workflow Step          | Possible Node-RED Equivalent           |
| ---------------------- | -------------------------------------- |
| HTTP Input             | `http in` node                         |
| Header Validation      | `function` node                        |
| Body Validation        | `function` node                        |
| Request Transformation | `function` node                        |
| External Service Call  | Reusable subflow / `http request` node |
| Database Query         | Database node / `function` node        |
| Response Mapping       | `function` node                        |
| HTTP Response          | `http response` node                   |

---

## Workflow Representation and Go Code Generation

The same workflow representation can also help generate Go code.

Example mapping:

| Workflow Step         | Go Layer                   |
| --------------------- | -------------------------- |
| HTTP Input            | Handler route              |
| Header Validation     | Handler / validation layer |
| Body Validation       | DTO validation             |
| Business Rule         | Service layer              |
| External Service Call | Client layer               |
| Database Query        | Repository layer           |
| Response Mapping      | Mapper / service layer     |
| Error Handling        | Error package / middleware |

---

## Expected Output of This Layer

The workflow representation layer should produce a structured flow that can be consumed by later stages.

Example output:

```text
Endpoint: POST /api/v1/example

STEP-001: Receive HTTP Request
STEP-002: Validate Headers
STEP-003: Validate Body
STEP-004: Apply Business Rule
STEP-005: Build Downstream Request
STEP-006: Call External Service
STEP-007: Map Downstream Response
STEP-008: Return HTTP Response
```

---

## What Day 7 Does Not Include

Day 7 does not include:

* Building a parser
* Writing code
* Creating actual Node-RED flows
* Generating Go services
* Creating the final Excel intake file
* Calling an LLM
* Testing generated output

Those implementation activities will begin later in the challenge.

---

## Summary

The workflow representation layer is a critical design step in the AI Software Engineer pipeline.

It converts extracted requirements into an ordered, structured, and reviewable execution flow.

This makes the system easier to validate before generating Node-RED flows, Excel intake sheets, or Go code.
