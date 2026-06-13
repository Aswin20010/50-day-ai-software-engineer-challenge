# Day 12 — Requirement Accuracy Scoring

## Purpose

This document defines how to score the accuracy of a generated Workflow Schema v2 output.

Requirement accuracy measures whether the extracted schema correctly represents the original requirement source.

Schema completeness asks:

```text
Did the schema include the required sections and fields?
```

Requirement accuracy asks:

```text
Are the extracted values and meanings correct?
```

No coding is performed in this phase.

---

## Background

Day 10 created Workflow Schema v2.

Day 11 defined how raw API requirements should map into Workflow Schema v2.

Day 12 defines how to evaluate whether that extraction was successful.

Phase 2 defined schema completeness scoring.

Phase 3 defines requirement accuracy scoring.

Completeness and accuracy are related, but they are not the same.

A schema can be complete but inaccurate.

For example:

```text
The schema includes api_contract.path, but the path value is wrong.
```

That is complete but inaccurate.

A schema can also be accurate but incomplete.

For example:

```text
The endpoint path and method are correct, but response mappings are missing.
```

That is accurate for the fields present, but incomplete overall.

---

# What Requirement Accuracy Means

Requirement accuracy measures how correctly the extracted Schema v2 content matches the original requirement source.

It answers:

```text
Did the extraction capture the correct requirement details?
```

Accuracy scoring should compare the generated schema against:

* Official requirement document
* API contract
* Request examples
* Response examples
* Node-RED prototype flow, if used as supporting evidence
* Existing workflow notes
* User-confirmed clarifications
* Excel intake expectations, if relevant

---

# Why Accuracy Scoring Matters

Accuracy scoring matters because incorrect schema values can cause incorrect downstream artifacts.

If a schema contains inaccurate information, future generators may produce:

* Wrong Node-RED flows
* Wrong Excel intake files
* Wrong Go validators
* Wrong downstream service clients
* Wrong business logic
* Wrong SQL/repository methods
* Wrong response mapping
* Wrong test cases
* Wrong documentation

Therefore, extracted schema values must be evaluated against the original requirement source before generation.

---

# Accuracy Scoring Overview

Requirement accuracy should be scored across these major categories:

```text
1. API Contract Accuracy
2. Header Accuracy
3. Body Field Accuracy
4. Validation Accuracy
5. Workflow Step Accuracy
6. Branching Accuracy
7. External Service Accuracy
8. Authentication Accuracy
9. Database Reference Accuracy
10. Business Rule Accuracy
11. Calculation Rule Accuracy
12. Request Mapping Accuracy
13. Response Mapping Accuracy
14. Error Mapping Accuracy
15. Test Scenario Accuracy
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

# Default Accuracy Score Distribution

The following distribution can be used as the default scoring model.

| Accuracy Area               |  Points |
| --------------------------- | ------: |
| API Contract Accuracy       |      10 |
| Header Accuracy             |       8 |
| Body Field Accuracy         |       8 |
| Validation Accuracy         |      10 |
| Workflow Step Accuracy      |      12 |
| Branching Accuracy          |       7 |
| External Service Accuracy   |       7 |
| Authentication Accuracy     |       5 |
| Database Reference Accuracy |       7 |
| Business Rule Accuracy      |       8 |
| Calculation Rule Accuracy   |       5 |
| Request Mapping Accuracy    |       5 |
| Response Mapping Accuracy   |       4 |
| Error Mapping Accuracy      |       3 |
| Test Scenario Accuracy      |       1 |
| **Total**                   | **100** |

---

# Important Scoring Rule

If a category is not applicable to a workflow, its points should be redistributed across applicable categories.

Example:

A simple wrapper API may not use:

```text
database_references
calculation_rules
```

Those categories should be marked:

```text
not_applicable
```

and the final score should be normalized.

Use this formula:

```text
Requirement Accuracy Score = (Earned Applicable Points / Total Applicable Points) * 100
```

---

# Accuracy Status Values

Each area can receive one of the following statuses:

```text
accurate
mostly_accurate
partially_accurate
inaccurate
missing
not_applicable
```

## accurate

Use when the extracted value fully matches the requirement.

## mostly_accurate

Use when the extracted value is mostly correct but minor details are missing or slightly incomplete.

## partially_accurate

Use when the extracted value captures the general idea but misses important details.

## inaccurate

Use when the extracted value contradicts the requirement.

## missing

Use when the detail should have been extracted but is absent.

## not_applicable

Use when the category does not apply to the workflow.

---

# 1. API Contract Accuracy

## Purpose

API contract accuracy checks whether public endpoint details match the source requirement.

## Items to Check

```text
endpoint path
HTTP method
API version
operation name
content type
response type
summary
tags
```

## Scoring

| API Contract Accuracy                                    | Points |
| -------------------------------------------------------- | -----: |
| All API contract details correct                         |     10 |
| Endpoint and method correct, minor metadata issues       |    8–9 |
| Endpoint correct but method/version/summary issues exist |    5–7 |
| Endpoint partially wrong or inferred incorrectly         |    2–4 |
| API contract incorrect or missing                        |      0 |

## Example Correct Extraction

Requirement:

```text
POST /api/v1/fabrics
```

Schema:

```yaml
api_contract:
  path: /api/v1/fabrics
  method: POST
  api_version: v1
