# Day 14 — Phase 3: Benchmark Dataset Plan

## Purpose

This document defines the benchmark dataset plan for evaluating the Gap Analysis Framework.

The purpose of the benchmark dataset is to provide controlled examples of enterprise API artifacts that can be used to test whether the framework correctly detects gaps between:

1. Confluence requirement documents
2. Node-RED workflow implementations
3. Excel intake workbooks

The benchmark dataset will be used later to measure gap detection accuracy, false positives, false negatives, artifact completeness, and code generation readiness.

---

## Why a Benchmark Dataset Is Needed

The Gap Analysis Framework needs a reliable way to evaluate its output.

If the framework is only tested on random examples, it will be difficult to know whether the detected gaps are correct.

A benchmark dataset solves this problem by creating known test cases where the expected gaps are already documented.

This allows the project to compare:

```text id="9c7pny"
Expected gaps from manual review
        vs
Detected gaps from the framework
```

The benchmark dataset helps answer:

```text id="jgwg3a"
Did the framework find the correct gaps?
Did it miss any known gaps?
Did it report false gaps?
Did it assign the right severity?
Did it provide useful recommendations?
```

---

## Benchmark Dataset Goal

The goal of the benchmark dataset is to create a small but realistic collection of API examples.

Each benchmark example should contain:

```text id="qkt3ne"
A Confluence-style requirement document
A Node-RED workflow representation
An Excel intake workbook representation
A manually prepared expected gap report
```

The expected gap report acts as the ground truth for evaluation.

---

## Dataset Structure

The benchmark dataset should be organized by API example.

Recommended folder structure:

```text id="fvq3oz"
benchmarks/
├── api_001_interrogate_account/
│   ├── confluence_requirement.md
│   ├── node_red_flow.json
│   ├── excel_intake_summary.md
│   ├── expected_gaps.json
│   └── benchmark_notes.md
│
├── api_002_evaluate_discount/
│   ├── confluence_requirement.md
│   ├── node_red_flow.json
│   ├── excel_intake_summary.md
│   ├── expected_gaps.json
│   └── benchmark_notes.md
│
├── api_003_fabrics_wrapper/
│   ├── confluence_requirement.md
│   ├── node_red_flow.json
│   ├── excel_intake_summary.md
│   ├── expected_gaps.json
│   └── benchmark_notes.md
│
└── dataset_index.md
```

For Day 14, this is only a design plan.

The actual benchmark files can be created later.

---

## Benchmark Artifact Types

Each benchmark example should contain four main artifact types.

---

# 1. Confluence Requirement Artifact

## Purpose

The Confluence requirement artifact represents the original source requirement.

It should be written in a structured Markdown format that simulates an enterprise Confluence page.

## Contents

Each Confluence-style artifact should include:

```text id="v9dpq9"
API overview
Endpoint path
HTTP method
Required headers
Optional headers
Request body fields
Response body fields
Validation rules
Enum values
Business rules
Downstream service dependencies
Database dependencies
Error handling expectations
Technology assumptions
```

## Example Sections

```text id="qevgpp"
# API Requirement

## Endpoint
POST /api/v1/discount/interrogateAccount

## Required Headers
- requestorInfo-clientId
- requestorInfo-subclientId
- x-macys-apikey

## Request Body
- account.encryptInfo.encryptedData
- account.encryptInfo.keyType
- account.entryMode

## Business Rules
- If keyType is RSA, entryMode is required.
- Invoke PIIDLookup downstream service.

## Response
- accountInfo.internalAccount
- accountInfo.internalAccountToken
- accountInfo.lookupType
- accountInfo.rc
```

---

# 2. Node-RED Workflow Artifact

## Purpose

The Node-RED workflow artifact represents the workflow implementation or prototype.

It should show how the API requirement is modeled as a sequence of steps.

## Contents

Each Node-RED artifact should include:

```text id="cys4ao"
HTTP In node
Validation nodes
Mapping nodes
Business rule nodes
REST or SOAP wrapper nodes
Database lookup nodes
Response mapping nodes
HTTP Response node
Error handling nodes
```

## Representation Format

For later implementation, the workflow can be stored as actual Node-RED JSON.

For early documentation and evaluation design, it can also be summarized as Markdown.

