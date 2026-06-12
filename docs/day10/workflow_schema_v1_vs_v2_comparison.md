# Day 10 — Workflow Schema v1 vs v2 Comparison

## Purpose

This document compares Workflow Schema v1 from Day 8 with Workflow Schema v2 from Day 10.

The purpose of this comparison is to clearly show how Schema v2 improves the original workflow schema based on the validation gaps found on Day 9.

No coding is performed in this phase.

---

## Background

On Day 8, Workflow Schema v1 was created as the first formal structure for representing API workflows.

On Day 9, Schema v1 was validated against two sample APIs:

1. FEED Discount API
2. Customize IT Fabric API

The validation showed that Schema v1 was useful, but it needed stronger structure before it could support reliable artifact generation.

Day 10 introduces Workflow Schema v2 as an improved version of that schema.

---

## High-Level Summary

Schema v1 focused on representing workflows as ordered steps.

Schema v2 improves that structure by adding formal blocks for:

* Field validation
* Branching
* External service authentication
* Database query linking
* Business rule traceability
* Calculation rules
* Request mappings
* Response mappings
* Standard error mappings
* Test scenarios
* Source traceability
* Generator readiness

---

# Summary Comparison Table

| Area                     | Schema v1                                          | Schema v2 Improvement                                                                              |
| ------------------------ | -------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Workflow metadata        | Basic workflow details                             | Adds stronger versioning, workflow type, source domain, schema version, and status                 |
| API contract             | Endpoint and method captured                       | Adds operation name, content type, response type, controller, handler, and tags                    |
| Field validation         | Validation described within steps                  | Adds formal field_validations block                                                                |
| Header validation        | Captured as validation step                        | Adds header location, type, required flag, allowed values, and failure behavior                    |
| Body validation          | Captured as validation step                        | Adds nested field paths, array validation, item validation, and format rules                       |
| Enum validation          | Described in validation notes                      | Adds allowed_values directly in field-level validation                                             |
| Workflow steps           | Step ID, name, type, inputs, outputs, dependencies | Keeps core step structure and adds clearer conditions, success behavior, and failure behavior      |
| Branching                | Informal dependency notes                          | Adds formal branching block with condition, true path, false path, and merge step                  |
| External services        | Service reference captured                         | Adds detailed service metadata, request source, response usage, timeout, retry, and error behavior |
| Authentication           | Limited or embedded in service notes               | Adds formal authentication block for API key, HMAC, OAuth, service config, or none                 |
| Database references      | Query references listed                            | Adds step-linked and business-rule-linked query references                                         |
| Business rules           | Business rules listed                              | Adds conditions, inputs, outputs, related queries, success behavior, and failure behavior          |
| Calculation rules        | Calculation step described                         | Adds structured calculation_rules block                                                            |
| Request mapping          | Mapping described in workflow step                 | Adds formal request_mappings block with source, target, transformation, and optional pass-through  |
| Response mapping         | Final response described                           | Adds formal response_mappings block                                                                |
| Error handling           | Step-level failure behavior                        | Adds reusable error_mappings block                                                                 |
| Test cases               | Not formally included                              | Adds test_scenarios block                                                                          |
| Source traceability      | Limited                                            | Adds source_traceability block                                                                     |
| Generator readiness      | Discussed in notes                                 | Adds generator_readiness block                                                                     |
| Complex workflow support | Partial                                            | Stronger support for orchestration, branching, DB rules, and calculations                          |
| Simple wrapper support   | Supported but not optimized                        | Allows optional sections so wrapper APIs are not forced into unnecessary blocks                    |

---

# 1. Workflow Metadata Comparison

## Schema v1

Schema v1 captured basic workflow metadata.

Example:

```yaml
workflow_id: WF-DISCOUNT-001
workflow_name: Evaluate Transaction Discount Workflow
endpoint: /api/v1/discount/evaluateTransactionDiscount
method: POST
version: v1
status: draft
```

## Limitation in Schema v1

Schema v1 did not consistently separate:

* Workflow version
* API version
* Schema version
* Workflow type
* Source domain
* Review status

This could create confusion when multiple versions of a workflow or schema exist.

## Schema v2 Improvement

Schema v2 introduces a stronger metadata block.

