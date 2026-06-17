# Day 17 — Validation Failure Categories

## Purpose

This document defines the validation failure categories for the 50-Day AI Software Engineer Challenge.

The purpose of this document is to classify the types of mistakes that can occur when enterprise API requirements are converted into structured workflow steps.

These failure categories help reviewers identify what went wrong in a generated workflow and how serious the issue is.

The categories also support the research evaluation layer by making workflow validation more measurable and repeatable.

---

## Why Failure Categories Are Needed

A generated workflow may fail validation for different reasons.

Some failures are minor, such as a missing description or incomplete traceability reference.

Other failures are major, such as missing a business rule, calling the wrong downstream service, or generating unsupported logic.

Without clear failure categories, every issue may be treated the same way.

Failure categories help separate:

* missing information
* incorrect mappings
* wrong step order
* hallucinated logic
* incomplete error handling
* unsupported assumptions
* artifact-readiness gaps

This makes validation more structured and easier to improve over time.

---

## Failure Severity Levels

Each validation failure should be assigned a severity level.

| Severity | Meaning                                                         |
| -------- | --------------------------------------------------------------- |
| Critical | The workflow is unsafe or incorrect and should not move forward |
| High     | The workflow has major gaps and needs correction                |
| Medium   | The workflow is partially correct but needs review              |
| Low      | The workflow has minor documentation or traceability gaps       |

---

## Failure Category Summary

| Category ID | Failure Category                       | Severity Range   |
| ----------- | -------------------------------------- | ---------------- |
| FC-001      | Missing Requirement Coverage           | High to Critical |
| FC-002      | Incorrect Endpoint Mapping             | High to Critical |
| FC-003      | Header Validation Failure              | Medium to High   |
| FC-004      | Request Body Validation Failure        | Medium to High   |
| FC-005      | Business Rule Coverage Failure         | High to Critical |
| FC-006      | Incorrect Workflow Step Order          | High to Critical |
| FC-007      | Missing Downstream Service Call        | High to Critical |
| FC-008      | Incorrect Downstream Mapping           | High to Critical |
| FC-009      | Incorrect Database Mapping             | High to Critical |
| FC-010      | Response Mapping Failure               | Medium to High   |
| FC-011      | Error Handling Failure                 | Medium to High   |
| FC-012      | Traceability Failure                   | Low to Medium    |
| FC-013      | Hallucinated Logic                     | High to Critical |
| FC-014      | Ambiguous Requirement Handling Failure | Medium to High   |
| FC-015      | Artifact Readiness Failure             | Medium to High   |

---

## FC-001 — Missing Requirement Coverage

### Description

This failure occurs when the generated workflow does not represent one or more important parts of the source requirement.

### Examples

* A required validation rule is missing.
* A business rule is not converted into a workflow step.
* A sample response field is ignored.
* A required downstream call is not included.
* A required database check is not represented.

### Impact

This is a serious issue because the workflow becomes incomplete.

If the workflow moves forward, downstream artifacts such as Node-RED flows, Excel files, Go code, and tests may also be incomplete.

### Severity

High to Critical

### Example

```text
Source Requirement:
The API must reject requests when Client-Id is missing.

Generated Workflow:
1. Receive request
2. Process request
3. Return response

Failure:
Missing Client-Id validation step.
```

---

## FC-002 — Incorrect Endpoint Mapping

### Description

This failure occurs when endpoint-level details are incorrectly captured.

### Examples

* Wrong HTTP method
* Wrong endpoint path
* Incorrect operation name
* Missing API version
* Incorrect endpoint description
* Endpoint generalized without support from the requirement

### Impact

Incorrect endpoint mapping can cause generated artifacts to expose the wrong contract.

### Severity

High to Critical

### Example

```text
Source Requirement:
POST /api/v1/clienteling/messages

Generated Workflow:
GET /api/v1/messages

Failure:
HTTP method and endpoint path are incorrect.
```

---

## FC-003 — Header Validation Failure

### Description

This failure occurs when request header rules are missing, incorrect, or unsupported.

### Examples

* Required header is missing from workflow.
* Optional header is treated as required.
* Header name is changed incorrectly.
* Blank header behavior is not defined.
* Downstream header propagation is missing.
* Unsupported header is added.

### Impact