```

Accuracy status:

```text
accurate
```

## Example Incorrect Extraction

Requirement:

```text
POST /api/v1/fabrics
```

Schema:

```yaml
api_contract:
  path: /api/v1/fabric
  method: GET
```

Accuracy status:

```text
inaccurate
```

---

# 2. Header Accuracy

## Purpose

Header accuracy checks whether request headers are correctly extracted.

## Items to Check

```text
header name
header casing or canonical format
required or optional status
data type
allowed values
failure behavior
downstream pass-through behavior, if applicable
```

## Scoring

| Header Accuracy                                      | Points |
| ---------------------------------------------------- | -----: |
| All headers correctly captured                       |      8 |
| Required headers correct, minor optional header gaps |    6–7 |
| Some required headers missing or incorrect           |    3–5 |
| Most headers missing or wrong                        |    1–2 |
| Header extraction absent                             |      0 |

## Example Correct Extraction

Requirement:

```text
Division-Id is required and must be either 12 or 13.
Trace-Id is optional.
```

Schema:

```yaml
field_validations:
  - field_path: request.headers.Division-Id
    location: header
    required: true
    allowed_values:
      - 12
      - 13

  - field_path: request.headers.Trace-Id
    location: header
    required: false
```

Accuracy status:

```text
accurate
```

## Common Header Accuracy Errors

```text
wrong header name
missing required header
optional header marked required
required header marked optional
missing allowed values
incorrect allowed values
wrong data type
missing pass-through rule
```

---

# 3. Body Field Accuracy

## Purpose

Body field accuracy checks whether request body fields are correctly extracted.

## Items to Check

```text
field name
nested field path
required or optional status
data type
array structure
item type
object structure
field format
```

## Scoring

| Body Field Accuracy                               | Points |
| ------------------------------------------------- | -----: |
| All body fields correctly captured                |      8 |
| Required body fields correct, minor optional gaps |    6–7 |
| Some required body fields incomplete              |    3–5 |
| Most body fields missing or wrong                 |    1–2 |
| Body field extraction absent                      |      0 |

## Example Correct Extraction

Requirement:

```text
skuNumbers is required and must be a non-empty array of strings.
fabricTypeId is required and must be an integer.
```

Schema:

```yaml
field_validations:
  - field_path: request.body.skuNumbers
    location: body
    type: array
    required: true
    min_items: 1

  - field_path: request.body.skuNumbers[]
    location: body
    type: string
    required: true
    allow_blank: false

  - field_path: request.body.fabricTypeId
    location: body
    type: integer
    required: true
