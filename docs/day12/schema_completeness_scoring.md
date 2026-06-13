# Day 12 — Schema Completeness Scoring

## Purpose

This document defines how to score the completeness of a generated Workflow Schema v2 output.

Schema completeness measures whether the extracted workflow contains the expected Schema v2 sections and required fields for a given API workflow.

No coding is performed in this phase.

---

## Background

Day 10 created Workflow Schema v2.

Day 11 defined how raw API requirements should be mapped into Workflow Schema v2.

Day 12 defines how to evaluate the quality of that extraction.

This file focuses only on the first evaluation dimension:

```text
Schema Completeness
```

Schema completeness does not check whether every value is correct.

Instead, it checks whether the generated schema contains the required structure and expected fields.

Accuracy is handled separately in:

```text
docs/day12/requirement_accuracy_scoring.md
```

---

## What Schema Completeness Means

Schema completeness answers the question:

```text
Did the generated Workflow Schema v2 include all required sections and fields for this API workflow?
```

A schema can be complete even if some values later need correction.

For example:

```text
The schema may include an api_contract.path field, but the path may be wrong.
```

That is not a completeness issue.

That is an accuracy issue.

A completeness issue means the field or section is missing.

Example:

```text
The schema does not include any field_validations section even though the API has required request fields.
```

That is a completeness issue.

---

## Why Schema Completeness Matters

Schema completeness matters because future generators depend on structured fields.

If sections are missing, downstream generators may fail or produce incomplete artifacts.

For example:

* Missing `field_validations` may prevent validator generation.
* Missing `workflow_steps` may prevent Node-RED flow generation.
* Missing `external_services` may prevent downstream client generation.
* Missing `database_references` may prevent repository/query generation.
* Missing `response_mappings` may prevent response mapper generation.
* Missing `error_mappings` may prevent consistent error handling.
* Missing `test_scenarios` may prevent useful test generation.
* Missing `source_traceability` may prevent research evaluation.

Completeness scoring helps detect these issues before generation begins.

---

# Completeness Scoring Overview

The completeness score should evaluate whether required Schema v2 blocks are present and sufficiently populated.

The major Schema v2 blocks are:

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
source_traceability
generator_readiness
```

Not every workflow needs every block.

A complex business orchestration API may require most blocks.

A simple wrapper API may not require database references or calculation rules.

Therefore, completeness scoring must consider workflow type.

---

# Workflow Type Sensitivity

Completeness should be scored based on the type of API being evaluated.

## 1. Simple Wrapper API

A simple wrapper API usually validates input, calls a downstream service, maps the response, and returns it.

Example:

```text
POST /api/v1/fabrics
```

Expected blocks:

```text
metadata
api_contract
field_validations
workflow_steps
external_services
authentication, if required
request_mappings
response_mappings
error_mappings
test_scenarios
source_traceability
generator_readiness
```

Usually optional:

```text
database_references
business_rules
calculation_rules
```

---

## 2. Business Orchestration API

A business orchestration API validates input, calls services, checks rules, queries databases, branches paths, calculates results, and maps response.

Example:

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

Expected blocks:

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
source_traceability
generator_readiness
```

---

## 3. Database-Only API

A database-only API may not call external services but may validate input and read/write database records.

Expected blocks:

```text
metadata
api_contract
field_validations
workflow_steps
database_references
business_rules, if required
request_mappings
response_mappings
error_mappings
test_scenarios
source_traceability
generator_readiness
```

Usually optional:

```text
external_services
authentication, unless database/service authentication is modeled
calculation_rules
```

---

## 4. Calculation-Heavy API

A calculation-heavy API may include complex business formulas.

Expected blocks:

```text
metadata
api_contract
field_validations
workflow_steps
business_rules
calculation_rules
request_mappings
response_mappings
error_mappings
test_scenarios
source_traceability
generator_readiness
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

# Default Completeness Score Distribution

The following distribution can be used as the default scoring model.

| Schema Area         |  Points |
| ------------------- | ------: |
| Metadata            |       5 |
| API Contract        |      10 |
| Field Validations   |      15 |
| Workflow Steps      |      15 |
| Branching           |       7 |
| External Services   |       7 |
| Authentication      |       5 |
| Database References |       7 |
| Business Rules      |       7 |
| Calculation Rules   |       5 |
| Request Mappings    |       5 |
| Response Mappings   |       5 |
| Error Mappings      |       5 |
| Test Scenarios      |       4 |
| Source Traceability |       5 |
| Generator Readiness |       3 |
| **Total**           | **100** |

---

# Important Scoring Rule

If a block is not applicable to a workflow type, its points should be redistributed across the applicable blocks.

For example, a Fabric wrapper API may not need:

```text
database_references
calculation_rules
```

Those points should not be counted as missing.

Instead, the evaluator should mark those blocks as:

```text
not_applicable
```

and redistribute the points to relevant blocks such as:

```text
external_services
request_mappings
response_mappings
error_mappings
```

---

# Completeness Status Values

Each block can receive one of the following completeness statuses:

```text
complete
mostly_complete
partially_complete
missing
not_applicable
```

## complete

Use when the block exists and contains all required fields.

## mostly_complete

Use when the block exists and contains most required fields, but minor fields are missing.

## partially_complete

Use when the block exists but important fields are missing.

## missing

Use when the block should exist but is absent.

## not_applicable

Use when the block is not needed for the workflow type.

---

# 1. Metadata Completeness

## Purpose

The metadata block identifies the workflow and gives basic context.

## Required Fields

```text
workflow_id
workflow_name
workflow_description
workflow_version
workflow_type
source_domain
schema_version
status
```

## Scoring

| Metadata Quality                     | Points |
| ------------------------------------ | -----: |
| All required metadata fields present |      5 |
| Most metadata fields present         |      4 |
| Only basic name/ID present           |    2–3 |
| Metadata block missing               |      0 |

## Example Complete Metadata

```yaml
metadata:
  workflow_id: WF-DISCOUNT-001
  workflow_name: Evaluate Transaction Discount Workflow
  workflow_description: Evaluates transaction discount eligibility.
  workflow_version: 1.0.0
  workflow_type: business_orchestration
  source_domain: FEED Discount
  schema_version: v2
  status: draft
```

---

# 2. API Contract Completeness

## Purpose

The API contract block captures public endpoint details.

## Required Fields

```text
endpoint_id
path
method
api_version
content_type
response_type
operation_name
summary
```

Recommended fields:

```text
controller_name
handler_name
tags
```

## Scoring

| API Contract Quality                                    | Points |
| ------------------------------------------------------- | -----: |
| All required and recommended fields present             |     10 |
| All required fields present, recommended fields missing |      8 |
| Path and method present but several fields missing      |    5–7 |
| Only partial endpoint information present               |    2–4 |
| API contract block missing                              |      0 |

## Example Complete API Contract

```yaml
api_contract:
  endpoint_id: EP-FABRIC-001
  path: /api/v1/fabrics
  method: POST
  api_version: v1
  content_type: application/json
  response_type: application/json
  operation_name: getFabrics
  summary: Validate fabric request and return fabric configuration.
  tags:
    - fabric
    - customization
