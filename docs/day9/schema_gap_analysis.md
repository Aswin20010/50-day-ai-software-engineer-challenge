# Day 9 — Workflow Schema Gap Analysis

## Purpose

This document captures the gaps identified while validating the Day 8 workflow schema against realistic API workflows.

The goal of this analysis is to determine whether the workflow schema is strong enough to support future artifact generation for:

* Node-RED flows
* Excel intake files
* Go code
* Documentation
* Test cases
* Evaluation scripts

No coding is performed in this phase.

---

## Background

On Day 8, a workflow schema was created to define how extracted API requirements should be stored as structured workflow steps.

On Day 9, the schema was validated using two sample APIs:

1. FEED Discount API
2. Customize IT Fabric API

These APIs were selected because they represent two different workflow patterns.

The FEED Discount API represents a complex business orchestration workflow.

The Customize IT Fabric API represents a simpler service wrapper workflow.

Together, they help validate whether the schema can support both complex and simple enterprise API workflows.

---

## APIs Used for Validation

## API 1 — FEED Discount API

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

## Why This API Was Used

This API was selected because it includes:

* Request header validation
* Request body validation
* Enum validation
* External account interrogation service call
* HMAC authentication metadata
* Database-backed business rules
* Employee discount path
* New account discount path
* Conditional branching
* Exclusion rules
* Discount calculation
* Final response mapping
* Error handling

This API stress-tests the schema against a complex enterprise workflow.

---

## API 2 — Customize IT Fabric API

```text
POST /api/v1/fabrics
```

## Why This API Was Used

This API was selected because it includes:

* Header validation
* Body validation
* Array validation
* Optional header pass-through
* Downstream request mapping
* External REST service call
* Success response mapping
* Error response mapping

This API validates whether the schema can represent a lightweight wrapper-style API without unnecessary complexity.

---

# Overall Validation Summary

## What the Schema Handles Well

The Day 8 workflow schema is strong in the following areas:

* Workflow-level metadata
* API endpoint representation
* Header validation
* Body validation
* Step sequencing
* Step dependencies
* External service references
* Business rule references
* Database query references
* Response mapping
* Error mapping
* Generator-readiness thinking

The schema is directionally correct and provides a strong foundation for future workflow-to-artifact generation.

---

## What the Schema Does Not Fully Handle Yet

The validation revealed several areas that need improvement before implementation begins.

The most important gaps are:

1. Conditional branching needs a stronger formal structure.
2. Error mapping needs a standard reusable format.
3. External service authentication metadata needs to be captured consistently.
4. Query references need stronger linking to workflow steps and business rules.
5. Calculation rules need a structured format.
6. Array and nested field validation need more detail.
7. Optional pass-through mappings need explicit support.
8. Test case generation metadata is not strong enough yet.
9. Simple wrapper workflows should not be forced to include unused database or calculation sections.
10. Traceability from requirement to workflow step needs to be improved.

---

# Gap 1 — Conditional Branching Needs More Detail

## Description

The Discount API contains branching logic between employee discount and new account discount paths.

Example:

```text
If employeeFlag is true, execute employee discount path.
If employeeFlag is false, execute new account discount path.
After either path completes, continue to exclusion checks and discount calculation.
```

The Fabric API also contains a smaller form of branching after the downstream service call.

Example:

```text
If downstream response is successful, map success response.
If downstream response is an error, map error response.
```

## Current Schema Limitation

The Day 8 schema can describe steps and dependencies, but it does not yet define a strong structure for branching.

Without a formal branching structure, generators may struggle to understand:

* Branch condition
* True path
* False path
* Branch start step
* Branch end step
* Merge step
* Direct-return branches

## Impact

This affects:

* Node-RED flow generation
* Go service orchestration generation
* Test case generation
* Documentation clarity

## Recommended Improvement

Add a formal `branching` block to workflow steps.

Example:

```yaml
branching:
  branch_id: BRANCH-DISCOUNT-001
  condition: validatedAccountInfo.employeeFlag == true
  true_path: STEP-009
  false_path: STEP-010
  merge_step: STEP-011
  branch_type: conditional
```

For branches that return directly:

```yaml
branching:
  branch_id: BRANCH-FABRIC-001
  condition: downstreamFabricResponse.statusCode is success
  true_path: STEP-007
  false_path: STEP-008
  merge_step: none
  branch_type: direct_response
```

## Priority

High

---

# Gap 2 — Error Mapping Needs a Standard Format

## Description

Both sample APIs require error handling.

The Discount API has multiple error sources:

* Validation errors
* Downstream service errors
* Database errors
* Business rule errors
* Calculation errors
* Mapping errors
* Unexpected system errors

The Fabric API has simpler errors:

* Missing header
* Invalid division
* Invalid request body
* Downstream service error
* Downstream validation error
* Response mapping error

## Current Schema Limitation

The schema can describe failure behavior per step, but it does not yet define a reusable standard error mapping structure.

Without a standard format, different workflows may describe errors inconsistently.

## Impact

This affects:

* Go error handler generation
* Node-RED error response nodes
* API documentation
* Test case generation
* Consistent API behavior

## Recommended Improvement

Add a reusable `error_mappings` section.

Example:

```yaml
error_mappings:
  - error_source: validation
    condition: required field missing
    source_error_code: null
    target_error_code: BAD_REQUEST
    target_http_status: 400
    target_message: Required field is missing.
    retryable: false
    fail_fast: true

  - error_source: downstream_service
    condition: service unavailable
    source_error_code: null
    target_error_code: DOWNSTREAM_SERVICE_ERROR
    target_http_status: 502
    target_message: Downstream service failed.
    retryable: true
    fail_fast: true
```

## Priority

High

---

# Gap 3 — External Service Authentication Metadata Is Incomplete

## Description

The Discount API requires external service authentication for account interrogation.

The downstream service requires:

* API key
* HMAC signature
* Authorization header format
* Request body used for signature generation

The Fabric API may rely on service configuration or runtime headers.

## Current Schema Limitation

The schema can identify external services, but authentication requirements are not detailed enough.

It should be possible to describe:

* Authentication type
* Required auth headers
* Signature source
* Signature algorithm
* Header format
* Secret source
* Whether authentication is static or generated dynamically

## Impact

This affects:

* External client generation
* Go client layer
* Node-RED HTTP request configuration
* Environment configuration
* Security documentation

## Recommended Improvement

Add an `authentication` block to external service definitions.

Example:

```yaml
authentication:
  type: HMAC
  required: true
  api_key:
    header_name: x-macys-apikey
    source: request.headers.x-macys-apikey
  signature:
    header_name: Authorization
    format: MAC <signature>
    algorithm: HMAC-SHA256
    encoding: base64
    payload_source: downstream_request_body
    secret_source: request.headers.x-hmac-secret
```

For simpler service-config authentication:

```yaml
authentication:
  type: service_config
  required: true
  description: Authentication is managed by runtime service configuration.
```

## Priority

High

---

# Gap 4 — Query References Need Stronger Linking

## Description

The Discount API uses database-backed rules for employee discount, new account discount, exclusions, and configuration.

The workflow steps reference query IDs such as:

* SQL-EMP-001
* SQL-EMP-002
* SQL-NA-001
* SQL-EXCL-001
* SQL-CONFIG-001

## Current Schema Limitation

The schema supports database query references, but the relationship between workflow steps, business rules, and queries needs to be stronger.

A future generator must know:

* Which query belongs to which step
* Which business rule uses which query
* Which input fields feed the query
* Which query output feeds the next step
* What happens when no record is found

## Impact

This affects:

* Repository generation
* SQL generation
* Excel `queries` sheet generation
* Endpoint-service mapping
* Test case mocking

## Recommended Improvement

Add query references directly under database-backed workflow steps.

Example:

```yaml
database_references:
  - query_id: SQL-EMP-002
    used_by_business_rule: BR-EMP-003
    purpose: Lookup employee discount profile.
    inputs:
      division: discountDbContext.division
      department: item.department
      vendor: item.vendor
      class: item.class
    outputs:
      discountProfileName: employeeDiscountProfile.name
    no_record_behavior: continue_without_discount
    failure_behavior: database_error
```

## Priority

High

---

# Gap 5 — Calculation Rules Need Structured Representation

## Description

The Discount API includes calculation logic.

Examples:

* Apply base employee discount percentage.
* Apply additional employee discount percentage.
* Apply new account discount percentage.
* Apply cap amount.
* Skip excluded items.
* Return item-level discount results.

## Current Schema Limitation

The schema can identify a calculation step, but the calculation rules are not structured enough for future code generation.

A generator needs to understand:

* Calculation inputs
* Formula or rule name
* Calculation order
* Item-level behavior
* Cap behavior
* Rounding behavior
* Output fields

## Impact

This affects:

* Go service logic generation
* Node-RED calculation function generation
* Test generation
* Documentation

## Recommended Improvement

Add a `calculation_rules` block.

Example:

```yaml
calculation_rules:
  - rule_id: CALC-DISCOUNT-001
    name: Apply Base Employee Discount
    applies_when: discountType == employee
    inputs:
      - item.amount
      - employeeDiscountProfile.discountPercent
    output: item.employeeDiscountAmount

  - rule_id: CALC-DISCOUNT-002
    name: Apply New Account Cap
    applies_when: discountType == new_account
    inputs:
      - calculatedNewAccountDiscount
      - config.newAccountCapAmount
    output: cappedNewAccountDiscount
    cap_behavior: min(calculatedNewAccountDiscount, config.newAccountCapAmount)
```

## Priority

Medium to High

---

# Gap 6 — Array and Nested Field Validation Needs More Detail

## Description

Both APIs contain nested or array-based request structures.

The Discount API has nested fields like:

```text
account.encryptInfo.encryptedData
account.encryptInfo.keyType
transaction.items[]
```

The Fabric API has:

```text
skuNumbers[]
```

## Current Schema Limitation

The schema supports fields, but it should be clearer about nested paths and array item validation.

For example, it should distinguish between:

* Array required
* Array non-empty
* Array item type
* Array item non-blank
* Nested object required
* Nested field required

## Impact

This affects:

* DTO generation
* Validator generation
* API documentation
* Negative test case generation

## Recommended Improvement

Add structured field-level validation metadata.

Example:

```yaml
field_validations:
  - field_path: body.skuNumbers
    type: array
    required: true
    min_items: 1

  - field_path: body.skuNumbers[]
    type: string
    required: true
    allow_blank: false

  - field_path: body.account.encryptInfo.keyType
    type: string
    required: true
    allowed_values:
      - AES
      - RSA
      - TOKEN
```

## Priority

Medium

---

# Gap 7 — Optional Pass-Through Mapping Needs Explicit Support

## Description

The Fabric API has an optional `Trace-Id` header.

The rule is:

```text
If Trace-Id is present, pass it to the downstream service.
If Trace-Id is not present, continue without failure.
```

## Current Schema Limitation

The schema can list optional fields, but it should explicitly support optional source-to-target pass-through mappings.

## Impact

This affects:

* Request mapper generation
* External service client generation
* Documentation
* Test case generation

## Recommended Improvement

Add mapping metadata for optional pass-through behavior.

Example:

```yaml
request_mapping:
  - source: request.headers.Trace-Id
    target: downstream.headers.Trace-Id
    required: false
    pass_through_when_present: true
    omit_when_missing: true
```

## Priority

Medium

---

# Gap 8 — Wrapper APIs Should Not Require Unused Sections

## Description

The Fabric API does not need:

* Database queries
* Internal calculation
* Complex business branching
* Multiple business paths

It mainly needs validation, mapping, external service invocation, and response mapping.

## Current Schema Limitation

If the schema is too rigid, simple wrapper APIs may be forced to include empty or irrelevant sections.

## Impact

This affects:

* Schema usability
* Documentation clarity
* Excel generation cleanliness
* Generator simplicity

## Recommended Improvement

Make workflow sections optional based on workflow type.

Example:

```yaml
workflow_type: service_wrapper
optional_sections:
  database_references: false
  calculation_rules: false
  complex_branching: false
```

Or allow missing sections when not applicable:

```yaml
database_references: []
calculation_rules: []
```

## Priority

Medium

---

# Gap 9 — Test Case Generation Metadata Is Missing

## Description

Both APIs need test scenarios.

The Discount API needs tests for:

* Missing headers
* Invalid enum values
* Downstream account interrogation failure
* Employee discount path
* New account discount path
* Exclusion rule path
* No discount path
* Calculation correctness

The Fabric API needs tests for:

* Valid request
* Missing Division-Id
* Unsupported Division-Id
* Missing skuNumbers
* Empty skuNumbers
* Missing fabricTypeId
* Invalid downstream response
* Downstream unavailable

## Current Schema Limitation

The schema does not yet include structured test metadata.

A future test generator needs:

* Scenario ID
* Scenario name
* Input request
* Mock downstream response
* Mock database result
* Expected HTTP status
* Expected response body
* Expected executed path

## Impact

This affects:

* Test case generation
* Evaluation scripts
* Quality measurement
* Research metrics

## Recommended Improvement

Add a `test_scenarios` section.

Example:

```yaml
test_scenarios:
  - scenario_id: TC-FABRIC-001
    name: Valid fabric request
    type: positive
    input:
      headers:
        Division-Id: 12
      body:
        skuNumbers:
          - SKU123
        fabricTypeId: 3
    expected:
      http_status: 200
      executed_steps:
        - STEP-001
        - STEP-002
        - STEP-003
        - STEP-004
        - STEP-005
        - STEP-006
        - STEP-007

  - scenario_id: TC-DISCOUNT-001
    name: Employee discount path
    type: positive
    mocks:
      accountInterrogationResponse:
        employeeFlag: true
      databaseResults:
        employeeDiscountProfileFound: true
    expected:
      http_status: 200
      discountType: Employee
```

## Priority

High

---

# Gap 10 — Requirement Traceability Needs Improvement

## Description

For research and generator reliability, every workflow element should trace back to a source requirement.

This is important because future evaluation will ask:

```text
Did the generated workflow correctly represent the original requirement?
```

## Current Schema Limitation

The schema does not yet strongly require traceability fields for:

* Workflow
* Step
* Business rule
* Query
* External service
* Request field
* Response field
* Error mapping

## Impact

This affects:

* Gap analysis generation
* Evaluation scripts
* Research paper metrics
* Debugging incorrect generated artifacts
* Human review

## Recommended Improvement

Add source reference fields.

Example:

```yaml
source_traceability:
  requirement_doc: FEED Discount Requirement
  requirement_section: Employee Discount Rules
  source_text_reference: Section 3.2
  confidence: high
```

At step level:

```yaml
step_id: STEP-009
step_name: Execute Employee Discount Path
source_traceability:
  requirement_section: Employee Discount Logic
  confidence: high
```

## Priority

Medium to High

---

# Gap 11 — Generator-Specific Readiness Should Be Explicit

## Description

The schema is intended to support several future generators.

However, each generator needs different details.

For example:

Node-RED needs:

* Node ordering
* Node type
* Wires
* Function node logic
* HTTP request config

Excel intake needs:

* Sheet names
* Row IDs
* Cross-sheet references

Go generation needs:

* DTOs
* Handler names
* Service method names
* Repository method names
* Client method names

Documentation generation needs:

* Human-readable summaries
* Request examples
* Response examples

Test generation needs:

* Scenarios
* Inputs
* Expected outputs
* Mocks

## Current Schema Limitation

The schema does not yet clearly indicate readiness by generator type.

## Impact

This affects:

* Future artifact generation planning
* Evaluation clarity
* Development prioritization

## Recommended Improvement

Add a `generator_readiness` block.

Example:

```yaml
generator_readiness:
  node_red:
    status: partially_ready
    missing_fields:
      - branching.node_type
      - error_mapping.standard_format

  excel:
    status: mostly_ready
    missing_fields:
      - sheet_mapping_rules

  go:
    status: partially_ready
    missing_fields:
      - dto_contract
      - repository_method_names
      - typed_errors

  documentation:
    status: ready
    missing_fields: []

  tests:
    status: not_ready
    missing_fields:
      - test_scenarios
      - mock_responses
```

## Priority

Medium

---

# Consolidated Gap Summary

| Gap ID  | Gap Name                                               | Priority    | Affected Generators            |
| ------- | ------------------------------------------------------ | ----------- | ------------------------------ |
| GAP-001 | Conditional branching needs formal structure           | High        | Node-RED, Go, Tests, Docs      |
| GAP-002 | Error mapping needs standard format                    | High        | Node-RED, Go, Tests, Docs      |
| GAP-003 | External service authentication metadata is incomplete | High        | Node-RED, Go, Docs             |
| GAP-004 | Query references need stronger linking                 | High        | Excel, Go, Tests               |
| GAP-005 | Calculation rules need structured representation       | Medium-High | Go, Node-RED, Tests            |
| GAP-006 | Array and nested validation needs more detail          | Medium      | Go, Docs, Tests                |
| GAP-007 | Optional pass-through mapping needs explicit support   | Medium      | Go, Node-RED, Docs             |
| GAP-008 | Wrapper APIs should not require unused sections        | Medium      | Excel, Docs, Go                |
| GAP-009 | Test case generation metadata is missing               | High        | Tests, Evaluation              |
| GAP-010 | Requirement traceability needs improvement             | Medium-High | Evaluation, Docs, Gap Analysis |
| GAP-011 | Generator-specific readiness should be explicit        | Medium      | All generators                 |

---

# Recommended Schema Enhancements

Based on the Day 9 validation, the workflow schema should be enhanced with the following sections.

## 1. Branching Block

```yaml
branching:
  branch_id: string
  condition: string
  true_path: step_id
  false_path: step_id
  merge_step: step_id | none
  branch_type: conditional | direct_response
```

## 2. Error Mapping Block

