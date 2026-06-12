# Day 11 — Workflow Step Mapping Rules

## Purpose

This document defines how workflow actions from raw API requirements should be mapped into Workflow Schema v2.

Workflow step mapping is important because it converts requirement statements into an ordered sequence of executable workflow actions.

No coding is performed in this phase.

---

## Background

On Day 10, Workflow Schema v2 introduced a formal `workflow_steps` block.

Day 11 Phase 1 defined the overall requirement-to-workflow mapping approach.

Day 11 Phase 2 defined API contract mapping rules.

Day 11 Phase 3 defined validation mapping rules.

Phase 4 now focuses on how raw requirement statements become workflow steps.

Workflow steps are the core of the workflow representation because they describe what the API actually does after receiving a request.

---

## Workflow Step Mapping Objective

The objective of workflow step mapping is to convert requirement actions into structured workflow steps.

A requirement such as:

```text
Validate the request body, call the account interrogation service, check employee discount rules, calculate discount, and return response.
```

should become a sequence like:

```text
STEP-001 Validate Request Headers
STEP-002 Validate Request Body
STEP-003 Build Account Interrogation Request
STEP-004 Call Interrogate Account Service
STEP-005 Validate Account Interrogation Response
STEP-006 Prepare Discount Context
STEP-007 Determine Discount Path
STEP-008 Evaluate Employee Discount
STEP-009 Evaluate New Account Discount
STEP-010 Apply Exclusion Rules
STEP-011 Calculate Discount
STEP-012 Map Final Response
```

Each step should have:

* Step ID
* Step name
* Step type
* Description
* Inputs
* Outputs
* Dependencies
* Condition, if applicable
* Success behavior
* Failure behavior

---

# Schema v2 Blocks Used

Workflow step mapping primarily fills these Schema v2 blocks:

```text
workflow_steps
branching
request_mappings
response_mappings
external_services
database_references
business_rules
calculation_rules
error_mappings
test_scenarios
source_traceability
generator_readiness
```

The `workflow_steps` block gives the main sequence.

Other blocks provide supporting detail for each step.

---

# Workflow Step Structure

Each workflow step should follow this structure:

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
    condition: none
    success_behavior: Continue to STEP-002.
    failure_behavior:
      error_code: BAD_REQUEST
      http_status: 400
      message: Request header validation failed.
```

---

# Supported Step Types

Workflow Schema v2 supports the following step types:

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

Each type should be used consistently.

---

# 1. Validation Step Mapping

## Requirement Signal

Look for phrases such as:

```text
validate request
validate headers
validate body
check required fields
verify input
ensure field is present
validate enum values
```

## Mapping Rule

Map validation actions into workflow steps with:

```yaml
step_type: validation
```

Detailed field-level validation rules should still be stored in `field_validations`.

The workflow step should represent the action of running validation.

## Example Requirement

```text
Validate required request headers before processing the request.
```

## Schema v2 Mapping

```yaml
workflow_steps:
  - step_id: STEP-001
    step_name: Validate Request Headers
    step_type: validation
    description: Validate required request headers before workflow execution.
    inputs:
      - request.headers
    outputs:
      - validatedHeaders
    depends_on: []
    condition: none
    success_behavior: Continue to STEP-002.
    failure_behavior:
      error_code: BAD_REQUEST
      http_status: 400
      message: Request header validation failed.