```

---

# 3. Field Validation Completeness

## Purpose

The field validation block captures request validation requirements.

## Required Coverage

The block should include validations for:

```text
required headers
optional headers
required body fields
optional body fields, if behavior matters
path parameters, if present
query parameters, if present
enum values
array rules
nested object rules
format rules
failure behavior
```

## Scoring

| Validation Quality                                              | Points |
| --------------------------------------------------------------- | -----: |
| All important field validations present with failure behavior   |     15 |
| Most validations present, minor optional validations missing    |  12–14 |
| Required fields present, but enum/array/nested rules incomplete |   8–11 |
| Only basic validation captured                                  |    4–7 |
| Field validations block missing                                 |      0 |

## Example Completeness Check

If the requirement says:

```text
skuNumbers is required and must contain at least one non-empty string.
```

A complete schema should include both:

```text
request.body.skuNumbers
request.body.skuNumbers[]
```

If only `skuNumbers` is present but item-level validation is missing, the block is partially complete.

---

# 4. Workflow Steps Completeness

## Purpose

The workflow steps block captures the ordered flow of execution.

## Required Fields Per Step

```text
step_id
step_name
step_type
description
inputs
outputs
depends_on
condition
success_behavior
failure_behavior
```

## Required Coverage

The steps should cover the full API behavior.

Typical expected steps include:

```text
validation
mapping
external_service
database_query
business_rule
branching
calculation
response_mapping
error_mapping
context_preparation
```

Only applicable steps are required.

## Scoring

| Workflow Step Quality                                 | Points |
| ----------------------------------------------------- | -----: |
| Complete step sequence with all required fields       |     15 |
| Complete sequence but some step metadata missing      |  12–14 |
| Major steps present but some workflow actions missing |   8–11 |
| Only high-level steps present                         |    4–7 |
| Workflow steps block missing                          |      0 |

## Example

A Discount API should not only say:

```text
Process Discount
```

It should break the workflow into logical steps such as:

```text
Validate Request
Call Interrogate Account
Prepare Discount Context
Determine Discount Path
Evaluate Employee Discount
Evaluate New Account Discount
Apply Exclusions
Calculate Discount
Map Final Response
```

---

# 5. Branching Completeness

## Purpose

The branching block captures conditional routing.

## Required Fields

```text
branch_id
branch_name
branch_type
condition
source_step
true_path
false_path
merge_step
description
```

## When Required

Branching is required when the workflow contains:

```text
if/else logic
conditional routing
success/error classification
employee vs new account path
record found vs no record path
downstream success vs downstream error path
```

## Scoring

| Branching Quality                                 |              Points |
| ------------------------------------------------- | ------------------: |
| All required branch paths and conditions captured |                   7 |
| Branches captured but minor metadata missing      |                 5–6 |
| Branch condition captured but paths unclear       |                 3–4 |
| Branching required but missing                    |                   0 |
| Not applicable                                    | redistribute points |

---

# 6. External Services Completeness

## Purpose

The external services block captures downstream service dependencies.

## Required Fields

```text
service_id
service_name
service_type
purpose
method
endpoint_reference
operation_name
request_source
response_used_by
timeout_required
retry_required
error_behavior
```

## Scoring

| External Service Quality                                            |              Points |
| ------------------------------------------------------------------- | ------------------: |
| All downstream services fully captured                              |                   7 |
| Services captured but timeout/retry/error behavior missing          |                 5–6 |
| Service name captured but endpoint/request/response details missing |                 3–4 |
| Required external service missing                                   |                   0 |
| Not applicable                                                      | redistribute points |

---

# 7. Authentication Completeness

## Purpose

The authentication block captures security requirements for downstream calls.

## Required Fields

```text
auth_id
applies_to_service
type
required
required_headers
```

For API key:

```text
api_key.header_name
api_key.source
```

For HMAC:

```text
signature.header_name
signature.format
signature.algorithm
signature.encoding
signature.payload_source
signature.secret_source
```

## Scoring

| Authentication Quality                                           |              Points |
| ---------------------------------------------------------------- | ------------------: |
| Authentication fully captured                                    |                   5 |
| Auth type and headers captured, but signature details incomplete |                 3–4 |
| Auth mentioned but not structured                                |                 1–2 |
| Required authentication missing                                  |                   0 |
| Not applicable                                                   | redistribute points |

---

# 8. Database References Completeness

## Purpose

The database references block captures query and table usage.

## Required Fields

```text
query_id
query_name
database_type
table_names
used_by_step
used_by_business_rule
purpose
inputs
outputs
no_record_behavior
failure_behavior
```

## Scoring

| Database Reference Quality                              |              Points |
| ------------------------------------------------------- | ------------------: |
| All required queries fully captured                     |                   7 |
| Queries captured but no-record/failure behavior missing |                 5–6 |
| Table names captured but inputs/outputs incomplete      |                 3–4 |
| Required database references missing                    |                   0 |
| Not applicable                                          | redistribute points |

---

# 9. Business Rules Completeness

## Purpose

The business rules block captures decision logic.

## Required Fields

```text
rule_id
rule_name
category
description
applies_to_step
condition
inputs
outputs
related_queries
success_behavior
failure_behavior
```

## Scoring

| Business Rule Quality                          |              Points |
| ---------------------------------------------- | ------------------: |
| All business rules fully captured              |                   7 |
| Rules captured but some inputs/outputs missing |                 5–6 |
| Rules summarized but conditions incomplete     |                 3–4 |
| Required business rules missing                |                   0 |
| Not applicable                                 | redistribute points |

---

# 10. Calculation Rules Completeness

## Purpose

The calculation rules block captures formulas, percentages, caps, totals, and derived values.

## Required Fields

```text
calculation_id
calculation_name
applies_when
calculation_type
inputs
output
calculation_behavior
cap_behavior
item_level
rounding_behavior
failure_behavior
```

## Scoring

| Calculation Rule Quality                                   |              Points |
| ---------------------------------------------------------- | ------------------: |
| All calculation behavior fully captured                    |                   5 |
| Calculations captured but rounding/cap behavior incomplete |                 3–4 |
| Calculation mentioned but not structured                   |                 1–2 |
| Required calculation rules missing                         |                   0 |
| Not applicable                                             | redistribute points |

---

# 11. Request Mappings Completeness

## Purpose

Request mappings capture source-to-target transformations.

## Required Fields

```text
mapping_id
mapping_name
applies_to_step
source
target
required
transformation
pass_through_when_present
omit_when_missing
```

## Scoring

| Request Mapping Quality                               | Points |
| ----------------------------------------------------- | -----: |
| All required request mappings captured                |      5 |
| Main mappings captured, optional pass-through missing |    3–4 |
| Mapping mentioned but source/target incomplete        |    1–2 |
| Required request mappings missing                     |      0 |

---

# 12. Response Mappings Completeness

## Purpose

Response mappings capture how workflow results become API responses.

## Required Fields

```text
mapping_id
mapping_name
applies_to_step
response_type
http_status
source
target
transformation
required
```

## Scoring

| Response Mapping Quality                                   | Points |
| ---------------------------------------------------------- | -----: |
| All success and important error response mappings captured |      5 |
| Success mappings captured, error mappings incomplete       |    3–4 |
| Response example captured but source mappings missing      |    1–2 |
| Required response mappings missing                         |      0 |

---

# 13. Error Mappings Completeness

## Purpose

Error mappings capture standardized failure behavior.

## Required Fields

```text
error_mapping_id
error_source
condition
source_error_code
target_error_code
target_http_status
target_message
retryable
fail_fast
applies_to_steps
```

## Scoring

| Error Mapping Quality                                                                  | Points |
| -------------------------------------------------------------------------------------- | -----: |
| Validation, downstream, database, business, and internal errors captured as applicable |      5 |
| Main errors captured but some edge errors missing                                      |    3–4 |
| Error behavior mentioned but not structured                                            |    1–2 |
| Required error mappings missing                                                        |      0 |

---

# 14. Test Scenarios Completeness

## Purpose

Test scenarios capture expected behavior for validation, workflow paths, services, database results, and errors.

## Required Fields

```text
scenario_id
scenario_name
scenario_type
description
input
mocks
expected
```

Recommended expected fields:

```text
http_status
response_body
executed_steps
skipped_steps
```

## Scoring

| Test Scenario Quality                                                                   | Points |
| --------------------------------------------------------------------------------------- | -----: |
| Positive, negative, branch, downstream failure, and DB scenarios captured as applicable |      4 |
| Positive and major negative scenarios captured                                          |      3 |
| Only one or two basic scenarios captured                                                |    1–2 |
| Test scenarios missing                                                                  |      0 |

---

# 15. Source Traceability Completeness

## Purpose

Source traceability captures where extracted schema items came from.

## Required Coverage

Traceability should include:

```text
workflow_source
step_sources
field_sources
business_rule_sources
```

## Scoring

| Traceability Completeness                        | Points |
| ------------------------------------------------ | -----: |
| Workflow, field, step, and rule sources captured |      5 |
| Workflow and most field/step sources captured    |      4 |
| Only workflow source captured                    |    2–3 |
| Traceability missing                             |      0 |

Detailed traceability quality is scored separately in:

```text
docs/day12/traceability_scoring.md
```

---

# 16. Generator Readiness Completeness

## Purpose

Generator readiness captures whether the schema is ready for downstream artifact generation.

## Required Generator Categories

```text
node_red
excel
go
documentation
tests
evaluation
```

Each category should include:

```text
status
missing_fields
notes
```

## Scoring

| Generator Readiness Completeness                                         | Points |
| ------------------------------------------------------------------------ | -----: |
| All generator categories captured with status, missing fields, and notes |      3 |
| All categories captured but notes incomplete                             |      2 |
| Only some categories captured                                            |      1 |
| Generator readiness missing                                              |      0 |

---

# Completeness Scorecard Template

Use this template to evaluate schema completeness.

```text
Workflow Name:
Workflow ID:
Workflow Type:
Requirement Source:
Evaluator:
Evaluation Date:

Schema Completeness Scorecard
-------------------------------------------------
Metadata:                 __ / 5
API Contract:             __ / 10
Field Validations:         __ / 15
Workflow Steps:            __ / 15
Branching:                 __ / 7
External Services:         __ / 7
Authentication:            __ / 5
Database References:       __ / 7
Business Rules:            __ / 7
Calculation Rules:         __ / 5
Request Mappings:          __ / 5
Response Mappings:         __ / 5
Error Mappings:            __ / 5
Test Scenarios:            __ / 4
Source Traceability:       __ / 5
Generator Readiness:       __ / 3
-------------------------------------------------
Total:                     __ / 100

Completeness Rating:
Gap Notes:
```

---

# Example Completeness Evaluation — Fabric API

## Workflow Type

```text
simple_wrapper
```

## Expected Required Blocks

```text
metadata
api_contract
field_validations
workflow_steps
external_services
request_mappings
response_mappings
error_mappings
test_scenarios
source_traceability
generator_readiness
```

## Not Applicable Blocks

```text
database_references
business_rules, unless Division-Id rule is modeled separately
calculation_rules
```

## Example Score

```text
Metadata:                 5 / 5
API Contract:             10 / 10
Field Validations:         13 / 15
Workflow Steps:            13 / 15
Branching:                 5 / 7
External Services:         6 / 7
Authentication:            3 / 5
Database References:       N/A
Business Rules:            N/A
Calculation Rules:         N/A
Request Mappings:          5 / 5
Response Mappings:         4 / 5
Error Mappings:            3 / 5
Test Scenarios:            3 / 4
Source Traceability:       3 / 5
Generator Readiness:       3 / 3
```

After redistributing N/A points, the final completeness score may be calculated as a percentage of applicable points.

---

# Example Completeness Evaluation — Discount API

## Workflow Type

```text
business_orchestration
```

## Expected Required Blocks

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
source_traceability
generator_readiness
```