```

Accuracy status:

```text
accurate
```

## Common Body Field Accuracy Errors

```text
array treated as string
nested object path flattened incorrectly
required field marked optional
optional field marked required
item-level validation missing
wrong field name
wrong field type
missing object parent field
```

---

# 4. Validation Accuracy

## Purpose

Validation accuracy checks whether validation logic matches the requirement.

## Items to Check

```text
required field validation
optional field handling
enum validation
numeric validation
string validation
array validation
nested object validation
date/timestamp validation
conditional validation
validation failure behavior
```

## Scoring

| Validation Accuracy                                                     | Points |
| ----------------------------------------------------------------------- | -----: |
| All validation rules correctly captured                                 |     10 |
| Most validation rules correct, minor gaps                               |    8–9 |
| Required validation present but enum/array/conditional rules incomplete |    5–7 |
| Validation mostly vague or partially wrong                              |    2–4 |
| Validation logic absent or incorrect                                    |      0 |

## Example Correct Extraction

Requirement:

```text
messageSeverity must be INFO or CRITICAL.
```

Schema:

```yaml
field_validations:
  - field_path: request.body.messageSeverity
    type: string
    required: true
    allowed_values:
      - INFO
      - CRITICAL
```

Accuracy status:

```text
accurate
```

## Example Incorrect Extraction

Requirement:

```text
messageSeverity must be INFO or CRITICAL.
```

Schema:

```yaml
field_validations:
  - field_path: request.body.messageSeverity
    type: string
    required: true
    allowed_values:
      - LOW
      - HIGH
```

Accuracy status:

```text
inaccurate
```

---

# 5. Workflow Step Accuracy

## Purpose

Workflow step accuracy checks whether the extracted steps correctly represent the actual processing flow.

## Items to Check

```text
step order
step type
step purpose
step inputs
step outputs
step dependencies
step conditions
success behavior
failure behavior
```

## Scoring

| Workflow Step Accuracy                                    | Points |
| --------------------------------------------------------- | -----: |
| Workflow sequence correctly matches requirement           |     12 |
| Main sequence correct, minor step metadata issues         |  10–11 |
| Major steps present but order or dependencies have issues |    7–9 |
| Workflow is vague or misses important actions             |    3–6 |
| Workflow steps are mostly wrong or absent                 |    0–2 |

## Example Correct Extraction

Requirement:

```text
Validate request, build downstream request, call fabric service, map response.
```

Schema steps:

```text
STEP-001 Validate Request Headers
STEP-002 Validate Request Body
STEP-003 Build Downstream Fabric Request
STEP-004 Call Fabric Customization Service
STEP-005 Map Final Response
```

Accuracy status:

```text
accurate
```

## Common Workflow Step Accuracy Errors

```text
wrong step order
missing downstream call step
missing response mapping step
missing validation step
business rule modeled as validation
validation modeled as business rule
branching step omitted
calculation step omitted
```

---

# 6. Branching Accuracy

## Purpose

Branching accuracy checks whether conditional paths are correctly represented.

## Items to Check

```text
branch condition
source step
true path
false path
merge step
branch type
branch description
```

## Scoring

| Branching Accuracy                             |              Points |
| ---------------------------------------------- | ------------------: |
| All branch conditions and paths correct        |                   7 |
| Branches correct but minor metadata missing    |                 5–6 |
| Condition captured but path routing incomplete |                 3–4 |
| Branching logic partially wrong                |                 1–2 |
| Required branching missing or incorrect        |                   0 |
| Not applicable                                 | redistribute points |

## Example Correct Extraction

Requirement:

```text
If employeeFlag is true, process employee discount. Otherwise, process new account discount.
```

Schema:

```yaml
branching:
  - condition: validatedAccountInfo.employeeFlag == true
    true_path: STEP-008
    false_path: STEP-009
    merge_step: STEP-010
