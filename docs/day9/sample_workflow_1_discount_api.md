# Day 9 — Sample Workflow 1: Discount API

## Purpose

This document validates the Day 8 workflow schema by applying it to a realistic complex API workflow.

The selected API is the FEED Discount API:

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

This API was selected because it contains multiple workflow patterns that future generators must support, including validation, external service calls, database lookups, conditional branching, business rules, exclusion checks, calculations, and response mapping.

No coding is performed in this phase.

---

## API Overview

## API Name

Evaluate Transaction Discount API

## Endpoint

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

## Purpose

The purpose of this API is to evaluate whether a transaction is eligible for employee discount, new account discount, or no discount.

The workflow receives transaction and account-related input, validates the request, resolves account details using an account interrogation service, checks discount eligibility using database-backed business rules, calculates applicable discount values, and maps the final response.

---

## Workflow Metadata

```yaml
workflow_id: WF-DISCOUNT-001
workflow_name: Evaluate Transaction Discount Workflow
workflow_description: >
  Validates a transaction discount request, resolves account information,
  evaluates employee and new account discount rules, applies exclusion logic,
  calculates discount values, and returns the final discount response.
api_endpoint: /api/v1/discount/evaluateTransactionDiscount
http_method: POST
api_version: v1
workflow_type: business_orchestration
source_domain: FEED Discount
schema_validation_status: draft
```

---

## Request Contract Summary

## Required Headers

The schema should be able to capture the following request headers:

```yaml
headers:
  - name: requestorInfo-clientId
    required: true
    type: string
    description: Client identifier for the request.

  - name: requestorInfo-subclientId
    required: true
    type: string
    description: Subclient identifier for the request.

  - name: requestorInfo-traceId
    required: false
    type: string
    description: Optional trace identifier used for request tracking.

  - name: requestorinfo-version
    required: true
    type: string
    description: Request version header.

  - name: x-macys-apikey
    required: true
    type: string
    description: API key required for downstream service access.

  - name: x-hmac-secret
    required: true
    type: string
    description: Secret used to generate downstream HMAC signature.
```

---

## Request Body Summary

The exact production request body may evolve, but the workflow schema should support a transaction-level body containing account data, entry mode, and transaction items.

```yaml
body:
  account:
    required: true
    type: object
    fields:
      encryptInfo:
        required: true
        type: object
        fields:
          encryptedData:
            required: true
            type: string
          keyType:
            required: true
            type: string
          keyName:
            required: true
            type: string
          keyVersion:
            required: true
            type: string
          division:
            required: true
            type: string
          storeNbr:
            required: true
            type: string
          aesType:
            required: false
            type: string

      entryMode:
        required: true
        type: string

  transaction:
    required: true
    type: object
    fields:
      division:
        required: true
        type: integer
      storeNbr:
        required: true
        type: integer
      items:
        required: true
        type: array
        description: List of transaction items used for discount calculation.
```

---

## Enum Validation Requirements

The schema should support allowed value validation for fields such as `keyType` and `entryMode`.

## keyType Allowed Values

```text
AES
RSA
IRN
AES_PAN
TOKEN
MAN
AES_RSA_HYBRID
CLEAR_TEXT
EZ_ENCRYPTION
ECV1
ECV2
```

## entryMode Allowed Values

```text
KEYED
BARCODE_SCAN
ACCT_LOOKUP_WITHIN_TRANSACTION
ACCT_LOOKUP_SHOPPING_PASS
CONTACTLESS
B_CONNECTED
CLIENTELE
DCR_LOOKUP
MOBILE_WALLET_GOOGLE
NEW_ACCT_TRANSACTION
MOBILE_WALLET_ISIS
NEW_ACCT_APPROVED
MOBILE_WALLET_SOFTCARD
NEW_ACCT_NOT_APPROVED
PIN
SWIPED
TAP
TOKENIZED
```

---

## Workflow Step Summary

The Discount API workflow can be represented as the following high-level sequence:

```text
STEP-001 Validate headers
STEP-002 Validate request body
STEP-003 Validate enum fields
STEP-004 Build account interrogation request
STEP-005 Call Interrogate Account service
STEP-006 Validate account interrogation response
STEP-007 Prepare discount DB context
STEP-008 Determine account discount path
STEP-009 Execute employee discount path
STEP-010 Execute new account discount path
STEP-011 Apply exclusion rules
STEP-012 Calculate discount
STEP-013 Map final response
STEP-014 Handle errors
```

