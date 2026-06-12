# Day 11 — Service, Query, and Business Rule Mapping

## Purpose

This document defines how external services, database queries, business rules, calculation rules, request mappings, response mappings, and error mappings should be extracted from raw requirements and mapped into Workflow Schema v2.

This phase builds on the previous Day 11 documents:

* Phase 1: Requirement-to-workflow mapping overview
* Phase 2: API contract mapping rules
* Phase 3: Validation mapping rules
* Phase 4: Workflow step mapping rules

Phase 5 focuses on the deeper workflow details that make an API useful beyond just request validation.

No coding is performed in this phase.

---

## Background

Workflow Schema v2 is designed to represent enterprise API workflows in a structured and generator-ready format.

Many APIs require more than simple input validation. They may need to:

* Call downstream services
* Authenticate external requests
* Query databases
* Apply business rules
* Perform calculations
* Transform requests
* Transform responses
* Standardize errors

These details are often described across different parts of a requirement document, prototype flow, or existing system notes.

This document defines how those details should be mapped into Schema v2.

---

# Schema v2 Blocks Covered

This document covers mapping rules for the following Schema v2 blocks:

```text
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

# 1. External Service Mapping

## Purpose

External service mapping captures downstream APIs or systems that the workflow calls.

This is needed for:

* Node-RED HTTP request node generation
* Go client generation
* Documentation
* Test mock generation
* Error handling

---

## Requirement Signal

Look for phrases such as:

```text
call downstream service
invoke external API
send request to
REST call
SOAP call
call account service
call product customization service
call PIIDLookup
call Interrogate Account
call wrapper service
```

Also look for:

```text
service URL
endpoint
operation
host
provider
SOAP operation
downstream request
downstream response
```

---

## Mapping Rule

Map downstream service information into:

```yaml
external_services
```

Each external service should include:

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

---

## Example Requirement

```text
Call the Interrogate Account service to resolve account information before discount evaluation.
```

## Schema v2 Mapping

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
      - STEP-005
      - STEP-006
      - STEP-007
    timeout_required: true
    retry_required: false
    error_behavior:
      timeout: Return downstream timeout error.
      unauthorized: Return downstream authentication error.
      invalid_response: Return invalid downstream response error.
```

---

## Service ID Naming Rule

Use the prefix:

```text
SRV-
```

Examples:

```text
SRV-INTERROGATE-ACCOUNT
SRV-FABRIC-CUSTOMIZATION
SRV-PIIDLOOKUP
SRV-XSTX564F
```

---

# 2. Authentication Mapping

## Purpose

Authentication mapping captures how downstream services are secured.

This is needed for:

* Go client generation
* Node-RED HTTP request configuration
* Environment configuration
* Security documentation
* Test mock setup

---

## Requirement Signal

Look for phrases such as:

```text
API key
HMAC
Authorization
Bearer token
OAuth
MAC signature
secret
x-macys-apikey
x-hmac-secret
generate signature
service config authentication
```

---

## Mapping Rule

Map authentication details into:

```yaml
authentication
```

Each authentication entry should include:

```text
auth_id
applies_to_service
type
required
required_headers
api_key
signature
```

---

## Example Requirement

```text
The downstream account interrogation call requires x-macys-apikey and HMAC signature in Authorization header using MAC <signature>.
```

## Schema v2 Mapping

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

## Authentication Type Values

Use one of:

```text
api_key
hmac
oauth
bearer_token
service_config
none
```

---

# 3. Database Query Reference Mapping

## Purpose

Database query mapping captures database lookups, persistence operations, and configuration reads required by the workflow.

This is needed for:

* Repository generation
* SQL generation
* Excel queries sheet generation
* Test mock generation
* Business rule traceability

---

## Requirement Signal

Look for phrases such as:

```text
lookup
query
fetch
select
insert
update
delete
check table
read from database
store in database
get configuration
find matching record
if record exists
if no record found
```

Also look for table names.

Examples:

```text
Emp_Acct_Profile
Emp_Disc_Profile
Disc_By_Dept_Vnd_Class
FEED_CONFIG
APP_MSGS
```

---

## Mapping Rule

Map database usage into:

```yaml
database_references
```

Each database reference should include:

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

---

## Example Requirement

```text
Lookup employee discount profile by division, department, vendor, and class.
```

## Schema v2 Mapping

```yaml
database_references:
  - query_id: SQL-EMP-002
    query_name: Lookup Employee Discount Profile
    database_type: PostgreSQL
    table_names:
      - Disc_By_Dept_Vnd_Class
      - Emp_Disc_Profile
    used_by_step: STEP-008
    used_by_business_rule: BR-EMP-003
    purpose: Lookup employee discount profile by division, department, vendor, and class.
    inputs:
      division: discountContext.division
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

## Query ID Naming Rule

Use the prefix:

```text
SQL-
```

Examples:

```text
SQL-EMP-001
SQL-NA-001
SQL-EXCL-001
SQL-CONFIG-001
SQL-APP-001
```

---

## No-Record Behavior Mapping

If the requirement says:

```text
if no record is found, continue
```

map as:

```yaml
no_record_behavior: continue_without_match
```

If the requirement says:

```text
if no record is found, fail
```

map as:

```yaml
no_record_behavior: return_not_found_or_business_error
```

If unclear:

```yaml
no_record_behavior: unknown_review_required
```

---

# 4. Business Rule Mapping

## Purpose

Business rule mapping captures business decisions and eligibility logic.

This is needed for:

* Service orchestration
* Documentation
* Test generation
* Evaluation
* Human review

---

## Requirement Signal

Look for phrases such as:

```text
if eligible
must be eligible
apply rule
business rule
check eligibility
exclusion
cap
employee account
new account
home shop
cross shop
priority
condition
only if
not allowed if
```

---

## Mapping Rule

Map business logic into:

```yaml
business_rules
```

Each business rule should include:

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

---

## Example Requirement

```text
If employeeFlag is true, evaluate employee discount eligibility using employee account profile.
```

## Schema v2 Mapping

```yaml
business_rules:
  - rule_id: BR-EMP-001
    rule_name: Employee Account Eligibility
    category: employee_discount
    description: Determine whether the resolved account is eligible for employee discount processing.
    applies_to_step: STEP-008
    condition: validatedAccountInfo.employeeFlag == true
    inputs:
      - validatedAccountInfo
      - employeeAccountProfile
    outputs:
      - employeeDiscountEligible
    related_queries:
      - SQL-EMP-001
    success_behavior: Continue employee discount evaluation.
    failure_behavior: Continue without employee discount if employee eligibility is not found.
```

---

## Business Rule ID Naming Rule

Use the prefix:

```text
BR-
```

Examples:

```text
BR-EMP-001
BR-NA-001
BR-EXCL-001
BR-FABRIC-001
BR-APPMSG-001
```

---

## Business Rule Categories

Recommended categories:

```text
validation
eligibility
employee_discount
new_account_discount
exclusion
configuration
mapping
response_mapping
service_wrapper
calculation
```

---

# 5. Calculation Rule Mapping

## Purpose

Calculation rule mapping captures formulas, percentage rules, caps, totals, and derived values.

This is needed for:

* Go service logic generation
* Node-RED function generation
* Test case generation
* Documentation

---

## Requirement Signal

Look for phrases such as:

```text
calculate
compute
apply percentage
apply discount
apply cap
sum
total
derive
round
discount percent
cap amount
maximum amount
minimum amount
```

---

## Mapping Rule

Map calculation logic into:

```yaml
calculation_rules
```

Each calculation rule should include:

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

---

## Example Requirement

```text
Apply employee discount percentage to each eligible item.
```

## Schema v2 Mapping

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

## Calculation ID Naming Rule

Use the prefix:

```text
CALC-
```

Examples:

```text
CALC-DISCOUNT-001
CALC-CAP-001
CALC-TOTAL-001
```

---

# 6. Request Mapping Extraction

## Purpose

Request mapping captures how public API input becomes internal workflow context or downstream request payloads.

This is needed for:

* Mapper generation
* Node-RED function nodes
* Go client request construction
* Documentation
* Test setup

---

## Requirement Signal

Look for phrases such as:

```text
map
copy
transform
build request
populate
send as
derive from
pass through
rename field
```

---

## Mapping Rule

Map request transformations into:

```yaml
request_mappings
```

Each request mapping should include:

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

---

## Example Requirement

```text
Map skuNumbers to downstream customSkuNumbers.
```

## Schema v2 Mapping

```yaml
request_mappings:
  - mapping_id: MAP-FABRIC-001
    mapping_name: Map SKU Numbers to Downstream Custom SKU Numbers
    applies_to_step: STEP-003
    source: request.body.skuNumbers
    target: downstreamFabricRequest.customSkuNumbers
    required: true
    transformation: direct_copy
    pass_through_when_present: false
    omit_when_missing: false
