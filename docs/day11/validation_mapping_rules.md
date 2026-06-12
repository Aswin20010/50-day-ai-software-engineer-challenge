# Day 11 — Validation Mapping Rules

## Purpose

This document defines how validation requirements should be mapped from raw API requirements into Workflow Schema v2.

Validation mapping is important because most enterprise APIs require strong input checks before workflow execution begins.

This document explains how to identify validation rules in requirement sources and convert them into structured Schema v2 blocks.

No coding is performed in this phase.

---

## Background

On Day 10, Workflow Schema v2 introduced a formal `field_validations` block.

Day 11 Phase 2 defined how API contract details map into Schema v2.

Phase 3 focuses specifically on validation rules.

Validation rules may come from:

* Confluence requirement documents
* API contract tables
* Request examples
* Error examples
* Node-RED validation nodes
* Existing service behavior
* User-confirmed clarifications

---

## Validation Mapping Objective

The objective of validation mapping is to convert human-readable validation requirements into structured schema entries.

A raw requirement such as:

```text
Division-Id is required and must be either 12 or 13.
```

should become:

```yaml
field_validations:
  - validation_id: VAL-FABRIC-HDR-001
    field_path: request.headers.Division-Id
    location: header
    type: integer
    required: true
    allowed_values:
      - 12
      - 13
    failure_behavior:
      http_status: 400
      error_code: INVALID_DIVISION
      message: Division-Id must be 12 or 13.
```

The mapped validation should be specific enough for future generators to create:

* Validator logic
* Node-RED validation nodes
* API documentation
* Negative test cases
* Error responses

---

# Schema v2 Blocks Used

Validation mapping primarily fills these Schema v2 blocks:

```text
field_validations
workflow_steps
error_mappings
test_scenarios
source_traceability
generator_readiness
```

Validation requirements may also affect:

```text
api_contract
request_mappings
```

For example, an optional `Trace-Id` header may be validated as optional and also mapped downstream when present.

---

# 1. Required Field Mapping

## Requirement Signal

Look for phrases such as:

```text
required
mandatory
must be provided
cannot be missing
is needed
should not be null
field is required
```

## Mapping Rule

Map required fields into `field_validations` with:

```yaml
required: true
```

## Example Requirement

```text
fabricTypeId is required.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-FABRIC-BODY-003
    field_path: request.body.fabricTypeId
    location: body
    type: integer
    required: true
    failure_behavior:
      http_status: 400
      error_code: BAD_REQUEST
      message: fabricTypeId is required.
```

## Notes

Required field validation should always include failure behavior.

If the requirement does not specify the failure status, use `400` for invalid client input and mark confidence as medium.

---

# 2. Optional Field Mapping

## Requirement Signal

Look for phrases such as:

```text
optional
if present
when provided
may be provided
not mandatory
can be omitted
```

## Mapping Rule

Map optional fields into `field_validations` with:

```yaml
required: false
```

## Example Requirement

```text
Trace-Id is optional and should be passed downstream when present.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-FABRIC-HDR-002
    field_path: request.headers.Trace-Id
    location: header
    type: string
    required: false
```

If the optional field is passed downstream, also map it into `request_mappings`:

```yaml
request_mappings:
  - mapping_id: MAP-FABRIC-TRACE-001
    mapping_name: Pass Trace-Id Downstream
    source: request.headers.Trace-Id
    target: downstream.headers.Trace-Id
    required: false
    transformation: direct_copy
    pass_through_when_present: true
    omit_when_missing: true
```

---

# 3. Header Validation Mapping

## Requirement Signal

Look for requirement sections such as:

```text
Headers
Required Headers
Request Headers
Header Parameters
```

## Mapping Rule

Headers should map into `field_validations` with:

```yaml
location: header
field_path: request.headers.<Header-Name>
```

## Example Requirement

```text
Division-Id header is required.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-FABRIC-HDR-001
    field_path: request.headers.Division-Id
    location: header
    type: integer
    required: true
    failure_behavior:
      http_status: 400
      error_code: BAD_REQUEST
      message: Division-Id header is required.
```

## Notes

Header names should preserve the exact spelling and casing used by the requirement unless the project has a confirmed canonical format.

---

# 4. Body Field Validation Mapping

## Requirement Signal

Look for:

```text
Request body
Payload
Body fields
JSON body
Input object
```

## Mapping Rule

Body fields should map into `field_validations` with:

```yaml
location: body
field_path: request.body.<fieldName>
```