---

# Workflow Steps

## STEP-001 — Validate Required Headers

```yaml
step_id: STEP-001
step_name: Validate Required Headers
step_type: validation
description: >
  Validate that all required FEED Discount request headers are present.
inputs:
  - request.headers
outputs:
  - validatedHeaders
depends_on: []
success_behavior: Continue to STEP-002.
failure_behavior:
  error_code: BAD_REQUEST
  http_status: 400
  message: Required request header is missing.
```

## Schema Validation Notes

This step validates whether the workflow schema can represent header-level validation.

The schema should clearly capture:

* Required header names
* Optional header names
* Header data types
* Failure response
* Next step on success

---

## STEP-002 — Validate Request Body

```yaml
step_id: STEP-002
step_name: Validate Request Body
step_type: validation
description: >
  Validate that the request body contains account information, entry mode,
  transaction information, and transaction items.
inputs:
  - request.body
outputs:
  - validatedBody
depends_on:
  - STEP-001
success_behavior: Continue to STEP-003.
failure_behavior:
  error_code: BAD_REQUEST
  http_status: 400
  message: Required request body field is missing or invalid.
```

## Schema Validation Notes

This step validates whether the schema can capture body-level required fields and nested object validation.

The schema should support:

* Nested body fields
* Required object validation
* Required array validation
* Type validation
* Missing field behavior

---

## STEP-003 — Validate Enum Fields

```yaml
step_id: STEP-003
step_name: Validate Enum Fields
step_type: validation
description: >
  Validate enum fields such as keyType and entryMode against the allowed values.
inputs:
  - request.body.account.encryptInfo.keyType
  - request.body.account.entryMode
outputs:
  - validatedEnumFields
depends_on:
  - STEP-002
success_behavior: Continue to STEP-004.
failure_behavior:
  error_code: BAD_REQUEST
  http_status: 400
  message: Request contains unsupported enum value.
```

## Schema Validation Notes

This step validates whether the schema can capture allowed values for request fields.

The schema should support:

* Enum field name
* Allowed values
* Failure message
* Failure HTTP status

---

## STEP-004 — Build Account Interrogation Request

```yaml
step_id: STEP-004
step_name: Build Account Interrogation Request
step_type: mapping
description: >
  Map the FEED Discount request into the request format required by the
  Interrogate Account service.
inputs:
  - request.headers.requestorInfo-clientId
  - request.headers.requestorInfo-subclientId
  - request.headers.requestorInfo-traceId
  - request.headers.requestorinfo-version
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

## Mapping Notes

The account interrogation request should carry account encryption information and entry mode.

PIIDs are derived internally from `encryptedData`; the client does not provide PIIDs directly.

## Schema Validation Notes

This step validates whether the schema can represent request transformation before calling an external service.

The schema should support:

* Source fields
* Target fields
* Transformation rules
* Derived field notes
* Mapping failure behavior

---

## STEP-005 — Call Interrogate Account Service

```yaml
step_id: STEP-005
step_name: Call Interrogate Account Service
step_type: external_service
description: >
  Invoke the Interrogate Account service to resolve account information
  required for discount eligibility checks.
inputs:
  - accountInterrogationRequest
  - request.headers.x-macys-apikey
  - request.headers.x-hmac-secret
outputs:
  - accountInterrogationResponse
depends_on:
  - STEP-004
external_service:
  service_id: SRV-INTERROGATE-ACCOUNT
  service_name: Interrogate Account Service
  protocol: REST
  method: POST
  endpoint_reference: /accountinterrogation/api/v1/accountinterrogation/piidLookup
  authentication:
    type: HMAC
    api_key_header: x-macys-apikey
    signature_header: Authorization
    signature_format: MAC <signature>
  timeout_required: true
  retry_required: false
success_behavior: Continue to STEP-006.
failure_behavior:
  error_code: DOWNSTREAM_SERVICE_ERROR
  http_status: 502
  message: Account interrogation service failed.
```

## Schema Validation Notes

This step validates whether the schema can represent external REST service calls.

The schema should support:

* Service identity
* Endpoint reference
* HTTP method
* Request mapping
* Response mapping
* Authentication metadata
* Timeout metadata
* Downstream error behavior

---

## STEP-006 — Validate Account Interrogation Response

```yaml
step_id: STEP-006
step_name: Validate Account Interrogation Response
step_type: validation
description: >
  Validate that the account interrogation response contains the account
  details required for discount processing.
