# Day 19 — Manual Process Baseline

## Purpose

This document defines the manual process baseline for the 50-Day AI Software Engineer Challenge.

The manual process baseline represents the traditional way an engineer, analyst, or developer converts enterprise API requirements into structured workflow artifacts.

This baseline is needed so the project can compare the AI-assisted workflow generation process against a realistic manual approach.

The goal is not to say the manual process is bad. The goal is to measure how much time, effort, and review work is involved when the same workflow artifact is created manually.

---

## Why a Manual Baseline Is Needed

A research project needs a comparison point.

If the project only measures the AI-generated workflow, it cannot prove whether AI improved the process.

The manual baseline helps answer:

* How long does it take to manually understand an API requirement?
* How long does it take to manually identify workflow steps?
* How many requirement details are missed manually?
* How much correction is needed after manual workflow creation?
* How consistent is the manual workflow structure?
* How does manual output compare against the ground truth workflow?
* How much effort can AI potentially reduce?

The manual baseline acts as the traditional reference process.

---

## Definition of Manual Process Baseline

The manual process baseline is the process where a human reviewer reads a source requirement and manually creates the workflow representation without AI assistance.

The manual workflow should be created using the same source requirement that will later be given to the AI-assisted process.

The manual output should then be compared against the approved ground truth workflow.

The comparison should use the same validation checklist and workflow quality metrics used for the AI-assisted output.

---

## Manual Process Scope

The manual process includes requirement understanding, workflow drafting, and review preparation.

It does not include final implementation or coding.

The manual baseline should remain aligned with the no-coding-until-Day-25 rule.

The manual process may include:

* reading the source requirement
* identifying endpoint details
* identifying required headers
* identifying request body fields
* identifying validation rules
* identifying business rules
* identifying downstream services
* identifying database interactions
* defining workflow steps
* defining response mapping
* defining error handling
* creating traceability notes
* reviewing the workflow for gaps

---

## Manual Process Input

The manual baseline uses the same dataset item used by the AI-assisted approach.

Input should include:

```text
Source Requirement
Sample Request
Sample Response
Business Rule Notes
Downstream Service Notes
Database Notes
Error Handling Notes
Ambiguity Notes
```

The reviewer should not use the ground truth workflow while creating the initial manual workflow.

The ground truth workflow should only be used later for evaluation and scoring.

---

## Manual Process Output

The manual process should produce a structured workflow artifact.

The output should include:

* endpoint details
* request headers
* request body fields
* validation rules
* business rules
* downstream service calls
* database interactions
* workflow steps
* response mapping
* error handling
* traceability references
* assumptions and ambiguity notes

This output will be compared against the ground truth workflow.

---

## Manual Workflow Creation Steps

The manual baseline should follow the steps below.

---

## Step 1 — Read the Source Requirement

The reviewer reads the complete source requirement.

The goal is to understand the API purpose, behavior, and expected output.

The reviewer should record the reading time separately.

### Activities

* Read the full requirement text.
* Review sample request, if available.
* Review sample response, if available.
* Review notes and special conditions.
* Identify unclear or missing details.

### Output

```text
Initial understanding of API purpose and requirement scope.
```

---

## Step 2 — Identify Endpoint Details

The reviewer identifies the endpoint contract.

### Details to Capture

* HTTP method
* endpoint path
* operation name
* API version
* request type
* response type
* endpoint purpose

### Example

```text
HTTP Method: POST
Endpoint Path: /api/v1/clienteling/messages
Operation Name: createApplicationMessage
```

### Common Manual Risks

* Missing API version
* Using an incomplete path
* Renaming the operation inconsistently
* Misunderstanding the endpoint purpose

---

## Step 3 — Identify Request Headers

The reviewer manually extracts all header requirements.

### Details to Capture

* required headers
* optional headers
* header data types
* header validation rules
* downstream propagation rules

### Example

```text
Client-Id: required
Subclient-Id: required
Division-Id: required numeric
Trace-Id: required
```

### Common Manual Risks

* Missing required headers
* Treating optional headers as required
* Changing header names
* Missing downstream header propagation
* Missing validation behavior for blank headers

---

## Step 4 — Identify Request Body Fields

The reviewer manually identifies the request body structure.

### Details to Capture

* required body fields
* optional body fields
* nested fields
* data types
* enum values
* format rules
* null or blank handling

### Example