```

## Notes

Common validation steps include:

```text
Validate Request Headers
Validate Request Body
Validate Enum Fields
Validate Path Parameters
Validate Query Parameters
```

Do not create a separate workflow step for every individual field unless the requirement explicitly separates them.

---

# 2. Mapping Step Mapping

## Requirement Signal

Look for phrases such as:

```text
map request
build downstream request
transform payload
prepare request
convert input
copy field
derive field
construct payload
```

## Mapping Rule

Map transformation actions into workflow steps with:

```yaml
step_type: mapping
```

Detailed source-to-target field mapping should be stored in `request_mappings`.

## Example Requirement

```text
Build the account interrogation request using account encryptInfo and entryMode.
```

## Schema v2 Mapping

```yaml
workflow_steps:
  - step_id: STEP-003
    step_name: Build Account Interrogation Request
    step_type: mapping
    description: Build the downstream account interrogation request from account encryption data and entry mode.
    inputs:
      - validatedBody.account.encryptInfo
      - validatedBody.account.entryMode
      - validatedHeaders
    outputs:
      - accountInterrogationRequest
    depends_on:
      - STEP-002
    condition: none
    success_behavior: Continue to STEP-004.
    failure_behavior:
      error_code: INTERNAL_MAPPING_ERROR
      http_status: 500
      message: Failed to build account interrogation request.
```

## Supporting Request Mapping

```yaml
request_mappings:
  - mapping_id: MAP-INTERROGATE-001
    mapping_name: Map Encrypt Info to Account Interrogation Request
    applies_to_step: STEP-003
    source: request.body.account.encryptInfo
    target: accountInterrogationRequest.elements[0].encryptInfo
    required: true
    transformation: direct_copy
```

---

# 3. External Service Step Mapping

## Requirement Signal

Look for phrases such as:

```text
call downstream service
invoke external API
send request to
call account service
call product customization service
REST call
SOAP call
```

## Mapping Rule

Map downstream service calls into workflow steps with:

```yaml
step_type: external_service
```

Service details should also be mapped into `external_services`.

Authentication details should map into `authentication`.

## Example Requirement

```text
Call the Interrogate Account service to resolve account information.
```

## Schema v2 Mapping

```yaml
workflow_steps:
  - step_id: STEP-004
    step_name: Call Interrogate Account Service
    step_type: external_service
    description: Invoke account interrogation service to resolve account information before discount evaluation.
    inputs:
      - accountInterrogationRequest
    outputs:
      - accountInterrogationResponse
    depends_on:
      - STEP-003
    condition: none
    success_behavior: Continue to STEP-005.
    failure_behavior:
      error_code: DOWNSTREAM_SERVICE_ERROR
      http_status: 502
      message: Account interrogation service failed.
```

## Supporting External Service Mapping

```yaml
external_services:
  - service_id: SRV-INTERROGATE-ACCOUNT
    service_name: Interrogate Account Service
    service_type: REST
    purpose: Resolve account information before discount evaluation.
    method: POST
    endpoint_reference: /accountinterrogation/api/v1/accountinterrogation/piidLookup
    operation_name: piidLookup
```

---

# 4. Database Query Step Mapping

## Requirement Signal

Look for phrases such as:

```text
lookup
query
fetch from table
check database
read from
select from
find matching record
get configuration value
```

## Mapping Rule

Map direct database lookup actions into workflow steps with:

```yaml
step_type: database_query
```

However, if the database query is part of a larger business rule step, the main workflow step may use:

```yaml
step_type: business_rule
```

and the query should be linked through `database_references`.

## Example Requirement

```text
Lookup discount configuration value from FEED_CONFIG.
```

## Schema v2 Mapping

```yaml
workflow_steps:
  - step_id: STEP-009A
    step_name: Lookup Discount Configuration
    step_type: database_query
    description: Retrieve discount configuration value from FEED_CONFIG.
    inputs:
      - configName
    outputs:
      - discountConfigValue
    depends_on:
      - STEP-007
    condition: discountPath == new_account
    success_behavior: Continue new account discount evaluation.
    failure_behavior:
      error_code: DATABASE_ERROR
      http_status: 500
      message: Failed to lookup discount configuration.
```

## Supporting Database Reference

```yaml
database_references:
  - query_id: SQL-CONFIG-001
    query_name: Lookup Discount Configuration
    database_type: PostgreSQL
    table_names:
      - FEED_CONFIG
    used_by_step: STEP-009A
    purpose: Lookup configured discount cap amount.
    inputs:
      configName: MACYS_CAP_AMOUNT
    outputs:
      configValue: newAccountCapAmount
