# Day 10 — Workflow Schema v2 Blocks

## Purpose

This document defines the formal blocks of Workflow Schema v2.

Workflow Schema v2 is an improved version of the Day 8 workflow schema. It uses the gaps identified during Day 9 validation to create a stronger structure for future artifact generation.

The purpose of this file is to define each major schema block, explain why it is needed, and describe what fields it should contain.

No coding is performed in this phase.

---

## Background

On Day 8, Workflow Schema v1 was created to represent API workflows as structured steps.

On Day 9, Schema v1 was validated using two sample APIs:

1. FEED Discount API
2. Customize IT Fabric API

The validation showed that Schema v1 was useful, but it needed improvements in several areas.

The most important improvements needed were:

* Formal branching support
* Standard error mapping
* External service authentication metadata
* Stronger database query references
* Structured calculation rules
* Field-level validation
* Optional pass-through mapping
* Test scenario metadata
* Source traceability
* Generator readiness tracking

Workflow Schema v2 introduces these improvements as formal schema blocks.

---

# Workflow Schema v2 — High-Level Structure

Schema v2 is organized into the following major blocks:

```yaml
workflow:
  metadata:
  api_contract:
  field_validations:
  workflow_steps:
  branching:
  external_services:
  database_references:
  business_rules:
  calculation_rules:
  request_mappings:
  response_mappings:
  error_mappings:
  test_scenarios:
  source_traceability:
  generator_readiness:
```

Each block has a clear purpose.

Some blocks are required for all workflows.

Some blocks are optional depending on the workflow type.

---

# 1. Metadata Block

## Purpose

The metadata block identifies the workflow and provides high-level descriptive information.

This block helps with:

* Workflow identification
* Documentation
* Versioning
* Traceability
* Generator planning
* Review status tracking

---

## Required Fields

```yaml
metadata:
  workflow_id: string
  workflow_name: string
  workflow_description: string
  workflow_version: string
  workflow_type: string
  source_domain: string
  schema_version: string
  status: string
```

---

## Field Descriptions

| Field                | Description                                                         |
| -------------------- | ------------------------------------------------------------------- |
| workflow_id          | Unique identifier for the workflow                                  |
| workflow_name        | Human-readable workflow name                                        |
| workflow_description | Summary of what the workflow does                                   |
| workflow_version     | Version of this workflow definition                                 |
| workflow_type        | Type of workflow, such as service_wrapper or business_orchestration |
| source_domain        | Business or technical domain of the workflow                        |
| schema_version       | Workflow schema version, such as v2                                 |
| status               | Draft, reviewed, complete, or deprecated                            |

---

## Example

```yaml
metadata:
  workflow_id: WF-DISCOUNT-001
  workflow_name: Evaluate Transaction Discount Workflow
  workflow_description: >
    Validates a transaction discount request, resolves account information,
    evaluates discount rules, calculates discounts, and returns a response.
  workflow_version: 1.0.0
  workflow_type: business_orchestration
  source_domain: FEED Discount
  schema_version: v2
  status: draft
```

---

# 2. API Contract Block

## Purpose

The API contract block describes the public API endpoint that starts the workflow.

This block supports:

* Handler generation
* API documentation
* DTO generation
* Request validation
* Test generation

---

## Required Fields

```yaml
api_contract:
  endpoint_id: string
  path: string
  method: string
  api_version: string
  content_type: string
  response_type: string
  operation_name: string
  summary: string
```

---

## Optional Fields

```yaml
api_contract:
  controller_name: string
  handler_name: string
  tags:
    - string
```

---

## Field Descriptions

| Field           | Description                          |
| --------------- | ------------------------------------ |
| endpoint_id     | Unique endpoint identifier           |
| path            | API route path                       |
| method          | HTTP method                          |
| api_version     | API version                          |
| content_type    | Request content type                 |
| response_type   | Response content type                |
| operation_name  | Logical API operation name           |
| summary         | Short description of the endpoint    |
| controller_name | Optional controller or router name   |
| handler_name    | Optional handler method name         |
| tags            | Optional tags used for documentation |

---

## Example