```text
title: required string
message: required string
messageSeverity: required enum INFO, CRITICAL
startDateTime: required timestamp
endDateTime: optional timestamp
```

### Common Manual Risks

* Missing nested fields
* Missing enum values
* Incorrect required/optional status
* Incorrect data type
* Missing timestamp or numeric format rules

---

## Step 5 — Identify Validation Rules

The reviewer converts header and body requirements into validation rules.

### Validation Types

* required header validation
* required field validation
* enum validation
* numeric validation
* timestamp validation
* conditional validation
* blank or null validation

### Example

```text
Validate that messageSeverity is either INFO or CRITICAL.
Validate that Division-Id is numeric.
Validate that title is present and not blank.
```

### Common Manual Risks

* Writing generic validation instead of specific rules
* Missing enum validation
* Missing conditional validation
* Missing blank value handling
* Not separating header validation from body validation

---

## Step 6 — Identify Business Rules

The reviewer extracts business rules from the source requirement.

### Business Rule Types

* eligibility rules
* reject conditions
* branching conditions
* overlap checks
* calculation rules
* threshold or cap rules
* ordering rules
* special cases

### Example

```text
Before inserting an application message, check whether an active message already exists for the same division with an overlapping time window.
If overlap exists, reject the request.
```

### Common Manual Risks

* Missing important business rules
* Capturing business rules too vaguely
* Placing rules in the wrong order
* Missing reject conditions
* Not separating validation rules from business rules

---

## Step 7 — Identify Downstream Service Calls

If the requirement mentions external or internal service calls, the reviewer identifies them manually.

### Details to Capture

* service name
* service purpose
* HTTP method
* endpoint path
* request mapping
* header mapping
* response mapping
* authentication or signature rules
* downstream error behavior

### Example

```text
Call PIID Lookup Service after request validation.
Pass encrypted account data in the downstream request.
Map downstream account token response into final account status response.
```

### Common Manual Risks

* Missing downstream call
* Incorrect service path
* Missing required downstream headers
* Missing authentication or HMAC rule
* Incorrect request mapping
* Incorrect downstream response handling

---

## Step 8 — Identify Database Interactions

If the requirement includes database operations, the reviewer identifies them manually.

### Details to Capture

* database type
* table name
* query purpose
* query type
* input parameters
* output fields
* no-record behavior
* database error behavior

### Example

```text
DB-001:
Check APP_MSGS for active overlapping messages using division and time window.

DB-002:
Insert new message into APP_MSGS and return generated id.
```

### Common Manual Risks

* Wrong table name
* Missing database query
* Missing no-record behavior
* Missing input parameters
* Missing output mapping
* Missing database error behavior

---

## Step 9 — Define Workflow Steps

The reviewer converts extracted details into ordered workflow steps.

### Expected Step Structure

Each step should include:

* step ID
* step name
* step type
* description
* input
* output
* success path
* failure path
* source reference

### Example

```text
STEP-001: Receive request
STEP-002: Validate required headers
STEP-003: Validate request body
STEP-004: Check message overlap in APP_MSGS
STEP-005: If overlap exists, return business validation error
STEP-006: Insert message into APP_MSGS
STEP-007: Map generated id to response
STEP-008: Return success response
```

### Common Manual Risks

* Missing step IDs
* Wrong step order
* Vague steps such as “process request”
* Missing failure paths
* Missing source references
* Combining too many actions into one step

---

## Step 10 — Define Response Mapping

The reviewer manually maps source data to the final API response.

### Details to Capture

* success response structure
* error response structure
* source field
* target field
* transformation rule
* default values, if supported
* fields to exclude

### Example

```text
APP_MSGS.id maps to response.id.
```

### Common Manual Risks

* Returning unsupported fields
* Missing response fields
* Incorrect field names
* Exposing downstream response directly
* Missing error response format

---

## Step 11 — Define Error Handling

The reviewer manually identifies expected failure scenarios.

### Error Scenarios

* missing required headers
* invalid header values
* missing required body fields
* invalid body values
* business rule failures
* downstream service failures
* database failures
* no-record scenarios
* unexpected failures

### Example

```text
Missing required header returns validation error.
Invalid messageSeverity returns validation error.
Overlap detected returns business validation error.
Database insert failure returns internal error.
```

### Common Manual Risks

* Only documenting happy path
* Missing downstream errors
* Missing database errors
* Missing business rejection errors
* Not mapping HTTP status or error format
* Vague error behavior