Nested fields should use dot notation.

## Example Requirement

```text
account.encryptInfo.encryptedData is required.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-DISCOUNT-BODY-003
    field_path: request.body.account.encryptInfo.encryptedData
    location: body
    type: string
    required: true
    allow_blank: false
    failure_behavior:
      http_status: 400
      error_code: BAD_REQUEST
      message: encryptedData is required.
```

---

# 5. Path Parameter Validation Mapping

## Requirement Signal

Look for route variables:

```text
/api/v1/accounts/{accountId}
/api/v1/items/{itemId}
```

## Mapping Rule

Path parameters should map into `field_validations` with:

```yaml
location: path
field_path: request.path.<parameterName>
```

## Example

```yaml
field_validations:
  - validation_id: VAL-ACCOUNT-PATH-001
    field_path: request.path.accountId
    location: path
    type: string
    required: true
    allow_blank: false
```

---

# 6. Query Parameter Validation Mapping

## Requirement Signal

Look for query parameters:

```text
?storeNbr=101
?includeInactive=true
?startDate=2026-01-01
```

## Mapping Rule

Query parameters should map into `field_validations` with:

```yaml
location: query
field_path: request.query.<parameterName>
```

## Example

```yaml
field_validations:
  - validation_id: VAL-ITEM-QUERY-001
    field_path: request.query.storeNbr
    location: query
    type: integer
    required: false
```

---

# 7. Data Type Validation Mapping

## Requirement Signal

Look for field type descriptions:

```text
string
integer
number
boolean
array
object
timestamp
date
decimal
```

## Mapping Rule

Map the type to:

```yaml
type: <type>
```

## Example Requirement

```text
fabricTypeId must be an integer.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-FABRIC-BODY-003
    field_path: request.body.fabricTypeId
    location: body
    type: integer
    required: true
```

## Recommended Type Values

```text
string
integer
number
boolean
array
object
timestamp
date
decimal
```

---

# 8. String Validation Mapping

## Requirement Signal

Look for:

```text
non-empty string
must not be blank
cannot be empty
max length
min length
must match pattern
```

## Mapping Rule

Use fields such as:

```yaml
allow_blank: false
min_length: integer
max_length: integer
pattern: string
```

## Example Requirement

```text
Each SKU number must be a non-blank string.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-FABRIC-BODY-002
    field_path: request.body.skuNumbers[]
    location: body
    type: string
    required: true
    allow_blank: false
    failure_behavior:
      http_status: 400
      error_code: BAD_REQUEST
      message: Each SKU number must be a non-blank string.
```

---

# 9. Numeric Validation Mapping

## Requirement Signal

Look for:

```text
must be numeric
must be an integer
must be positive
greater than zero
minimum value
maximum value
allowed values
```

## Mapping Rule

Use fields such as:

```yaml
type: integer
minimum: number
maximum: number
allowed_values:
  - value
```

## Example Requirement

```text
Division-Id must be either 12 or 13.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-FABRIC-HDR-001
    field_path: request.headers.Division-Id
    location: header
    type: integer
    required: true
    allowed_values:
      - 12
      - 13
    failure_behavior:
      http_status: 400
      error_code: INVALID_DIVISION
      message: Division-Id must be 12 or 13.
```

---

# 10. Enum Validation Mapping

## Requirement Signal

Look for:

```text
allowed values
valid values
enum
must be one of
supported values
```

## Mapping Rule

Map allowed values into:

```yaml
allowed_values:
  - value1
  - value2
```

## Example Requirement

```text
messageSeverity must be INFO or CRITICAL.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-MSG-BODY-001
    field_path: request.body.messageSeverity
    location: body
    type: string
    required: true
    allowed_values:
      - INFO
      - CRITICAL
    failure_behavior:
      http_status: 400
      error_code: INVALID_ENUM_VALUE
      message: messageSeverity must be INFO or CRITICAL.
```

## Notes

Preserve exact enum spelling from the source requirement.

If case sensitivity is unclear, add a source traceability note.

---

# 11. Array Validation Mapping

## Requirement Signal

Look for:

```text
list of
array of
must contain at least one
cannot be empty
each item
```

## Mapping Rule

Array fields should have at least two validation entries when item-level validation is required:

1. Array-level validation
2. Item-level validation

## Example Requirement