## Example Score

```text
Metadata:                 5 / 5
API Contract:             9 / 10
Field Validations:         13 / 15
Workflow Steps:            14 / 15
Branching:                 7 / 7
External Services:         7 / 7
Authentication:            5 / 5
Database References:       5 / 7
Business Rules:            6 / 7
Calculation Rules:         4 / 5
Request Mappings:          4 / 5
Response Mappings:         4 / 5
Error Mappings:            4 / 5
Test Scenarios:            2 / 4
Source Traceability:       3 / 5
Generator Readiness:       3 / 3
-------------------------------------------------
Total:                     95 / 110 raw possible if incorrectly counted
Normalized Total:          86 / 100
```

## Important Note

Always normalize to 100.

If custom weights are used, the final score should still be reported as a percentage.

---

# Normalized Scoring Formula

When some blocks are not applicable, use this formula:

```text
Completeness Score = (Earned Applicable Points / Total Applicable Points) * 100
```

Example:

```text
Earned Applicable Points = 72
Total Applicable Points = 80

Completeness Score = (72 / 80) * 100 = 90%
```

---

# Completeness Gap Categories

When points are lost, classify the gap.

Recommended categories:

```text
missing_required_block
missing_required_field
missing_optional_but_useful_field
incomplete_step_metadata
incomplete_mapping
missing_failure_behavior
missing_traceability
missing_test_scenario
generator_readiness_gap
```

---

# Completeness Review Checklist

Use this checklist during review.

```text
- Is metadata present?
- Is api_contract present?
- Are endpoint path and method present?
- Are request validations present?
- Are required headers captured?
- Are required body fields captured?
- Are workflow steps present?
- Are workflow steps ordered?
- Are step inputs and outputs present?
- Are dependencies defined?
- Is branching captured when needed?
- Are external services captured when needed?
- Is authentication captured when needed?
- Are database references captured when needed?
- Are business rules captured when needed?
- Are calculation rules captured when needed?
- Are request mappings captured?
- Are response mappings captured?
- Are error mappings captured?
- Are test scenarios included?
- Is source traceability included?
- Is generator readiness included?
- Are non-applicable sections marked as not_applicable instead of missing?
```

---

# Common Mistakes to Avoid

## Mistake 1 — Penalizing Non-Applicable Blocks

Do not penalize a simple wrapper API for missing database references if the API does not use a database.

Mark the block as:

```text
not_applicable
```

and normalize the score.

---

## Mistake 2 — Counting a Block as Complete Because It Exists

A block is not complete just because it exists.

For example:

```yaml
workflow_steps: []
```

is not complete.

The block must contain useful and required content.

---

## Mistake 3 — Ignoring Required Fields Inside Blocks

A schema may include `external_services`, but if the endpoint, method, request source, and error behavior are missing, it is only partially complete.

---

## Mistake 4 — Confusing Completeness with Accuracy

Completeness checks whether fields are present.

Accuracy checks whether the values are correct.

Example:

```text
api_contract.path exists but has the wrong endpoint.
```

This is complete but inaccurate.

---

## Mistake 5 — Ignoring Test Scenarios

Test scenarios are part of Schema v2 readiness.

Even if they are simple, the schema should include test scenario candidates.

---

# Final Summary

Schema completeness scoring measures whether a generated Workflow Schema v2 output contains the required sections and fields for a given API workflow.

It evaluates the structure of the extracted schema, not whether every value is correct.

The main scoring areas are:

* Metadata
* API contract
* Field validations
* Workflow steps
* Branching
* External services
* Authentication
* Database references
* Business rules
* Calculation rules
* Request mappings
* Response mappings
* Error mappings
* Test scenarios
* Source traceability
* Generator readiness

This scoring framework helps detect missing schema structure before any artifact generation begins.

This completes Phase 2 of Day 12.