---

## Step 12 — Add Traceability Notes

The reviewer links workflow elements back to the source requirement.

### Traceability Should Include

* source requirement section
* business rule ID
* endpoint reference
* sample request reference
* sample response reference
* database reference
* downstream service reference

### Example

| Workflow Element              | Source Reference     |
| ----------------------------- | -------------------- |
| Validate Client-Id            | Header section       |
| Validate messageSeverity enum | Request body section |
| Check overlap                 | Business rule BR-001 |
| Insert APP_MSGS record        | Database section     |
| Return generated id           | Response section     |

### Common Manual Risks

* Missing source references
* Adding unsupported logic without noting it
* Not marking ambiguous areas
* Weak traceability for business rules

---

## Step 13 — Review the Manual Workflow

After creating the workflow, the reviewer performs a self-review.

### Review Areas

* endpoint correctness
* header completeness
* body completeness
* validation rules
* business rules
* step order
* downstream mapping
* database mapping
* response mapping
* error handling
* traceability
* artifact readiness

### Output

```text
Manual workflow draft ready for scoring.
```

---

## Manual Baseline Timing Categories

Time should be recorded consistently.

Recommended categories:

| Time Category          | Description                                                                 |
| ---------------------- | --------------------------------------------------------------------------- |
| Reading Time           | Time spent reading and understanding the requirement                        |
| Extraction Time        | Time spent identifying headers, body, rules, services, and database details |
| Workflow Drafting Time | Time spent writing workflow steps                                           |
| Review Time            | Time spent checking the workflow                                            |
| Correction Time        | Time spent fixing issues after review                                       |
| Total Time             | Total manual effort                                                         |

---

## Manual Baseline Measurement Template

Use the following template for each dataset item.

```text
Dataset Item ID:
API Name:
Requirement Category:
Complexity Level:

Reading Time:
Extraction Time:
Workflow Drafting Time:
Review Time:
Correction Time:
Total Time:

Manual Initial Workflow Score:
Manual Corrected Workflow Score:

Missed Requirement Count:
Incorrect Mapping Count:
Error Handling Gaps:
Business Rule Gaps:
Traceability Gaps:
Final Manual Decision:
```

---

## Manual Baseline Quality Evaluation

The manual workflow should be scored using the Day 17 workflow quality metrics.

Metrics include:

* requirement coverage score
* step completeness score
* validation accuracy score
* business rule coverage score
* step ordering accuracy score
* downstream mapping accuracy score
* database mapping accuracy score
* response mapping accuracy score
* error handling completeness score
* traceability score
* hallucination control score
* artifact readiness score

The same scoring method should be used later for the AI-assisted workflow.

---

## Manual Baseline Failure Categories

If the manual workflow has issues, they should be classified using the Day 17 failure categories.

Examples:

| Manual Issue            | Failure Category               |
| ----------------------- | ------------------------------ |
| Missing required header | Header Validation Failure      |
| Missing business rule   | Business Rule Coverage Failure |
| Wrong table name        | Incorrect Database Mapping     |
| Wrong response field    | Response Mapping Failure       |
| Missing error scenario  | Error Handling Failure         |
| Unsupported assumption  | Hallucinated Logic             |

This makes the manual baseline comparable to the AI-assisted output.

---

## Manual Baseline Example

### Dataset Item

```text
Dataset Item ID: REQ-WF-001
API Name: Create Application Message API
Category: Simple Create API
Complexity: MEDIUM
```

### Manual Process Timing

```text
Reading Time: 10 minutes
Extraction Time: 15 minutes
Workflow Drafting Time: 20 minutes
Review Time: 10 minutes
Correction Time: 8 minutes
Total Time: 63 minutes
```

### Manual Workflow Score

```text
Manual Initial Workflow Score: 86%
Manual Corrected Workflow Score: 95%
```

### Manual Gaps Found

```text
- Missed invalid enum error handling
- Missing traceability reference for database insert
- Response mapping was initially written as messageId instead of id
```

### Final Decision

```text
Manual Workflow Decision: PASS WITH WARNINGS before correction
Manual Corrected Decision: PASS
```

---

## Strengths of Manual Process

The manual process has several strengths.

### 1. Human Context Understanding

A human reviewer may understand implied business context better than AI.

### 2. Better Handling of Ambiguity

A careful reviewer may know when to ask questions or mark unclear areas.

### 3. Domain Awareness