```yaml
error_mappings:
  - error_source: string
    condition: string
    source_error_code: string | null
    target_error_code: string
    target_http_status: integer
    target_message: string
    retryable: boolean
    fail_fast: boolean
```

## 3. External Service Authentication Block

```yaml
authentication:
  type: api_key | hmac | oauth | service_config | none
  required: boolean
  required_headers:
    - string
  signature:
    algorithm: string
    encoding: string
    payload_source: string
    secret_source: string
    header_name: string
    format: string
```

## 4. Query Reference Block

```yaml
database_references:
  - query_id: string
    used_by_business_rule: string
    purpose: string
    inputs: object
    outputs: object
    no_record_behavior: string
    failure_behavior: string
```

## 5. Calculation Rules Block

```yaml
calculation_rules:
  - rule_id: string
    name: string
    applies_when: string
    inputs:
      - string
    output: string
    calculation_behavior: string
```

## 6. Field Validation Block

```yaml
field_validations:
  - field_path: string
    type: string
    required: boolean
    allowed_values:
      - string
    min_items: integer
    allow_blank: boolean
    failure_behavior:
      http_status: integer
      error_code: string
      message: string
```

## 7. Optional Pass-Through Mapping Block

```yaml
request_mapping:
  - source: string
    target: string
    required: boolean
    pass_through_when_present: boolean
    omit_when_missing: boolean
```

## 8. Test Scenario Block

```yaml
test_scenarios:
  - scenario_id: string
    name: string
    type: positive | negative | boundary | failure
    input: object
    mocks: object
    expected:
      http_status: integer
      response_body: object
      executed_steps:
        - step_id
```

## 9. Source Traceability Block

```yaml
source_traceability:
  requirement_doc: string
  requirement_section: string
  source_text_reference: string
  confidence: high | medium | low
```

## 10. Generator Readiness Block

```yaml
generator_readiness:
  node_red:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
  excel:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
  go:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
  documentation:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
  tests:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
```

---

# Impact on Future Days

The Day 9 gap analysis will guide future schema refinement and artifact generation design.

## Impact on Node-RED Generation

Node-RED generation will require stronger support for:

* Branching
* Error handling
* External service authentication
* Request and response mapping
* Direct response paths

## Impact on Excel Intake Generation

Excel generation will require stronger support for:

* Cross-sheet references
* Query IDs
* Business rule IDs
* Endpoint-service mappings
* External service metadata

## Impact on Go Code Generation

Go generation will require stronger support for:

* DTO contracts
* Validation rules
* Service orchestration
* Branching logic
* Repository methods
* External clients
* Error types
* Mapper rules

## Impact on Documentation Generation

Documentation generation will benefit from:

* Workflow summaries
* Step descriptions
* Business rule descriptions
* Error mappings
* Source traceability
* External service details

## Impact on Test Generation

Test generation requires the most improvement.

The schema should eventually include:

* Test scenarios
* Mock inputs
* Mock downstream service responses
* Mock database results
* Expected response body
* Expected HTTP status
* Expected executed workflow path

---

# Final Assessment

## Overall Schema Readiness

| Area                      | Status          |
| ------------------------- | --------------- |
| Workflow Metadata         | Ready           |
| Request Validation        | Mostly Ready    |
| Workflow Steps            | Mostly Ready    |
| Conditional Branching     | Partially Ready |
| External Services         | Partially Ready |
| Database Query References | Partially Ready |
| Business Rules            | Mostly Ready    |
| Calculation Rules         | Partially Ready |
| Response Mapping          | Mostly Ready    |
| Error Handling            | Partially Ready |
| Generator Readiness       | Partially Ready |
| Test Generation           | Not Fully Ready |

---

## Final Conclusion

The Day 8 workflow schema is a strong foundation.

Day 9 validation confirms that the schema can represent the main structure of both complex and simple API workflows.

The FEED Discount API proved that the schema can model enterprise business orchestration, but it also exposed weaknesses in branching, calculation rules, query linking, authentication metadata, and error handling.

The Customize IT Fabric API proved that the schema can model simple wrapper APIs, but it also showed the need for better array validation, optional pass-through mapping, and lightweight workflow support.

The schema is ready for continued design work, but it should be refined before being used for production-grade artifact generation.

The most important improvements before implementation are:

1. Add formal branching support.
2. Add standard error mapping.
3. Add authentication metadata for external services.
4. Strengthen database query references.
5. Add structured test scenario metadata.
6. Improve traceability from source requirements to workflow steps.

This gap analysis completes Phase 5 of Day 9.