```

Accuracy status:

```text
accurate
```

## Example Incorrect Extraction

If the schema routes `employeeFlag == true` to new account discount, the branch is inaccurate even if the branching block exists.

---

# 7. External Service Accuracy

## Purpose

External service accuracy checks whether downstream service details match the requirement.

## Items to Check

```text
service name
service type
endpoint reference
HTTP method
SOAP operation
request source
response usage
timeout behavior
retry behavior
error behavior
```

## Scoring

| External Service Accuracy                                     |              Points |
| ------------------------------------------------------------- | ------------------: |
| All downstream service details correct                        |                   7 |
| Service and endpoint correct, minor metadata gaps             |                 5–6 |
| Service captured but endpoint/method/request usage incomplete |                 3–4 |
| Wrong service or wrong endpoint                               |                 1–2 |
| Required external service missing                             |                   0 |
| Not applicable                                                | redistribute points |

## Example Correct Extraction

Requirement:

```text
Call Product Customization Fabric service at /productcustomization/v1/config/fabrics using POST.
```

Schema:

```yaml
external_services:
  - service_name: Product Customization Fabric Service
    service_type: REST
    method: POST
    endpoint_reference: /productcustomization/v1/config/fabrics
```

Accuracy status:

```text
accurate
```

---

# 8. Authentication Accuracy

## Purpose

Authentication accuracy checks whether downstream authentication rules are correctly represented.

## Items to Check

```text
auth type
required headers
API key source
authorization header
signature format
algorithm
encoding
payload source
secret source
```

## Scoring

| Authentication Accuracy                               |              Points |
| ----------------------------------------------------- | ------------------: |
| Authentication fully correct                          |                   5 |
| Auth type and headers correct, minor detail missing   |                   4 |
| Auth captured but signature/source details incomplete |                 2–3 |
| Auth type wrong or major details wrong                |                   1 |
| Required authentication missing                       |                   0 |
| Not applicable                                        | redistribute points |

## Example Correct Extraction

Requirement:

```text
Use HMAC-SHA256 and send Authorization header as MAC <signature>.
```

Schema:

```yaml
authentication:
  - type: hmac
    signature:
      header_name: Authorization
      format: MAC <signature>
      algorithm: HMAC-SHA256
      encoding: base64
```

Accuracy status:

```text
accurate
```

---

# 9. Database Reference Accuracy

## Purpose

Database reference accuracy checks whether database table and query information matches the requirement.

## Items to Check

```text
database type
table names
query purpose
query inputs
query outputs
used_by_step
used_by_business_rule
no-record behavior
failure behavior
```

## Scoring

| Database Accuracy                                   |              Points |
| --------------------------------------------------- | ------------------: |
| All database references correct                     |                   7 |
| Tables and purpose correct, minor input/output gaps |                 5–6 |
| Table names present but query usage incomplete      |                 3–4 |
| Wrong table or wrong query purpose                  |                 1–2 |
| Required database reference missing                 |                   0 |
| Not applicable                                      | redistribute points |

## Example Correct Extraction

Requirement:

```text
Lookup employee account profile from Emp_Acct_Profile using internalAccountToken.
```

Schema:

```yaml
database_references:
  - query_name: Lookup Employee Account Profile
    table_names:
      - Emp_Acct_Profile
    inputs:
      internalAccountToken: validatedAccountInfo.internalAccountToken
```

Accuracy status:

```text
accurate
```

---

# 10. Business Rule Accuracy

## Purpose

Business rule accuracy checks whether extracted business decisions match the requirement.

## Items to Check

```text
rule name
rule category
condition
inputs
outputs
related queries
success behavior
failure behavior
```

## Scoring

| Business Rule Accuracy                              |              Points |
| --------------------------------------------------- | ------------------: |
| All business rules correctly captured               |                   8 |
| Main business rules correct, minor metadata gaps    |                 6–7 |
| Business rules summarized but conditions incomplete |                 4–5 |
| Rules partially wrong or mixed with validation      |                 1–3 |
| Required business rules missing                     |                   0 |
| Not applicable                                      | redistribute points |

## Example Correct Extraction

Requirement:

```text
Employee discount applies only when employeeFlag is true.
```

Schema:

```yaml
business_rules:
  - rule_name: Employee Account Eligibility
    condition: validatedAccountInfo.employeeFlag == true
