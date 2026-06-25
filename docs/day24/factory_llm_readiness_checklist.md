# Day 24 — Phase 3: Factory LLM Readiness Checklist

## Purpose

This document defines the readiness checklist for using a factory LLM to generate implementation artifacts from structured API requirements.

The goal of this phase is to verify whether the requirement artifacts contain enough clear, structured, and traceable information for a factory LLM to generate accurate code without hallucinating missing details.

A factory LLM should not be treated as a magic code generator. It needs strong input contracts, clear workflow steps, exact mappings, validation rules, error behavior, and approved assumptions.

This checklist ensures that before any code generation happens, the inputs are complete enough to reduce incorrect implementation risk.

---

## Why Factory LLM Readiness Matters

A factory LLM can only generate reliable code if the input is reliable.

If the requirement package is vague, incomplete, or inconsistent, the generated code may:

* Add fields that were never requested
* Skip validations
* Mis-handle downstream calls
* Map response fields incorrectly
* Use the wrong database table
* Invent business rules
* Ignore error scenarios
* Generate code that looks correct but does not match the requirement

The readiness checklist prevents this by forcing every requirement artifact to be reviewed before code generation.

---

## Factory LLM Input Package

Before a factory LLM is used, the following artifacts should be available:

```text
1. Original requirement document
2. Requirement extraction JSON
3. Workflow JSON
4. Excel intake workbook
5. Gap analysis report
6. Approved assumptions list
7. Target framework instructions
8. Example request and response payloads
```

For Day 25, only the first working version of requirement extraction will be implemented. However, this checklist defines the full readiness standard for future days.

---

# 1. Original Requirement Readiness

## Purpose

The original requirement document is the source of truth.

The factory LLM should never rely only on summarized assumptions if the original requirement is available.

---

## Checklist

* [ ] Requirement document exists
* [ ] Requirement ID is available
* [ ] API purpose is clearly described
* [ ] Business owner or source context is known, if available
* [ ] Endpoint path is mentioned
* [ ] HTTP method is mentioned
* [ ] Request format is described
* [ ] Response format is described
* [ ] Business rules are documented
* [ ] Validation rules are documented
* [ ] Error scenarios are documented
* [ ] Dependencies are documented
* [ ] Database usage is documented, if applicable
* [ ] Open questions are separated from confirmed requirements

---

## Readiness Decision

```text
READY only if the requirement document contains enough information to identify the API contract and core behavior.
```

If endpoint, method, request, response, or business logic is missing, the requirement should be marked as partially ready or not ready.

---

# 2. Requirement Extraction JSON Readiness

## Purpose

The extraction JSON converts the original requirement into structured machine-readable data.

This is the first major input the factory LLM can use.

---

## Required Fields

The extraction JSON should include:

```json
{
  "requirement_id": "",
  "source_file": "",
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
  "open_questions": [],
  "extraction_metadata": {}
}
```

---

## Checklist

* [ ] JSON is valid
* [ ] Requirement ID is present
* [ ] Source file is traceable
* [ ] API name is captured
* [ ] Endpoint path is captured
* [ ] HTTP method is captured
* [ ] Request headers are listed
* [ ] Request body fields are listed
* [ ] Response fields are listed
* [ ] Business rules are listed
* [ ] Validation rules are listed
* [ ] Downstream dependencies are listed
* [ ] Database dependencies are listed
* [ ] Error scenarios are listed
* [ ] Assumptions are clearly marked
* [ ] Open questions are clearly marked
* [ ] Missing information is not hallucinated
* [ ] Extraction metadata is included

---

## Quality Checks

The extraction JSON should be checked for:

* Empty required fields
* Incorrect field names
* Duplicated rules
* Vague descriptions
* Mixed business rules and validation rules
* Missing source traceability
* Missing open questions
* Unsupported assumptions

---

## Readiness Decision

```text
READY only if the extraction JSON is valid, traceable, and complete enough to support workflow generation.
```

If too many fields are empty, the extraction is not ready for factory LLM usage.

---

# 3. API Contract Readiness

## Purpose

The API contract defines how clients will call the service and what response they will receive.

A factory LLM must have a precise API contract to generate handlers, routes, DTOs, validation logic, and response mapping.

---

## Checklist

### Endpoint

* [ ] Endpoint path is available
* [ ] HTTP method is available
* [ ] Base path is clear
* [ ] Versioning is clear, if applicable
* [ ] Path parameters are identified
* [ ] Query parameters are identified
* [ ] Request body requirement is clear

---

### Headers

* [ ] Required headers are listed
* [ ] Optional headers are listed
* [ ] Header names are exact
* [ ] Header casing expectations are documented, if needed
* [ ] Security headers are identified
* [ ] Trace/correlation headers are identified

---

### Request Body