```

---

# 5. Business Rule Step Mapping

## Requirement Signal

Look for phrases such as:

```text
if eligible
apply rule
determine eligibility
check employee account
check new account
apply exclusion
evaluate profile
business condition
must satisfy
```

## Mapping Rule

Map business decisions into workflow steps with:

```yaml
step_type: business_rule
```

Detailed rule definitions should also be mapped into `business_rules`.

## Example Requirement

```text
If the account belongs to an employee, evaluate employee discount eligibility.
```

## Schema v2 Mapping

```yaml
workflow_steps:
  - step_id: STEP-008
    step_name: Evaluate Employee Discount
    step_type: business_rule
    description: Evaluate employee discount eligibility, profile, and additional discount rules.
    inputs:
      - discountContext
      - validatedAccountInfo
    outputs:
      - employeeDiscountResult
    depends_on:
      - STEP-007
    condition: discountPath == employee
    success_behavior: Continue to STEP-010.
    failure_behavior:
      error_code: EMPLOYEE_DISCOUNT_EVALUATION_FAILED
      http_status: 500
      message: Employee discount evaluation failed.
```

## Supporting Business Rule

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
```

---

# 6. Calculation Step Mapping

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
derive amount
round
```

## Mapping Rule

Map calculation actions into workflow steps with:

```yaml
step_type: calculation
```

Detailed formulas or calculation behavior should map into `calculation_rules`.

## Example Requirement

```text
Calculate the final discount amount using the discount percentage and eligible item amount.
```

## Schema v2 Mapping

```yaml
workflow_steps:
  - step_id: STEP-011
    step_name: Calculate Discount
    step_type: calculation
    description: Calculate final discount values based on eligibility, profiles, exclusions, and cap rules.
    inputs:
      - discountEligibilityResult
      - discountContext.transaction.items
    outputs:
      - calculatedDiscount
    depends_on:
      - STEP-010
    condition: none
    success_behavior: Continue to STEP-012.
    failure_behavior:
      error_code: DISCOUNT_CALCULATION_FAILED
      http_status: 500
      message: Discount calculation failed.
```

## Supporting Calculation Rule

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
    item_level: true
```

---

# 7. Response Mapping Step Mapping

## Requirement Signal

Look for phrases such as:

```text
return response
map final response
build response
respond with
return success payload
map downstream response to API response
```

## Mapping Rule

Map response construction into workflow steps with:

```yaml
step_type: response_mapping
```

Detailed response fields should map into `response_mappings`.

## Example Requirement

```text
Return discount type, discount percentages, and item-level discount details.
```

## Schema v2 Mapping

```yaml
workflow_steps:
  - step_id: STEP-012
    step_name: Map Final Response
    step_type: response_mapping
    description: Map calculated discount and account information to the final API response.
    inputs:
      - calculatedDiscount
      - validatedAccountInfo
    outputs:
      - finalApiResponse
    depends_on:
      - STEP-011
    condition: none
    success_behavior: Return final API response.
    failure_behavior:
      error_code: RESPONSE_MAPPING_FAILED
      http_status: 500
      message: Failed to map final response.
```

## Supporting Response Mapping

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

# 8. Error Mapping Step Mapping

## Requirement Signal

Look for phrases such as:

```text
return error
handle failure
if downstream fails
if validation fails
map error response
standardize error
```

## Mapping Rule

If the requirement describes centralized error handling, create a workflow step with:

```yaml
step_type: error_mapping
```

For most workflows, detailed reusable errors should go into `error_mappings`.

## Example Requirement

```text
If downstream service fails, return a standardized downstream service error.
```

## Schema v2 Mapping

```yaml
workflow_steps:
  - step_id: STEP-ERR-001
    step_name: Map Workflow Error
    step_type: error_mapping
    description: Map workflow errors into standardized API error responses.
    inputs:
      - workflowError
    outputs:
      - errorApiResponse
    depends_on: []
    condition: workflowError exists
    success_behavior: Return standardized error response.
    failure_behavior:
      error_code: INTERNAL_SERVER_ERROR
      http_status: 500
      message: Failed to map workflow error.
```