```

Accuracy status:

```text
accurate
```

## Common Business Rule Accuracy Errors

```text
eligibility condition reversed
employee logic mixed with new account logic
exclusion rule omitted
cap rule omitted
business rule treated as simple validation
database lookup disconnected from rule
```

---

# 11. Calculation Rule Accuracy

## Purpose

Calculation rule accuracy checks whether formulas, percentages, caps, and rounding behavior match the requirement.

## Items to Check

```text
calculation type
applies_when condition
inputs
output
calculation behavior
cap behavior
item-level flag
rounding behavior
failure behavior
```

## Scoring

| Calculation Accuracy                                    |              Points |
| ------------------------------------------------------- | ------------------: |
| All calculation rules correct                           |                   5 |
| Main formula correct, minor rounding/cap detail missing |                   4 |
| Calculation captured but incomplete                     |                 2–3 |
| Calculation wrong or vague                              |                   1 |
| Required calculation missing                            |                   0 |
| Not applicable                                          | redistribute points |

## Example Correct Extraction

Requirement:

```text
Apply new account discount and cap it using configured cap amount.
```

Schema:

```yaml
calculation_rules:
  - calculation_name: Apply New Account Cap
    calculation_type: cap
    inputs:
      - calculatedNewAccountDiscount
      - newAccountCapAmount
    cap_behavior: min(calculatedNewAccountDiscount, newAccountCapAmount)
```

Accuracy status:

```text
accurate
```

---

# 12. Request Mapping Accuracy

## Purpose

Request mapping accuracy checks whether source-to-target request transformations are correct.

## Items to Check

```text
source field
target field
required flag
transformation
optional pass-through behavior
omit behavior
applies_to_step
```

## Scoring

| Request Mapping Accuracy                             | Points |
| ---------------------------------------------------- | -----: |
| All request mappings correct                         |      5 |
| Main mappings correct, optional mappings incomplete  |      4 |
| Some mappings present but source/target issues exist |    2–3 |
| Mapping mostly wrong                                 |      1 |
| Required request mappings missing                    |      0 |

## Example Correct Extraction

Requirement:

```text
Map skuNumbers to customSkuNumbers.
```

Schema:

```yaml
request_mappings:
  - source: request.body.skuNumbers
    target: downstreamFabricRequest.customSkuNumbers
    transformation: direct_copy
```

Accuracy status:

```text
accurate
```

---

# 13. Response Mapping Accuracy

## Purpose

Response mapping accuracy checks whether output fields are correctly mapped.

## Items to Check

```text
response type
HTTP status
source field
target field
transformation
required flag
```

## Scoring

| Response Mapping Accuracy                             | Points |
| ----------------------------------------------------- | -----: |
| All response mappings correct                         |      4 |
| Main response mappings correct, minor fields missing  |      3 |
| Response fields present but source mapping incomplete |      2 |
| Response mapping mostly wrong                         |      1 |
| Required response mapping missing                     |      0 |

## Example Correct Extraction

Requirement:

```text
Return discountType from calculated discount result.
```

Schema:

```yaml
response_mappings:
  - source: calculatedDiscount.discountType
    target: response.discountType
    response_type: success
    http_status: 200
```

Accuracy status:

```text
accurate
```

---

# 14. Error Mapping Accuracy

## Purpose

Error mapping accuracy checks whether failure behavior matches the requirement.

## Items to Check

```text
error source
condition
source error code
target error code
target HTTP status
target message
retryable flag
fail-fast flag
applies_to_steps
```

## Scoring

| Error Mapping Accuracy                              | Points |
| --------------------------------------------------- | -----: |
| All important error mappings correct                |      3 |
| Main error mappings correct, edge cases missing     |      2 |
| Errors present but status/code/message issues exist |      1 |
| Required error mappings missing or wrong            |      0 |

## Example Correct Extraction

Requirement:

```text
Validation failures should return 400 BAD_REQUEST.
```

Schema:

```yaml
error_mappings:
  - error_source: validation
    target_error_code: BAD_REQUEST
    target_http_status: 400