```yaml
metadata:
  workflow_id: WF-DISCOUNT-001
  workflow_name: Evaluate Transaction Discount Workflow
  workflow_description: >
    Validates a transaction discount request and evaluates discount rules.
  workflow_version: 1.0.0
  workflow_type: business_orchestration
  source_domain: FEED Discount
  schema_version: v2
  status: draft
```

## Benefit

This makes workflows easier to version, review, document, and generate from.

---

# 2. API Contract Comparison

## Schema v1

Schema v1 captured endpoint path and method.

Example:

```yaml
api_endpoint: /api/v1/fabrics
http_method: POST
api_version: v1
```

## Limitation in Schema v1

Schema v1 did not consistently capture:

* Operation name
* Controller name
* Handler name
* Content type
* Response type
* Documentation tags

## Schema v2 Improvement

Schema v2 adds a formal API contract block.

```yaml
api_contract:
  endpoint_id: EP-FABRIC-001
  path: /api/v1/fabrics
  method: POST
  api_version: v1
  content_type: application/json
  response_type: application/json
  operation_name: getFabricConfiguration
  summary: Retrieve fabric configuration for SKU numbers and fabric type.
  controller_name: FabricController
  handler_name: GetFabrics
  tags:
    - fabric
    - customization
```

## Benefit

This improves readiness for:

* Go handler generation
* API documentation
* OpenAPI-style documentation
* Test generation
* Excel endpoint mapping

---

# 3. Field Validation Comparison

## Schema v1

Schema v1 represented validation mostly as workflow steps.

Example:

```yaml
step_id: STEP-002
step_name: Validate Request Body
step_type: validation
```

## Limitation in Schema v1

Schema v1 could describe validation, but it did not define each field in a structured way.

It was not precise enough for:

* DTO validation
* Field-level test generation
* Array validation
* Nested field validation
* Enum validation

## Schema v2 Improvement

Schema v2 adds a formal `field_validations` block.

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
```

## Benefit

This makes validation rules machine-readable and generator-friendly.

---

# 4. Header Validation Comparison

## Schema v1

Schema v1 could list headers and describe required headers.

Example:

```yaml
headers:
  - name: Division-Id
    required: true
    type: integer
```

## Limitation in Schema v1

Schema v1 did not always define:

* Header allowed values
* Header format
* Failure behavior
* Optional pass-through behavior

## Schema v2 Improvement

Schema v2 supports detailed header validation.

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

## Benefit

This supports consistent validation logic and test generation.

---

# 5. Body Validation Comparison

## Schema v1

Schema v1 could represent required request body fields.

Example:

```yaml
body:
  skuNumbers:
    required: true
    type: array
  fabricTypeId:
    required: true
    type: integer
```

## Limitation in Schema v1

Schema v1 did not strongly support:

* Nested field paths
* Array item validation
* Non-empty arrays
* Non-blank array items
* Field formats
* Field-specific failure behavior

## Schema v2 Improvement

Schema v2 explicitly supports nested and array validation.

```yaml
field_validations:
  - validation_id: VAL-FABRIC-BODY-001
    field_path: request.body.skuNumbers
    location: body
    type: array
    required: true
    min_items: 1

  - validation_id: VAL-FABRIC-BODY-002
    field_path: request.body.skuNumbers[]
    location: body
    type: string
    required: true
    allow_blank: false
```

## Benefit

This makes the schema more useful for validator generation and negative testing.

---

# 6. Workflow Steps Comparison

## Schema v1

Schema v1 introduced workflow steps as the core structure.

Example:

```yaml
step_id: STEP-005
step_name: Call Interrogate Account Service
step_type: external_service
inputs:
  - accountInterrogationRequest
outputs:
  - accountInterrogationResponse
depends_on:
  - STEP-004
```

## Strength of Schema v1

This was one of the strongest parts of Schema v1.

It successfully introduced:

* Step IDs
* Step names
* Step types
* Inputs
* Outputs
* Dependencies

## Schema v2 Improvement

Schema v2 keeps this structure but makes it more consistent.

```yaml
workflow_steps:
  - step_id: STEP-005
    step_name: Call Interrogate Account Service
    step_type: external_service
    description: Invoke account interrogation downstream service.
    inputs:
      - accountInterrogationRequest
    outputs:
      - accountInterrogationResponse
    depends_on:
      - STEP-004
    condition: none
    success_behavior: Continue to STEP-006.
    failure_behavior:
      error_code: DOWNSTREAM_SERVICE_ERROR
      http_status: 502
      message: Account interrogation service failed.