Header validation failures can cause generated APIs to reject valid requests or accept invalid requests.

### Severity

Medium to High

### Example

```text
Source Requirement:
Client-Id, Subclient-Id, and User-Id are required headers.

Generated Workflow:
Only validates Client-Id and Subclient-Id.

Failure:
User-Id validation is missing.
```

---

## FC-004 — Request Body Validation Failure

### Description

This failure occurs when request body fields or body validation rules are incorrectly represented.

### Examples

* Required body field is missing.
* Optional body field is treated as required.
* Nested field is flattened incorrectly.
* Enum values are missing.
* Field type is incorrect.
* Null or blank handling is missing.
* Unsupported body field is added.

### Impact

Request body validation failures can break the API contract and produce incorrect generated DTOs, tests, and validation logic.

### Severity

Medium to High

### Example

```text
Source Requirement:
messageSeverity must be either INFO or CRITICAL.

Generated Workflow:
messageSeverity is captured as a free text field.

Failure:
Enum validation is missing.
```

---

## FC-005 — Business Rule Coverage Failure

### Description

This failure occurs when business logic from the source requirement is missing, incomplete, or incorrectly represented.

### Examples

* Eligibility check is missing.
* Reject condition is missing.
* Calculation rule is incorrect.
* Conditional branch is missing.
* Business rule is simplified incorrectly.
* Rule applies to the wrong scenario.
* Business rule is not linked to source requirement.

### Impact

This is one of the most serious failure categories because business rules define the actual behavior of enterprise APIs.

### Severity

High to Critical

### Example

```text
Source Requirement:
If an active message already overlaps the requested time window for the same division, reject the request.

Generated Workflow:
Creates the message without checking overlap.

Failure:
Overlap business rule is missing.
```

---

## FC-006 — Incorrect Workflow Step Order

### Description

This failure occurs when workflow steps are present but placed in the wrong execution order.

### Examples

* Business logic runs before request validation.
* Downstream service is called before required preconditions.
* Database insert occurs before duplicate check.
* Response mapping happens before all required data is available.
* Error handling is placed after success response generation.

### Impact

Incorrect step order can produce wrong behavior even if all required steps are present.

### Severity

High to Critical

### Example

```text
Incorrect Workflow:
1. Insert record
2. Check if duplicate exists
3. Return response

Failure:
Duplicate check should happen before insert.
```

---

## FC-007 — Missing Downstream Service Call

### Description

This failure occurs when a required external service call is not represented in the workflow.

### Examples

* Requirement mentions a downstream lookup service, but workflow does not call it.
* Requirement requires an external validation service, but workflow skips it.
* Workflow returns a response using only local data when downstream response is required.

### Impact

Missing downstream calls can make the generated workflow logically invalid.

### Severity

High to Critical

### Example

```text
Source Requirement:
The API must call the PIID lookup service to get account details.

Generated Workflow:
Validates request and returns account status directly.

Failure:
PIID lookup downstream call is missing.
```

---

## FC-008 — Incorrect Downstream Mapping

### Description

This failure occurs when a downstream service is included but mapped incorrectly.

### Examples

* Wrong downstream endpoint path
* Wrong HTTP method
* Incorrect request body mapping
* Incorrect header mapping
* Missing authentication or signature rule
* Incorrect downstream response mapping
* Missing downstream error behavior

### Impact

The generated workflow may call the correct service but send the wrong request or mishandle the response.

### Severity

High to Critical

### Example

```text
Source Requirement:
encryptedData from the API request must be passed to downstream encryptInfo.encryptedData.

Generated Workflow:
Maps encryptedData to accountNumber.

Failure:
Downstream request mapping is incorrect.
```

---

## FC-009 — Incorrect Database Mapping

### Description

This failure occurs when database operations are missing or incorrectly represented.

### Examples

* Wrong table name
* Wrong query type
* Wrong input parameter
* Wrong output field
* Missing no-record behavior
* Missing database error behavior
* Invented table or column
* Query purpose does not match requirement

### Impact

Incorrect database mapping can cause generated code to query the wrong data or apply wrong business decisions.

### Severity

High to Critical

### Example

```text
Source Requirement:
Check APP_MSGS for active overlapping messages.

Generated Workflow:
Checks CUSTOMER_MESSAGES table.

Failure:
Database table mapping is incorrect.
```