## Supporting Error Mapping

```yaml
error_mappings:
  - error_mapping_id: ERR-DOWNSTREAM-001
    error_source: downstream_service
    condition: downstream timeout or service unavailable
    target_error_code: DOWNSTREAM_SERVICE_ERROR
    target_http_status: 502
    target_message: Downstream service failed.
    retryable: true
    fail_fast: true
```

---

# 9. Branching Step Mapping

## Requirement Signal

Look for phrases such as:

```text
if
else
when
otherwise
based on
route to
determine path
success path
error path
employee path
new account path
```

## Mapping Rule

Create a workflow step with:

```yaml
step_type: branching
```

Also create a formal entry in the `branching` block.

## Example Requirement

```text
If employeeFlag is true, process employee discount. Otherwise process new account discount.
```

## Schema v2 Mapping

```yaml
workflow_steps:
  - step_id: STEP-007
    step_name: Determine Discount Path
    step_type: branching
    description: Determine whether the workflow should use employee discount path or new account discount path.
    inputs:
      - validatedAccountInfo.employeeFlag
      - discountContext
    outputs:
      - discountPath
    depends_on:
      - STEP-006
    condition: none
    success_behavior: Route to STEP-008 or STEP-009.
    failure_behavior:
      error_code: DISCOUNT_PATH_DETERMINATION_FAILED
      http_status: 500
      message: Failed to determine discount path.
```

## Supporting Branching Block

```yaml
branching:
  - branch_id: BRANCH-DISCOUNT-001
    branch_name: Employee versus New Account Discount Path
    branch_type: conditional
    condition: validatedAccountInfo.employeeFlag == true
    source_step: STEP-007
    true_path: STEP-008
    false_path: STEP-009
    merge_step: STEP-010
```

---

# 10. Context Preparation Step Mapping

## Requirement Signal

Look for phrases such as:

```text
prepare context
build processing context
combine account and transaction information
prepare DB lookup context
derive internal values
create intermediate data
```

## Mapping Rule

Map internal context preparation into workflow steps with:

```yaml
step_type: context_preparation
```

## Example Requirement

```text
Prepare discount context using transaction details and resolved account information.
```

## Schema v2 Mapping

```yaml
workflow_steps:
  - step_id: STEP-006
    step_name: Prepare Discount Context
    step_type: context_preparation
    description: Prepare discount context using transaction details and validated account information.
    inputs:
      - validatedBody.transaction
      - validatedAccountInfo
    outputs:
      - discountContext
    depends_on:
      - STEP-005
    condition: none
    success_behavior: Continue to STEP-007.
    failure_behavior:
      error_code: CONTEXT_PREPARATION_FAILED
      http_status: 500
      message: Failed to prepare discount context.
```

---

# Step Ordering Rules

Workflow steps should be ordered based on execution flow.

Recommended default order:

```text
1. Validate headers
2. Validate body
3. Validate enums and formats
4. Build internal or downstream request
5. Call external service if needed
6. Validate downstream response
7. Prepare internal context
8. Determine branching path
9. Execute business rules
10. Execute database lookups
11. Apply exclusions or special rules
12. Perform calculations
13. Map success response
14. Map errors
```

Not every workflow needs every step.

A simple wrapper API may only need:

```text
1. Validate headers
2. Validate body
3. Build downstream request
4. Call downstream service
5. Classify downstream response
6. Map success response
7. Map error response
```

---

# Step ID Naming Rules

Use stable and sequential step IDs.

## Recommended Format

```text
STEP-001
STEP-002
STEP-003
```

## Optional Specialized Error Step

```text
STEP-ERR-001
```

## Notes

Do not reuse step IDs.

If inserting a new step later, either renumber carefully or use suffixes temporarily, such as:

```text
STEP-009A
```

For final documents, clean sequential IDs are preferred.