```yaml
api_contract:
  endpoint_id: EP-DISCOUNT-001
  path: /api/v1/discount/evaluateTransactionDiscount
  method: POST
  api_version: v1
  content_type: application/json
  response_type: application/json
  operation_name: evaluateTransactionDiscount
  summary: Evaluate transaction-level discount eligibility and return discount details.
  controller_name: DiscountController
  handler_name: EvaluateTransactionDiscount
  tags:
    - discount
    - feed
```

---

# 3. Field Validations Block

## Purpose

The field validations block defines validation rules for headers, path parameters, query parameters, and request body fields.

This block improves Schema v1 by making field-level validation explicit.

It supports:

* DTO validation
* Node-RED validation nodes
* API documentation
* Negative test generation
* Error mapping

---

## Structure

```yaml
field_validations:
  - validation_id: string
    field_path: string
    location: header | path | query | body
    type: string
    required: boolean
    allowed_values:
      - string
    min_items: integer
    max_items: integer
    allow_blank: boolean
    format: string
    failure_behavior:
      http_status: integer
      error_code: string
      message: string
```

---

## Field Descriptions

| Field            | Description                                  |
| ---------------- | -------------------------------------------- |
| validation_id    | Unique validation rule ID                    |
| field_path       | Field path in request                        |
| location         | Field location: header, path, query, or body |
| type             | Expected data type                           |
| required         | Whether the field is mandatory               |
| allowed_values   | Enum or allowed values                       |
| min_items        | Minimum array items                          |
| max_items        | Maximum array items                          |
| allow_blank      | Whether blank string is allowed              |
| format           | Optional format, such as timestamp or UUID   |
| failure_behavior | Error returned when validation fails         |

---

## Example — Required Header

```yaml
field_validations:
  - validation_id: VAL-DISCOUNT-HDR-001
    field_path: request.headers.requestorInfo-clientId
    location: header
    type: string
    required: true
    allow_blank: false
    failure_behavior:
      http_status: 400
      error_code: BAD_REQUEST
      message: requestorInfo-clientId header is required.
```

---

## Example — Enum Validation

```yaml
field_validations:
  - validation_id: VAL-DISCOUNT-BODY-002
    field_path: request.body.account.entryMode
    location: body
    type: string
    required: true
    allowed_values:
      - KEYED
      - BARCODE_SCAN
      - TOKENIZED
      - SWIPED
      - TAP
    failure_behavior:
      http_status: 400
      error_code: INVALID_ENUM_VALUE
      message: entryMode contains an unsupported value.
```

---

## Example — Array Validation

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
      message: Each sku number must be a non-blank string.
```

---

# 4. Workflow Steps Block

## Purpose

The workflow steps block defines the ordered actions that make up the API workflow.

This is the core of the schema.

It supports:

* Service orchestration
* Node generation
* Documentation
* Testing
* Step-level traceability

---

## Structure

```yaml
workflow_steps:
  - step_id: string
    step_name: string
    step_type: string
    description: string
    inputs:
      - string
    outputs:
      - string
    depends_on:
      - string
    condition: string
    success_behavior: string
    failure_behavior:
      error_code: string
      http_status: integer
      message: string
```

---

## Supported Step Types

```text
validation
mapping
external_service
database_query
business_rule
calculation
response_mapping
error_mapping
branching
context_preparation
```

---

## Example

```yaml
workflow_steps:
  - step_id: STEP-004
    step_name: Build Account Interrogation Request
    step_type: mapping
    description: >
      Build downstream request for account interrogation using account
      encryption information and request headers.
    inputs:
      - request.headers
      - request.body.account.encryptInfo
      - request.body.account.entryMode
    outputs:
      - accountInterrogationRequest
    depends_on:
      - STEP-003
    success_behavior: Continue to STEP-005.
    failure_behavior:
      error_code: INTERNAL_MAPPING_ERROR
      http_status: 500
      message: Failed to build account interrogation request.
```

---

# 5. Branching Block

## Purpose

The branching block defines conditional workflow paths.

This is one of the major improvements in Schema v2.

It supports:

* Employee versus new account path
* Success versus error response path
* Conditional routing
* Branch merge behavior
* Direct-return branches

---

## Structure

```yaml
branching:
  - branch_id: string
    branch_name: string
    branch_type: conditional | direct_response | multi_path
    condition: string
    source_step: string
    true_path: string
    false_path: string
    merge_step: string | none
    description: string
