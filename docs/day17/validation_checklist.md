# Day 17 — Validation Checklist

## Purpose

This document defines the validation checklist used to review generated workflows in the 50-Day AI Software Engineer Challenge.

The checklist is used to verify whether a workflow generated from enterprise API requirements is complete, accurate, traceable, and ready for downstream artifact generation.

The checklist acts as a structured review guide before the workflow is used to generate:

- Node-RED flows
- Excel intake files
- Go code
- test cases
- documentation
- gap analysis reports

---

## Why a Validation Checklist Is Needed

AI-generated workflows may appear correct at a high level but still miss important requirement details.

A checklist helps ensure that each workflow is reviewed consistently.

Without a checklist, reviewers may focus only on the visible happy path and miss important details such as:

- missing headers
- missing request body validations
- missing enum rules
- missing downstream service calls
- wrong database mappings
- incomplete response mappings
- missing error handling
- hallucinated business logic
- missing traceability

The checklist helps convert workflow review into a repeatable quality-control process.

---

## Checklist Usage

Each generated workflow should be reviewed using the sections below.

For every checklist item, mark one of the following statuses:

| Status | Meaning |
|---|---|
| PASS | The item is correctly represented in the workflow |
| FAIL | The item is missing or incorrect |
| WARNING | The item is partially represented but needs review |
| NOT APPLICABLE | The item does not apply to this workflow |

---

## 1. Source Requirement Coverage Checklist

This section verifies whether the workflow covers the original requirement.

| Check ID | Validation Check | Status |
|---|---|---|
| SRC-001 | Source requirement document is identified |  |
| SRC-002 | API purpose is captured correctly |  |
| SRC-003 | Main business process is represented |  |
| SRC-004 | All explicitly stated business rules are captured |  |
| SRC-005 | All sample request fields are considered |  |
| SRC-006 | All sample response fields are considered |  |
| SRC-007 | Notes and special conditions from the requirement are reviewed |  |
| SRC-008 | No unsupported requirement is added by assumption |  |

---

## 2. Endpoint Checklist

This section validates the endpoint-level details.

| Check ID | Validation Check | Status |
|---|---|---|
| EP-001 | HTTP method is correct |  |
| EP-002 | Endpoint path is correct |  |
| EP-003 | Operation name is captured |  |
| EP-004 | Endpoint description matches the source requirement |  |
| EP-005 | Request type is correctly identified |  |
| EP-006 | Response type is correctly identified |  |
| EP-007 | Endpoint version is captured when available |  |
| EP-008 | Endpoint is not renamed or generalized without support |  |

---

## 3. Header Validation Checklist

This section checks whether request headers are correctly captured.

| Check ID | Validation Check | Status |
|---|---|---|
| HDR-001 | All required headers are listed |  |
| HDR-002 | Optional headers are listed separately |  |
| HDR-003 | Header names match the source requirement exactly |  |
| HDR-004 | Header required/optional status is correct |  |
| HDR-005 | Header data types are captured when available |  |
| HDR-006 | Blank or missing header behavior is defined |  |
| HDR-007 | Header validation rules are represented as workflow steps |  |
| HDR-008 | Headers passed to downstream services are identified |  |
| HDR-009 | No unsupported headers are added |  |

---

## 4. Request Body Validation Checklist

This section verifies request body mapping and validation.

| Check ID | Validation Check | Status |
|---|---|---|
| BODY-001 | All required request body fields are captured |  |
| BODY-002 | Optional request body fields are captured |  |
| BODY-003 | Nested request body fields are represented correctly |  |
| BODY-004 | Field names match the source requirement |  |
| BODY-005 | Field data types are captured |  |
| BODY-006 | Required field validation is defined |  |
| BODY-007 | Blank or null field behavior is defined |  |
| BODY-008 | Enum values are captured where applicable |  |
| BODY-009 | Format validations are captured where applicable |  |
| BODY-010 | No unsupported request body fields are added |  |

---

## 5. Business Rule Checklist

This section verifies whether business logic is correctly represented.

| Check ID | Validation Check | Status |
|---|---|---|
| BR-001 | All business rules are captured |  |
| BR-002 | Business rules are mapped to workflow steps |  |
| BR-003 | Rule execution order is correct |  |
| BR-004 | Conditional branches are represented |  |
| BR-005 | Eligibility checks are represented |  |
| BR-006 | Reject conditions are represented |  |
| BR-007 | Calculation rules are represented |  |
| BR-008 | Business rules are not simplified incorrectly |  |
| BR-009 | No new business logic is added by assumption |  |

---

## 6. Workflow Step Checklist

This section validates the structure and order of workflow steps.

| Check ID | Validation Check | Status |
|---|---|---|
| WF-001 | Workflow begins with request intake |  |
| WF-002 | Header validation occurs before business processing |  |
| WF-003 | Request body validation occurs before business processing |  |
| WF-004 | Precondition checks occur before downstream calls when required |  |
| WF-005 | Downstream service calls occur at the correct point |  |
| WF-006 | Database checks occur at the correct point |  |
| WF-007 | Business logic is executed after required data is available |  |
| WF-008 | Response mapping happens after processing is complete |  |
| WF-009 | Error paths are represented |  |
| WF-010 | Workflow does not skip required decision points |  |

---

## 7. Downstream Service Checklist

This section validates external service usage.