```

## Benefit

This preserves Schema v1 strengths while improving consistency.

---

# 7. Branching Comparison

## Schema v1

Schema v1 represented branching informally through descriptions or dependencies.

Example:

```text
If employeeFlag is true, execute employee path.
If employeeFlag is false, execute new account path.
```

## Limitation in Schema v1

This was not enough for generation.

Generators need to know:

* Branch condition
* Source step
* True path
* False path
* Merge step
* Branch type

## Schema v2 Improvement

Schema v2 adds a formal branching block.

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
```

## Benefit

This makes branching usable for:

* Node-RED wire generation
* Go service orchestration
* Test path validation
* Documentation

---

# 8. External Service Comparison

## Schema v1

Schema v1 could reference external services.

Example:

```yaml
service_id: SRV-INTERROGATE-ACCOUNT
service_name: Interrogate Account Service
protocol: REST
method: POST
```

## Limitation in Schema v1

Schema v1 did not consistently define:

* Request source
* Response usage
* Timeout behavior
* Retry behavior
* Error behavior
* Authentication details

## Schema v2 Improvement

Schema v2 defines a stronger external service block.

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
```

## Benefit

This improves readiness for external client generation and service documentation.

---

# 9. Authentication Comparison

## Schema v1

Schema v1 either omitted authentication or described it in notes.

Example:

```text
Use HMAC authentication.
```

## Limitation in Schema v1

This is not enough for generation.

HMAC requires structured details such as:

* Header name
* Signature format
* Algorithm
* Encoding
* Payload source
* Secret source

## Schema v2 Improvement

Schema v2 adds a formal authentication block.

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

## Benefit

This supports future Go client generation and Node-RED HTTP configuration.

---

# 10. Database Query Reference Comparison

## Schema v1

Schema v1 listed query references.

Example:

```yaml
query_id: SQL-EMP-002
purpose: Lookup employee discount profile.
inputs:
  - division
  - department
  - vendor
  - class
```

## Limitation in Schema v1

Schema v1 did not strongly link queries to:

* Workflow steps
* Business rules
* Input source paths
* Output target paths
* No-record behavior
* Failure behavior

## Schema v2 Improvement

Schema v2 introduces stronger database references.

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
    purpose: Lookup employee discount profile.
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

## Benefit

This improves readiness for repository generation, Excel query sheets, and test mocks.

---

# 11. Business Rules Comparison

## Schema v1

Schema v1 could list business rules.

Example:

```yaml
business_rule_id: BR-EMP-001
name: Employee Account Eligibility Check
condition: employeeFlag is true
```

## Limitation in Schema v1

Schema v1 did not always capture:

* Applies-to step
* Inputs
* Outputs
* Related queries
* Success behavior
* Failure behavior

## Schema v2 Improvement

Schema v2 strengthens the business rules block.

```yaml
business_rules:
  - rule_id: BR-EMP-003
    rule_name: Employee Discount Profile Selection
    category: employee_discount
    description: Select employee discount profile based on item attributes.
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

## Benefit

This makes business rules more traceable and generation-ready.

---

# 12. Calculation Rules Comparison

## Schema v1

Schema v1 could mark a step as a calculation.

Example:

```yaml
step_type: calculation
description: Calculate final discount.
```

## Limitation in Schema v1

Schema v1 did not define:

* Calculation ID
* Applies-when condition
* Inputs
* Output
* Calculation type
* Cap behavior
* Rounding behavior
* Item-level behavior

## Schema v2 Improvement

Schema v2 adds a structured calculation rules block.

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
    calculation_behavior: Apply percentage to eligible item amount.
    cap_behavior: none
    item_level: true
    rounding_behavior: standard_currency_rounding
    failure_behavior: discount_calculation_error