inputs:
  - accountInterrogationResponse
outputs:
  - validatedAccountInfo
depends_on:
  - STEP-005
success_behavior: Continue to STEP-007.
failure_behavior:
  error_code: ACCOUNT_INTERROGATION_INVALID_RESPONSE
  http_status: 502
  message: Account interrogation response is missing required account information.
```

## Required Response Fields

The workflow schema should be able to represent expected response fields such as:

```yaml
accountInfo:
  internalAccount:
    required: false
    type: string
  internalAccountToken:
    required: true
    type: string
  internalAccountLast4:
    required: false
    type: string
  data:
    required: false
    type: string
  dataToken:
    required: false
    type: string
  lookupType:
    required: false
    type: string
  rc:
    required: false
    type: string
  employeeFlag:
    required: false
    type: boolean
```

## Schema Validation Notes

This step validates whether the schema can represent validation of downstream service responses.

The schema should support:

* Required downstream response fields
* Optional downstream response fields
* Failure behavior for missing fields
* Output variables for later workflow steps

---

## STEP-007 — Prepare Discount DB Context

```yaml
step_id: STEP-007
step_name: Prepare Discount DB Context
step_type: mapping
description: >
  Prepare database lookup context using transaction details and resolved
  account information.
inputs:
  - request.body.transaction.division
  - request.body.transaction.storeNbr
  - request.body.transaction.items
  - validatedAccountInfo.internalAccountToken
  - validatedAccountInfo.employeeFlag
outputs:
  - discountDbContext
depends_on:
  - STEP-006
success_behavior: Continue to STEP-008.
failure_behavior:
  error_code: INTERNAL_MAPPING_ERROR
  http_status: 500
  message: Failed to prepare discount database context.
```

## Schema Validation Notes

This step validates whether the schema can represent an internal preparation step that feeds later database queries.

The schema should support:

* Combining request data with downstream service output
* Creating intermediate workflow context
* Passing intermediate context to later steps

---

## STEP-008 — Determine Account Discount Path

```yaml
step_id: STEP-008
step_name: Determine Account Discount Path
step_type: business_rule
description: >
  Determine whether the workflow should evaluate employee discount rules,
  new account discount rules, or no discount path.
inputs:
  - validatedAccountInfo.employeeFlag
  - discountDbContext
outputs:
  - discountPath
depends_on:
  - STEP-007
branching:
  condition: validatedAccountInfo.employeeFlag == true
  true_path: STEP-009
  false_path: STEP-010
  merge_step: STEP-011
success_behavior: Route to employee or new account discount path.
failure_behavior:
  error_code: DISCOUNT_PATH_DETERMINATION_FAILED
  http_status: 500
  message: Failed to determine discount path.
```

## Schema Validation Notes

This step is important because it validates whether the Day 8 schema can support conditional branching.

The schema should support:

* Branch condition
* True path
* False path
* Merge step
* Output path variable

---

## STEP-009 — Execute Employee Discount Path

```yaml
step_id: STEP-009
step_name: Execute Employee Discount Path
step_type: business_rule
description: >
  Evaluate employee discount eligibility using employee account profile,
  home shop or cross-shop logic, discount profile lookup, and additional
  discount rules.
inputs:
  - discountDbContext
  - validatedAccountInfo
outputs:
  - employeeDiscountResult
depends_on:
  - STEP-008
condition: discountPath == employee
database_references:
  - query_id: SQL-EMP-001
    purpose: Lookup employee account profile.
  - query_id: SQL-EMP-002
    purpose: Lookup employee discount profile.
  - query_id: SQL-EMP-003
    purpose: Lookup employee additional discount rules.
business_rules:
  - rule_id: BR-EMP-001
    name: Employee Account Eligibility
  - rule_id: BR-EMP-002
    name: Home Shop versus Cross Shop Evaluation
  - rule_id: BR-EMP-003
    name: Employee Discount Profile Selection
  - rule_id: BR-EMP-004
    name: Additional Employee Discount Evaluation
success_behavior: Continue to STEP-011.
failure_behavior:
  error_code: EMPLOYEE_DISCOUNT_EVALUATION_FAILED
  http_status: 500
  message: Employee discount evaluation failed.
