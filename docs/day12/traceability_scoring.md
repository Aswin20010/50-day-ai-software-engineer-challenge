# Day 12 — Traceability Scoring

## Purpose

This document defines how to score traceability in a generated Workflow Schema v2 output.

Traceability measures whether extracted schema elements can be traced back to the original requirement source.

No coding is performed in this phase.

---

## Background

Day 10 created Workflow Schema v2.

Day 11 defined how raw requirements should map into Workflow Schema v2.

Day 12 defines how to evaluate whether the extracted schema is complete, accurate, traceable, and generator-ready.

Phase 2 defined schema completeness scoring.

Phase 3 defined requirement accuracy scoring.

Phase 4 focuses on source traceability scoring.

---

## What Traceability Means

Traceability answers the question:

```text
Can we prove where this extracted schema detail came from?
```

A generated schema is more trustworthy when important extracted items include references to the original source.

These references may include:

* Requirement document name
* Requirement section
* Source text reference
* Source type
* Confidence level
* Notes explaining inferred values

Traceability does not only help review.

It also supports research evaluation by showing how much of the generated schema is grounded in the original requirements.

---

## Why Traceability Matters

Traceability matters because generated schemas may contain:

* Correct details
* Incorrect details
* Inferred details
* Unsupported assumptions
* Missing source references
* Prototype-based details
* User-confirmed clarifications

Without traceability, it is difficult to know which details are grounded and which details need review.

For example:

```text
Division-Id is required and must be either 12 or 13.
```

If the schema includes this validation rule, the reviewer should know where it came from.

A strong traceability record may say:

```yaml
source_traceability:
  field_sources:
    - field_path: request.headers.Division-Id
      requirement_section: Header Requirements
      source_text_reference: Section 2.2
      confidence: high
```

This makes the extracted schema easier to audit.

---

# Traceability Scoring Objective

Traceability scoring evaluates whether the generated Workflow Schema v2 includes enough source references to support human review and research evaluation.

It checks:

```text
- Is the workflow linked to the original requirement source?
- Are important fields linked to requirement sections?
- Are workflow steps linked to requirement sections?
- Are business rules linked to requirement sections?
- Are external services linked to requirement sections?
- Are database references linked to requirement sections?
- Are inferred values clearly marked?
- Are confidence levels assigned correctly?
```

---

# Schema v2 Block Used

Traceability scoring focuses mainly on:

```text
source_traceability
```

However, traceability should support every important block:

```text
metadata
api_contract
field_validations
workflow_steps
branching
external_services
authentication
database_references
business_rules
calculation_rules
request_mappings
response_mappings
error_mappings
test_scenarios
generator_readiness
```

---

# Source Traceability Structure

The recommended Schema v2 traceability structure is:

```yaml
source_traceability:
  workflow_source:
    requirement_doc: string
    requirement_section: string
    source_text_reference: string
    confidence: high | medium | low

  step_sources:
    - step_id: string
      requirement_section: string
      source_text_reference: string
      confidence: high | medium | low

  field_sources:
    - field_path: string
      requirement_section: string
      source_text_reference: string
      confidence: high | medium | low

  business_rule_sources:
    - rule_id: string
      requirement_section: string
      source_text_reference: string
      confidence: high | medium | low
```

Schema v2 may later expand this to include:

```yaml
service_sources:
  - service_id: string
    requirement_section: string
    source_text_reference: string
    confidence: high | medium | low

query_sources:
  - query_id: string
    requirement_section: string
    source_text_reference: string
    confidence: high | medium | low

mapping_sources:
  - mapping_id: string
    requirement_section: string
    source_text_reference: string
    confidence: high | medium | low

error_sources:
  - error_mapping_id: string
    requirement_section: string
    source_text_reference: string
    confidence: high | medium | low
```

---

# Traceability Scoring Categories

Traceability should be scored across these categories:

```text
1. Workflow Source Traceability
2. API Contract Traceability
3. Field Validation Traceability
4. Workflow Step Traceability
5. Branching Traceability
6. External Service Traceability
7. Authentication Traceability
8. Database Reference Traceability
9. Business Rule Traceability
10. Calculation Rule Traceability
11. Mapping Traceability
12. Error Mapping Traceability
13. Confidence Quality
14. Inference Transparency
```

---

# Recommended Scoring Scale

Use a 100-point scale.

```text
90–100 = Excellent
80–89  = Good
70–79  = Acceptable with review
60–69  = Weak, needs correction
Below 60 = Not ready
```

---

# Default Traceability Score Distribution

| Traceability Area               |  Points |
| ------------------------------- | ------: |
| Workflow Source Traceability    |       8 |
| API Contract Traceability       |       8 |
| Field Validation Traceability   |      12 |
| Workflow Step Traceability      |      12 |
| Branching Traceability          |       7 |
| External Service Traceability   |       7 |
| Authentication Traceability     |       5 |
| Database Reference Traceability |       7 |
| Business Rule Traceability      |      10 |
| Calculation Rule Traceability   |       5 |
| Mapping Traceability            |       7 |
| Error Mapping Traceability      |       5 |
| Confidence Quality              |       4 |
| Inference Transparency          |       3 |
| **Total**                       | **100** |

---

# Important Scoring Rule

If a category is not applicable to the workflow, its points should be redistributed across applicable categories.

Example:

A simple wrapper API may not use:

```text
database_references
calculation_rules
```

Those traceability categories should be marked:

```text
not_applicable
```

and the final score should be normalized.

Use this formula:

```text
Traceability Score = (Earned Applicable Points / Total Applicable Points) * 100
```

---

# Traceability Status Values

Each area can receive one of the following statuses:

```text
strong
adequate
weak
missing
not_applicable
```

## strong

Use when source references are specific and confidence is appropriate.

## adequate

Use when source references exist but are somewhat broad.

## weak

Use when references are vague, incomplete, or mostly inferred.

## missing

Use when traceability should exist but is absent.

## not_applicable

Use when the traceability category does not apply to the workflow.

---

# 1. Workflow Source Traceability

## Purpose

Workflow source traceability identifies the main requirement source for the entire workflow.

## Required Fields

```text
requirement_doc
requirement_section
source_text_reference
confidence
```

## Scoring

| Quality                                                                 | Points |
| ----------------------------------------------------------------------- | -----: |
| Requirement document, section, source reference, and confidence present |      8 |
| Requirement document and section present, reference broad               |    6–7 |
| Only document name present                                              |    3–5 |
| Workflow source missing                                                 |      0 |

## Example Strong Traceability

```yaml
source_traceability:
  workflow_source:
    requirement_doc: Fabric API Requirement
    requirement_section: API Overview
    source_text_reference: Section 1.1
    confidence: high
```

---

# 2. API Contract Traceability

## Purpose

API contract traceability links endpoint details to the requirement source.

## Items to Trace

```text
endpoint path
HTTP method
API version
operation name
content type
response type
success status
error status
```

## Scoring

| Quality                                                  | Points |
| -------------------------------------------------------- | -----: |
| All major API contract items traceable                   |      8 |
| Endpoint and method traceable, minor metadata not traced |    6–7 |
| Only endpoint traceable                                  |    3–5 |
| API contract traceability missing                        |      0 |

## Example

```yaml
source_traceability:
  api_contract_sources:
    - field_path: api_contract.path
      requirement_section: API Contract
      source_text_reference: Section 2.1
      confidence: high

    - field_path: api_contract.method
      requirement_section: API Contract
      source_text_reference: Section 2.1
      confidence: high
```

---

# 3. Field Validation Traceability

## Purpose

Field validation traceability links request fields and validation rules to their source.

## Items to Trace

```text
required headers
optional headers
body fields
path parameters
query parameters
enum rules
array rules
nested object rules
format rules
conditional validations
```

## Scoring