Example summary:

```text id="sp4xsl"
HTTP In
  ↓
Validate Headers
  ↓
Validate Body
  ↓
Validate Enum Fields
  ↓
Build PIIDLookup Request
  ↓
Invoke PIIDLookup
  ↓
Map Account Response
  ↓
HTTP Response
```

---

# 3. Excel Intake Workbook Artifact

## Purpose

The Excel intake workbook artifact represents the structured input used by the Go code generation factory.

For benchmark planning, the workbook can first be represented as a Markdown summary.

Later, it can be converted into actual `.xlsx` files.

## Expected Sheets

Each benchmark Excel intake artifact should include summaries for:

```text id="g2qp73"
technical_requirements
api_endpoints
entities
external_services
business_requirements
endpoint_service_mapping
queries
```

## Example Workbook Summary

```text id="2488wq"
technical_requirements:
- language: Go
- database: PostgreSQL

api_endpoints:
- endpoint_id: EP-001
- method: POST
- path: /api/v1/discount/interrogateAccount
- required_headers: requestorInfo-clientId, requestorInfo-subclientId
- request_fields: account.encryptInfo.encryptedData, account.encryptInfo.keyType
- validation_rules: keyType is required

external_services:
- service_id: SRV-REST-PIIDLOOKUP
- type: REST
- method: POST
```

This summary intentionally may contain missing or incorrect information so the gap analysis framework can detect it.

---

# 4. Expected Gap Report Artifact

## Purpose

The expected gap report is the manually prepared ground truth.

It defines the gaps that should be detected for each benchmark example.

## Contents

Each expected gap report should include:

```text id="x8iztd"
gap_id
gap_type
gap_category_group
source_of_truth
compared_source
missing_from
affected_artifact
affected_section
affected_field
severity
description
recommendation
```

## Example

```json id="iqc4fl"
{
  "gap_id": "GAP-001",
  "gap_type": "Missing Header Gap",
  "gap_category_group": "Contract Gap",
  "source_of_truth": "Confluence",
  "compared_source": "Excel",
  "missing_from": "Excel",
  "affected_artifact": "api_endpoints",
  "affected_section": "required_headers",
  "affected_field": "x-macys-apikey",
  "severity": "High",
  "description": "The required header x-macys-apikey exists in Confluence but is missing from the Excel api_endpoints sheet.",
  "recommendation": "Add x-macys-apikey to the required_headers field in api_endpoints."
}
```

---

## Benchmark API Examples

The initial benchmark dataset should contain realistic API examples from the project domain.

---

# Benchmark API 001 — Interrogate Account

## Purpose

This benchmark tests API contract extraction, header validation, request body validation, enum validation, downstream service mapping, and response mapping.

## Source Requirement Focus

```text id="pl2slr"
POST /api/v1/discount/interrogateAccount
```

## Expected Elements

```text id="5kglfu"
Required headers
Request body fields
keyType enum values
entryMode validation
PIIDLookup downstream service
HMAC downstream header behavior
accountInfo response mapping
```

## Planned Gap Types

This benchmark can include gaps such as:

```text id="dyc5x5"
Missing Header Gap
Missing Request Field Gap
Missing Enum Validation Gap
Missing Downstream Service Gap
Incorrect Response Mapping Gap
Error Handling Gap
```

---

# Benchmark API 002 — Evaluate Transaction Discount

## Purpose

This benchmark tests more complex business logic and database-related gap detection.

## Source Requirement Focus

```text id="nlxo1k"
POST /api/v1/discount/evaluateTransactionDiscount
```

## Expected Elements

```text id="5v92az"
Employee account discount logic
New account discount logic
Home shop vs cross-shop logic
Discount profile lookup
Exclusion lookup
Additional discount lookup
Additional exclusion lookup
CAP calculation
PostgreSQL query references
Final discount response mapping
```

## Planned Gap Types

This benchmark can include gaps such as:

```text id="1d9i0e"
Missing Business Rule Gap
Missing Database Query Gap
Missing Entity Gap
Workflow Step Gap
Technology Mismatch Gap
Incorrect Field Mapping Gap
```

---

# Benchmark API 003 — Fabrics Wrapper

## Purpose

This benchmark tests REST wrapper mapping, request validation, downstream service behavior, and simple response transformation.