```

---

## Field Descriptions

| Field       | Description                             |
| ----------- | --------------------------------------- |
| branch_id   | Unique branch identifier                |
| branch_name | Human-readable branch name              |
| branch_type | Type of branch                          |
| condition   | Condition expression                    |
| source_step | Step that produces the branch condition |
| true_path   | Step executed when condition is true    |
| false_path  | Step executed when condition is false   |
| merge_step  | Step where paths merge, or none         |
| description | Explanation of the branch               |

---

## Example — Employee vs New Account

```yaml
branching:
  - branch_id: BRANCH-DISCOUNT-001
    branch_name: Employee versus New Account Discount Path
    branch_type: conditional
    condition: validatedAccountInfo.employeeFlag == true
    source_step: STEP-008
    true_path: STEP-009
    false_path: STEP-010
    merge_step: STEP-011
    description: >
      Routes the workflow to employee discount evaluation when employeeFlag is
      true. Otherwise routes to new account discount evaluation.
```

---

## Example — Success vs Error Direct Response

```yaml
branching:
  - branch_id: BRANCH-FABRIC-001
    branch_name: Downstream Success or Error Response Path
    branch_type: direct_response
    condition: downstreamFabricResponse.statusCode is success
    source_step: STEP-006
    true_path: STEP-007
    false_path: STEP-008
    merge_step: none
    description: >
      Routes downstream success responses to success mapping and downstream
      error responses to error mapping.
```

---

# 6. External Services Block

## Purpose

The external services block defines downstream systems used by the workflow.

This block supports:

* REST client generation
* SOAP client generation
* Node-RED HTTP request nodes
* Documentation
* Error handling
* Test mocking

---

## Structure

```yaml
external_services:
  - service_id: string
    service_name: string
    service_type: REST | SOAP
    purpose: string
    method: string
    endpoint_reference: string
    operation_name: string
    request_source:
      - string
    response_used_by:
      - string
    timeout_required: boolean
    retry_required: boolean
    error_behavior:
      timeout: string
      unauthorized: string
      invalid_response: string
```

---

## Example

```yaml
external_services:
  - service_id: SRV-INTERROGATE-ACCOUNT
    service_name: Interrogate Account Service
    service_type: REST
    purpose: Resolve account information before discount evaluation.
    method: POST
    endpoint_reference: /accountinterrogation/api/v1/accountinterrogation/piidLookup
    operation_name: piidLookup
    request_source:
      - accountInterrogationRequest
    response_used_by:
      - STEP-006
      - STEP-007
      - STEP-008
    timeout_required: true
    retry_required: false
    error_behavior:
      timeout: Return downstream timeout error.
      unauthorized: Return downstream authentication error.
      invalid_response: Return invalid downstream response error.
```

---

# 7. Authentication Block

## Purpose

The authentication block defines how external services are authenticated.

This block improves Schema v1 by explicitly representing API keys, HMAC signatures, service configuration authentication, and other authentication methods.

---

## Structure

```yaml
authentication:
  - auth_id: string
    applies_to_service: string
    type: api_key | hmac | oauth | service_config | none
    required: boolean
    required_headers:
      - string
    api_key:
      header_name: string
      source: string
    signature:
      header_name: string
      format: string
      algorithm: string
      encoding: string
      payload_source: string
      secret_source: string
```

---

## Example — HMAC

```yaml
authentication:
  - auth_id: AUTH-INTERROGATE-001
    applies_to_service: SRV-INTERROGATE-ACCOUNT
    type: hmac
    required: true
    required_headers:
      - x-macys-apikey
      - Authorization
    api_key:
      header_name: x-macys-apikey
      source: request.headers.x-macys-apikey
    signature:
      header_name: Authorization
      format: MAC <signature>
      algorithm: HMAC-SHA256
      encoding: base64
      payload_source: accountInterrogationRequest
      secret_source: request.headers.x-hmac-secret
```

---

## Example — Service Config

```yaml
authentication:
  - auth_id: AUTH-FABRIC-001
    applies_to_service: SRV-FABRIC-CUSTOMIZATION
    type: service_config
    required: true
    required_headers: []