```

Accuracy status:

```text
accurate
```

---

# 15. Test Scenario Accuracy

## Purpose

Test scenario accuracy checks whether generated scenarios reflect real requirement behavior.

## Items to Check

```text
scenario type
input data
mock setup
expected HTTP status
expected response
expected executed steps
expected skipped steps
```

## Scoring

| Test Scenario Accuracy                                        | Points |
| ------------------------------------------------------------- | -----: |
| Test scenarios accurately reflect important requirement paths |      1 |
| Test scenarios absent or misleading                           |      0 |

## Notes

In this initial scoring model, test scenario accuracy receives a small number of points because test scenario completeness is evaluated more strongly under schema completeness and generator readiness.

This weighting can be increased later.

---

# Accuracy Scorecard Template

Use this template to evaluate requirement accuracy.

```text
Workflow Name:
Workflow ID:
Workflow Type:
Requirement Source:
Evaluator:
Evaluation Date:

Requirement Accuracy Scorecard
-------------------------------------------------
API Contract Accuracy:       __ / 10
Header Accuracy:             __ / 8
Body Field Accuracy:         __ / 8
Validation Accuracy:         __ / 10
Workflow Step Accuracy:      __ / 12
Branching Accuracy:          __ / 7
External Service Accuracy:   __ / 7
Authentication Accuracy:     __ / 5
Database Reference Accuracy: __ / 7
Business Rule Accuracy:      __ / 8
Calculation Rule Accuracy:   __ / 5
Request Mapping Accuracy:    __ / 5
Response Mapping Accuracy:   __ / 4
Error Mapping Accuracy:      __ / 3
Test Scenario Accuracy:      __ / 1
-------------------------------------------------
Total:                       __ / 100

Accuracy Rating:
Incorrect Items:
Missing Items:
Assumptions Found:
Reviewer Notes:
```

---

# Example Accuracy Evaluation — Fabric API

## Requirement Summary

```text
POST /api/v1/fabrics
Division-Id is required and must be 12 or 13.
skuNumbers is required and must be a non-empty array of strings.
fabricTypeId is required and must be an integer.
Map skuNumbers to customSkuNumbers.
Call /productcustomization/v1/config/fabrics using POST.
```

## Generated Schema Findings

```text
Correct:
- Endpoint path
- HTTP method
- Division-Id required flag
- Division-Id allowed values
- fabricTypeId type
- External service endpoint
- Request mapping from skuNumbers to customSkuNumbers

Incorrect or incomplete:
- skuNumbers[] item-level validation missing
- Trace-Id marked required even though it is optional
- Downstream error mapping status inferred without source
```

## Example Score

```text
API Contract Accuracy:       10 / 10
Header Accuracy:             6 / 8
Body Field Accuracy:         6 / 8
Validation Accuracy:         7 / 10
Workflow Step Accuracy:      11 / 12
Branching Accuracy:          5 / 7
External Service Accuracy:   7 / 7
Authentication Accuracy:     N/A
Database Reference Accuracy: N/A
Business Rule Accuracy:      N/A
Calculation Rule Accuracy:   N/A
Request Mapping Accuracy:    5 / 5
Response Mapping Accuracy:   3 / 4
Error Mapping Accuracy:      2 / 3
Test Scenario Accuracy:      1 / 1
```

After normalizing not-applicable categories, the final score should be reported as a percentage.

---

# Example Accuracy Evaluation — Discount API

## Requirement Summary

```text
The Discount API validates request headers and body, calls Interrogate Account, determines employee or new account path, applies discount rules, checks exclusions, calculates discount, and returns response.
```

## Generated Schema Findings

```text
Correct:
- Endpoint path and method
- Required headers
- Interrogate Account external service
- HMAC authentication
- Employee vs new account branch
- Employee discount rule
- Response mapping for discountType