```text
skuNumbers is required and must contain at least one non-empty string.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-FABRIC-BODY-001
    field_path: request.body.skuNumbers
    location: body
    type: array
    required: true
    min_items: 1
    failure_behavior:
      http_status: 400
      error_code: BAD_REQUEST
      message: skuNumbers must contain at least one value.

  - validation_id: VAL-FABRIC-BODY-002
    field_path: request.body.skuNumbers[]
    location: body
    type: string
    required: true
    allow_blank: false
    failure_behavior:
      http_status: 400
      error_code: BAD_REQUEST
      message: Each SKU number must be a non-blank string.
```

---

# 12. Nested Object Validation Mapping

## Requirement Signal

Look for nested JSON objects such as:

```json
{
  "account": {
    "encryptInfo": {
      "encryptedData": "..."
    }
  }
}
```

## Mapping Rule

Map parent objects and required nested fields separately.

## Example Requirement

```text
account.encryptInfo is required and encryptedData must be provided.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-DISCOUNT-BODY-001
    field_path: request.body.account
    location: body
    type: object
    required: true

  - validation_id: VAL-DISCOUNT-BODY-002
    field_path: request.body.account.encryptInfo
    location: body
    type: object
    required: true

  - validation_id: VAL-DISCOUNT-BODY-003
    field_path: request.body.account.encryptInfo.encryptedData
    location: body
    type: string
    required: true
    allow_blank: false
```

---

# 13. Date and Timestamp Validation Mapping

## Requirement Signal

Look for:

```text
date
timestamp
startDateTime
endDateTime
must be ISO format
must be after
must be before
```

## Mapping Rule

Use:

```yaml
type: timestamp
format: ISO-8601
```

or:

```yaml
type: date
format: YYYY-MM-DD
```

## Example Requirement

```text
startDateTime is required and must be a valid timestamp.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-MSG-BODY-002
    field_path: request.body.startDateTime
    location: body
    type: timestamp
    required: true
    format: ISO-8601
    failure_behavior:
      http_status: 400
      error_code: INVALID_TIMESTAMP
      message: startDateTime must be a valid timestamp.
```

---

# 14. Conditional Validation Mapping

## Requirement Signal

Look for:

```text
required when
required if
only required for
if keyType is RSA, entryMode is required
if endDateTime is provided, it must be after startDateTime
```

## Mapping Rule

Use a condition field inside `field_validations`.

## Example Requirement

```text
endDateTime is optional, but if provided it must be after startDateTime.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-MSG-BODY-003
    field_path: request.body.endDateTime
    location: body
    type: timestamp
    required: false
    format: ISO-8601
    condition: request.body.endDateTime exists
    comparison:
      operator: greater_than
      compare_to: request.body.startDateTime
    failure_behavior:
      http_status: 400
      error_code: INVALID_DATE_RANGE
      message: endDateTime must be after startDateTime.
```

---

# 15. Validation Workflow Step Mapping

## Requirement Signal

Validation requirements should also create or support workflow validation steps.

For example:

```text
Validate headers
Validate body
Validate enum fields
Validate request payload
```

## Mapping Rule

Create workflow steps with:

```yaml
step_type: validation
```

## Example

```yaml
workflow_steps:
  - step_id: STEP-001
    step_name: Validate Request Headers
    step_type: validation
    description: Validate required request headers.
    inputs:
      - request.headers
    outputs:
      - validatedHeaders
    depends_on: []
    success_behavior: Continue to STEP-002.
    failure_behavior:
      error_code: BAD_REQUEST
      http_status: 400
      message: Request header validation failed.
```

## Notes

Do not create one workflow step for every field unless the workflow explicitly separates them.

A common pattern is:

```text
STEP-001 Validate Headers
STEP-002 Validate Body
STEP-003 Validate Enum Fields
```

The detailed field-level rules still belong in `field_validations`.

---

# 16. Validation Error Mapping

## Requirement Signal

Look for error details such as:

```text
Return 400 when validation fails.
Return BAD_REQUEST for missing required fields.
Invalid enum should return INVALID_ENUM_VALUE.
```

## Mapping Rule

Validation failure behavior should map to `error_mappings`.

## Example

```yaml
error_mappings:
  - error_mapping_id: ERR-VALIDATION-001
    error_source: validation
    condition: required field missing or invalid
    source_error_code: null
    target_error_code: BAD_REQUEST
    target_http_status: 400
    target_message: Required request field is missing or invalid.
    retryable: false
    fail_fast: true
    applies_to_steps:
      - STEP-001
      - STEP-002
```

---

# 17. Validation Test Scenario Mapping

## Requirement Signal

Every important validation rule should create at least one negative test candidate.