```

---

# 8. Database References Block

## Purpose

The database references block connects workflow steps and business rules to database queries.

This block improves Schema v1 by making query usage explicit.

It supports:

* Repository generation
* SQL generation
* Excel query sheet generation
* Business rule traceability
* Test mocking

---

## Structure

```yaml
database_references:
  - query_id: string
    query_name: string
    database_type: string
    table_names:
      - string
    used_by_step: string
    used_by_business_rule: string
    purpose: string
    inputs:
      field_name: source_path
    outputs:
      field_name: target_path
    no_record_behavior: string
    failure_behavior: string
```

---

## Example

```yaml
database_references:
  - query_id: SQL-EMP-002
    query_name: Lookup Employee Discount Profile
    database_type: PostgreSQL
    table_names:
      - Disc_By_Dept_Vnd_Class
      - Emp_Disc_Profile
    used_by_step: STEP-009
    used_by_business_rule: BR-EMP-003
    purpose: Lookup employee discount profile by division, department, vendor, and class.
    inputs:
      division: discountDbContext.division
      department: transactionItem.department
      vendor: transactionItem.vendor
      class: transactionItem.class
    outputs:
      discountProfileName: employeeDiscountProfile.profileName
      discountPercent: employeeDiscountProfile.discountPercent
    no_record_behavior: continue_without_employee_discount
    failure_behavior: database_error
```

---

# 9. Business Rules Block

## Purpose

The business rules block defines business logic in a structured and traceable way.

It supports:

* Business documentation
* Service logic generation
* Test case generation
* Gap analysis
* Requirement traceability

---

## Structure

```yaml
business_rules:
  - rule_id: string
    rule_name: string
    category: string
    description: string
    applies_to_step: string
    condition: string
    inputs:
      - string
    outputs:
      - string
    related_queries:
      - string
    success_behavior: string
    failure_behavior: string
```

---

## Example

```yaml
business_rules:
  - rule_id: BR-EMP-003
    rule_name: Employee Discount Profile Selection
    category: employee_discount
    description: >
      Select the employee discount profile based on item division, department,
      vendor, and class.
    applies_to_step: STEP-009
    condition: employeeFlag == true
    inputs:
      - discountDbContext
      - transactionItem
    outputs:
      - employeeDiscountProfile
    related_queries:
      - SQL-EMP-002
    success_behavior: Continue employee discount calculation.
    failure_behavior: Continue without employee discount if no profile is found.
```

---

# 10. Calculation Rules Block

## Purpose

The calculation rules block defines calculation logic in a structured format.

This block is important for APIs that calculate discounts, totals, caps, eligibility results, scores, or derived values.

It supports:

* Service logic generation
* Test generation
* Documentation
* Validation of calculation behavior

---

## Structure

```yaml
calculation_rules:
  - calculation_id: string
    calculation_name: string
    applies_when: string
    calculation_type: string
    inputs:
      - string
    output: string
    calculation_behavior: string
    cap_behavior: string
    item_level: boolean
    rounding_behavior: string
    failure_behavior: string
```

---

## Example

```yaml
calculation_rules:
  - calculation_id: CALC-DISCOUNT-001
    calculation_name: Apply Employee Base Discount
    applies_when: discountType == employee
    calculation_type: percentage
    inputs:
      - transactionItem.amount
      - employeeDiscountProfile.discountPercent
    output: transactionItem.employeeDiscountAmount
    calculation_behavior: Apply employee discount percentage to eligible item amount.
    cap_behavior: none
    item_level: true
    rounding_behavior: standard_currency_rounding
    failure_behavior: discount_calculation_error
```

---

## Example — Cap Rule

```yaml
calculation_rules:
  - calculation_id: CALC-DISCOUNT-002
    calculation_name: Apply New Account Cap
    applies_when: discountType == new_account
    calculation_type: cap
    inputs:
      - calculatedNewAccountDiscount
      - config.newAccountCapAmount
    output: cappedNewAccountDiscount
    calculation_behavior: Use the lower value between calculated discount and configured cap.
    cap_behavior: min(calculatedNewAccountDiscount, config.newAccountCapAmount)
    item_level: false
    rounding_behavior: standard_currency_rounding
    failure_behavior: discount_calculation_error
```

---

# 11. Request Mappings Block

## Purpose

The request mappings block defines how input data is transformed into internal context or downstream service requests.

It supports:

* Mapper generation
* External client request construction
* Node-RED mapping nodes
* Documentation
* Test setup

---

## Structure

```yaml
request_mappings:
  - mapping_id: string
    mapping_name: string
    applies_to_step: string
    source: string
    target: string
    required: boolean
    transformation: string
    pass_through_when_present: boolean
    omit_when_missing: boolean
