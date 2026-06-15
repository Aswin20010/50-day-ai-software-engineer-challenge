# Day 15 — Phase 2: Experiment Cases

## Purpose

This document defines the experiment cases that will be used to evaluate the AI-assisted enterprise API artifact generation and gap analysis framework.

The purpose of defining experiment cases is to create a controlled set of API examples that can be used to measure:

* Requirement coverage
* Workflow completeness
* Excel intake completeness
* Gap detection accuracy
* False positive rate
* False negative rate
* Manual review reduction
* Code generation readiness

These experiment cases will later become the benchmark examples for evaluating the project.

---

## Why Experiment Cases Are Needed

The research experiment needs realistic API examples.

Without defined experiment cases, the evaluation would be too abstract.

Experiment cases help answer:

```text
Can the framework work on simple API contract examples?
Can the framework work on downstream service wrapper APIs?
Can the framework work on database-heavy business logic APIs?
Can the framework detect gaps across Confluence, Node-RED, and Excel?
Can the framework measure readiness for Go code generation?
```

Each experiment case should represent a different API pattern.

This makes the evaluation stronger because the framework is tested against multiple types of enterprise API requirements.

---

## Experiment Case Selection Criteria

Each experiment case should be selected based on the following criteria.

---

## 1. Realistic Enterprise API Pattern

The API should represent a realistic enterprise development scenario.

Examples include:

```text
Account lookup
Discount calculation
REST wrapper
Clienteling message creation
Database lookup
Downstream service integration
```

---

## 2. Multiple Artifact Types Available

Each case should support comparison across at least three artifacts:

```text
Confluence requirement
Node-RED workflow
Excel intake workbook
```

These artifacts are required for gap analysis.

---

## 3. Known Expected Gaps

Each case should contain known intentional gaps.

The goal is not to create perfect examples.

The goal is to test whether the framework can identify missing, incorrect, or inconsistent information.

---

## 4. Different Complexity Levels

The experiment cases should include both simple and complex APIs.

This helps evaluate whether the framework works across different difficulty levels.

---

## 5. Useful for Future Go Code Generation

Each case should be relevant to the final goal of preparing structured input for Go code generation.

This ensures that the experiment remains aligned with the project mission.

---

# Planned Experiment Cases

The initial experiment dataset will contain four API cases.

```text
API-001 — Interrogate Account API
API-002 — Evaluate Transaction Discount API
API-003 — Fabrics Wrapper API
API-004 — Clienteling Create Message API
```

Each case is described below.

---

# API-001 — Interrogate Account API

## Case ID

```text
API-001
```

## API Name

```text
Interrogate Account API
```

## Endpoint

```text
POST /api/v1/discount/interrogateAccount
```

## Purpose

This case evaluates whether the framework can detect gaps in an API that performs account interrogation by calling a downstream account lookup service.

The API receives encrypted account information, validates request headers and body fields, invokes a downstream PIIDLookup service, and maps the downstream response into a REST response.

---

## Why This Case Is Useful

This case is useful because it tests several common enterprise API concerns:

```text
Required request headers
Nested request body fields
Enum validation
Downstream REST service invocation
HMAC/security-related header behavior
Response mapping
Error handling
```

It is a good medium-complexity case for evaluating API contract and integration gaps.

---

## Source Artifacts

The case should include:

```text
confluence_requirement.md
node_red_flow.json
excel_intake_summary.md
expected_gaps.json
```

---

## Requirement Elements to Track

The framework should track elements such as:

```text
Endpoint path
HTTP method
requestorInfo-clientId header
requestorInfo-subclientId header
requestorInfo-traceId header
x-macys-apikey header
x-hmac-secret header
account.encryptInfo.encryptedData
account.encryptInfo.keyType
account.encryptInfo.keyName
account.encryptInfo.keyVersion
account.encryptInfo.division
account.encryptInfo.storeNbr
account.entryMode
keyType enum values
entryMode enum values
PIIDLookup downstream service
HMAC authorization behavior
accountInfo response mapping
```

---

## Planned Gap Types

This case can include the following gap types:

```text
Missing Header Gap
Missing Request Field Gap
Missing Enum Validation Gap
Missing Downstream Service Gap
Incorrect Field Mapping Gap
Incorrect Response Mapping Gap
Error Handling Gap
Incorrect Required or Optional Status Gap
```

---

## Example Intentional Gaps

The benchmark version of this case may intentionally include gaps such as:

```text
Excel missing x-macys-apikey in required headers
Excel missing account.entryMode in request body
Excel missing entryMode enum validation
Node-RED missing downstream error handling
Excel missing HMAC authorization mapping
Excel maps downstream return code to the wrong response field
```

---

## Expected Difficulty

```text
Level 2 — Validation and Mapping Gaps
```

This case is more complex than a simple CRUD endpoint but not as complex as the discount calculation API.

---

# API-002 — Evaluate Transaction Discount API

## Case ID

```text
API-002
```

## API Name

```text
Evaluate Transaction Discount API
```

## Endpoint

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

## Purpose

This case evaluates whether the framework can detect gaps in a business-rule-heavy API.

The API evaluates whether a transaction is eligible for employee or new account discounts. It may require account classification, discount profile lookup, exclusion checks, additional discount logic, cap calculation, and final response mapping.

---

## Why This Case Is Useful

This case is useful because it tests complex enterprise API behavior.

It includes:

```text
Nested request payloads
Multiple business decision paths
Employee discount rules
New account discount rules
Database lookups
Exclusion logic
Discount calculation
Cap calculation
Response mapping
```

This is the strongest case for evaluating business logic gap detection.

---

## Source Artifacts

The case should include:

```text
confluence_requirement.md
node_red_flow.json
excel_intake_summary.md
expected_gaps.json
```

---

## Requirement Elements to Track

The framework should track elements such as:

```text
Endpoint path
HTTP method
Transaction details
Account details
Item details
Employee account path
New account path
Home shop logic
Cross-shop logic
Discount profile lookup
Department/vendor/class exclusion lookup
Additional discount lookup
Additional exclusion lookup
New account cap amount
Discount percentage calculation
Discount amount calculation
Final response structure
PostgreSQL table references
Database query references
```

---

## Planned Gap Types

This case can include the following gap types:

```text
Missing Business Rule Gap
Missing Database Query Gap
Missing Entity Gap
Workflow Step Gap
Incorrect Field Mapping Gap
Incorrect Response Mapping Gap
Technology Mismatch Gap
Incorrect Data Type Gap
Error Handling Gap
```

---

## Example Intentional Gaps

The benchmark version of this case may intentionally include gaps such as:

```text
Excel missing employee discount business rule
Excel missing new account cap calculation rule
Excel missing database query for exclusion lookup
Excel mentions SQLite instead of PostgreSQL
Node-RED missing additional exclusion step
Excel missing endpoint_service_mapping sequence for discount calculation
Response mapping missing discountPercentApplied
```

---

## Expected Difficulty

```text
Level 3 — Business Logic and Integration Gaps
```

This is a complex case and should be used to test deeper reasoning and completeness.

---

# API-003 — Fabrics Wrapper API

## Case ID

```text
API-003
```

## API Name

```text
Fabrics Wrapper API
```

## Endpoint

```text
POST /api/v1/fabrics
```

## Purpose

This case evaluates whether the framework can detect gaps in a REST wrapper API.

The API receives a request from the client, validates fields and headers, maps the request into a downstream wrapper format, invokes the downstream fabrics service, and maps the response back to the client.

---

## Why This Case Is Useful

This case is useful because it is simpler than the discount API but still includes real integration and mapping behavior.

It tests:

```text
Request field validation
Header handling
REST downstream service mapping
Wrapper request transformation
Response transformation
Error response mapping
Optional trace header behavior
```

This makes it a good case for evaluating wrapper-style APIs.

---

## Source Artifacts

The case should include:

```text
confluence_requirement.md
node_red_flow.json
excel_intake_summary.md
expected_gaps.json
```

---

## Requirement Elements to Track

The framework should track elements such as:

```text
Endpoint path
HTTP method
Client-Id header
Subclient-Id header
Division-Id header
Store-Number header
Trace-Id header
skuNumbers list
fabricTypeId
customSkuNumbers downstream mapping
Downstream fabrics endpoint
Downstream response fields
Error response behavior
```

---

## Planned Gap Types

This case can include the following gap types:

```text
Missing Header Gap
Missing Validation Gap
Incorrect Field Mapping Gap
Incorrect Service Mapping Gap
Incorrect Response Mapping Gap
Incorrect Required or Optional Status Gap
Error Handling Gap
```

---

## Example Intentional Gaps

The benchmark version of this case may intentionally include gaps such as:

```text
Excel marks Trace-Id as required even though it is optional
Excel does not include skuNumbers non-empty validation
Excel maps skuNumbers directly instead of customSkuNumbers
Excel missing downstream service URL
Node-RED handles wrapper error response but Excel does not capture it
```

---

## Expected Difficulty

```text
Level 2 — Validation and Mapping Gaps
```

This case is useful for testing integration mapping without heavy database logic.

---

# API-004 — Clienteling Create Message API

## Case ID

```text
API-004
```

## API Name

```text
Clienteling Create Message API
```

## Endpoint

```text
POST /api/v1/clienteling/messages
```

## Purpose

This case evaluates whether the framework can detect gaps in an API that creates application messages and performs database validation before insert.

The API receives message details, validates headers and body fields, checks for overlapping active messages, inserts a new message into the database, and returns the created ID.

---

## Why This Case Is Useful

This case is useful because it combines API contract validation with database query behavior.

It tests:

```text
Required headers
Header value validation
Request body validation
Enum validation
Database overlap check
Database insert query
Response ID mapping
Business rule representation
```

This makes it a strong case for validating Excel intake readiness for generated Go repository and service layers.

---

## Source Artifacts

The case should include:

```text
confluence_requirement.md
node_red_flow.json
excel_intake_summary.md
expected_gaps.json
```

---

## Requirement Elements to Track

The framework should track elements such as:

```text
Endpoint path
HTTP method
Client-id header
Subclient-id header
Division-id header
Store-number header
Trace-id header
Associateid header
X-Acting-As-Id header
X-Acting-As-Type header
X-Acting-As-Type must equal global
message field
title field
messageSeverity enum
startDateTime
endDateTime
targetRoles enum
APP_MSGS insert query
Active message overlap check
Response id mapping
```

---

## Planned Gap Types

This case can include the following gap types:

```text
Missing Header Gap
Missing Request Field Gap
Missing Enum Validation Gap
Missing Business Rule Gap
Missing Database Query Gap
Incorrect Data Type Gap
Incorrect Response Mapping Gap
Workflow Step Gap
```

---

## Example Intentional Gaps

The benchmark version of this case may intentionally include gaps such as:

```text
Excel missing X-Acting-As-Type validation
Excel missing messageSeverity enum values
Excel missing active message overlap business rule
Excel missing APP_MSGS insert query
Node-RED includes overlap validation but Excel does not
Excel response mapping missing created id
```

---

## Expected Difficulty

```text
Level 3 — Business Logic and Integration Gaps
```

This case is useful for testing database and validation completeness.

---

# Experiment Case Summary Table

| Case ID | API Name                          | Endpoint                                          | Main Focus                                           | Difficulty |
| ------- | --------------------------------- | ------------------------------------------------- | ---------------------------------------------------- | ---------- |
| API-001 | Interrogate Account API           | POST /api/v1/discount/interrogateAccount          | Headers, enums, downstream service, response mapping | Level 2    |
| API-002 | Evaluate Transaction Discount API | POST /api/v1/discount/evaluateTransactionDiscount | Business rules, DB queries, discount logic           | Level 3    |
| API-003 | Fabrics Wrapper API               | POST /api/v1/fabrics                              | REST wrapper, request mapping, validation            | Level 2    |
| API-004 | Clienteling Create Message API    | POST /api/v1/clienteling/messages                 | Validation, DB insert, overlap rule                  | Level 3    |

---

# Expected Gap Distribution

The initial experiment should aim for 30 to 40 expected gaps across all cases.

Recommended distribution:

| Case ID | Expected Gap Count | Reason                                               |
| ------- | -----------------: | ---------------------------------------------------- |
| API-001 |                7–9 | Medium-complexity contract and service mapping       |
| API-002 |              10–12 | Complex business logic and database mapping          |
| API-003 |                6–8 | Wrapper mapping and validation                       |
| API-004 |               8–10 | Header validation, enums, DB queries, business rules |

This distribution gives enough examples to calculate useful metrics without making the first benchmark too large.

---

# Difficulty Level Distribution

The experiment cases should cover different levels of difficulty.

| Difficulty Level | Meaning                              | Cases                                  |
| ---------------- | ------------------------------------ | -------------------------------------- |
| Level 1          | Simple contract gaps                 | Covered inside simpler gaps of API-003 |
| Level 2          | Validation and mapping gaps          | API-001, API-003                       |
| Level 3          | Business logic and integration gaps  | API-002, API-004                       |
| Level 4          | Ambiguous or unsupported assumptions | Can be added inside all cases          |

This ensures that the framework is not only tested on easy examples.

---

# Experiment Case Artifact Template

Each case should eventually follow this folder format:

```text
benchmarks/
└── api_001_interrogate_account/
    ├── confluence_requirement.md
    ├── node_red_flow.json
    ├── excel_intake_summary.md
    ├── expected_gaps.json
    ├── detected_gaps.json
    ├── evaluation_result.json
    └── benchmark_notes.md
```

The same structure should be repeated for every experiment case.

---

# Case Design Rules

## Rule 1: Each Case Must Have a Clear Source of Truth

The Confluence-style requirement should clearly define the expected behavior.

This helps the manual baseline and AI-assisted review compare artifacts correctly.

---

## Rule 2: Each Case Must Include Intentional Gaps

The Excel and Node-RED artifacts should not be perfect.

They should contain known missing or incorrect details so the framework can be tested.

---

## Rule 3: Each Gap Must Be Traceable

Every expected gap should be supported by evidence.

Example:

```text
Confluence says X-Acting-As-Type must equal global.
Excel does not include this validation.
```

---

## Rule 4: Include Both Missing and Incorrect Information

The dataset should not only test missing fields.

It should also test incorrect mappings, wrong data types, wrong optionality, technology mismatch, and unsupported assumptions.

---

## Rule 5: Avoid Overly Large Cases at the Start

The first version should stay manageable.

Four API cases are enough for the first research evaluation.

More cases can be added after the framework is implemented.

---

# How These Cases Support the Experiment

These cases support the experiment by giving realistic examples to compare manual review and AI-assisted review.

They allow the project to calculate:

```text
Requirement Coverage
Workflow Completeness Score
Excel Completeness Score
Gap Detection Accuracy
False Positive Rate
False Negative Rate
Severity Assignment Accuracy
Manual Review Reduction
Code Generation Readiness Score
```

The cases also provide examples that can later be used in a research paper or portfolio explanation.

---

# Expected Research Value

The selected cases provide research value because they cover different enterprise API patterns.

```text
API-001 tests account lookup and downstream service mapping.
API-002 tests complex business logic and database-driven discount calculation.
API-003 tests REST wrapper transformation.
API-004 tests database insert and validation logic.
```

Together, these cases show that the framework can be evaluated across multiple API types rather than one narrow example.

---

## What This Phase Does Not Include

This phase does not include:

* Creating the actual benchmark files
* Running the experiment
* Writing Node-RED JSON
* Creating Excel workbooks
* Implementing gap detection logic
* Calculating live metrics
* Generating Go code

This phase only defines the planned experiment cases.

---

## Summary

The experiment cases define the API examples that will be used to evaluate the AI-assisted artifact generation and gap analysis framework.

The initial cases are:

```text
API-001 — Interrogate Account API
API-002 — Evaluate Transaction Discount API
API-003 — Fabrics Wrapper API
API-004 — Clienteling Create Message API
```

These cases cover API contract validation, downstream service integration, business rules, database queries, wrapper mapping, and Excel intake readiness.

They provide the foundation for future benchmark creation and research evaluation.