```

---

## Optional Pass-Through Example

```yaml
request_mappings:
  - mapping_id: MAP-FABRIC-TRACE-001
    mapping_name: Pass Trace-Id Header Downstream
    applies_to_step: STEP-003
    source: request.headers.Trace-Id
    target: downstream.headers.Trace-Id
    required: false
    transformation: direct_copy
    pass_through_when_present: true
    omit_when_missing: true
```

---

## Mapping ID Naming Rule

Use the prefix:

```text
MAP-
```

Examples:

```text
MAP-FABRIC-001
MAP-INTERROGATE-001
MAP-SOAP-001
```

---

# 7. Response Mapping Extraction

## Purpose

Response mapping captures how downstream responses, calculated outputs, or workflow results become the public API response.

This is needed for:

* Mapper generation
* Response DTO generation
* Documentation
* Test assertions

---

## Requirement Signal

Look for phrases such as:

```text
return
respond with
map response
response field
output
final response
success response
error response
```

---

## Mapping Rule

Map response transformations into:

```yaml
response_mappings
```

Each response mapping should include:

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

---

## Example Requirement

```text
Return discountType from calculated discount result.
```

## Schema v2 Mapping

```yaml
response_mappings:
  - mapping_id: RESP-DISCOUNT-001
    mapping_name: Map Discount Type
    applies_to_step: STEP-012
    response_type: success
    http_status: 200
    source: calculatedDiscount.discountType
    target: response.discountType
    transformation: direct_copy
    required: true
```

---

## Response Mapping ID Naming Rule

Use the prefix:

```text
RESP-
```

Examples:

```text
RESP-DISCOUNT-001
RESP-FABRIC-001
RESP-ERROR-001
```

---

# 8. Error Mapping Extraction

## Purpose

Error mapping captures how workflow failures become standardized API error responses.

This is needed for:

* Go error handler generation
* Node-RED error paths
* Documentation
* Negative test generation

---

## Requirement Signal

Look for phrases such as:

```text
return error
bad request
unauthorized
not found
downstream failure
database error
internal server error
business rule failure
timeout
invalid response
```

---

## Mapping Rule

Map errors into:

```yaml
error_mappings
```

Each error mapping should include:

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

---

## Example Requirement

```text
If the downstream service is unavailable, return a downstream service error.
```

## Schema v2 Mapping

```yaml
error_mappings:
  - error_mapping_id: ERR-DOWNSTREAM-001
    error_source: downstream_service
    condition: downstream timeout or service unavailable
    source_error_code: null
    target_error_code: DOWNSTREAM_SERVICE_ERROR
    target_http_status: 502
    target_message: Downstream service failed.
    retryable: true
    fail_fast: true
    applies_to_steps:
      - STEP-004
```

---

## Error Mapping ID Naming Rule

Use the prefix:

```text
ERR-
```

Examples:

```text
ERR-VALIDATION-001
ERR-DOWNSTREAM-001
ERR-DATABASE-001
ERR-BUSINESS-001
ERR-CALCULATION-001
ERR-RESPONSE-001
```

---

# 9. Test Scenario Mapping

## Purpose

Service, query, and business rule requirements should produce test scenario candidates.

This is needed for future test generation.

---

## Mapping Rule

Create test scenarios for:

```text
successful downstream service call
downstream service failure
database record found
database no record found
database error
business rule success
business rule failure
calculation success
calculation cap behavior
error mapping behavior
```

---

## Example

```yaml
test_scenarios:
  - scenario_id: TC-DISCOUNT-EMP-001
    scenario_name: Employee Discount Path
    scenario_type: positive
    description: Employee account should execute employee discount path.
    mocks:
      external_services:
        SRV-INTERROGATE-ACCOUNT:
          status: 200
          body:
            employeeFlag: true
      database:
        SQL-EMP-001:
          found: true
        SQL-EMP-002:
          found: true
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
        - STEP-008