```

## Benefit

This helps future service logic generation and calculation test generation.

---

# 13. Request Mapping Comparison

## Schema v1

Schema v1 described request mappings inside workflow steps.

Example:

```text
Map skuNumbers to downstream customSkuNumbers.
```

## Limitation in Schema v1

Schema v1 did not consistently define:

* Mapping ID
* Applies-to step
* Source path
* Target path
* Transformation type
* Optional pass-through behavior
* Omit-when-missing behavior

## Schema v2 Improvement

Schema v2 adds a formal request mappings block.

```yaml
request_mappings:
  - mapping_id: MAP-FABRIC-001
    mapping_name: Map SKU Numbers to Downstream Custom SKU Numbers
    applies_to_step: STEP-004
    source: request.body.skuNumbers
    target: downstreamRequest.customSkuNumbers
    required: true
    transformation: direct_copy
    pass_through_when_present: false
    omit_when_missing: false
```

## Benefit

This supports mapper generation and documentation.

---

# 14. Response Mapping Comparison

## Schema v1

Schema v1 described response mapping at a high level.

Example:

```yaml
source: calculatedDiscount.discountType
target: response.discountType
```

## Limitation in Schema v1

Schema v1 did not always include:

* Mapping ID
* Mapping name
* Applies-to step
* Response type
* HTTP status
* Transformation
* Required flag

## Schema v2 Improvement

Schema v2 adds a formal response mappings block.

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

## Benefit

This improves DTO generation, mapper generation, and response test validation.

---

# 15. Error Mapping Comparison

## Schema v1

Schema v1 defined failure behavior inside each step.

Example:

```yaml
failure_behavior:
  error_code: BAD_REQUEST
  http_status: 400
  message: Required request header is missing.
```

## Limitation in Schema v1

This approach can cause duplicated and inconsistent errors across steps.

It also does not clearly support:

* Retryable flag
* Error source
* Source error code
* Reusable error mappings
* Fail-fast behavior
* Applies-to steps

## Schema v2 Improvement

Schema v2 introduces reusable error mappings.

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
```

## Benefit

This supports consistent error handler generation and negative test generation.

---

# 16. Test Scenario Comparison

## Schema v1

Schema v1 did not include formal test scenario metadata.

Test cases were discussed as future needs.

## Limitation in Schema v1

Without test scenarios, future test generation would not know:

* Input request
* Mock downstream response
* Mock database response
* Expected HTTP status
* Expected response body
* Expected executed workflow path

## Schema v2 Improvement

Schema v2 adds a `test_scenarios` block.

```yaml
test_scenarios:
  - scenario_id: TC-FABRIC-001
    scenario_name: Valid Fabric Request
    scenario_type: positive
    input:
      headers:
        Division-Id: 12
      body:
        skuNumbers:
          - SKU123
        fabricTypeId: 3
    mocks:
      external_services:
        SRV-FABRIC-CUSTOMIZATION:
          status: 200
    expected:
      http_status: 200
      executed_steps:
        - STEP-001
        - STEP-002
        - STEP-003
        - STEP-004
        - STEP-005
```

## Benefit

This enables future automated test generation and evaluation.

---

# 17. Source Traceability Comparison

## Schema v1

Schema v1 had limited traceability.

Some source references could be mentioned in descriptions, but there was no formal traceability block.

## Limitation in Schema v1

This made it difficult to evaluate:

* Which requirement created each workflow step
* Which requirement created each field
* Which requirement created each business rule
* Which requirement created each query
* Whether the workflow fully covers the source document

## Schema v2 Improvement

Schema v2 adds source traceability.

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

## Benefit

This supports research evaluation, gap analysis, and human review.

---

# 18. Generator Readiness Comparison

## Schema v1

Schema v1 discussed generator readiness in notes.

## Limitation in Schema v1

There was no formal way to mark whether a workflow was ready for:

* Node-RED generation
* Excel generation
* Go generation
* Documentation generation
* Test generation
* Evaluation

## Schema v2 Improvement

Schema v2 adds a generator readiness block.

```yaml
generator_readiness:
  node_red:
    status: mostly_ready
    missing_fields:
      - node_type_mapping
      - wire_layout_strategy

  excel:
    status: ready
    missing_fields: []

  go:
    status: partially_ready
    missing_fields:
      - dto_names
      - repository_method_names

  tests:
    status: mostly_ready
    missing_fields:
      - boundary_test_cases
```

## Benefit

This gives a clear readiness status before attempting generation.

---

# Required and Optional Block Comparison

## Schema v1

Schema v1 did not clearly separate required and optional schema sections.

This could make simple APIs look incomplete if they did not include database or calculation sections.