A human may understand enterprise-specific patterns, naming conventions, and business constraints.

### 4. Lower Risk of Unsupported Automation

Manual review may avoid some unsupported generated assumptions.

---

## Weaknesses of Manual Process

The manual process also has several weaknesses.

### 1. Time-Consuming

Manual workflow creation can take significant time for each API.

### 2. Inconsistent Structure

Different reviewers may create workflows in different formats.

### 3. Missed Details

Humans may miss headers, validations, business rules, or error scenarios.

### 4. Repetitive Work

Many parts of workflow extraction are repetitive and documentation-heavy.

### 5. Hard to Scale

Manual workflow creation becomes difficult when many APIs need to be processed.

---

## Manual Baseline Risks

The manual baseline has the following risks:

| Risk                 | Description                                     | Mitigation                                            |
| -------------------- | ----------------------------------------------- | ----------------------------------------------------- |
| Reviewer Bias        | Manual creator may know the ground truth        | Create manual workflow before looking at ground truth |
| Inconsistent Timing  | Time may be recorded differently                | Use defined timing categories                         |
| Over-Correction      | Reviewer may fix issues during initial creation | Separate initial draft and corrected version          |
| Missing Assumptions  | Assumptions may not be documented               | Require assumptions section                           |
| Vague Workflow Steps | Steps may not be artifact-ready                 | Use required workflow step structure                  |

---

## Fair Manual Baseline Rules

To keep the manual baseline fair:

1. Use the same source requirement as the AI process.
2. Do not view the ground truth workflow before creating the initial manual workflow.
3. Record time honestly.
4. Record assumptions separately.
5. Do not correct the initial workflow before scoring it.
6. Score the initial workflow and corrected workflow separately.
7. Use the same validation checklist as the AI workflow.
8. Use the same quality metrics as the AI workflow.
9. Classify failures using the same failure categories.
10. Keep the process documentation consistent across dataset samples.

---

## Manual Baseline Output Template

The manual workflow output should follow this structure.

```text
# Manual Workflow Output — REQ-WF-001

## Metadata

Dataset Item ID:
API Name:
Requirement Category:
Complexity Level:
Reviewer:
Creation Date:
Manual Workflow Status:

---

## Endpoint

HTTP Method:
Endpoint Path:
Operation Name:

---

## Headers

List required and optional headers.

---

## Request Body

List required and optional body fields.

---

## Validations

List validation rules.

---

## Business Rules

List business rules.

---

## Downstream Services

List downstream services, if applicable.

---

## Database Interactions

List database interactions, if applicable.

---

## Workflow Steps

List ordered workflow steps.

---

## Response Mapping

Define response mapping.

---

## Error Handling

Define error scenarios.

---

## Traceability Notes

Link workflow elements to source requirement.

---

## Assumptions and Ambiguity

List any assumptions or unclear areas.

---

## Timing

Reading Time:
Extraction Time:
Workflow Drafting Time:
Review Time:
Correction Time:
Total Time:

---

## Review Result

Initial Score:
Corrected Score:
Final Decision:
```

---

## Relationship to AI-Assisted Baseline

The manual baseline will be compared against the AI-assisted baseline.

Both processes should be evaluated on the same dataset item.

The comparison should measure:

```text
Manual Workflow
        vs
AI-Assisted Workflow
        vs
Ground Truth Workflow
```

The goal is not necessarily for AI to beat manual quality on the first attempt.

The more realistic goal is to determine whether AI can reduce initial drafting effort while still producing a workflow that can be corrected and validated efficiently.

---

## Expected Role in Research Paper

The manual process baseline supports the research paper by providing the traditional comparison point.

The paper can use the manual baseline to report:

* average manual workflow creation time
* average manual workflow quality score
* average manual correction count
* common manual workflow errors
* comparison against AI-assisted workflow generation
* time reduction achieved by AI-assisted drafting

Example research statement:

```text
Manual workflow creation required an average of 55 minutes per API requirement, while AI-assisted workflow drafting reduced initial creation time to 12 minutes before validation and correction.
```

---

## Summary

The manual process baseline defines how enterprise API workflows are created without AI assistance.

It captures the traditional process of reading requirements, extracting details, writing workflow steps, reviewing output, and correcting gaps.

This baseline is essential for comparing the AI-assisted approach against a realistic manual workflow creation process.

Day 19 Phase 2 establishes the manual baseline needed for future evaluation and research comparison.