* [ ] Request body schema is available
* [ ] Nested objects are represented clearly
* [ ] Arrays are represented clearly
* [ ] Required fields are marked
* [ ] Optional fields are marked
* [ ] Field types are documented
* [ ] Field examples are available where possible
* [ ] Enum values are listed where applicable
* [ ] Numeric constraints are documented where applicable
* [ ] Date/time formats are documented where applicable

---

### Response Body

* [ ] Success response schema is available
* [ ] Error response schema is available
* [ ] Nested response objects are clear
* [ ] Array response fields are clear
* [ ] Response field types are documented
* [ ] Response examples are available
* [ ] Nullability expectations are documented
* [ ] Empty array behavior is documented
* [ ] Field mapping source is clear

---

## Readiness Decision

```text
READY only if the factory LLM can generate route, DTO, validation, and response mapping code without guessing field names.
```

---

# 4. Workflow Readiness

## Purpose

The workflow describes the API logic as ordered processing steps.

This helps the factory LLM generate service orchestration logic correctly.

---

## Expected Workflow Step Structure

```json
{
  "step_id": "STEP-001",
  "step_name": "Validate Request",
  "step_type": "validation",
  "description": "",
  "inputs": [],
  "outputs": [],
  "rules": [],
  "dependencies": [],
  "error_handling": []
}
```

---

## Checklist

* [ ] Workflow ID is available
* [ ] Requirement ID is linked
* [ ] API name is linked
* [ ] Endpoint and method are linked
* [ ] Steps are ordered correctly
* [ ] Each step has a clear purpose
* [ ] Each step has a valid step type
* [ ] Inputs are defined for each step
* [ ] Outputs are defined for each step
* [ ] Business rules are attached to the correct step
* [ ] Validations are separated from business rules
* [ ] Downstream calls are represented as separate steps
* [ ] Database operations are represented as separate steps
* [ ] Response mapping is represented as a separate step
* [ ] Error handling is defined where applicable

---

## Step Type Checklist

The workflow should use clear step types such as:

* [ ] input_normalization
* [ ] validation
* [ ] transformation
* [ ] downstream_call
* [ ] database_read
* [ ] database_write
* [ ] business_rule
* [ ] response_mapping
* [ ] error_handling

---

## Readiness Decision

```text
READY only if the workflow can be followed by an engineer or LLM without needing to infer hidden logic.
```

---

# 5. Business Rule Readiness

## Purpose

Business rules define the actual behavior of the API.

A factory LLM must receive business rules in a clear, atomic, and testable format.

---

## Checklist

* [ ] Each business rule is written separately
* [ ] Each rule has a clear condition
* [ ] Each rule has a clear outcome
* [ ] Rule priority is documented if order matters
* [ ] Rule conflicts are identified
* [ ] Default behavior is documented
* [ ] Edge cases are documented
* [ ] Calculations are documented
* [ ] Configuration values are identified
* [ ] Rules are linked to request fields, database fields, or downstream response fields where applicable

---

## Good Business Rule Example

```text
If the account is classified as a new account, calculate the discount using the configured new account percentage and ensure the discounted amount does not exceed the remaining cap amount.
```

---

## Poor Business Rule Example

```text
Apply discount.
```

The poor example is not ready because it does not explain the condition, calculation, or limit.

---

## Readiness Decision

```text
READY only if business rules are specific enough to generate service logic and test cases.
```

---

# 6. Validation Readiness

## Purpose

Validation rules define what must be checked before business logic runs.

The factory LLM must know exactly which fields are required and what constraints apply.

---

## Checklist

* [ ] Required request headers are listed
* [ ] Required path parameters are listed
* [ ] Required query parameters are listed
* [ ] Required body fields are listed
* [ ] Optional fields are clearly marked
* [ ] Enum values are listed
* [ ] Numeric range checks are defined
* [ ] String length checks are defined, if applicable
* [ ] Date format checks are defined, if applicable
* [ ] Conditional validations are documented
* [ ] Validation error response format is documented
* [ ] Validation error status code is documented

---

## Conditional Validation Example

```text
If keyType is RSA, then entryMode must be present.
```

---

## Readiness Decision

```text
READY only if the factory LLM can generate validation code without inventing required fields.
```

---

# 7. Downstream Dependency Readiness

## Purpose

Downstream dependencies are external services that the API calls.

The factory LLM needs exact dependency details to generate clients, request builders, response parsing, and error handling.

---

## Checklist

* [ ] Downstream service name is known
* [ ] Downstream endpoint is known
* [ ] Downstream method is known
* [ ] Downstream request headers are known
* [ ] Downstream request body is known
* [ ] Downstream response body is known
* [ ] Request mapping from API input to downstream input is documented
* [ ] Response mapping from downstream output to API output is documented
* [ ] Timeout behavior is documented, if available
* [ ] Retry behavior is documented, if available
* [ ] Authentication requirements are documented
* [ ] Error handling behavior is documented
* [ ] Fallback behavior is documented, if applicable