| Quality                                             | Points |
| --------------------------------------------------- | -----: |
| All important validation rules traceable            |     12 |
| Most required fields traceable, minor optional gaps |   9–11 |
| Some required fields traceable, several gaps        |    5–8 |
| Validation traceability very weak                   |    1–4 |
| Field validation traceability missing               |      0 |

## Example Strong Traceability

```yaml
source_traceability:
  field_sources:
    - field_path: request.body.skuNumbers
      requirement_section: Request Body
      source_text_reference: Section 2.3
      confidence: high

    - field_path: request.body.skuNumbers[]
      requirement_section: Request Body
      source_text_reference: Section 2.3
      confidence: high
```

---

# 4. Workflow Step Traceability

## Purpose

Workflow step traceability links each major workflow step to requirement evidence.

## Items to Trace

```text
validation steps
mapping steps
external service steps
database steps
business rule steps
calculation steps
response mapping steps
error mapping steps
branching steps
context preparation steps
```

## Scoring

| Quality                                    | Points |
| ------------------------------------------ | -----: |
| All major workflow steps traceable         |     12 |
| Most major workflow steps traceable        |   9–11 |
| Only high-level steps traceable            |    5–8 |
| Step traceability vague or mostly inferred |    1–4 |
| Workflow step traceability missing         |      0 |

## Example

```yaml
source_traceability:
  step_sources:
    - step_id: STEP-004
      requirement_section: Downstream Service Integration
      source_text_reference: Section 3.1
      confidence: high
```

---

# 5. Branching Traceability

## Purpose

Branching traceability links conditional paths to requirement evidence.

## Items to Trace

```text
branch condition
true path
false path
merge path
branch source step
```

## Scoring

| Quality                                               |              Points |
| ----------------------------------------------------- | ------------------: |
| All branch conditions and paths traceable             |                   7 |
| Branch condition traceable, paths partially traceable |                 5–6 |
| Branching generally traceable but vague               |                 3–4 |
| Required branching traceability missing               |                   0 |
| Not applicable                                        | redistribute points |

## Example

```yaml
source_traceability:
  branch_sources:
    - branch_id: BRANCH-DISCOUNT-001
      requirement_section: Discount Path Selection
      source_text_reference: Section 4.1
      confidence: high
```

---

# 6. External Service Traceability

## Purpose

External service traceability links downstream service details to the requirement source.

## Items to Trace

```text
service name
service type
endpoint reference
HTTP method
SOAP operation
request source
response usage
timeout behavior
error behavior
```

## Scoring

| Quality                                              |              Points |
| ---------------------------------------------------- | ------------------: |
| All downstream service details traceable             |                   7 |
| Service and endpoint traceable, minor metadata broad |                 5–6 |
| Service name traceable but endpoint or method vague  |                 3–4 |
| Required service traceability missing                |                   0 |
| Not applicable                                       | redistribute points |

## Example

```yaml
source_traceability:
  service_sources:
    - service_id: SRV-FABRIC-CUSTOMIZATION
      requirement_section: Downstream Service
      source_text_reference: Section 3.2
      confidence: high
```

---

# 7. Authentication Traceability

## Purpose

Authentication traceability links security and downstream auth details to source evidence.

## Items to Trace

```text
auth type
required headers
API key source
authorization format
signature algorithm
signature payload source
secret source
```

## Scoring

| Quality                                                  |              Points |
| -------------------------------------------------------- | ------------------: |
| Authentication details fully traceable                   |                   5 |
| Auth type and headers traceable, signature details broad |                 3–4 |
| Auth mentioned but weak source reference                 |                 1–2 |
| Required authentication traceability missing             |                   0 |
| Not applicable                                           | redistribute points |

## Example

```yaml
source_traceability:
  authentication_sources:
    - auth_id: AUTH-INTERROGATE-001
      requirement_section: Downstream Authentication
      source_text_reference: Section 3.3
      confidence: high
```

---

# 8. Database Reference Traceability

## Purpose

Database reference traceability links table and query usage to source evidence.

## Items to Trace