```

## Employee Discount Logic Summary

The employee discount path should support the following rule concepts:

* Identify whether the account belongs to an employee.
* Determine home shop versus cross-shop behavior.
* Check employee account profile.
* Check employee discount profile by division, department, vendor, and class.
* Apply additional discount events when eligible.
* Prepare employee discount result.

## Schema Validation Notes

This step validates whether the schema can represent complex business logic using linked business rules and query references.

The schema should support:

* Multiple database references per step
* Multiple business rules per step
* Conditional execution
* Aggregated output from multiple rule checks

---

## STEP-010 — Execute New Account Discount Path

```yaml
step_id: STEP-010
step_name: Execute New Account Discount Path
step_type: business_rule
description: >
  Evaluate new account discount eligibility using account status, transaction
  details, discount rules, and cap amount configuration.
inputs:
  - discountDbContext
  - validatedAccountInfo
outputs:
  - newAccountDiscountResult
depends_on:
  - STEP-008
condition: discountPath == new_account
database_references:
  - query_id: SQL-NA-001
    purpose: Lookup new account discount eligibility.
  - query_id: SQL-NA-002
    purpose: Lookup new account discount profile.
  - query_id: SQL-CONFIG-001
    purpose: Lookup discount cap amount from FEED configuration.
business_rules:
  - rule_id: BR-NA-001
    name: New Account Eligibility
  - rule_id: BR-NA-002
    name: New Account Department Vendor Class Check
  - rule_id: BR-NA-003
    name: New Account Cap Amount Rule
success_behavior: Continue to STEP-011.
failure_behavior:
  error_code: NEW_ACCOUNT_DISCOUNT_EVALUATION_FAILED
  http_status: 500
  message: New account discount evaluation failed.
```

## New Account Discount Logic Summary

The new account discount path should support the following rule concepts:

* Identify whether new account discount is applicable.
* Ensure the account is not treated as employee discount when employeeFlag is true.
* Check department, vendor, and class eligibility.
* Check configured new account discount percentage.
* Apply cap amount from configuration.
* Prepare new account discount result.

## Schema Validation Notes

This step validates whether the schema can represent an alternate business path.

The schema should support:

* Conditional execution based on previous step output
* Different database references from the employee path
* Different business rules from the employee path
* Shared merge point after path completion

---

## STEP-011 — Apply Exclusion Rules

```yaml
step_id: STEP-011
step_name: Apply Exclusion Rules
step_type: business_rule
description: >
  Apply department, vendor, class, and additional discount exclusion rules
  before final discount calculation.
inputs:
  - employeeDiscountResult
  - newAccountDiscountResult
  - discountDbContext
outputs:
  - discountEligibilityResult
depends_on:
  - STEP-009
  - STEP-010
database_references:
  - query_id: SQL-EXCL-001
    purpose: Check department, vendor, and class exclusions.
  - query_id: SQL-EXCL-002
    purpose: Check additional discount exclusions.
business_rules:
  - rule_id: BR-EXCL-001
    name: Department Vendor Class Exclusion
  - rule_id: BR-EXCL-002
    name: Additional Discount Exclusion
success_behavior: Continue to STEP-012.
failure_behavior:
  error_code: DISCOUNT_EXCLUSION_CHECK_FAILED
  http_status: 500
  message: Discount exclusion check failed.
```

## Exclusion Logic Summary

The exclusion step should support the following:

* If an item is excluded, discount should not be applied to that item.
* If no exclusion exists, continue discount calculation.
* Exclusions may apply differently for employee discount and new account discount.
* Additional discount exclusions should be checked separately from base discount exclusions.

## Schema Validation Notes

This step validates whether the schema can represent exclusion-based business logic.

The schema should support:

* Rule-based eligibility modification
* Item-level checks
* Query references
* Business rule references
* Shared handling across multiple discount paths

---

## STEP-012 — Calculate Discount

```yaml
step_id: STEP-012
step_name: Calculate Discount
step_type: calculation
description: >
  Calculate the final discount result based on base discount percentage,
  additional discount percentage, new account discount percentage, cap amount,
  and exclusion results.
inputs:
  - discountEligibilityResult
  - request.body.transaction.items
outputs:
  - calculatedDiscount