---

## Readiness Decision

```text
READY only if the factory LLM can generate the downstream client without guessing request or response mapping.
```

---

# 8. Database Readiness

## Purpose

Database requirements define which tables are read or updated and how fields are mapped.

The factory LLM needs this information to generate repository logic correctly.

---

## Checklist

* [ ] Database name or system is identified
* [ ] Table names are listed
* [ ] Operation type is listed: SELECT, INSERT, UPDATE, DELETE
* [ ] Lookup keys are documented
* [ ] Insert fields are documented
* [ ] Update fields are documented
* [ ] Response/output mapping is documented
* [ ] Transaction behavior is documented, if applicable
* [ ] Row locking requirement is documented, if applicable
* [ ] Commit and rollback behavior is documented, if applicable
* [ ] Null handling is documented
* [ ] Default values are documented
* [ ] Error behavior is documented

---

## Readiness Decision

```text
READY only if the factory LLM can generate repository methods and SQL mapping without inventing table or column names.
```

---

# 9. Error Handling Readiness

## Purpose

Error handling defines how the API should respond when something fails.

A factory LLM needs this to avoid inconsistent or incomplete error responses.

---

## Checklist

* [ ] Validation errors are documented
* [ ] Authentication errors are documented
* [ ] Authorization errors are documented, if applicable
* [ ] Downstream timeout errors are documented
* [ ] Downstream failure errors are documented
* [ ] Database not found errors are documented
* [ ] Database update errors are documented
* [ ] Business rule failure errors are documented
* [ ] Default internal error behavior is documented
* [ ] Status codes are listed
* [ ] Error response schema is listed
* [ ] Error messages are listed or message strategy is documented

---

## Readiness Decision

```text
READY only if the factory LLM can generate consistent error handling without inventing response formats.
```

---

# 10. Excel Intake Readiness

## Purpose

The Excel intake workbook is the structured artifact used by the factory LLM to generate code.

It must be clean, consistent, and machine-readable.

---

## Expected Sheets

* [ ] api_endpoint
* [ ] request_headers
* [ ] request_body
* [ ] response_body
* [ ] business_rules
* [ ] validations
* [ ] downstream_dependencies
* [ ] database_mapping
* [ ] error_handling
* [ ] open_questions

---

## Checklist

* [ ] All required sheets exist
* [ ] Sheet names are exact
* [ ] Column names are consistent
* [ ] No merged cells are used
* [ ] One row represents one field, rule, mapping, or dependency
* [ ] Required/optional indicators are clear
* [ ] Field paths use consistent dot notation
* [ ] Data types are listed
* [ ] Source-to-target mappings are clear
* [ ] Assumptions are explicitly marked
* [ ] Open questions are included
* [ ] Empty values are intentional and traceable

---

## Readiness Decision

```text
READY only if the Excel can be consumed by a factory LLM without manual interpretation.
```

---

# 11. Gap Analysis Readiness

## Purpose

Gap analysis identifies missing or unclear requirement details before implementation.

The factory LLM should not generate final code from unresolved critical gaps.

---

## Checklist

* [ ] Critical gaps are listed
* [ ] Medium gaps are listed
* [ ] Low gaps are listed
* [ ] Open questions are listed
* [ ] Unsupported assumptions are listed
* [ ] Conflicting requirements are listed
* [ ] Missing endpoint details are identified
* [ ] Missing request details are identified
* [ ] Missing response details are identified
* [ ] Missing database mappings are identified
* [ ] Missing downstream mappings are identified
* [ ] Missing error behavior is identified
* [ ] Final implementation recommendation is provided

---

## Gap Severity Rules

| Severity | Meaning                              |
| -------- | ------------------------------------ |
| Critical | Blocks correct implementation        |
| Medium   | Implementation can proceed with risk |
| Low      | Minor clarification needed           |

---

## Readiness Decision

```text
READY only if there are no unresolved critical gaps.
```

---

# 12. Target Framework Readiness

## Purpose

The factory LLM must know the target code framework before generating implementation.

Without this, it may generate code in the wrong structure or style.

---

## Checklist

* [ ] Programming language is defined
* [ ] Framework is defined
* [ ] Project folder structure is defined
* [ ] Handler/controller pattern is defined
* [ ] Service layer pattern is defined
* [ ] Repository layer pattern is defined
* [ ] DTO/model pattern is defined
* [ ] Client pattern is defined
* [ ] Config pattern is defined
* [ ] Error handling pattern is defined
* [ ] Logging pattern is defined
* [ ] Test pattern is defined
* [ ] Naming conventions are defined