```text
database type
table names
query purpose
query inputs
query outputs
no-record behavior
failure behavior
```

## Scoring

| Quality                                            |              Points |
| -------------------------------------------------- | ------------------: |
| All database references traceable                  |                   7 |
| Tables and purpose traceable, inputs/outputs broad |                 5–6 |
| Table names traceable but query behavior vague     |                 3–4 |
| Required database traceability missing             |                   0 |
| Not applicable                                     | redistribute points |

## Example

```yaml
source_traceability:
  query_sources:
    - query_id: SQL-EMP-001
      requirement_section: Employee Account Lookup
      source_text_reference: Section 5.1
      confidence: high
```

---

# 9. Business Rule Traceability

## Purpose

Business rule traceability links business decisions and eligibility logic to requirement evidence.

## Items to Trace

```text
rule condition
rule category
inputs
outputs
related queries
success behavior
failure behavior
```

## Scoring

| Quality                                            |              Points |
| -------------------------------------------------- | ------------------: |
| All important business rules traceable             |                  10 |
| Main business rules traceable, minor details broad |                 8–9 |
| Some business rules traceable, several gaps        |                 5–7 |
| Business rule traceability weak                    |                 1–4 |
| Required business rule traceability missing        |                   0 |
| Not applicable                                     | redistribute points |

## Example

```yaml
source_traceability:
  business_rule_sources:
    - rule_id: BR-EMP-001
      requirement_section: Employee Discount Eligibility
      source_text_reference: Section 4.1
      confidence: high
```

---

# 10. Calculation Rule Traceability

## Purpose

Calculation traceability links formulas, percentages, caps, and derived values to source evidence.

## Items to Trace

```text
calculation type
inputs
outputs
formula behavior
cap behavior
rounding behavior
item-level behavior
```

## Scoring

| Quality                                                     |              Points |
| ----------------------------------------------------------- | ------------------: |
| All calculation rules traceable                             |                   5 |
| Main calculation traceable, minor rounding/cap detail broad |                 3–4 |
| Calculation mentioned but source vague                      |                 1–2 |
| Required calculation traceability missing                   |                   0 |
| Not applicable                                              | redistribute points |

## Example

```yaml
source_traceability:
  calculation_sources:
    - calculation_id: CALC-DISCOUNT-001
      requirement_section: Discount Calculation
      source_text_reference: Section 4.4
      confidence: high
```

---

# 11. Mapping Traceability

## Purpose

Mapping traceability links request and response transformations to source evidence.

## Items to Trace

```text
request source field
request target field
response source field
response target field
transformation rule
optional pass-through behavior
```

## Scoring

| Quality                                                | Points |
| ------------------------------------------------------ | -----: |
| All important request and response mappings traceable  |      7 |
| Main mappings traceable, minor optional mappings broad |    5–6 |
| Mapping exists but source evidence weak                |    3–4 |
| Required mapping traceability missing                  |      0 |

## Example

```yaml
source_traceability:
  mapping_sources:
    - mapping_id: MAP-FABRIC-001
      requirement_section: Downstream Request Mapping
      source_text_reference: Section 3.1
      confidence: high
```

---

# 12. Error Mapping Traceability

## Purpose

Error mapping traceability links failure behavior to source evidence.

## Items to Trace

```text
validation error behavior
downstream error behavior
database error behavior
business rule error behavior
internal error behavior
HTTP status code
target error code
target message
```

## Scoring

| Quality                                       | Points |
| --------------------------------------------- | -----: |
| All important error mappings traceable        |      5 |
| Main errors traceable, minor edge cases broad |    3–4 |
| Error behavior traceability weak              |    1–2 |
| Required error traceability missing           |      0 |

## Example

```yaml
source_traceability:
  error_sources:
    - error_mapping_id: ERR-VALIDATION-001
      requirement_section: Error Handling
      source_text_reference: Section 6.1
      confidence: high
```

---

# 13. Confidence Quality

## Purpose

Confidence quality checks whether confidence levels are used correctly.