---

## FC-010 — Response Mapping Failure

### Description

This failure occurs when the final API response is missing, incorrect, or not aligned with the source requirement.

### Examples

* Success response structure is wrong.
* Error response structure is missing.
* Field names do not match sample response.
* Downstream fields are exposed directly without transformation.
* Required response field is missing.
* Unsupported response field is added.

### Impact

Response mapping failures can break client integrations and generated contract tests.

### Severity

Medium to High

### Example

```text
Source Requirement:
Response should return:
{
  "id": "12345"
}

Generated Workflow:
Response returns:
{
  "messageId": "12345",
  "status": "CREATED"
}

Failure:
Response structure does not match requirement.
```

---

## FC-011 — Error Handling Failure

### Description

This failure occurs when required error scenarios are not represented.

### Examples

* Missing validation error handling
* Missing downstream error handling
* Missing database error handling
* Missing no-record handling
* Missing business rejection response
* Incorrect HTTP status mapping
* Incorrect error response format

### Impact

A workflow with weak error handling may only support the happy path and may not be production-ready.

### Severity

Medium to High

### Example

```text
Source Requirement:
Reject missing required headers with HTTP 400.

Generated Workflow:
Does not define response behavior for missing headers.

Failure:
Validation error handling is missing.
```

---

## FC-012 — Traceability Failure

### Description

This failure occurs when workflow steps cannot be traced back to the source requirement.

### Examples

* Workflow step has no source requirement reference.
* Business rule has no business requirement ID.
* Downstream mapping has no source reference.
* Response field mapping cannot be linked to a sample response.
* Unsupported logic is not flagged.

### Impact

Traceability failures make the workflow harder to review, defend, and evaluate.

### Severity

Low to Medium

### Example

```text
Workflow Step:
Validate Authorization token.

Source Requirement:
No authorization rule is mentioned.

Failure:
Step has no traceability and may be unsupported.
```

---

## FC-013 — Hallucinated Logic

### Description

This failure occurs when the workflow includes logic that was not present in the source requirement.

### Examples

* Adding authorization when not required
* Adding Trace-Id as required when it is optional
* Adding a database table not mentioned
* Adding a downstream service not specified
* Adding response fields not in the sample response
* Adding business rules by assumption
* Adding default values without requirement support

### Impact

Hallucinated logic is dangerous because it may make the generated workflow look complete while actually violating the requirement.

### Severity

High to Critical

### Example

```text
Source Requirement:
Do not require Authorization.

Generated Workflow:
Validate Authorization header.

Failure:
Authorization validation is hallucinated logic.
```

---

## FC-014 — Ambiguous Requirement Handling Failure

### Description

This failure occurs when ambiguous or unclear requirement areas are handled incorrectly.

### Examples

* Ambiguous field is guessed instead of marked for review.
* Missing response details are invented.
* Unclear error behavior is assumed.
* Unknown downstream behavior is treated as confirmed.
* Optional field behavior is guessed without source support.

### Impact

Ambiguous areas should be flagged for review instead of silently converted into assumed workflow logic.

### Severity

Medium to High

### Example

```text
Source Requirement:
Trace-Id may be passed if available.

Generated Workflow:
Trace-Id is marked as required.

Failure:
Ambiguous or optional behavior was incorrectly converted into mandatory logic.
```

---

## FC-015 — Artifact Readiness Failure

### Description

This failure occurs when the workflow is understandable to a human but not structured enough to support downstream artifact generation.

### Examples

* Workflow steps are too vague.
* Input and output mappings are missing.
* Business rules are written as paragraphs only.
* No step IDs are assigned.
* No service IDs are assigned.
* No query IDs are assigned.
* No response mapping fields are defined.
* Workflow does not contain enough structure for Excel, Node-RED, Go, or test generation.

### Impact

The workflow may be conceptually correct but not useful for automation.

### Severity

Medium to High

### Example

```text
Generated Workflow:
Process the request and apply discount rules.

Failure:
The step is too vague to generate code, tests, or Node-RED flow.
```

---

## Failure Category Review Format

Each validation issue should be documented using the following format.

```text
Failure ID:
Category:
Severity:
Workflow Step:
Source Requirement Reference:
Issue Description:
Expected Behavior:
Actual Workflow Behavior:
Recommended Correction:
```