---

## Example Target Framework Package

```text
Language: Go
Framework: Gin or company base API framework
Layers:
- handler
- service
- repository
- client
- dto
- config
- errors
```

---

## Readiness Decision

```text
READY only if the factory LLM knows exactly where each generated component should go.
```

---

# 13. Approved Assumptions Readiness

## Purpose

Assumptions must be explicitly approved before implementation.

The factory LLM should not silently convert guesses into code.

---

## Checklist

* [ ] Assumptions are listed separately
* [ ] Each assumption has a reason
* [ ] Each assumption has an approval status
* [ ] Risky assumptions are marked
* [ ] Assumptions are not mixed with confirmed requirements
* [ ] Unapproved assumptions are moved to open questions
* [ ] The final factory input clearly separates confirmed details from assumed details

---

## Example Approved Assumption Format

```json
{
  "assumption": "Missing optional fields will be represented as empty strings or empty arrays.",
  "reason": "Required for consistent JSON output.",
  "approval_status": "approved",
  "risk_level": "low"
}
```

---

## Readiness Decision

```text
READY only if all assumptions used for generation are approved or marked as low-risk implementation defaults.
```

---

# 14. Factory LLM No-Hallucination Rules

## Purpose

The factory LLM must be constrained from inventing implementation details.

---

## The Factory LLM Must Not Invent

* [ ] New API endpoints
* [ ] New request fields
* [ ] New response fields
* [ ] New headers
* [ ] New database tables
* [ ] New database columns
* [ ] New downstream APIs
* [ ] New business rules
* [ ] New validation rules
* [ ] New error response structures
* [ ] New configuration keys
* [ ] New authentication behavior
* [ ] New retry behavior
* [ ] New fallback behavior

---

## Required Behavior for Missing Details

If information is missing, the factory LLM should:

```text
1. Mark the detail as missing
2. Add it to the gap analysis report
3. Avoid generating unsupported logic
4. Use safe placeholders only when explicitly allowed
```

---

# 15. Overall Factory LLM Readiness Score

A readiness score can be used to decide whether code generation should proceed.

## Scoring Model

| Area                            | Weight |
| ------------------------------- | -----: |
| API contract completeness       |    20% |
| Request/response schema clarity |    20% |
| Business rule clarity           |    15% |
| Validation clarity              |    10% |
| Downstream dependency clarity   |    10% |
| Database mapping clarity        |    10% |
| Error handling clarity          |    10% |
| Approved assumptions and gaps   |     5% |

---

## Readiness Levels

|    Score | Status          | Meaning                                     |
| -------: | --------------- | ------------------------------------------- |
|   90–100 | Ready           | Safe for factory LLM generation             |
|    75–89 | Mostly Ready    | Can generate prototype, but review required |
|    50–74 | Partially Ready | Too risky for full generation               |
| Below 50 | Not Ready       | Requirement must be clarified first         |

---

# 16. Day 25 Readiness Focus

For Day 25, the factory LLM readiness focus is not full code generation yet.

Day 25 should focus only on making sure the requirement extraction output can support future factory usage.

The Day 25 implementation should therefore satisfy these minimum readiness items:

* [ ] Source requirement file can be loaded
* [ ] Requirement ID can be captured
* [ ] Endpoint can be extracted if present
* [ ] Method can be extracted if present
* [ ] Request-related details can be grouped
* [ ] Response-related details can be grouped
* [ ] Business rules can be grouped
* [ ] Validation rules can be grouped
* [ ] Dependencies can be grouped
* [ ] Missing information can be captured as open questions
* [ ] Output JSON follows a stable structure
* [ ] Output can be reused by later workflow and Excel modules

---

# 17. Final Readiness Checklist

Before sending an artifact package to a factory LLM, confirm:

* [ ] Original requirement is available
* [ ] Extraction JSON is complete and valid
* [ ] Workflow JSON is complete and ordered
* [ ] Excel intake is machine-readable
* [ ] Gap analysis has no unresolved critical gaps
* [ ] Business rules are atomic and testable
* [ ] Validations are explicit
* [ ] Request and response mappings are clear
* [ ] Downstream dependencies are documented
* [ ] Database mappings are documented
* [ ] Error behavior is documented
* [ ] Target framework is defined
* [ ] Approved assumptions are separated
* [ ] Open questions are not hidden
* [ ] Factory LLM no-hallucination rules are included

---

## Final Summary

The factory LLM readiness checklist defines the minimum standard required before AI-generated implementation can be trusted.

The key principle is simple:

```text
The factory LLM should generate code only from confirmed, structured, and traceable requirements.
```

If information is missing, the system should flag the gap instead of inventing details.

This checklist will guide future implementation phases and help ensure that generated code stays aligned with the original enterprise API requirement.

---