depends_on:
  - STEP-011
calculation_rules:
  - Base employee discount percentage
  - Additional employee discount percentage
  - New account discount percentage
  - New account cap amount
  - Item-level exclusion handling
success_behavior: Continue to STEP-013.
failure_behavior:
  error_code: DISCOUNT_CALCULATION_FAILED
  http_status: 500
  message: Discount calculation failed.
```

## Calculation Logic Summary

The calculation step should support:

* Employee discount calculation
* Additional discount calculation
* New account discount calculation
* Item-level discount application
* Cap amount handling
* No-discount result handling

## Schema Validation Notes

This step validates whether the schema can represent calculation logic.

The schema should support:

* Calculation inputs
* Calculation rules
* Calculation outputs
* Failure behavior
* Reuse of prior business rule outputs

---

## STEP-013 — Map Final Response

```yaml
step_id: STEP-013
step_name: Map Final Response
step_type: response_mapping
description: >
  Map the calculated discount result into the final API response.
inputs:
  - calculatedDiscount
  - validatedAccountInfo
outputs:
  - finalApiResponse
depends_on:
  - STEP-012
response_mapping:
  http_status: 200
  mappings:
    - source: calculatedDiscount.discountType
      target: response.discountType
    - source: calculatedDiscount.discountPercentApplied
      target: response.discountPercentApplied
    - source: calculatedDiscount.additionalDiscountPercentApplied
      target: response.additionalDiscountPercentApplied
    - source: calculatedDiscount.naDiscountPercentApplied
      target: response.naDiscountPercentApplied
    - source: calculatedDiscount.items
      target: response.items
    - source: validatedAccountInfo.internalAccountToken
      target: response.accountInfo.internalAccountToken
success_behavior: Return final API response.
failure_behavior:
  error_code: RESPONSE_MAPPING_FAILED
  http_status: 500
  message: Failed to map discount response.
```

## Schema Validation Notes

This step validates whether the schema can represent final response mapping.

The schema should support:

* Source fields
* Target response fields
* HTTP status
* Response structure
* Optional response fields
* Mapping failure behavior

---

## STEP-014 — Handle Errors

```yaml
step_id: STEP-014
step_name: Handle Errors
step_type: error_mapping
description: >
  Standardize validation, downstream, database, business rule, calculation,
  and unexpected errors into API error responses.
inputs:
  - workflowError
outputs:
  - errorApiResponse
depends_on:
  - STEP-001
  - STEP-002
  - STEP-003
  - STEP-004
  - STEP-005
  - STEP-006
  - STEP-007
  - STEP-008
  - STEP-009
  - STEP-010
  - STEP-011
  - STEP-012
  - STEP-013
error_mappings:
  - source: validation
    http_status: 400
    error_code: BAD_REQUEST
    retryable: false

  - source: downstream_service
    http_status: 502
    error_code: DOWNSTREAM_SERVICE_ERROR
    retryable: true

  - source: database
    http_status: 500
    error_code: DATABASE_ERROR
    retryable: true

  - source: business_rule
    http_status: 422
    error_code: BUSINESS_RULE_FAILED
    retryable: false

  - source: unexpected
    http_status: 500
    error_code: INTERNAL_SERVER_ERROR
    retryable: true
success_behavior: Return standardized error response.
failure_behavior:
  error_code: INTERNAL_SERVER_ERROR
  http_status: 500
  message: Unexpected error occurred while handling workflow error.
```

## Schema Validation Notes

This step validates whether the schema can represent centralized error handling.

The schema should support:

* Error source
* Error code
* HTTP status
* Retryable flag
* Standardized error response
* Fail-fast behavior

---

# Database References Required by Workflow

The schema does not need to include full SQL during Day 9, but it should support references to database queries.

## Employee Discount Queries

```yaml
queries:
  - query_id: SQL-EMP-001
    purpose: Lookup employee account profile.
    inputs:
      - internalAccountToken
    outputs:
      - employeeAccountProfile

  - query_id: SQL-EMP-002
    purpose: Lookup employee discount profile.
    inputs:
      - division
      - department
      - vendor
      - class
    outputs:
      - employeeDiscountProfile

  - query_id: SQL-EMP-003
    purpose: Lookup additional employee discount rules.
    inputs:
      - eventId
      - division
      - department
      - vendor
      - class
    outputs:
      - additionalDiscountProfile