## Confidence Levels

```text
high
medium
low
```

## Scoring

| Quality                                                       | Points |
| ------------------------------------------------------------- | -----: |
| Confidence levels are consistently and appropriately assigned |      4 |
| Confidence mostly correct, minor issues                       |      3 |
| Confidence levels present but overused or inconsistent        |    1–2 |
| Confidence levels missing                                     |      0 |

## Recommended Confidence Usage

Use `high` when:

```text
The requirement explicitly states the extracted detail.
```

Use `medium` when:

```text
The detail is supported by examples, prototypes, or secondary evidence.
```

Use `low` when:

```text
The detail is inferred and needs review.
```

---

# 14. Inference Transparency

## Purpose

Inference transparency checks whether assumptions are clearly marked.

## What to Check

```text
inferred values
defaulted HTTP statuses
derived operation names
derived handler names
missing source details
prototype-based behavior
user-confirmed clarifications
```

## Scoring

| Quality                                                       | Points |
| ------------------------------------------------------------- | -----: |
| All inferred details clearly marked with notes and confidence |      3 |
| Most inferred details marked                                  |      2 |
| Inferences exist but are not clearly marked                   |      1 |
| Unsupported assumptions hidden as facts                       |      0 |

## Example

```yaml
source_traceability:
  step_sources:
    - step_id: STEP-007
      requirement_section: Prototype-derived logic
      source_text_reference: Derived from Node-RED flow because primary requirement was ambiguous
      confidence: medium
      notes: Employee/new-account branch condition needs confirmation.
```

---

# Traceability Scorecard Template

Use this template to evaluate traceability.

```text
Workflow Name:
Workflow ID:
Workflow Type:
Requirement Source:
Evaluator:
Evaluation Date:

Traceability Scorecard
-------------------------------------------------
Workflow Source Traceability:    __ / 8
API Contract Traceability:       __ / 8
Field Validation Traceability:   __ / 12
Workflow Step Traceability:      __ / 12
Branching Traceability:          __ / 7
External Service Traceability:   __ / 7
Authentication Traceability:     __ / 5
Database Reference Traceability: __ / 7
Business Rule Traceability:      __ / 10
Calculation Rule Traceability:   __ / 5
Mapping Traceability:            __ / 7
Error Mapping Traceability:      __ / 5
Confidence Quality:              __ / 4
Inference Transparency:          __ / 3
-------------------------------------------------
Total:                           __ / 100

Traceability Rating:
Traceability Gaps:
Unsupported Assumptions:
Reviewer Notes:
```

---

# Example Traceability Evaluation — Fabric API

## Requirement Summary

```text
POST /api/v1/fabrics
Division-Id required, allowed values 12 and 13.
skuNumbers required as non-empty array of strings.
fabricTypeId required integer.
Call Product Customization Fabric service.
```

## Traceability Findings

```text
Strong:
- Endpoint path traced to API contract section.
- Division-Id validation traced to header section.
- skuNumbers traced to request body section.
- External service traced to downstream service section.

Weak:
- Downstream error behavior inferred from prototype.
- Trace-Id pass-through traced only to user clarification.
- Response mapping source is broad.
```

## Example Score

```text
Workflow Source Traceability:    8 / 8
API Contract Traceability:       8 / 8
Field Validation Traceability:   10 / 12
Workflow Step Traceability:      9 / 12
Branching Traceability:          5 / 7
External Service Traceability:   7 / 7
Authentication Traceability:     N/A
Database Reference Traceability: N/A
Business Rule Traceability:      N/A
Calculation Rule Traceability:   N/A
Mapping Traceability:            5 / 7
Error Mapping Traceability:      3 / 5
Confidence Quality:              3 / 4
Inference Transparency:          2 / 3
```

After normalizing not-applicable categories, the final traceability score should be reported as a percentage.

---

# Example Traceability Evaluation — Discount API

## Requirement Summary