```

---

## Example — Downstream Mapping

```yaml
request_mappings:
  - mapping_id: MAP-INTERROGATE-001
    mapping_name: Map Encrypt Info to Account Interrogation Request
    applies_to_step: STEP-004
    source: request.body.account.encryptInfo
    target: accountInterrogationRequest.elements[0].encryptInfo
    required: true
    transformation: direct_copy
    pass_through_when_present: false
    omit_when_missing: false
```

---

## Example — Optional Pass-Through

```yaml
request_mappings:
  - mapping_id: MAP-FABRIC-002
    mapping_name: Pass Trace-Id Header to Downstream
    applies_to_step: STEP-004
    source: request.headers.Trace-Id
    target: downstream.headers.Trace-Id
    required: false
    transformation: direct_copy
    pass_through_when_present: true
    omit_when_missing: true
```

---

# 12. Response Mappings Block

## Purpose

The response mappings block defines how workflow outputs or downstream responses are transformed into public API responses.

It supports:

* Response DTO generation
* Mapper generation
* Documentation
* Test expected response validation

---

## Structure

```yaml
response_mappings:
  - mapping_id: string
    mapping_name: string
    applies_to_step: string
    response_type: success | error
    http_status: integer
    source: string
    target: string
    transformation: string
    required: boolean
```

---

## Example

```yaml
response_mappings:
  - mapping_id: RESP-DISCOUNT-001
    mapping_name: Map Discount Type
    applies_to_step: STEP-013
    response_type: success
    http_status: 200
    source: calculatedDiscount.discountType
    target: response.discountType
    transformation: direct_copy
    required: true
```

---

# 13. Error Mappings Block

## Purpose

The error mappings block standardizes error behavior across workflow steps.

This block is one of the major Schema v2 improvements.

It supports:

* Consistent API error responses
* Error handler generation
* Node-RED error paths
* Documentation
* Negative test generation

---

## Structure

```yaml
error_mappings:
  - error_mapping_id: string
    error_source: string
    condition: string
    source_error_code: string
    target_error_code: string
    target_http_status: integer
    target_message: string
    retryable: boolean
    fail_fast: boolean
    applies_to_steps:
      - string
```

---

## Example

```yaml
error_mappings:
  - error_mapping_id: ERR-VALIDATION-001
    error_source: validation
    condition: required field missing
    source_error_code: null
    target_error_code: BAD_REQUEST
    target_http_status: 400
    target_message: Required field is missing.
    retryable: false
    fail_fast: true
    applies_to_steps:
      - STEP-001
      - STEP-002
      - STEP-003
```

---

## Example — Downstream Error

```yaml
error_mappings:
  - error_mapping_id: ERR-DOWNSTREAM-001
    error_source: downstream_service
    condition: service unavailable or timeout
    source_error_code: null
    target_error_code: DOWNSTREAM_SERVICE_ERROR
    target_http_status: 502
    target_message: Downstream service failed.
    retryable: true
    fail_fast: true
    applies_to_steps:
      - STEP-005
```

---

# 14. Test Scenarios Block

## Purpose

The test scenarios block defines structured test cases for the workflow.

This block is required for future test generation and evaluation.

It supports:

* Positive test generation
* Negative test generation
* Boundary testing
* Mock downstream responses
* Mock database responses
* Expected workflow path validation

---

## Structure

```yaml
test_scenarios:
  - scenario_id: string
    scenario_name: string
    scenario_type: positive | negative | boundary | failure
    description: string
    input:
      headers: object
      path_params: object
      query_params: object
      body: object
    mocks:
      external_services: object
      database: object
    expected:
      http_status: integer
      response_body: object
      executed_steps:
        - string
      skipped_steps:
        - string
```

---

## Example

```yaml
test_scenarios:
  - scenario_id: TC-FABRIC-001
    scenario_name: Valid Fabric Request
    scenario_type: positive
    description: Valid request should call downstream fabric service and return success.
    input:
      headers:
        Division-Id: 12
        Trace-Id: trace-123
      body:
        skuNumbers:
          - SKU123
          - SKU456
        fabricTypeId: 3
    mocks:
      external_services:
        SRV-FABRIC-CUSTOMIZATION:
          status: 200
          body:
            fabrics:
              - skuNumber: SKU123
    expected:
      http_status: 200
      response_body:
        contains: fabrics
      executed_steps:
        - STEP-001
        - STEP-002
        - STEP-003
        - STEP-004
        - STEP-005
        - STEP-006
        - STEP-007
      skipped_steps:
        - STEP-008
