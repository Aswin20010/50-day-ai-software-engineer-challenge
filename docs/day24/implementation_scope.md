# Day 24 — Phase 1: Implementation Scope Definition

## Purpose

This document defines the implementation scope for Day 25 of the 50-Day AI Software Engineer Challenge.

Day 24 is the final planning and readiness day before coding begins. The purpose of this phase is to clearly define what will be implemented first, what will not be implemented yet, and how the first coding phase will connect to the overall project goal.

The project has already completed multiple planning, architecture, schema, prompt-design, and documentation phases. Starting from Day 25, the focus shifts from conceptual planning to building the first working version of the system.

---

## Project Context

The overall goal of this challenge is to build an AI-assisted system that can transform enterprise API requirements into structured engineering artifacts.

The long-term system should support the following flow:

```text
Confluence Requirement
        ↓
Requirement Extraction
        ↓
Structured Workflow Representation
        ↓
Excel Intake Sheet Generation
        ↓
Gap Analysis
        ↓
Factory LLM Code Generation
        ↓
Generated API Prototype
```

The system is intended to reduce manual effort in enterprise API development by standardizing how requirements are captured, validated, transformed, and prepared for implementation.

---

## Day 25 Implementation Goal

The first implementation milestone will focus on building the foundation for the requirement extraction layer.

Day 25 should begin with a small but working implementation that can:

1. Accept a sample API requirement document
2. Extract important requirement details
3. Convert those details into a structured format
4. Save the extracted output for future workflow and Excel generation phases

Day 25 is not expected to build the full end-to-end system. It should create the first working foundation that later days can improve.

---

## Day 25 Primary Scope

The primary implementation scope for Day 25 is:

```text
Build the first version of the requirement extraction pipeline.
```

This means the system should take a requirement input and produce a structured requirement extraction output.

The first implementation should focus on correctness, clarity, and extensibility rather than advanced automation.

---

## Inputs for Day 25

The Day 25 implementation should accept a sample API requirement in markdown format.

Example input type:

```text
datasets/confluence_requirements/REQ-001-feed-discount.md
```

The input may contain details such as:

* API name
* Endpoint path
* HTTP method
* Request headers
* Request body
* Response body
* Business rules
* Validation rules
* Downstream systems
* Database tables
* Error scenarios
* Assumptions
* Open questions

The system should not assume that every section will always be present.

---

## Outputs for Day 25

The Day 25 implementation should generate a structured extraction output.

Recommended output format:

```text
outputs/day25/extracted_requirements/REQ-001-extraction.json
```

The output should contain structured fields such as:

```json
{
  "requirement_id": "REQ-001",
  "api_name": "",
  "endpoint": "",
  "method": "",
  "description": "",
  "request_headers": [],
  "request_body_fields": [],
  "response_fields": [],
  "business_rules": [],
  "validation_rules": [],
  "downstream_dependencies": [],
  "database_dependencies": [],
  "error_scenarios": [],
  "assumptions": [],
  "open_questions": []
}
```

This output will later be used by workflow generation, Excel generation, and gap analysis modules.

---

## Initial Folder Scope

Day 25 should introduce only the minimum folder structure required for implementation.

Recommended structure:

```text
src/
  requirement_extraction/
    __init__.py
    extractor.py

datasets/
  confluence_requirements/
    REQ-001-feed-discount.md

outputs/
  day25/
    extracted_requirements/

docs/
  day25/
    day25_notes.md
```

The folder structure should remain simple because the goal is to create a working foundation first.

---

## Components to Build on Day 25

### 1. Requirement Input Loader

The input loader should read a markdown requirement file from the dataset folder.

Responsibilities:

* Accept the file path
* Read the requirement content
* Return the raw text
* Handle missing file errors clearly

---

### 2. Basic Requirement Extractor

The extractor should identify and organize important information from the requirement text.

Responsibilities:

* Extract API-level metadata
* Identify endpoint and method if present
* Capture request fields
* Capture response fields
* Capture business rules
* Capture validations
* Capture downstream dependencies
* Capture database references
* Capture errors, assumptions, and open questions

The first version can be rule-based and simple. It does not need to be perfect.

---

### 3. Structured JSON Output Writer

The output writer should save extracted information into a JSON file.

Responsibilities:

* Create output directory if it does not exist
* Save extracted data as formatted JSON
* Preserve the requirement ID in the filename
* Make the output easy for future modules to consume

---

### 4. Basic Test Run

Day 25 should include a basic manual test run using one sample requirement file.

The goal is to confirm:

* Input file can be read
* Extraction runs without failure
* Output JSON is created
* Output follows the expected schema

---

## What Day 25 Should Not Include

Day 25 should not attempt to build the full system.

The following items are out of scope for Day 25:

* Full LLM integration
* Full Excel generation
* Full Node-RED flow generation
* Full Go code generation
* Complete gap analysis engine
* Production-level parsing
* Database integration
* UI or frontend
* Multiple API support
* Advanced prompt orchestration
* Automated evaluation scoring

These will be handled in later days.

---

## Implementation Style

The implementation should follow a simple and readable style.

Recommended principles:

* Keep functions small
* Use clear file names
* Avoid over-engineering
* Prefer readable logic over complex abstraction
* Make the output easy to inspect manually
* Add comments only where useful
* Keep the code flexible for future LLM integration

---

## Assumptions

The following assumptions are allowed for Day 25:

1. The input requirement is available as a markdown file.
2. The first extractor can be rule-based.
3. The output format can be JSON.
4. The first version does not need perfect extraction accuracy.
5. Missing fields can be returned as empty strings or empty arrays.
6. One sample requirement is enough for the first implementation test.

---

## Non-Assumptions

The implementation should not assume:

1. Every requirement document has the same structure.
2. All endpoints are clearly labeled.
3. All request and response fields are explicitly formatted.
4. All business rules are complete.
5. Downstream systems are always mentioned.
6. Database tables are always present.
7. The requirement document is production-ready.

Any missing or unclear information should be captured under `open_questions`.

---

## Success Criteria for Day 25

Day 25 will be considered successful if:

* A sample requirement markdown file can be loaded
* A structured extraction JSON file is generated
* The JSON follows the expected schema
* Important requirement sections are captured where available
* Missing information is handled safely
* The implementation can be extended in later days

---

## Connection to Future Days

The Day 25 output will become the input for later phases.

Future usage:

```text
Day 25 Requirement Extraction Output
        ↓
Workflow Representation Generator
        ↓
Excel Intake Sheet Generator
        ↓
Gap Analysis Module
        ↓
Factory LLM Code Generation
```

This means the Day 25 implementation should focus heavily on producing a clean, structured, reusable output.

---

## Final Scope Statement

Day 25 will implement the first working version of the requirement extraction pipeline.

The implementation will read one sample markdown requirement, extract key API requirement details, and save the result as structured JSON.

The purpose is not to complete the full automation system in one day. The purpose is to create the first reliable implementation foundation that future days can build on.

---