## Source Requirement Focus

```text id="sp7mmb"
POST /api/v1/fabrics
```

## Expected Elements

```text id="t9zhzr"
skuNumbers list validation
fabricTypeId validation
Trace-Id optional header
Division-Id allowed values
Downstream fabrics service call
Error response mapping
```

## Planned Gap Types

This benchmark can include gaps such as:

```text id="de2ei1"
Missing Validation Gap
Incorrect Required or Optional Status Gap
Missing Downstream Service Gap
Incorrect Service Mapping Gap
Incorrect Response Mapping Gap
```

---

# Benchmark API 004 — Clienteling Create Message

## Purpose

This benchmark tests API creation logic, header validation, body validation, database insert mapping, and overlap validation.

## Source Requirement Focus

```text id="nxuo4d"
POST /api/v1/clienteling/messages
```

## Expected Elements

```text id="cmyj7k"
Required headers
X-Acting-As-Type validation
messageSeverity enum validation
targetRoles enum validation
APP_MSGS insert query
Active message overlap check
Response id mapping
```

## Planned Gap Types

This benchmark can include gaps such as:

```text id="uk72dg"
Missing Header Gap
Missing Enum Validation Gap
Missing Database Query Gap
Missing Business Rule Gap
Incorrect Data Type Gap
```

---

## Dataset Difficulty Levels

The benchmark dataset should include different difficulty levels.

---

## Level 1: Simple Contract Gaps

These examples test basic API contract checks.

Examples:

```text id="radb6s"
Missing required header
Missing request field
Missing response field
Wrong HTTP method
Wrong endpoint path
```

Purpose:

```text id="7s8d95"
Verify that the framework can detect straightforward contract gaps.
```

---

## Level 2: Validation and Mapping Gaps

These examples test validation and transformation logic.

Examples:

```text id="yzj2fx"
Missing enum validation
Wrong required or optional status
Incorrect field mapping
Incorrect response mapping
Missing error handling rule
```

Purpose:

```text id="tb8smz"
Verify that the framework can detect logic-level mismatches.
```

---

## Level 3: Business Logic and Integration Gaps

These examples test complex API behavior.

Examples:

```text id="3imcdv"
Missing business rule
Missing downstream service
Missing database query
Missing workflow step
Technology mismatch
```

Purpose:

```text id="7vc0df"
Verify that the framework can detect deeper implementation gaps.
```

---

## Level 4: Ambiguous or Unsupported Assumptions

These examples test uncertainty handling.

Examples:

```text id="vylbv3"
Excel adds a service not present in Confluence
Node-RED adds a default value not documented in the requirement
Confluence wording is unclear
Optional field is treated as required
```

Purpose:

```text id="umvwh2"
Verify that the framework can report assumptions and ambiguous cases without overclaiming.
```

---

## Ground Truth Creation Process

The ground truth is the manually prepared expected gap report for each benchmark.

The process should be:

```text id="q8z814"
Step 1: Read the Confluence-style requirement.
Step 2: Read the Node-RED workflow artifact.
Step 3: Read the Excel intake artifact.
Step 4: Manually identify all known gaps.
Step 5: Assign gap type and severity.
Step 6: Add evidence and recommendation.
Step 7: Store the expected gaps in expected_gaps.json.
```

The expected gap report will be used to evaluate framework output.

---

## Benchmark Evaluation Process

For each benchmark API example, the evaluation process should be:

```text id="hroopy"
Step 1: Run the Gap Analysis Framework on the benchmark artifacts.
Step 2: Generate detected_gaps.json.
Step 3: Compare detected_gaps.json with expected_gaps.json.
Step 4: Count true positives, false positives, and false negatives.
Step 5: Calculate evaluation metrics.
Step 6: Review severity assignment accuracy.
Step 7: Review recommendation usefulness.
Step 8: Store the evaluation result.
```

---

## Expected Evaluation Outputs

Each benchmark example should eventually produce:

```text id="p3580h"
detected_gaps.json
evaluation_result.json
evaluation_summary.md
```

Example result:

```json id="26kir6"
{
  "benchmark_id": "api_001_interrogate_account",
  "expected_gaps": 8,
  "detected_gaps": 9,
  "true_positives": 7,
  "false_positives": 2,
  "false_negatives": 1,
  "gap_detection_accuracy": "87.5%",
  "false_positive_rate": "22.2%",
  "false_negative_rate": "12.5%",
  "severity_assignment_accuracy": "85.7%",
  "recommendation_usefulness_score": "4.2/5"
}
```

---

## Dataset Index

The benchmark dataset should include an index file.

Recommended file:

```text id="kd2ap8"
benchmarks/dataset_index.md
```

The index should track:

```text id="shsvdi"
Benchmark ID
API name
Endpoint
Difficulty level
Artifact types included
Expected gap count
Primary gap categories
Evaluation status
```

Example table:

| Benchmark ID | API Name            | Endpoint                                          | Difficulty | Expected Gaps | Status  |
| ------------ | ------------------- | ------------------------------------------------- | ---------- | ------------- | ------- |
| API-001      | Interrogate Account | POST /api/v1/discount/interrogateAccount          | Level 2    | 8             | Planned |
| API-002      | Evaluate Discount   | POST /api/v1/discount/evaluateTransactionDiscount | Level 3    | 12            | Planned |
| API-003      | Fabrics Wrapper     | POST /api/v1/fabrics                              | Level 2    | 7             | Planned |
| API-004      | Create Message      | POST /api/v1/clienteling/messages                 | Level 3    | 10            | Planned |

---

## Dataset Quality Rules

To make the benchmark useful, each example should follow these rules.

---

## Rule 1: Each Benchmark Must Have Known Gaps

Every benchmark should include intentional gaps.

The goal is not to create perfect artifacts.

The goal is to test whether the framework can detect known issues.

---

## Rule 2: Gaps Must Be Traceable

Every expected gap must be traceable to evidence from at least one source.

Example:

```text id="x0a7kn"
Confluence says x-macys-apikey is required.
Excel does not include x-macys-apikey.
```

This makes the gap clear and reviewable.

---

## Rule 3: Include Both Simple and Complex Gaps

The dataset should include both easy and difficult examples.

Simple gaps test baseline functionality.

Complex gaps test deeper reasoning.

---

## Rule 4: Include False Positive Traps

Some artifacts should contain differences that are not real gaps.

Example:

```text id="r62g4h"
Confluence uses Division-id.
Excel uses division_id.
A mapping note says both refer to the same field.
```

This tests whether the framework avoids reporting unnecessary gaps.

---

## Rule 5: Include Ambiguous Cases

Some examples should include unclear requirements.

The framework should mark these as informational or low-confidence findings instead of making unsupported claims.

---

## Rule 6: Keep the Dataset Small at First

The first benchmark dataset should be small and focused.

Recommended initial size:

```text id="uu2rcj"
4 API examples
30 to 40 total expected gaps
```

This is enough to test the framework without making the evaluation too large.

---

## Future Expansion Plan

After the initial dataset is created, it can be expanded.

Future benchmark examples may include:

```text id="ob2rlm"
SOAP wrapper APIs
Database-heavy APIs
Multi-endpoint services
Authentication-heavy APIs
Event-driven APIs
Kafka or Pub/Sub APIs
Batch processing APIs
```

Future artifacts may also include:

```text id="o65x8j"
Generated Go code
OpenAPI specifications
Postman collections
Test case files
CI/CD validation reports
```

This will allow the framework to grow into a broader enterprise API quality evaluation system.

---

## Benchmark Dataset Success Criteria

The benchmark dataset is successful if it supports the following:

```text id="eb78eu"
Controlled evaluation
Known expected gaps
Repeatable testing
Metric calculation
Manual vs AI comparison
Artifact quality scoring
Research-ready evidence
```

A strong benchmark dataset should make it possible to evaluate the framework consistently across multiple APIs.

---

## Summary

The benchmark dataset plan defines how future evaluation examples will be organized.

Each benchmark should include a Confluence-style requirement, Node-RED workflow artifact, Excel intake summary, and manually prepared expected gap report.

The dataset will help measure gap detection accuracy, false positives, false negatives, severity assignment accuracy, recommendation usefulness, and code generation readiness.

This benchmark plan prepares the project for later implementation and supports the larger research goal of building measurable AI-assisted enterprise API development workflows.