---

# Step Name Rules

Step names should be short and action-oriented.

Good examples:

```text
Validate Request Headers
Build Downstream Request
Call Interrogate Account Service
Evaluate Employee Discount
Apply Exclusion Rules
Calculate Discount
Map Final Response
```

Avoid vague names like:

```text
Process Data
Do Logic
Handle Stuff
Step 1
```

---

# Step Input Mapping Rules

Each step should list the data it needs.

Examples:

```text
request.headers
request.body
validatedHeaders
validatedBody
accountInterrogationRequest
accountInterrogationResponse
validatedAccountInfo
discountContext
employeeDiscountResult
calculatedDiscount
```

Inputs should come from:

* Original request
* Previous step outputs
* External service responses
* Database query results
* Business rule outputs
* Internal context objects

---

# Step Output Mapping Rules

Each step should produce named outputs.

Examples:

```text
validatedHeaders
validatedBody
accountInterrogationRequest
accountInterrogationResponse
validatedAccountInfo
discountContext
discountPath
employeeDiscountResult
newAccountDiscountResult
discountEligibilityResult
calculatedDiscount
finalApiResponse
errorApiResponse
```

Outputs should be reusable by later steps.

---

# Step Dependency Mapping Rules

Use `depends_on` to show which steps must complete first.

## Sequential Example

```yaml
depends_on:
  - STEP-002
```

## Multiple Dependency Example

```yaml
depends_on:
  - STEP-008
  - STEP-009
```

## Notes

For branch merge steps, dependencies may include multiple possible prior steps.

Example:

```text
Apply Exclusion Rules depends on Employee Discount Path or New Account Discount Path.
```

---

# Step Condition Mapping Rules

Use `condition` when a step only runs in a certain path.

Examples:

```yaml
condition: discountPath == employee
condition: discountPath == new_account
condition: responseClassification == success
condition: responseClassification == error
```

If the step always runs, use:

```yaml
condition: none
```

---

# Success Behavior Mapping Rules

Every step should describe what happens when it succeeds.

Examples:

```text
Continue to STEP-002.
Continue to STEP-005.
Route to STEP-008 or STEP-009.
Return final API response.
```

Success behavior helps documentation and future flow generation.

---

# Failure Behavior Mapping Rules

Every step should describe what happens when it fails.

Failure behavior should include:

```text
error_code
http_status
message
```

Example:

```yaml
failure_behavior:
  error_code: DOWNSTREAM_SERVICE_ERROR
  http_status: 502
  message: Account interrogation service failed.
```

Detailed reusable failure behavior should also map into `error_mappings`.

---

# Mapping Workflow Steps from Requirement Paragraphs

Raw requirements often describe multiple actions in one paragraph.

## Example Raw Paragraph

```text
The API validates required headers and request body. It then calls the account interrogation service to resolve account information. If the account is an employee account, employee discount rules are applied. Otherwise, new account discount rules are applied. Exclusions are checked before calculating the final discount response.
```

## Mapped Workflow Steps

```text
STEP-001 Validate Request Headers
STEP-002 Validate Request Body
STEP-003 Build Account Interrogation Request
STEP-004 Call Interrogate Account Service
STEP-005 Validate Account Interrogation Response
STEP-006 Prepare Discount Context
STEP-007 Determine Discount Path
STEP-008 Evaluate Employee Discount
STEP-009 Evaluate New Account Discount
STEP-010 Apply Exclusion Rules
STEP-011 Calculate Discount
STEP-012 Map Final Response
```

## Notes

When a paragraph contains multiple actions, split it into separate workflow steps.

Each step should represent one logical unit of work.

---

# Example 1 — Fabric API Workflow Step Mapping

## Raw Requirement

```text
The Fabric API validates Division-Id, skuNumbers, and fabricTypeId. It maps skuNumbers to customSkuNumbers, calls the product customization fabric service, and maps the downstream success or error response.
```

## Schema v2 Workflow Steps

