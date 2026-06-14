# Day 13 — Phase 5: Gap Severity Levels

## Purpose

This document defines the severity levels used by the Gap Analysis Framework.

Severity levels help prioritize detected gaps based on their impact on requirement correctness, workflow completeness, Excel intake accuracy, and generated code reliability.

The purpose of severity classification is to answer one important question:

```text
How serious is this gap, and how quickly should it be fixed?
```

---

## Why Severity Levels Are Needed

Gap analysis may identify many issues across Confluence, Node-RED, and Excel artifacts.

Not every gap has the same level of impact.

Some gaps may completely break generated code.

Some gaps may cause incorrect business behavior.

Some gaps may only affect naming clarity or documentation quality.

Severity levels help reviewers decide:

* Which gaps must be fixed immediately
* Which gaps can be fixed after critical issues
* Which gaps need manual review
* Which gaps are minor documentation improvements
* Which gaps can be accepted as low-risk assumptions

Severity levels make the gap analysis report easier to act on.

---

## Severity Levels

The Gap Analysis Framework uses five severity levels:

```text
Critical
High
Medium
Low
Informational
```

Each severity level is defined below.

---

# 1. Critical Severity

## Definition

A critical gap is a gap that can cause the generated API or service to be fundamentally incorrect, unusable, unsafe, or incompatible with the target system.

Critical gaps must be fixed before code generation or implementation continues.

---

## When to Use Critical

Use `Critical` when the gap affects:

* Endpoint existence
* Core business logic
* Required downstream service calls
* Required database operations
* Target technology correctness
* Authentication or authorization behavior
* Data integrity
* Production compatibility

---

## Examples

### Example 1: Missing Endpoint

```text
Confluence defines POST /api/v1/discount/evaluateTransactionDiscount, but the Excel api_endpoints sheet does not contain this endpoint.
```

Severity:

```text
Critical
```

Reason:

```text
The generated code may not create the required API route.
```

---

### Example 2: Technology Mismatch

```text
The target implementation should use Go + PostgreSQL, but the Excel workbook refers to Node.js + SQLite.
```

Severity:

```text
Critical
```

Reason:

```text
The generated code may target the wrong implementation environment.
```

---

### Example 3: Missing Required Downstream Service

```text
Confluence requires a PIIDLookup downstream service call, but the external_services sheet does not define PIIDLookup.
```

Severity:

```text
Critical
```

Reason:

```text
The API cannot perform the required account lookup without this service.
```

---

## Critical Fix Rule

Critical gaps should block the pipeline.

The artifact should not be considered ready for code generation until all critical gaps are resolved.

---

# 2. High Severity

## Definition

A high severity gap is a gap that can cause incorrect behavior, failed validation, failed integration, or incorrect response mapping, but does not necessarily mean the entire API is missing.

High severity gaps should be fixed before final code generation or review.

---

## When to Use High

Use `High` when the gap affects:

* Required headers
* Required request fields
* Required response fields
* Required validation rules
* Enum validations
* Important business rules
* Service request mapping
* Service response mapping
* Database query parameters
* Error handling for major failures

---

## Examples

### Example 1: Missing Required Header

```text
Confluence requires x-macys-apikey, but Excel does not include it in required_headers.
```

Severity:

```text
High
```

Reason:

```text
The generated API may fail authentication or skip required request validation.
```

---

### Example 2: Missing Enum Validation

```text
Confluence defines allowed entryMode values, but Excel only marks entryMode as required and does not include the enum list.
```

Severity:

```text
High
```

Reason:

```text
The generated API may accept unsupported values.
```

---

### Example 3: Incorrect Response Mapping

```text
The downstream return_code_o should map to REST response code, but Excel maps it to message.
```

Severity:

```text
High
```

Reason:

```text
The API response contract may be incorrect.
```

---

## High Fix Rule

High severity gaps should be resolved before the generated artifact is considered complete.

They may not always block early experimentation, but they should block production-level output.

---

# 3. Medium Severity

## Definition

A medium severity gap is a gap that affects completeness, maintainability, or partial correctness but may not immediately break the core API behavior.

Medium gaps should be reviewed and fixed when practical.

---

## When to Use Medium

Use `Medium` when the gap affects:

* Optional fields
* Secondary response fields
* Non-critical mappings
* Documentation consistency
* Non-blocking workflow steps
* Additional metadata
* Logging or tracing details
* Incomplete but recoverable business rule descriptions

---

## Examples

### Example 1: Missing Optional Header

```text
Confluence defines requestorInfo-traceId as optional, but Excel does not include it.
```

Severity:

```text
Medium
```

Reason:

```text
The API may still work, but tracing support may be incomplete.
```

---

### Example 2: Incomplete Business Rule Description

```text
Excel includes a business rule ID but does not include acceptance criteria.
```

Severity:

```text
Medium
```

Reason:

```text
The generated logic may be harder to validate or test.
```

---

### Example 3: Missing Non-Critical Output Field

```text
The response includes an optional debug field in Confluence, but Excel does not map it.
```

Severity:

```text
Medium
```

Reason:

```text
The main API behavior is still functional, but response completeness is reduced.
```

---

## Medium Fix Rule

Medium severity gaps should be reviewed during artifact cleanup.

They do not always block the pipeline, but they should be tracked and resolved if they affect testability or completeness.

---

# 4. Low Severity

## Definition

A low severity gap is a minor issue that does not affect core behavior, integration, validation, or generated code correctness.

Low severity gaps usually affect readability, naming consistency, formatting, or documentation quality.

---

## When to Use Low

Use `Low` when the gap affects:

* Minor naming differences
* Formatting inconsistencies
* Non-critical documentation mismatch
* Redundant descriptions
* Slightly unclear wording
* Minor metadata gaps

---

## Examples

### Example 1: Naming Style Mismatch

```text
Confluence uses Division-id, Node-RED uses divisionNbr, and Excel uses division_id.
```

Severity:

```text
Low
```

Reason:

```text
The concepts are equivalent, but naming should be normalized for clarity.
```

---

### Example 2: Minor Description Difference

```text
The business rule description in Excel is shorter than the Confluence description but still captures the core logic.
```

Severity:

```text
Low
```

Reason:

```text
The requirement is not lost, but documentation quality can be improved.
```

---

### Example 3: Formatting Issue

```text
The Excel workbook stores enum values as comma-separated text instead of a clean list format.
```

Severity:

```text
Low
```

Reason:

```text
The information exists, but formatting can be improved.
```

---

## Low Fix Rule

Low severity gaps should not block the pipeline.

They should be corrected during cleanup if time permits.

---

# 5. Informational Severity

## Definition

An informational gap is not necessarily an error.

It is a note, assumption, observation, or item that may need future clarification.

Informational findings help reviewers understand uncertainty without treating it as a defect.

---

## When to Use Informational

Use `Informational` when the finding is:

* An assumption needing confirmation
* A possible enhancement
* A non-blocking observation
* A difference that may be intentional
* A note for future review
* A warning without immediate impact

---

## Examples

### Example 1: Assumption Added

```text
Excel assumes a default timeout of 30 seconds for a downstream service, but Confluence does not mention timeout behavior.
```

Severity:

```text
Informational
```

Reason:

```text
This may be acceptable, but it should be confirmed.
```

---

### Example 2: Future Enhancement

```text
The framework could capture retry behavior for external services, but the current requirement does not define it.
```

Severity:

```text
Informational
```

Reason:

```text
This is useful for future improvement but not a current defect.
```

---

### Example 3: Manual Review Needed

```text
The Confluence wording is ambiguous about whether a field is optional or conditionally required.
```

Severity:

```text
Informational
```

Reason:

```text
The framework cannot confidently classify it as a defect without human review.
```

---

## Informational Fix Rule

Informational findings do not block the pipeline.

They should be reviewed only when clarification or future enhancement is needed.

---

# Severity Decision Matrix

The following matrix can be used to assign severity.

| Question                                                                               | Severity      |
| -------------------------------------------------------------------------------------- | ------------- |
| Does the gap make the API endpoint missing or unusable?                                | Critical      |
| Does the gap use the wrong target technology or database?                              | Critical      |
| Does the gap remove core business logic?                                               | Critical      |
| Does the gap remove a required downstream service or database operation?               | Critical      |
| Does the gap affect required validation, required headers, or required request fields? | High          |
| Does the gap cause incorrect response or service mapping?                              | High          |
| Does the gap affect optional fields, tracing, or partial completeness?                 | Medium        |
| Does the gap mainly affect naming, formatting, or documentation clarity?               | Low           |
| Is the finding only an assumption, note, or future enhancement?                        | Informational |