| Check ID | Validation Check | Status |
|---|---|---|
| SRV-001 | Required downstream service is identified |  |
| SRV-002 | Downstream endpoint path is captured |  |
| SRV-003 | Downstream HTTP method is captured |  |
| SRV-004 | Downstream request body mapping is defined |  |
| SRV-005 | Downstream request header mapping is defined |  |
| SRV-006 | Downstream authentication or signature requirement is captured if specified |  |
| SRV-007 | Downstream response mapping is defined |  |
| SRV-008 | Downstream error behavior is defined |  |
| SRV-009 | Workflow does not call unsupported downstream services |  |

---

## 8. Database Mapping Checklist

This section validates database interaction details.

| Check ID | Validation Check | Status |
|---|---|---|
| DB-001 | Required database interaction is identified |  |
| DB-002 | Database type is captured when available |  |
| DB-003 | Table name is captured correctly |  |
| DB-004 | Query purpose is clearly defined |  |
| DB-005 | Query type is correct |  |
| DB-006 | Query input parameters are mapped |  |
| DB-007 | Query output fields are mapped |  |
| DB-008 | No-record behavior is defined |  |
| DB-009 | Database error behavior is defined |  |
| DB-010 | Workflow does not invent unsupported database tables or queries |  |

---

## 9. Response Mapping Checklist

This section validates the final response structure.

| Check ID | Validation Check | Status |
|---|---|---|
| RESP-001 | Success response structure is captured |  |
| RESP-002 | Error response structure is captured |  |
| RESP-003 | Response field names match the requirement |  |
| RESP-004 | Response field data types are correct |  |
| RESP-005 | Source-to-target field mapping is defined |  |
| RESP-006 | Downstream response transformation is defined when required |  |
| RESP-007 | Default values are only used when supported by the requirement |  |
| RESP-008 | Response does not expose unsupported downstream fields |  |
| RESP-009 | Response mapping matches sample response where available |  |

---

## 10. Error Handling Checklist

This section validates failure scenarios.

| Check ID | Validation Check | Status |
|---|---|---|
| ERR-001 | Missing required header error is handled |  |
| ERR-002 | Invalid header value error is handled |  |
| ERR-003 | Missing required body field error is handled |  |
| ERR-004 | Invalid body field value error is handled |  |
| ERR-005 | Business rule failure is handled |  |
| ERR-006 | Downstream service failure is handled |  |
| ERR-007 | Database failure is handled |  |
| ERR-008 | No-record scenario is handled |  |
| ERR-009 | Unexpected failure scenario is handled |  |
| ERR-010 | HTTP status mapping is defined |  |
| ERR-011 | Error response format is defined |  |

---

## 11. Traceability Checklist

This section verifies whether workflow steps can be traced back to the original requirement.

| Check ID | Validation Check | Status |
|---|---|---|
| TRACE-001 | Workflow has a source requirement reference |  |
| TRACE-002 | Each validation step has a source reference |  |
| TRACE-003 | Each business rule step has a source reference |  |
| TRACE-004 | Each downstream call has a source reference |  |
| TRACE-005 | Each database interaction has a source reference |  |
| TRACE-006 | Each response mapping has a source reference |  |
| TRACE-007 | Unsupported workflow steps are flagged |  |
| TRACE-008 | Ambiguous requirement areas are marked for review |  |

---

## 12. Hallucination Detection Checklist

This section checks whether the workflow contains unsupported assumptions.

| Check ID | Validation Check | Status |
|---|---|---|
| HAL-001 | No unsupported headers are added |  |
| HAL-002 | No unsupported authorization logic is added |  |
| HAL-003 | No unsupported request fields are added |  |
| HAL-004 | No unsupported downstream services are added |  |
| HAL-005 | No unsupported database tables are added |  |
| HAL-006 | No unsupported business rules are added |  |
| HAL-007 | No unsupported response fields are added |  |
| HAL-008 | No unsupported error scenarios are invented as required behavior |  |

---

## 13. Artifact Readiness Checklist

This section checks whether the workflow is ready to generate downstream artifacts.

| Check ID | Validation Check | Status |
|---|---|---|
| ART-001 | Workflow can support Node-RED generation |  |
| ART-002 | Workflow can support Excel intake generation |  |
| ART-003 | Workflow can support Go code generation later |  |
| ART-004 | Workflow can support test case generation |  |
| ART-005 | Workflow can support documentation generation |  |
| ART-006 | Workflow contains enough structure for automation |  |
| ART-007 | Workflow is not dependent on hidden assumptions |  |

---

## Final Validation Decision

After completing the checklist, assign one final decision.

| Decision | Meaning |
|---|---|
| PASS | Workflow is ready for downstream artifact generation |
| PASS WITH WARNINGS | Workflow is usable but needs minor review |
| FAIL | Workflow has major gaps and should not move forward |
| NEEDS REVIEW | Workflow contains ambiguity that requires human clarification |

---

## Example Review Summary

```text
Workflow ID: WF-001
Endpoint: POST /api/v1/example
Validation Decision: FAIL

Major Issues:
- Missing required header validation for User-Id
- Missing downstream service error handling
- Response mapping does not match sample response

Recommendation:
Workflow should not proceed to artifact generation until the missing validation and error handling steps are added.
```

---

## Checklist Summary

This validation checklist provides a structured way to review generated workflows.

It ensures that workflows are checked for:

- source requirement coverage
- endpoint correctness
- header validation
- body validation
- business rule coverage
- workflow step order
- downstream service mapping
- database mapping
- response mapping
- error handling
- traceability
- hallucination detection
- artifact readiness

This checklist strengthens the reliability of the AI-driven requirement-to-workflow pipeline by making validation repeatable, measurable, and reviewable.