## Schema v2 Improvement

Schema v2 separates required and optional blocks.

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

## Benefit

This makes Schema v2 flexible enough for both:

* Complex business orchestration APIs
* Simple wrapper APIs

---

# Impact on Future Generators

## Node-RED Generator

| Need                   | Schema v1 Support | Schema v2 Support |
| ---------------------- | ----------------- | ----------------- |
| Ordered workflow steps | Good              | Good              |
| Branching              | Weak              | Strong            |
| External services      | Partial           | Stronger          |
| Error paths            | Partial           | Stronger          |
| Request mappings       | Partial           | Stronger          |
| Response mappings      | Partial           | Stronger          |

## Excel Intake Generator

| Need                     | Schema v1 Support | Schema v2 Support |
| ------------------------ | ----------------- | ----------------- |
| API endpoint data        | Good              | Strong            |
| External services        | Partial           | Strong            |
| Business requirements    | Partial           | Strong            |
| Queries                  | Partial           | Stronger          |
| Endpoint-service mapping | Partial           | Stronger          |
| Cross-sheet references   | Weak              | Stronger          |

## Go Code Generator

| Need                      | Schema v1 Support | Schema v2 Support |
| ------------------------- | ----------------- | ----------------- |
| Handler metadata          | Partial           | Stronger          |
| DTO fields                | Partial           | Stronger          |
| Validation rules          | Partial           | Stronger          |
| Service orchestration     | Partial           | Stronger          |
| Client metadata           | Partial           | Stronger          |
| Repository query metadata | Partial           | Stronger          |
| Error handling            | Partial           | Stronger          |
| Tests                     | Weak              | Stronger          |

## Documentation Generator

| Need                 | Schema v1 Support | Schema v2 Support |
| -------------------- | ----------------- | ----------------- |
| API summary          | Good              | Strong            |
| Request contract     | Good              | Strong            |
| Response contract    | Partial           | Strong            |
| Workflow explanation | Good              | Strong            |
| Business rules       | Partial           | Stronger          |
| External services    | Partial           | Stronger          |
| Error behavior       | Partial           | Stronger          |

## Test Case Generator

| Need                    | Schema v1 Support | Schema v2 Support |
| ----------------------- | ----------------- | ----------------- |
| Test scenario IDs       | Not included      | Strong            |
| Input requests          | Not included      | Strong            |
| Mock responses          | Not included      | Strong            |
| Expected status         | Not included      | Strong            |
| Expected body           | Not included      | Strong            |
| Expected executed steps | Not included      | Strong            |

---

# Consolidated Improvement Summary

| Improvement Area                | Why It Matters                                     |
| ------------------------------- | -------------------------------------------------- |
| Formal field validations        | Enables validator and negative test generation     |
| Formal branching                | Enables reliable orchestration and flow generation |
| Authentication metadata         | Enables downstream client generation               |
| Step-linked database references | Enables repository and SQL generation              |
| Business rule traceability      | Enables business documentation and evaluation      |
| Calculation rules               | Enables service logic and test generation          |
| Request mappings                | Enables mapper and downstream request generation   |
| Response mappings               | Enables response DTO and mapper generation         |
| Error mappings                  | Enables consistent error handling                  |
| Test scenarios                  | Enables automated test generation                  |
| Source traceability             | Enables research evaluation                        |
| Generator readiness             | Prevents premature generation                      |

---

# Final Assessment

## Schema v1 Status

Schema v1 was a strong starting point.

It successfully introduced a structured workflow representation and proved that API requirements can be modeled as workflow steps.

However, Schema v1 was mostly descriptive and not detailed enough for reliable artifact generation.

## Schema v2 Status

Schema v2 is a stronger design.

It turns Day 9 validation gaps into formal schema blocks.

Schema v2 is more suitable for:

* Generator planning
* Workflow validation
* Human review
* Test generation
* Requirement traceability
* Research evaluation

---

# Conclusion

Workflow Schema v2 is a necessary improvement over Schema v1.

Schema v1 answered the question:

```text
Can we represent API workflows as structured steps?
```

Schema v2 answers the next question:

```text
Can we represent API workflows in enough detail for future generators to consume?
```

The answer is yes, with the new Schema v2 blocks.

This comparison confirms that Schema v2 is the right next step before moving toward artifact generation design in future days.