Incorrect or incomplete:
- New account cap rule missing
- Exclusion query no-record behavior missing
- Additional employee discount calculation incomplete
- Some database query inputs are not mapped
```

## Example Score

```text
API Contract Accuracy:       9 / 10
Header Accuracy:             8 / 8
Body Field Accuracy:         7 / 8
Validation Accuracy:         8 / 10
Workflow Step Accuracy:      11 / 12
Branching Accuracy:          7 / 7
External Service Accuracy:   7 / 7
Authentication Accuracy:     5 / 5
Database Reference Accuracy: 5 / 7
Business Rule Accuracy:      6 / 8
Calculation Rule Accuracy:   3 / 5
Request Mapping Accuracy:    4 / 5
Response Mapping Accuracy:   3 / 4
Error Mapping Accuracy:      2 / 3
Test Scenario Accuracy:      1 / 1
-------------------------------------------------
Total:                       86 / 100
```

Accuracy rating:

```text
Good
```

---

# Accuracy Gap Categories

When points are lost, classify the issue.

Recommended categories:

```text
incorrect_value
missing_required_detail
wrong_required_optional_status
wrong_data_type
wrong_enum_values
wrong_step_order
wrong_branch_condition
wrong_service_endpoint
wrong_authentication_method
wrong_table_or_query
wrong_business_condition
missing_calculation_behavior
wrong_request_mapping
wrong_response_mapping
wrong_error_status
unsupported_assumption
```

---

# Accuracy Review Checklist

Use this checklist during accuracy review.

```text
- Does the endpoint path match the requirement?
- Does the HTTP method match?
- Are all required headers correct?
- Are optional headers correctly marked?
- Are all body fields correct?
- Are nested paths correct?
- Are array item rules correct?
- Are enum values exact?
- Are validation failure behaviors correct?
- Does the workflow step order match the intended flow?
- Are step types correct?
- Are branch conditions correct?
- Are true and false paths correct?
- Are downstream services correct?
- Are service methods and endpoints correct?
- Is authentication correctly represented?
- Are database tables correct?
- Are query inputs and outputs correct?
- Are no-record behaviors correct?
- Are business rule conditions correct?
- Are calculations correct?
- Are source-to-target request mappings correct?
- Are response mappings correct?
- Are error statuses and codes correct?
- Are generated test scenarios aligned with the requirement?
```

---

# Common Mistakes to Avoid

## Mistake 1 — Giving Full Accuracy Credit for Vague Fields

A vague field like:

```text
Apply business logic
```

should not receive full accuracy credit if the requirement contains specific business rules.

## Mistake 2 — Treating Inferred Values as Fully Accurate

Inferred values can be useful, but they should not receive full accuracy credit unless supported by source evidence.

## Mistake 3 — Ignoring Required vs Optional Differences

Marking an optional field as required can break real clients.

Marking a required field as optional can allow invalid requests.

Both should reduce accuracy.

## Mistake 4 — Ignoring Direction of Mapping

Request and response mappings must preserve source and target direction.

Example:

```text
request.body.skuNumbers -> downstreamFabricRequest.customSkuNumbers
```

is different from:

```text
downstreamFabricRequest.customSkuNumbers -> request.body.skuNumbers
```

## Mistake 5 — Ignoring Branch Direction

A branch can exist but still be wrong if the true and false paths are reversed.

## Mistake 6 — Ignoring Business Meaning

Accuracy is not only about matching field names.

The extracted schema must preserve the meaning of the requirement.

---

# Final Summary

Requirement accuracy scoring measures whether a generated Workflow Schema v2 correctly represents the original requirement source.

It evaluates the correctness of:

* API contract details
* Headers
* Body fields
* Validation rules
* Workflow steps
* Branching logic
* External services
* Authentication
* Database references
* Business rules
* Calculation rules
* Request mappings
* Response mappings
* Error mappings
* Test scenarios

This scoring category is critical because a complete schema is not useful if the extracted values are wrong.

This completes Phase 3 of Day 12.