---

## Example Failure Report

```text
Failure ID: FAIL-001
Category: FC-005 — Business Rule Coverage Failure
Severity: Critical
Workflow Step: Missing
Source Requirement Reference: BR-001
Issue Description:
The source requirement says the system must reject overlapping active messages for the same division, but the workflow does not include this check.

Expected Behavior:
Workflow should include a database overlap check before inserting the message.

Actual Workflow Behavior:
Workflow directly inserts the message after request validation.

Recommended Correction:
Add a workflow step before insert:
Check APP_MSGS for active overlapping messages using division and time window.
If overlap exists, return business validation error.
```

---

## Failure Severity Decision Guide

Use the following guide to decide severity.

### Critical

Use Critical when:

* the workflow violates a core business rule
* the workflow calls the wrong system
* the workflow exposes an incorrect API contract
* hallucinated logic changes required behavior
* the workflow cannot safely proceed to artifact generation

### High

Use High when:

* an important validation rule is missing
* a downstream mapping is incomplete
* a database query is incorrect
* error handling is missing for major scenarios
* step order may cause incorrect behavior

### Medium

Use Medium when:

* logic is partially correct but incomplete
* traceability is weak
* a response field needs review
* optional behavior is unclear
* an artifact may need manual correction

### Low

Use Low when:

* wording is unclear but logic is correct
* metadata is incomplete
* documentation needs improvement
* source reference is missing for a minor step

---

## How Failure Categories Support Workflow Improvement

Failure categories help improve the workflow generation process by showing patterns over time.

For example:

```text
If many workflows fail due to missing error handling,
then the prompt design should be improved to explicitly extract error scenarios.
```

```text
If many workflows fail due to hallucinated authorization logic,
then the workflow generation rules should be updated to prevent unsupported security assumptions.
```

```text
If many workflows fail due to incorrect response mapping,
then sample response extraction should be improved.
```

This creates a feedback loop between validation and prompt design.

---

## How Failure Categories Support Research Evaluation

These categories support the research paper by creating measurable failure analysis.

They can be used to report:

* most common workflow generation errors
* percentage of workflows with hallucinated logic
* percentage of workflows missing business rules
* percentage of workflows missing downstream mapping
* improvement after validation
* reduction in critical failures after prompt refinement

Example research reporting format:

```text
Out of 20 generated workflows:
- 6 had missing validation rules
- 5 had incomplete response mappings
- 4 had weak error handling
- 3 had hallucinated logic
- 2 had incorrect downstream mappings
```

This helps prove where AI performs well and where human validation is still needed.

---

## Relationship to Workflow Quality Metrics

The failure categories are directly connected to the workflow quality metrics.

Examples:

| Failure Category               | Related Metric                    |
| ------------------------------ | --------------------------------- |
| Missing Requirement Coverage   | Requirement Coverage Score        |
| Header Validation Failure      | Validation Accuracy Score         |
| Business Rule Coverage Failure | Business Rule Coverage Score      |
| Incorrect Step Order           | Step Ordering Accuracy Score      |
| Incorrect Downstream Mapping   | Downstream Mapping Accuracy Score |
| Incorrect Database Mapping     | Database Mapping Accuracy Score   |
| Response Mapping Failure       | Response Mapping Accuracy Score   |
| Error Handling Failure         | Error Handling Completeness Score |
| Traceability Failure           | Traceability Score                |
| Hallucinated Logic             | Hallucination Control Score       |
| Artifact Readiness Failure     | Artifact Readiness Score          |

This allows both qualitative and quantitative evaluation of generated workflows.

---

## Summary

Validation failure categories define the major ways a generated workflow can be wrong.

The key failure categories are:

* missing requirement coverage
* incorrect endpoint mapping
* header validation failure
* request body validation failure
* business rule coverage failure
* incorrect workflow step order
* missing downstream service calls
* incorrect downstream mapping
* incorrect database mapping
* response mapping failure
* error handling failure
* traceability failure
* hallucinated logic
* ambiguous requirement handling failure
* artifact readiness failure

These categories make workflow validation more structured, measurable, and useful for improving the AI-driven requirement-to-workflow pipeline.

Day 17 Phase 4 establishes the failure classification layer needed to support reliable workflow validation and future research evaluation.