---

# Severity Assignment Rules

## Rule 1: Business Logic Comes First

If a gap affects core business behavior, it should be classified as either `Critical` or `High`.

Example:

```text
Employee discount logic is missing from Excel.
```

Recommended severity:

```text
Critical
```

---

## Rule 2: Required Contract Elements Are High or Critical

If a required API contract element is missing, the severity should usually be `High`.

If the entire endpoint is missing, the severity should be `Critical`.

Example:

```text
Required request field account.entryMode is missing.
```

Recommended severity:

```text
High
```

---

## Rule 3: Technology Mismatch Is Critical

If the artifact targets the wrong technology stack, database, or code generation environment, the severity should be `Critical`.

Example:

```text
Workbook says SQLite, but target is PostgreSQL.
```

Recommended severity:

```text
Critical
```

---

## Rule 4: Optional Elements Are Usually Medium

If an optional field or non-critical metadata item is missing, the severity should usually be `Medium`.

Example:

```text
Optional traceId header is missing from Excel.
```

Recommended severity:

```text
Medium
```

---

## Rule 5: Naming Differences Are Usually Low

If the same concept exists across sources but uses different names, the severity should usually be `Low`, unless the mismatch causes incorrect mapping.

Example:

```text
Division-id, divisionNbr, and division_id refer to the same value.
```

Recommended severity:

```text
Low
```

If the mismatch causes incorrect generated DTO or mapping logic, raise severity to `High`.

---

## Rule 6: Ambiguity Should Be Informational Until Confirmed

If the framework is unsure whether something is a true issue, it should be marked as `Informational` or assigned lower confidence.

Example:

```text
Confluence does not clearly state whether the field is optional or conditionally required.
```

Recommended severity:

```text
Informational
```

---

# Severity and Confidence

Severity and confidence are separate fields.

Severity describes the impact of the gap.

Confidence describes how certain the framework is that the gap is real.

Example:

```json
{
  "gap_id": "GAP-004",
  "severity": "High",
  "confidence": "Medium",
  "description": "The request field appears to be required in Confluence, but the wording is partially ambiguous."
}
```

This means the gap may have high impact, but the system is only moderately confident.

---

# Pipeline Blocking Rules

The following rules define whether a gap should block the pipeline.

| Severity      | Blocks Code Generation?                   | Review Required? |
| ------------- | ----------------------------------------- | ---------------- |
| Critical      | Yes                                       | Yes              |
| High          | Usually yes                               | Yes              |
| Medium        | No, unless repeated or business-impacting | Yes              |
| Low           | No                                        | Optional         |
| Informational | No                                        | Optional         |

---

# Recommended Severity Summary

| Severity      | Meaning                                                               | Typical Action           |
| ------------- | --------------------------------------------------------------------- | ------------------------ |
| Critical      | Breaks core API, business logic, integration, or technology alignment | Must fix immediately     |
| High          | Causes incorrect validation, mapping, contract, or important behavior | Fix before final output  |
| Medium        | Affects completeness, optional behavior, or maintainability           | Review and fix if needed |
| Low           | Minor naming, formatting, or documentation issue                      | Cleanup when possible    |
| Informational | Note, assumption, ambiguity, or future improvement                    | Review only if needed    |

---

# Example Severity Report

```json
{
  "severity_summary": {
    "critical": 1,
    "high": 3,
    "medium": 2,
    "low": 4,
    "informational": 2
  },
  "blocking_status": "Blocked",
  "blocking_reason": "One critical gap exists: target database mismatch between Confluence and Excel."
}
```

---

# Summary

Severity levels help the Gap Analysis Framework prioritize detected issues.

Critical and High gaps protect the correctness of the generated API.

Medium gaps improve completeness and maintainability.

Low gaps improve clarity and consistency.

Informational findings capture assumptions, ambiguity, and future improvements.

By using severity levels consistently, the framework can produce gap reports that are not only detailed, but also practical and actionable.