```

---

# 10. Source Traceability Mapping

## Purpose

Every major service, query, rule, mapping, or error should trace back to a requirement source when possible.

This is needed for:

* Human review
* Research evaluation
* Gap analysis
* Debugging

---

## Mapping Rule

Map source references into:

```yaml
source_traceability
```

---

## Example

```yaml
source_traceability:
  step_sources:
    - step_id: STEP-004
      requirement_section: Downstream Account Interrogation
      source_text_reference: Section 3.1
      confidence: high

  business_rule_sources:
    - rule_id: BR-EMP-001
      requirement_section: Employee Discount Eligibility
      source_text_reference: Section 4.1
      confidence: high
```

---

# Consolidated Mapping Table

| Requirement Detail           | Schema v2 Target                       |
| ---------------------------- | -------------------------------------- |
| Downstream REST/SOAP service | external_services                      |
| Downstream URL or endpoint   | external_services.endpoint_reference   |
| SOAP operation               | external_services.operation_name       |
| API key or token             | authentication                         |
| HMAC requirement             | authentication.signature               |
| Database table               | database_references.table_names        |
| Database lookup              | database_references                    |
| Query input                  | database_references.inputs             |
| Query output                 | database_references.outputs            |
| No-record behavior           | database_references.no_record_behavior |
| Eligibility rule             | business_rules                         |
| Exclusion rule               | business_rules                         |
| Cap rule                     | business_rules and calculation_rules   |
| Discount percentage rule     | calculation_rules                      |
| Request field transformation | request_mappings                       |
| Optional pass-through        | request_mappings                       |
| Success response field       | response_mappings                      |
| Error response               | error_mappings                         |
| Downstream failure behavior  | error_mappings                         |
| DB failure behavior          | error_mappings                         |
| Test scenario                | test_scenarios                         |
| Requirement source           | source_traceability                    |

---

# Example 1 — Fabric API Mapping

## Raw Requirement

```text
The Fabric API validates the request, maps skuNumbers to customSkuNumbers, calls the Product Customization Fabric service, and maps success or error response.
```

## Schema v2 Mapping

```yaml
external_services:
  - service_id: SRV-FABRIC-CUSTOMIZATION
    service_name: Product Customization Fabric Service
    service_type: REST
    purpose: Return fabric configuration for provided SKU numbers and fabric type.
    method: POST
    endpoint_reference: /productcustomization/v1/config/fabrics
    operation_name: getFabrics

request_mappings:
  - mapping_id: MAP-FABRIC-001
    mapping_name: Map SKU Numbers
    applies_to_step: STEP-003
    source: request.body.skuNumbers
    target: downstreamFabricRequest.customSkuNumbers
    required: true
    transformation: direct_copy

response_mappings:
  - mapping_id: RESP-FABRIC-001
    mapping_name: Map Fabric Success Response
    applies_to_step: STEP-006
    response_type: success
    http_status: 200
    source: downstreamFabricResponse.body
    target: response.body
    transformation: direct_copy
    required: true

error_mappings:
  - error_mapping_id: ERR-FABRIC-DOWNSTREAM-001
    error_source: downstream_service
    condition: downstream fabric service returns error
    source_error_code: downstreamFabricResponse.errorCode
    target_error_code: downstreamFabricResponse.errorCode
    target_http_status: 400
    target_message: downstreamFabricResponse.errorDesc
    retryable: false
    fail_fast: true
    applies_to_steps:
      - STEP-004
```

---

# Example 2 — Discount API Mapping

## Raw Requirement

```text
The Discount API calls Interrogate Account, determines employee or new account path, queries discount tables, applies exclusion rules, calculates discount, and returns discount response.
```

## Schema v2 Mapping

```yaml
external_services:
  - service_id: SRV-INTERROGATE-ACCOUNT
    service_name: Interrogate Account Service
    service_type: REST
    purpose: Resolve account information before discount evaluation.
    method: POST
    endpoint_reference: /accountinterrogation/api/v1/accountinterrogation/piidLookup
    operation_name: piidLookup