## Mapping Rule

Map validation scenarios into `test_scenarios`.

## Example

```yaml
test_scenarios:
  - scenario_id: TC-FABRIC-002
    scenario_name: Missing Division-Id Header
    scenario_type: negative
    input:
      headers: {}
      body:
        skuNumbers:
          - SKU123
        fabricTypeId: 3
    expected:
      http_status: 400
      response_body:
        code: BAD_REQUEST
      executed_steps:
        - STEP-001
```

## Recommended Validation Test Types

```text
missing required header
missing required body field
invalid enum value
invalid numeric value
empty array
blank array item
invalid timestamp
invalid conditional field
```

---

# 18. Validation Source Traceability

## Mapping Rule

Every validation rule should trace back to its source when possible.

Map source evidence into `source_traceability`.

## Example

```yaml
source_traceability:
  field_sources:
    - field_path: request.headers.Division-Id
      requirement_section: Header Requirements
      source_text_reference: Section 2.2
      confidence: high

    - field_path: request.body.skuNumbers[]
      requirement_section: Request Body Requirements
      source_text_reference: Section 2.3
      confidence: high
```

## Confidence Rules

Use:

```text
high
```

when the requirement explicitly states the validation.

Use:

```text
medium
```

when the validation comes from an example or prototype.

Use:

```text
low
```

when the validation is inferred.

---

# Validation Mapping Table

| Requirement Detail     | Schema v2 Target                    |
| ---------------------- | ----------------------------------- |
| Required header        | field_validations                   |
| Optional header        | field_validations                   |
| Required body field    | field_validations                   |
| Optional body field    | field_validations                   |
| Path parameter         | field_validations                   |
| Query parameter        | field_validations                   |
| Data type              | field_validations.type              |
| Enum values            | field_validations.allowed_values    |
| Numeric bounds         | field_validations.minimum / maximum |
| String non-blank rule  | field_validations.allow_blank       |
| Array non-empty rule   | field_validations.min_items         |
| Array item type        | field_validations with [] path      |
| Nested object          | field_validations with dot path     |
| Date/timestamp format  | field_validations.format            |
| Conditional validation | field_validations.condition         |
| Validation step        | workflow_steps                      |
| Validation error       | error_mappings                      |
| Validation test        | test_scenarios                      |
| Source evidence        | source_traceability                 |

---

# Example 1 — Fabric API Validation Mapping

## Raw Requirement

```text
Division-Id is required and must be either 12 or 13.
Trace-Id is optional.
skuNumbers is required and must contain at least one non-empty string.
fabricTypeId is required and must be an integer.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-FABRIC-HDR-001
    field_path: request.headers.Division-Id
    location: header
    type: integer
    required: true
    allowed_values:
      - 12
      - 13
    failure_behavior:
      http_status: 400
      error_code: INVALID_DIVISION
      message: Division-Id must be 12 or 13.

  - validation_id: VAL-FABRIC-HDR-002
    field_path: request.headers.Trace-Id
    location: header
    type: string
    required: false

  - validation_id: VAL-FABRIC-BODY-001
    field_path: request.body.skuNumbers
    location: body
    type: array
    required: true
    min_items: 1
    failure_behavior:
      http_status: 400
      error_code: BAD_REQUEST
      message: skuNumbers must contain at least one value.

  - validation_id: VAL-FABRIC-BODY-002
    field_path: request.body.skuNumbers[]
    location: body
    type: string
    required: true
    allow_blank: false
    failure_behavior:
      http_status: 400
      error_code: BAD_REQUEST
      message: Each sku number must be a non-blank string.

  - validation_id: VAL-FABRIC-BODY-003
    field_path: request.body.fabricTypeId
    location: body
    type: integer
    required: true
    failure_behavior:
      http_status: 400
      error_code: BAD_REQUEST
      message: fabricTypeId is required.
```

---

# Example 2 — Discount API Validation Mapping

## Raw Requirement

```text
requestorInfo-clientId is required.
requestorInfo-subclientId is required.
requestorInfo-traceId is optional.
x-macys-apikey is required.
x-hmac-secret is required.
account.encryptInfo is required.
account.entryMode is required.
keyType must be one of the supported key types.
entryMode must be one of the supported entry modes.
transaction.items is required and must contain at least one item.
```

## Schema v2 Mapping