```yaml
workflow_steps:
  - step_id: STEP-001
    step_name: Validate Request Headers
    step_type: validation
    description: Validate required Fabric API request headers.
    inputs:
      - request.headers
    outputs:
      - validatedHeaders
    depends_on: []
    condition: none
    success_behavior: Continue to STEP-002.
    failure_behavior:
      error_code: BAD_REQUEST
      http_status: 400
      message: Request header validation failed.

  - step_id: STEP-002
    step_name: Validate Request Body
    step_type: validation
    description: Validate skuNumbers and fabricTypeId.
    inputs:
      - request.body
    outputs:
      - validatedBody
    depends_on:
      - STEP-001
    condition: none
    success_behavior: Continue to STEP-003.
    failure_behavior:
      error_code: BAD_REQUEST
      http_status: 400
      message: Request body validation failed.

  - step_id: STEP-003
    step_name: Build Downstream Fabric Request
    step_type: mapping
    description: Map public API request into downstream fabric service request.
    inputs:
      - validatedHeaders
      - validatedBody
    outputs:
      - downstreamFabricRequest
    depends_on:
      - STEP-002
    condition: none
    success_behavior: Continue to STEP-004.
    failure_behavior:
      error_code: INTERNAL_MAPPING_ERROR
      http_status: 500
      message: Failed to build downstream fabric request.

  - step_id: STEP-004
    step_name: Call Fabric Customization Service
    step_type: external_service
    description: Invoke downstream fabric customization service.
    inputs:
      - downstreamFabricRequest
    outputs:
      - downstreamFabricResponse
    depends_on:
      - STEP-003
    condition: none
    success_behavior: Continue to STEP-005.
    failure_behavior:
      error_code: DOWNSTREAM_SERVICE_ERROR
      http_status: 502
      message: Fabric customization service failed.

  - step_id: STEP-005
    step_name: Classify Downstream Response
    step_type: branching
    description: Determine whether downstream response is success or error.
    inputs:
      - downstreamFabricResponse
    outputs:
      - responseClassification
    depends_on:
      - STEP-004
    condition: none
    success_behavior: Route to STEP-006 or STEP-007.
    failure_behavior:
      error_code: DOWNSTREAM_RESPONSE_VALIDATION_FAILED
      http_status: 502
      message: Unable to classify downstream response.

  - step_id: STEP-006
    step_name: Map Success Response
    step_type: response_mapping
    description: Map downstream success response to public API response.
    inputs:
      - downstreamFabricResponse
    outputs:
      - finalApiResponse
    depends_on:
      - STEP-005
    condition: responseClassification == success
    success_behavior: Return final API response.
    failure_behavior:
      error_code: RESPONSE_MAPPING_FAILED
      http_status: 500
      message: Failed to map success response.

  - step_id: STEP-007
    step_name: Map Error Response
    step_type: error_mapping
    description: Map downstream error response to public API error response.
    inputs:
      - downstreamFabricResponse
    outputs:
      - errorApiResponse
    depends_on:
      - STEP-005
    condition: responseClassification == error
    success_behavior: Return error API response.
    failure_behavior:
      error_code: ERROR_MAPPING_FAILED
      http_status: 500
      message: Failed to map error response.
```

---

# Example 2 — Discount API Workflow Step Mapping

## Raw Requirement

```text
The Discount API validates headers and body, calls Interrogate Account, prepares discount context, determines employee or new account path, applies discount rules, checks exclusions, calculates discount, and returns final response.
```

## Schema v2 Workflow Steps