business_rules:
  - rule_id: BR-EMP-001
    rule_name: Employee Account Eligibility
    category: employee_discount
    applies_to_step: STEP-008
    condition: validatedAccountInfo.employeeFlag == true
    inputs:
      - validatedAccountInfo
    outputs:
      - employeeDiscountEligible
    related_queries:
      - SQL-EMP-001

  - rule_id: BR-EXCL-001
    rule_name: Department Vendor Class Exclusion
    category: exclusion
    applies_to_step: STEP-010
    condition: discount candidate exists
    inputs:
      - discountContext
      - transactionItem
    outputs:
      - itemExclusionResult
    related_queries:
      - SQL-EXCL-001

database_references:
  - query_id: SQL-EMP-001
    query_name: Lookup Employee Account Profile
    database_type: PostgreSQL
    table_names:
      - Emp_Acct_Profile
    used_by_step: STEP-008
    used_by_business_rule: BR-EMP-001
    purpose: Determine whether resolved account has employee account profile.
    inputs:
      internalAccountToken: validatedAccountInfo.internalAccountToken
    outputs:
      employeeAccountProfile: employeeAccountProfile
    no_record_behavior: continue_without_employee_discount
    failure_behavior: database_error

calculation_rules:
  - calculation_id: CALC-DISCOUNT-001
    calculation_name: Apply Employee Discount
    applies_when: discountType == employee
    calculation_type: percentage
    inputs:
      - transactionItem.amount
      - employeeDiscountProfile.discountPercent
    output: transactionItem.employeeDiscountAmount
    calculation_behavior: Apply employee discount percentage to eligible item amount.
```

---

# Review Checklist

Use this checklist after completing service, query, and business rule mapping.

```text
- Are all downstream services captured?
- Are service endpoints or operation names captured?
- Are service request sources captured?
- Are service response users captured?
- Is authentication captured for each secured downstream service?
- Are all database tables captured?
- Are query purposes captured?
- Are query inputs and outputs captured?
- Is no-record behavior captured?
- Are business rules separated from validation rules?
- Are eligibility rules captured?
- Are exclusion rules captured?
- Are calculation rules captured?
- Are request mappings captured?
- Are response mappings captured?
- Are downstream and database errors captured?
- Are test scenario candidates captured?
- Is source traceability captured?
- Are inferred values marked with medium or low confidence?
```

---

# Common Mistakes to Avoid

## Mistake 1 — Treating External Services as Workflow Steps Only

An external service call should appear as a workflow step, but its detailed metadata should also be captured in `external_services`.

## Mistake 2 — Hiding Authentication in Notes

Authentication should not be buried in descriptions.

It should be mapped into the `authentication` block.

## Mistake 3 — Mixing Queries and Business Rules

A business rule explains why something is checked.

A database reference explains how supporting data is retrieved.

Both should be linked, but not merged.

## Mistake 4 — Missing No-Record Behavior

For every lookup query, capture what happens when no record is found.

This matters for code generation and test cases.

## Mistake 5 — Mapping Calculations Only as Text

Calculations should be structured in `calculation_rules`.

A vague note like "calculate discount" is not enough.

## Mistake 6 — Ignoring Error Behavior

Every downstream call and database operation should have error behavior.

## Mistake 7 — Losing Source Traceability

Every major service, query, business rule, calculation, or mapping should trace back to a source requirement when possible.

---

# Final Summary

Phase 5 defines how service calls, authentication, database queries, business rules, calculation rules, mappings, and errors should be extracted from raw requirements and represented in Workflow Schema v2.

This is a critical part of the project because it connects high-level requirement text to the deeper logic needed by future generators.

The main output blocks are:

* `external_services`
* `authentication`
* `database_references`
* `business_rules`
* `calculation_rules`
* `request_mappings`
* `response_mappings`
* `error_mappings`
* `test_scenarios`
* `source_traceability`

This completes Phase 5 of Day 11.