```

## New Account Discount Queries

```yaml
queries:
  - query_id: SQL-NA-001
    purpose: Lookup new account discount eligibility.
    inputs:
      - accountInfo
      - transactionContext
    outputs:
      - newAccountEligibility

  - query_id: SQL-NA-002
    purpose: Lookup new account discount profile.
    inputs:
      - division
      - department
      - vendor
      - class
    outputs:
      - newAccountDiscountProfile
```

## Exclusion Queries

```yaml
queries:
  - query_id: SQL-EXCL-001
    purpose: Check department, vendor, and class exclusions.
    inputs:
      - division
      - department
      - vendor
      - class
    outputs:
      - exclusionResult

  - query_id: SQL-EXCL-002
    purpose: Check additional discount exclusions.
    inputs:
      - eventId
      - division
      - department
      - vendor
      - class
    outputs:
      - additionalExclusionResult
```

## Configuration Queries

```yaml
queries:
  - query_id: SQL-CONFIG-001
    purpose: Lookup discount configuration values.
    inputs:
      - configName
    outputs:
      - configValue
```

---

# Business Rules Required by Workflow

The schema should support linking workflow steps to business rule IDs.

```yaml
business_rules:
  - rule_id: BR-REQ-001
    name: Required Header Validation
    category: validation

  - rule_id: BR-REQ-002
    name: Required Body Validation
    category: validation

  - rule_id: BR-REQ-003
    name: Enum Validation
    category: validation

  - rule_id: BR-EMP-001
    name: Employee Account Eligibility
    category: employee_discount

  - rule_id: BR-EMP-002
    name: Home Shop Versus Cross Shop Evaluation
    category: employee_discount

  - rule_id: BR-EMP-003
    name: Employee Discount Profile Selection
    category: employee_discount

  - rule_id: BR-EMP-004
    name: Additional Employee Discount Evaluation
    category: employee_discount

  - rule_id: BR-NA-001
    name: New Account Eligibility
    category: new_account_discount

  - rule_id: BR-NA-002
    name: New Account Department Vendor Class Check
    category: new_account_discount

  - rule_id: BR-NA-003
    name: New Account Cap Amount Rule
    category: new_account_discount

  - rule_id: BR-EXCL-001
    name: Department Vendor Class Exclusion
    category: exclusion

  - rule_id: BR-EXCL-002
    name: Additional Discount Exclusion
    category: exclusion
```

---

# External Service References

## SRV-INTERROGATE-ACCOUNT

```yaml
service_id: SRV-INTERROGATE-ACCOUNT
service_name: Interrogate Account Service
service_type: REST
purpose: Resolve account information before discount evaluation.
method: POST
endpoint_reference: /accountinterrogation/api/v1/accountinterrogation/piidLookup
authentication:
  type: HMAC
  required_headers:
    - x-macys-apikey
    - Authorization
request_source:
  - request.headers
  - request.body.account.encryptInfo
  - request.body.account.entryMode
response_used_by:
  - STEP-006
  - STEP-007
  - STEP-008
error_behavior:
  timeout: Return downstream service error.
  unauthorized: Return downstream authentication error.
  invalid_response: Return downstream invalid response error.
```

---

# Conditional Branching Representation

The Discount API confirms that the schema must support conditional branching.

## Branch 1 — Employee Discount Path

```yaml
branch_id: BRANCH-DISCOUNT-001
condition: validatedAccountInfo.employeeFlag == true
path_name: Employee Discount Path
start_step: STEP-009
merge_step: STEP-011
```

## Branch 2 — New Account Discount Path

```yaml
branch_id: BRANCH-DISCOUNT-002
condition: validatedAccountInfo.employeeFlag == false
path_name: New Account Discount Path
start_step: STEP-010
merge_step: STEP-011
```

## Branching Validation Result

The schema can represent branching only if it supports:

* Condition expression
* True path
* False path
* Branch-specific steps
* Merge step
* Shared downstream processing

This sample workflow proves that branching is a required schema capability.

---

# Response Mapping Representation

## Success Response Concept

```yaml
success_response:
  http_status: 200
  body:
    discountType:
      source: calculatedDiscount.discountType
      type: string
    discountPercentApplied:
      source: calculatedDiscount.discountPercentApplied
      type: number
    additionalDiscountPercentApplied:
      source: calculatedDiscount.additionalDiscountPercentApplied
      type: number
    naDiscountPercentApplied:
      source: calculatedDiscount.naDiscountPercentApplied
      type: number
    items:
      source: calculatedDiscount.items
      type: array
    accountInfo:
      source: validatedAccountInfo
      type: object