```text
The Discount API validates headers and body, calls Interrogate Account, determines employee/new account path, checks discount tables, applies exclusions, calculates discount, and returns response.
```

## Traceability Findings

```text
Strong:
- Endpoint traced to API contract section.
- Interrogate Account call traced to downstream integration section.
- Employee discount rule traced to employee discount section.
- Database table references traced to discount rule section.
- HMAC authentication traced to downstream authentication section.

Weak:
- Additional discount behavior partially inferred.
- New account cap behavior has broad source reference.
- Some error mappings are inferred from standard behavior.
- Test scenarios are derived from schema, not directly from requirement examples.
```

## Example Score

```text
Workflow Source Traceability:    8 / 8
API Contract Traceability:       8 / 8
Field Validation Traceability:   10 / 12
Workflow Step Traceability:      11 / 12
Branching Traceability:          7 / 7
External Service Traceability:   7 / 7
Authentication Traceability:     5 / 5
Database Reference Traceability: 6 / 7
Business Rule Traceability:      8 / 10
Calculation Rule Traceability:   3 / 5
Mapping Traceability:            5 / 7
Error Mapping Traceability:      3 / 5
Confidence Quality:              3 / 4
Inference Transparency:          2 / 3
-------------------------------------------------
Total:                           86 / 100
```

Traceability rating:

```text
Good
```

---

# Traceability Gap Categories

When points are lost, classify the issue.

Recommended categories:

```text
missing_workflow_source
missing_api_contract_source
missing_field_source
missing_step_source
missing_branch_source
missing_service_source
missing_auth_source
missing_query_source
missing_business_rule_source
missing_calculation_source
missing_mapping_source
missing_error_source
vague_source_reference
incorrect_confidence_level
unsupported_assumption
unmarked_inference
prototype_only_source
```

---

# Traceability Review Checklist

Use this checklist during traceability review.

```text
- Is the workflow source documented?
- Is the endpoint path traceable?
- Is the HTTP method traceable?
- Are headers traceable?
- Are body fields traceable?
- Are enum values traceable?
- Are array rules traceable?
- Are workflow steps traceable?
- Are branch conditions traceable?
- Are true and false paths traceable?
- Are downstream services traceable?
- Is authentication traceable?
- Are database tables traceable?
- Are query inputs and outputs traceable?
- Are business rules traceable?
- Are calculation rules traceable?
- Are request mappings traceable?
- Are response mappings traceable?
- Are error mappings traceable?
- Are confidence levels present?
- Are confidence levels reasonable?
- Are inferred values clearly marked?
- Are unsupported assumptions avoided?
```

---

# Common Mistakes to Avoid

## Mistake 1 — Treating Traceability as Optional

Traceability is not just documentation.

It is part of the evaluation framework.

Without traceability, it becomes difficult to prove extraction quality.

## Mistake 2 — Using Broad References Everywhere

A broad reference like:

```text
Requirement Document
```

is weaker than:

```text
Section 2.3 — Request Body Fields
```

Specific source references should receive higher scores.

## Mistake 3 — Marking Inferred Values as High Confidence

If a detail is inferred, it should not be marked as high confidence unless confirmed elsewhere.

## Mistake 4 — Hiding Prototype-Based Assumptions

If a detail comes from Node-RED behavior rather than the official requirement document, note that clearly.

## Mistake 5 — Not Tracing Business Rules

Business rules are often the most important part of enterprise APIs.

They should have strong traceability whenever possible.

---

# Final Summary

Traceability scoring measures whether generated Schema v2 elements can be traced back to their source requirement evidence.

It evaluates:

* Workflow source traceability
* API contract traceability
* Field validation traceability
* Workflow step traceability
* Branching traceability
* External service traceability
* Authentication traceability
* Database reference traceability
* Business rule traceability
* Calculation rule traceability
* Mapping traceability
* Error mapping traceability
* Confidence quality
* Inference transparency

Strong traceability makes the generated schema easier to review, easier to evaluate, and more useful for research.

This completes Phase 4 of Day 12.