```yaml
workflow_steps:
  - step_id: STEP-001
    step_name: Validate Request Headers
    step_type: validation

  - step_id: STEP-002
    step_name: Validate Request Body
    step_type: validation

  - step_id: STEP-003
    step_name: Build Account Interrogation Request
    step_type: mapping

  - step_id: STEP-004
    step_name: Call Interrogate Account Service
    step_type: external_service

  - step_id: STEP-005
    step_name: Validate Account Interrogation Response
    step_type: validation

  - step_id: STEP-006
    step_name: Prepare Discount Context
    step_type: context_preparation

  - step_id: STEP-007
    step_name: Determine Discount Path
    step_type: branching

  - step_id: STEP-008
    step_name: Evaluate Employee Discount
    step_type: business_rule

  - step_id: STEP-009
    step_name: Evaluate New Account Discount
    step_type: business_rule

  - step_id: STEP-010
    step_name: Apply Exclusion Rules
    step_type: business_rule

  - step_id: STEP-011
    step_name: Calculate Discount
    step_type: calculation

  - step_id: STEP-012
    step_name: Map Final Response
    step_type: response_mapping
```

---

# Workflow Step Mapping Table

| Requirement Signal   | Step Type           |
| -------------------- | ------------------- |
| Validate headers     | validation          |
| Validate body        | validation          |
| Validate enum values | validation          |
| Build request        | mapping             |
| Transform payload    | mapping             |
| Call REST service    | external_service    |
| Call SOAP service    | external_service    |
| Query database       | database_query      |
| Lookup table         | database_query      |
| Check eligibility    | business_rule       |
| Apply rule           | business_rule       |
| Check exclusion      | business_rule       |
| Determine path       | branching           |
| If/else logic        | branching           |
| Prepare context      | context_preparation |
| Calculate amount     | calculation         |
| Apply percentage     | calculation         |
| Apply cap            | calculation         |
| Return response      | response_mapping    |
| Map final response   | response_mapping    |
| Handle error         | error_mapping       |
| Map error response   | error_mapping       |

---

# Workflow Step Review Checklist

Use this checklist after mapping workflow steps.

```text
- Are all major requirement actions represented as workflow steps?
- Does each step have a unique step ID?
- Does each step have a clear action-oriented name?
- Is each step assigned the correct step type?
- Does each step have inputs?
- Does each step have outputs?
- Are dependencies defined?
- Are conditional steps marked with condition?
- Are branching rules represented in the branching block?
- Are external service steps linked to external_services?
- Are database steps linked to database_references?
- Are business rule steps linked to business_rules?
- Are calculation steps linked to calculation_rules?
- Are response steps linked to response_mappings?
- Are error steps linked to error_mappings?
- Is source traceability captured for important steps?
- Are inferred steps marked with appropriate confidence?
```

---

# Common Mistakes to Avoid

## Mistake 1 — Making Steps Too Large

Avoid creating one broad step like:

```text
Process Discount
```

Instead, split it into:

```text
Determine Discount Path
Evaluate Employee Discount
Evaluate New Account Discount
Apply Exclusion Rules
Calculate Discount
```

## Mistake 2 — Making Steps Too Small

Do not create one workflow step for every single field validation unless the workflow explicitly requires it.

Use field validations for field-level rules.

Use workflow steps for logical processing stages.

## Mistake 3 — Losing Branching Logic

If the requirement says `if`, `else`, `when`, or `otherwise`, check whether a branching block is needed.

## Mistake 4 — Not Naming Outputs

Every important step should produce a named output so later steps can reference it.

## Mistake 5 — Ignoring Failure Behavior

Every step should define failure behavior, even if the requirement does not explicitly describe it.

Use reasonable defaults and mark inferred behavior with medium or low confidence.

## Mistake 6 — Not Linking Supporting Blocks

Workflow steps should not stand alone.

They should link to:

```text
field_validations
request_mappings
external_services
database_references
business_rules
calculation_rules
response_mappings
error_mappings
```

when applicable.

---

# Final Summary

Workflow step mapping converts raw requirement actions into structured Schema v2 workflow steps.

The `workflow_steps` block is the main output, but each step should connect to supporting blocks such as branching, external services, business rules, database references, mappings, calculations, errors, tests, and traceability.

Strong workflow step mapping is essential because it defines the actual orchestration that future Node-RED, Excel, Go, documentation, and test generators will depend on.

This completes Phase 4 of Day 11.