```

## Error Response Concept

```yaml
error_response:
  body:
    code:
      source: workflowError.error_code
      type: string
    message:
      source: workflowError.message
      type: string
```

---

# Schema Validation Findings from Discount API

## Strengths Confirmed

The workflow schema can represent:

* API-level metadata
* Header validation
* Request body validation
* Enum validation
* External service invocation
* Downstream response validation
* Intermediate context preparation
* Database references
* Business rule references
* Response mapping
* Error mapping

---

## Gaps Identified

The Discount API reveals several schema gaps.

## Gap 1 — Branching Requires Explicit Structure

The workflow contains employee and new account discount paths.

The schema should explicitly support:

* Branch conditions
* True path
* False path
* Branch start step
* Branch merge step

## Gap 2 — Error Mapping Needs Standardization

Different errors can come from:

* Validation
* External service
* Database lookup
* Business rule failure
* Calculation failure
* Response mapping failure

The schema should include a standard error mapping block.

## Gap 3 — Authentication Metadata Is Required

The Interrogate Account service requires API key and HMAC authentication.

The schema should support external service authentication metadata.

## Gap 4 — Query References Should Be Strongly Linked

Business workflow steps need direct links to database query IDs.

The schema should require query references for database-backed steps.

## Gap 5 — Calculation Rules Need Better Representation

Discount calculation logic may involve:

* Base percentage
* Additional percentage
* Cap amount
* Item exclusions
* Account type

The schema should include a structured calculation rule section.

---

# Generator Readiness Validation

## Node-RED Generator Readiness

The workflow schema contains enough information to generate the following Node-RED nodes conceptually:

* HTTP In node
* Header validation node
* Body validation node
* Enum validation node
* Request mapping node
* REST client node
* Response validation node
* DB context preparation node
* Employee discount rule node
* New account discount rule node
* Exclusion rule node
* Calculation node
* Response mapping node
* HTTP Response node

Status:

```text
Partially ready
```

Reason:

The schema needs stronger branching and error mapping support before reliable Node-RED generation.

---

## Excel Intake Generator Readiness

The workflow schema contains enough information to populate these Excel sheets conceptually:

* api_endpoints
* external_services
* business_requirements
* endpoint_service_mapping
* queries
* entities

Status:

```text
Mostly ready
```

Reason:

The workflow provides enough metadata for Excel intake generation, but query references and schema-to-sheet mapping rules should be standardized.

---

## Go Code Generator Readiness

The workflow schema contains enough information to generate conceptual Go layers:

* Handler
* DTO
* Validator
* Service
* Client
* Repository
* Mapper
* Error handler

Status:

```text
Partially ready
```

Reason:

The schema describes the workflow well, but Go generation requires stricter field definitions, DTO contracts, error types, and branching structure.

---

## Documentation Generator Readiness

The workflow schema contains enough information to generate API documentation.

Status:

```text
Ready with minor improvements
```

Reason:

The schema includes endpoint, request, response, business rules, service references, and error behavior.

---

## Test Case Generator Readiness

The workflow schema contains partial information for test generation.

Status:

```text
Not fully ready
```

Reason:

Test generation requires additional metadata such as:

* Positive scenarios
* Negative scenarios
* Boundary cases
* Mock downstream responses
* Mock database results
* Expected status code
* Expected response body

---

# Validation Conclusion

The FEED Discount API is a strong validation case because it tests the workflow schema against a complex enterprise workflow.

The schema successfully represents the major parts of the workflow, including request validation, external service calls, database-backed business rules, discount calculation, response mapping, and error handling.

However, this sample also reveals important areas that need improvement before the schema can support reliable artifact generation.

The most important improvements are:

1. Add explicit conditional branching structure.
2. Add standard error mapping format.
3. Add external service authentication metadata.
4. Add stronger query reference linking.
5. Add structured calculation rule support.
6. Add test generation metadata.

This sample confirms that the Day 8 schema is directionally correct, but it must be enhanced before it can support production-grade Node-RED, Excel, Go, documentation, and test generation workflows.