```yaml
field_validations:
  - validation_id: VAL-DISCOUNT-HDR-001
    field_path: request.headers.requestorInfo-clientId
    location: header
    type: string
    required: true
    allow_blank: false

  - validation_id: VAL-DISCOUNT-HDR-002
    field_path: request.headers.requestorInfo-subclientId
    location: header
    type: string
    required: true
    allow_blank: false

  - validation_id: VAL-DISCOUNT-HDR-003
    field_path: request.headers.requestorInfo-traceId
    location: header
    type: string
    required: false

  - validation_id: VAL-DISCOUNT-HDR-004
    field_path: request.headers.x-macys-apikey
    location: header
    type: string
    required: true
    allow_blank: false

  - validation_id: VAL-DISCOUNT-HDR-005
    field_path: request.headers.x-hmac-secret
    location: header
    type: string
    required: true
    allow_blank: false

  - validation_id: VAL-DISCOUNT-BODY-001
    field_path: request.body.account.encryptInfo
    location: body
    type: object
    required: true

  - validation_id: VAL-DISCOUNT-BODY-002
    field_path: request.body.account.encryptInfo.keyType
    location: body
    type: string
    required: true
    allowed_values:
      - AES
      - RSA
      - IRN
      - AES_PAN
      - TOKEN
      - MAN
      - AES_RSA_HYBRID
      - CLEAR_TEXT
      - EZ_ENCRYPTION
      - ECV1
      - ECV2

  - validation_id: VAL-DISCOUNT-BODY-003
    field_path: request.body.account.entryMode
    location: body
    type: string
    required: true
    allowed_values:
      - KEYED
      - BARCODE_SCAN
      - ACCT_LOOKUP_WITHIN_TRANSACTION
      - ACCT_LOOKUP_SHOPPING_PASS
      - CONTACTLESS
      - B_CONNECTED
      - CLIENTELE
      - DCR_LOOKUP
      - MOBILE_WALLET_GOOGLE
      - NEW_ACCT_TRANSACTION
      - MOBILE_WALLET_ISIS
      - NEW_ACCT_APPROVED
      - MOBILE_WALLET_SOFTCARD
      - NEW_ACCT_NOT_APPROVED
      - PIN
      - SWIPED
      - TAP
      - TOKENIZED

  - validation_id: VAL-DISCOUNT-BODY-004
    field_path: request.body.transaction.items
    location: body
    type: array
    required: true
    min_items: 1
```

---

# Validation Review Checklist

Use this checklist after mapping validation rules.

```text
- Are all required headers mapped?
- Are all optional headers mapped?
- Are all required body fields mapped?
- Are all optional body fields mapped?
- Are path parameters mapped when present?
- Are query parameters mapped when present?
- Are field types captured?
- Are enum values captured exactly?
- Are array-level validations captured?
- Are array item validations captured?
- Are nested object validations captured?
- Are numeric constraints captured?
- Are string constraints captured?
- Are date/timestamp formats captured?
- Are conditional validations captured?
- Are validation workflow steps defined?
- Are validation errors mapped?
- Are negative test scenarios identified?
- Is source traceability captured?
- Are inferred rules marked with lower confidence?
```

---

# Common Mistakes to Avoid

## Mistake 1 — Only Creating Validation Steps

A workflow step like `Validate Request Body` is not enough.

The detailed field rules must also be captured in `field_validations`.

## Mistake 2 — Ignoring Array Item Rules

If the requirement says an array contains strings, map both the array and the item type.

Use:

```text
request.body.fieldName
request.body.fieldName[]
```

## Mistake 3 — Treating Optional Fields as Unimportant

Optional fields still matter.

They may be passed downstream, returned in responses, or validated when present.

## Mistake 4 — Missing Failure Behavior

Every required or restricted validation should define what happens when validation fails.

## Mistake 5 — Mixing Business Rules with Input Validation

Input validation checks whether the request is structurally valid.

Business rules decide whether the request is eligible or allowed after validation.

For example:

```text
Division-Id must be numeric
```

is validation.

```text
Employee account is eligible only if employeeFlag is true
```

is business logic.

## Mistake 6 — Losing Source Evidence

Every important validation should trace back to a requirement source when possible.

---

# Final Summary

Validation mapping converts raw input rules into structured Schema v2 validation entries.

The main Schema v2 block used is `field_validations`, but validation mapping also affects:

* `workflow_steps`
* `error_mappings`
* `test_scenarios`
* `source_traceability`
* `generator_readiness`

Strong validation mapping is essential because it supports future validator generation, Node-RED validation nodes, API documentation, and negative test cases.

This completes Phase 3 of Day 11.