```

---

# 15. Source Traceability Block

## Purpose

The source traceability block links workflow elements back to the original requirement source.

This is important for:

* Research evaluation
* Requirement coverage analysis
* Gap detection
* Human review
* Auditability

---

## Structure

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

---

## Example

```yaml
source_traceability:
  workflow_source:
    requirement_doc: FEED Discount Requirement
    requirement_section: Evaluate Transaction Discount
    source_text_reference: Section 2.1
    confidence: high

  step_sources:
    - step_id: STEP-009
      requirement_section: Employee Discount Logic
      source_text_reference: Section 4.2
      confidence: high
```

---

# 16. Generator Readiness Block

## Purpose

The generator readiness block identifies whether the workflow has enough information for each future generator.

This helps decide whether a workflow is ready for artifact generation or still needs refinement.

---

## Structure

```yaml
generator_readiness:
  node_red:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string

  excel:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string

  go:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string

  documentation:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string

  tests:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string

  evaluation:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string
```

---

## Example

```yaml
generator_readiness:
  node_red:
    status: mostly_ready
    missing_fields:
      - node_type_mapping
      - wire_layout_strategy
    notes: Workflow steps and branches are clear, but visual layout metadata is missing.

  excel:
    status: ready
    missing_fields: []
    notes: Workflow has endpoint, service, business rule, and query references.

  go:
    status: partially_ready
    missing_fields:
      - dto_names
      - repository_method_names
      - typed_error_definitions
    notes: More implementation-level naming is needed before code generation.

  documentation:
    status: ready
    missing_fields: []
    notes: Workflow has enough metadata for documentation.

  tests:
    status: mostly_ready
    missing_fields:
      - more_boundary_cases
    notes: Initial structured test scenarios are present.

  evaluation:
    status: partially_ready
    missing_fields:
      - full requirement traceability coverage
    notes: Source references exist but need broader coverage.
```

---

# Required vs Optional Blocks

Not every workflow needs every block.

Schema v2 should support both complex and simple workflows.

---

## Required for All Workflows

```text
metadata
api_contract
field_validations
workflow_steps
response_mappings
error_mappings
source_traceability
generator_readiness
```

---

## Optional Based on Workflow Type

```text
branching
external_services
authentication
database_references
business_rules
calculation_rules
request_mappings
test_scenarios
```

---

## Example — Simple Wrapper API

A simple wrapper API may use:

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

It may not need:

```text
database_references
calculation_rules
complex business_rules
```

---

## Example — Complex Business API

A complex business API may use:

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

# Naming Conventions

Schema v2 should use consistent IDs.

## Recommended Prefixes

| Artifact Type    | Prefix Example |
| ---------------- | -------------- |
| Workflow         | WF-            |
| Endpoint         | EP-            |
| Step             | STEP-          |
| Validation       | VAL-           |
| Branch           | BRANCH-        |
| External Service | SRV-           |
| Authentication   | AUTH-          |
| Query            | SQL-           |
| Business Rule    | BR-            |
| Calculation      | CALC-          |
| Request Mapping  | MAP-           |
| Response Mapping | RESP-          |
| Error Mapping    | ERR-           |
| Test Scenario    | TC-            |

---

# Schema v2 Design Benefits

Workflow Schema v2 provides the following benefits:

* Stronger workflow structure
* Better support for branching
* Standardized error handling
* Explicit authentication metadata
* Better database query linking
* Structured calculation rules
* Better validation details
* Stronger test generation support
* Improved traceability
* Better generator readiness assessment

---

# Final Summary

Workflow Schema v2 improves the Day 8 schema by converting Day 9 validation gaps into formal schema blocks.

The most important Schema v2 additions are:

* Field validations
* Branching
* Authentication
* Database references
* Calculation rules
* Error mappings
* Test scenarios
* Source traceability
* Generator readiness

These blocks make the workflow schema more complete, more consistent, and more useful for future artifact generation.

This completes Phase 2 of Day 10.
