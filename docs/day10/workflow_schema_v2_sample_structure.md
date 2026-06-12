# Day 10 — Workflow Schema v2 Sample Structure

## Purpose

This document provides a sample Workflow Schema v2 structure using YAML-style documentation.

The purpose of this file is to show how the refined Schema v2 blocks can be arranged together in a complete workflow representation.

This is not implementation code.

This is a design example that shows how future extracted API workflows may be stored before being used by artifact generators.

No coding is performed in this phase.

---

## Background

On Day 8, Workflow Schema v1 was created.

On Day 9, Schema v1 was validated against two realistic APIs:

1. FEED Discount API
2. Customize IT Fabric API

On Day 10, Schema v2 was designed to improve the original schema using the gaps found during validation.

The main Schema v2 improvements include:

* Formal field validation
* Formal branching
* External service authentication metadata
* Stronger database references
* Structured business rules
* Structured calculation rules
* Request mappings
* Response mappings
* Standard error mappings
* Test scenarios
* Source traceability
* Generator readiness

This file shows how those blocks can be combined into a single sample schema structure.

---

# Workflow Schema v2 — Full Sample Structure

The following sample uses a simplified Discount API workflow because it contains most of the schema features.

Sample API:

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

---

```yaml
workflow:
  metadata:
    workflow_id: WF-DISCOUNT-001
    workflow_name: Evaluate Transaction Discount Workflow
    workflow_description: >
      Validates a transaction discount request, resolves account information,
      evaluates employee and new account discount rules, applies exclusions,
      calculates discounts, and returns the final discount response.
    workflow_version: 1.0.0
    workflow_type: business_orchestration
    source_domain: FEED Discount
    schema_version: v2
    status: draft

  api_contract:
    endpoint_id: EP-DISCOUNT-001
    path: /api/v1/discount/evaluateTransactionDiscount
    method: POST
    api_version: v1
    content_type: application/json
    response_type: application/json
    operation_name: evaluateTransactionDiscount
    summary: Evaluate transaction discount eligibility and return applicable discount details.
    controller_name: DiscountController
    handler_name: EvaluateTransactionDiscount
    tags:
      - discount
      - feed
      - transaction

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

    - validation_id: VAL-DISCOUNT-HDR-002
      field_path: request.headers.requestorInfo-subclientId
      location: header
      type: string
      required: true
      allow_blank: false
      failure_behavior:
        http_status: 400
        error_code: BAD_REQUEST
        message: requestorInfo-subclientId header is required.

    - validation_id: VAL-DISCOUNT-HDR-003
      field_path: request.headers.requestorInfo-traceId
      location: header
      type: string
      required: false
      allow_blank: false
      failure_behavior:
        http_status: 400
        error_code: BAD_REQUEST
        message: requestorInfo-traceId must be valid when provided.

    - validation_id: VAL-DISCOUNT-HDR-004
      field_path: request.headers.x-macys-apikey
      location: header
      type: string
      required: true
      allow_blank: false
      failure_behavior:
        http_status: 400
        error_code: BAD_REQUEST
        message: x-macys-apikey header is required.

    - validation_id: VAL-DISCOUNT-HDR-005
      field_path: request.headers.x-hmac-secret
      location: header
      type: string
      required: true
      allow_blank: false
      failure_behavior:
        http_status: 400
        error_code: BAD_REQUEST
        message: x-hmac-secret header is required.

    - validation_id: VAL-DISCOUNT-BODY-001
      field_path: request.body.account
      location: body
      type: object
      required: true
      failure_behavior:
        http_status: 400
        error_code: BAD_REQUEST
        message: account object is required.

    - validation_id: VAL-DISCOUNT-BODY-002
      field_path: request.body.account.encryptInfo
      location: body
      type: object
      required: true
      failure_behavior:
        http_status: 400
        error_code: BAD_REQUEST
        message: account.encryptInfo object is required.

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

    - validation_id: VAL-DISCOUNT-BODY-004
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
      failure_behavior:
        http_status: 400
        error_code: INVALID_ENUM_VALUE
        message: keyType contains an unsupported value.

    - validation_id: VAL-DISCOUNT-BODY-005
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
      failure_behavior:
        http_status: 400
        error_code: INVALID_ENUM_VALUE
        message: entryMode contains an unsupported value.

    - validation_id: VAL-DISCOUNT-BODY-006
      field_path: request.body.transaction
      location: body
      type: object
      required: true
      failure_behavior:
        http_status: 400
        error_code: BAD_REQUEST
        message: transaction object is required.

    - validation_id: VAL-DISCOUNT-BODY-007
      field_path: request.body.transaction.items
      location: body
      type: array
      required: true
      min_items: 1
      failure_behavior:
        http_status: 400
        error_code: BAD_REQUEST
        message: transaction.items must contain at least one item.

  workflow_steps:
    - step_id: STEP-001
      step_name: Validate Request Headers
      step_type: validation
      description: Validate all required request headers.
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
        message: Required request header is missing or invalid.

    - step_id: STEP-002
      step_name: Validate Request Body
      step_type: validation
      description: Validate required body fields, nested objects, arrays, and enum values.
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
        message: Request body is missing required fields or contains invalid values.

    - step_id: STEP-003
      step_name: Build Account Interrogation Request
      step_type: mapping
      description: Build the downstream account interrogation request from request headers and account encrypt information.
      inputs:
        - validatedHeaders
        - validatedBody.account.encryptInfo
        - validatedBody.account.entryMode
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

    - step_id: STEP-004
      step_name: Call Interrogate Account Service
      step_type: external_service
      description: Invoke the account interrogation downstream service to resolve account information.
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

    - step_id: STEP-005
      step_name: Validate Account Interrogation Response
      step_type: validation
      description: Validate the downstream account interrogation response.
      inputs:
        - accountInterrogationResponse
      outputs:
        - validatedAccountInfo
      depends_on:
        - STEP-004
      condition: none
      success_behavior: Continue to STEP-006.
      failure_behavior:
        error_code: ACCOUNT_INTERROGATION_INVALID_RESPONSE
        http_status: 502
        message: Account interrogation response is invalid.

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

    - step_id: STEP-009
      step_name: Evaluate New Account Discount
      step_type: business_rule
      description: Evaluate new account discount eligibility, profile, and cap rules.
      inputs:
        - discountContext
        - validatedAccountInfo
      outputs:
        - newAccountDiscountResult
      depends_on:
        - STEP-007
      condition: discountPath == new_account
      success_behavior: Continue to STEP-010.
      failure_behavior:
        error_code: NEW_ACCOUNT_DISCOUNT_EVALUATION_FAILED
        http_status: 500
        message: New account discount evaluation failed.

    - step_id: STEP-010
      step_name: Apply Exclusion Rules
      step_type: business_rule
      description: Apply department, vendor, class, and additional discount exclusion rules.
      inputs:
        - employeeDiscountResult
        - newAccountDiscountResult
        - discountContext
      outputs:
        - discountEligibilityResult
      depends_on:
        - STEP-008
        - STEP-009
      condition: none
      success_behavior: Continue to STEP-011.
      failure_behavior:
        error_code: DISCOUNT_EXCLUSION_CHECK_FAILED
        http_status: 500
        message: Discount exclusion check failed.

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

  branching:
    - branch_id: BRANCH-DISCOUNT-001
      branch_name: Employee versus New Account Discount Path
      branch_type: conditional
      condition: validatedAccountInfo.employeeFlag == true
      source_step: STEP-007
      true_path: STEP-008
      false_path: STEP-009
      merge_step: STEP-010
      description: >
        Routes employee accounts to employee discount evaluation and non-employee
        accounts to new account discount evaluation.

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

  database_references:
    - query_id: SQL-EMP-001
      query_name: Lookup Employee Account Profile
      database_type: PostgreSQL
      table_names:
        - Emp_Acct_Profile
      used_by_step: STEP-008
      used_by_business_rule: BR-EMP-001
      purpose: Determine whether account has an employee profile.
      inputs:
        internalAccountToken: validatedAccountInfo.internalAccountToken
      outputs:
        employeeProfile: employeeAccountProfile
      no_record_behavior: continue_without_employee_discount
      failure_behavior: database_error

    - query_id: SQL-EMP-002
      query_name: Lookup Employee Discount Profile
      database_type: PostgreSQL
      table_names:
        - Disc_By_Dept_Vnd_Class
        - Emp_Disc_Profile
      used_by_step: STEP-008
      used_by_business_rule: BR-EMP-003
      purpose: Lookup employee discount profile by item attributes.
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

    - query_id: SQL-NA-001
      query_name: Lookup New Account Discount Profile
      database_type: PostgreSQL
      table_names:
        - Disc_By_Dept_Vnd_Class
      used_by_step: STEP-009
      used_by_business_rule: BR-NA-002
      purpose: Lookup new account discount eligibility by item attributes.
      inputs:
        division: discountContext.division
        department: transactionItem.department
        vendor: transactionItem.vendor
        class: transactionItem.class
      outputs:
        newAccountDiscountProfile: newAccountDiscountProfile
      no_record_behavior: continue_without_new_account_discount
      failure_behavior: database_error

    - query_id: SQL-EXCL-001
      query_name: Check Department Vendor Class Exclusion
      database_type: PostgreSQL
      table_names:
        - Disc_Dept_Vnd_Class_Exclusions
      used_by_step: STEP-010
      used_by_business_rule: BR-EXCL-001
      purpose: Determine whether an item is excluded from discount.
      inputs:
        division: discountContext.division
        department: transactionItem.department
        vendor: transactionItem.vendor
        class: transactionItem.class
      outputs:
        exclusionResult: discountExclusionResult
      no_record_behavior: item_is_not_excluded
      failure_behavior: database_error

    - query_id: SQL-CONFIG-001
      query_name: Lookup Discount Configuration
      database_type: PostgreSQL
      table_names:
        - FEED_CONFIG
      used_by_step: STEP-009
      used_by_business_rule: BR-NA-003
      purpose: Lookup new account discount cap amount.
      inputs:
        configName: MACYS_CAP_AMOUNT
      outputs:
        configValue: newAccountCapAmount
      no_record_behavior: use_default_or_fail_based_on_requirement
      failure_behavior: database_error

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

    - rule_id: BR-EMP-002
      rule_name: Home Shop versus Cross Shop Evaluation
      category: employee_discount
      description: Determine whether the employee discount is evaluated as home shop or cross shop.
      applies_to_step: STEP-008
      condition: employeeDiscountEligible == true
      inputs:
        - employeeAccountProfile.homeDivision
        - discountContext.division
      outputs:
        - shopType
      related_queries: []
      success_behavior: Continue employee discount profile selection.
      failure_behavior: Continue without employee discount if shop rules are not satisfied.

    - rule_id: BR-EMP-003
      rule_name: Employee Discount Profile Selection
      category: employee_discount
      description: Select employee discount profile using item division, department, vendor, and class.
      applies_to_step: STEP-008
      condition: employeeDiscountEligible == true
      inputs:
        - discountContext
        - transactionItem
      outputs:
        - employeeDiscountProfile
      related_queries:
        - SQL-EMP-002
      success_behavior: Continue employee discount calculation.
      failure_behavior: Continue without employee discount if no profile is found.

    - rule_id: BR-NA-001
      rule_name: New Account Eligibility
      category: new_account_discount
      description: Determine whether new account discount processing should be applied.
      applies_to_step: STEP-009
      condition: validatedAccountInfo.employeeFlag == false
      inputs:
        - validatedAccountInfo
        - discountContext
      outputs:
        - newAccountEligible
      related_queries: []
      success_behavior: Continue new account discount profile lookup.
      failure_behavior: Continue without new account discount.

    - rule_id: BR-NA-002
      rule_name: New Account Discount Profile Selection
      category: new_account_discount
      description: Select new account discount profile using item attributes.
      applies_to_step: STEP-009
      condition: newAccountEligible == true
      inputs:
        - discountContext
        - transactionItem
      outputs:
        - newAccountDiscountProfile
      related_queries:
        - SQL-NA-001
      success_behavior: Continue new account discount calculation.
      failure_behavior: Continue without new account discount if no profile is found.

    - rule_id: BR-NA-003
      rule_name: New Account Cap Rule
      category: new_account_discount
      description: Apply configured cap amount to new account discount.
      applies_to_step: STEP-009
      condition: newAccountEligible == true
      inputs:
        - calculatedNewAccountDiscount
        - newAccountCapAmount
      outputs:
        - cappedNewAccountDiscount
      related_queries:
        - SQL-CONFIG-001
      success_behavior: Apply cap and continue.
      failure_behavior: Return configuration error if cap is required and missing.

    - rule_id: BR-EXCL-001
      rule_name: Department Vendor Class Exclusion
      category: exclusion
      description: Check whether item is excluded from discount based on department, vendor, and class.
      applies_to_step: STEP-010
      condition: discount candidate exists
      inputs:
        - discountContext
        - transactionItem
      outputs:
        - itemExclusionResult
      related_queries:
        - SQL-EXCL-001
      success_behavior: Exclude item when exclusion is found.
      failure_behavior: Continue when no exclusion is found.

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

    - calculation_id: CALC-DISCOUNT-002
      calculation_name: Apply Additional Employee Discount
      applies_when: additionalDiscountProfile exists and item is eligible
      calculation_type: percentage
      inputs:
        - transactionItem.amount
        - additionalDiscountProfile.discountPercent
      output: transactionItem.additionalDiscountAmount
      calculation_behavior: Apply additional discount percentage to eligible item amount.
      cap_behavior: none
      item_level: true
      rounding_behavior: standard_currency_rounding
      failure_behavior: discount_calculation_error

    - calculation_id: CALC-DISCOUNT-003
      calculation_name: Apply New Account Discount
      applies_when: discountType == new_account
      calculation_type: percentage
      inputs:
        - transactionItem.amount
        - newAccountDiscountProfile.discountPercent
      output: transactionItem.newAccountDiscountAmount
      calculation_behavior: Apply new account discount percentage to eligible item amount.
      cap_behavior: apply_new_account_cap
      item_level: true
      rounding_behavior: standard_currency_rounding
      failure_behavior: discount_calculation_error

    - calculation_id: CALC-DISCOUNT-004
      calculation_name: Apply New Account Cap
      applies_when: discountType == new_account
      calculation_type: cap
      inputs:
        - calculatedNewAccountDiscount
        - newAccountCapAmount
      output: cappedNewAccountDiscount
      calculation_behavior: Use the lower value between calculated new account discount and configured cap.
      cap_behavior: min(calculatedNewAccountDiscount, newAccountCapAmount)
      item_level: false
      rounding_behavior: standard_currency_rounding
      failure_behavior: discount_calculation_error

  request_mappings:
    - mapping_id: MAP-INTERROGATE-001
      mapping_name: Map Encrypt Info to Account Interrogation Request
      applies_to_step: STEP-003
      source: request.body.account.encryptInfo
      target: accountInterrogationRequest.elements[0].encryptInfo
      required: true
      transformation: direct_copy
      pass_through_when_present: false
      omit_when_missing: false

    - mapping_id: MAP-INTERROGATE-002
      mapping_name: Map Entry Mode to Account Interrogation Request
      applies_to_step: STEP-003
      source: request.body.account.entryMode
      target: accountInterrogationRequest.elements[0].entryMode
      required: true
      transformation: direct_copy
      pass_through_when_present: false
      omit_when_missing: false

    - mapping_id: MAP-INTERROGATE-003
      mapping_name: Pass Trace ID to Account Interrogation Request
      applies_to_step: STEP-003
      source: request.headers.requestorInfo-traceId
      target: accountInterrogationRequest.traceId
      required: false
      transformation: direct_copy
      pass_through_when_present: true
      omit_when_missing: true

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

    - mapping_id: RESP-DISCOUNT-002
      mapping_name: Map Discount Percent Applied
      applies_to_step: STEP-012
      response_type: success
      http_status: 200
      source: calculatedDiscount.discountPercentApplied
      target: response.discountPercentApplied
      transformation: direct_copy
      required: false

    - mapping_id: RESP-DISCOUNT-003
      mapping_name: Map Additional Discount Percent Applied
      applies_to_step: STEP-012
      response_type: success
      http_status: 200
      source: calculatedDiscount.additionalDiscountPercentApplied
      target: response.additionalDiscountPercentApplied
      transformation: direct_copy
      required: false

    - mapping_id: RESP-DISCOUNT-004
      mapping_name: Map New Account Discount Percent Applied
      applies_to_step: STEP-012
      response_type: success
      http_status: 200
      source: calculatedDiscount.naDiscountPercentApplied
      target: response.naDiscountPercentApplied
      transformation: direct_copy
      required: false

    - mapping_id: RESP-DISCOUNT-005
      mapping_name: Map Discount Items
      applies_to_step: STEP-012
      response_type: success
      http_status: 200
      source: calculatedDiscount.items
      target: response.items
      transformation: direct_copy
      required: true

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

    - error_mapping_id: ERR-MAPPING-001
      error_source: mapping
      condition: request mapping failed
      source_error_code: null
      target_error_code: INTERNAL_MAPPING_ERROR
      target_http_status: 500
      target_message: Internal request mapping failed.
      retryable: true
      fail_fast: true
      applies_to_steps:
        - STEP-003

    - error_mapping_id: ERR-DOWNSTREAM-001
      error_source: downstream_service
      condition: downstream timeout or service unavailable
      source_error_code: null
      target_error_code: DOWNSTREAM_SERVICE_ERROR
      target_http_status: 502
      target_message: Downstream account interrogation service failed.
      retryable: true
      fail_fast: true
      applies_to_steps:
        - STEP-004

    - error_mapping_id: ERR-DATABASE-001
      error_source: database
      condition: database query execution failed
      source_error_code: null
      target_error_code: DATABASE_ERROR
      target_http_status: 500
      target_message: Database operation failed.
      retryable: true
      fail_fast: true
      applies_to_steps:
        - STEP-008
        - STEP-009
        - STEP-010

    - error_mapping_id: ERR-CALCULATION-001
      error_source: calculation
      condition: discount calculation failed
      source_error_code: null
      target_error_code: DISCOUNT_CALCULATION_FAILED
      target_http_status: 500
      target_message: Discount calculation failed.
      retryable: true
      fail_fast: true
      applies_to_steps:
        - STEP-011

    - error_mapping_id: ERR-RESPONSE-001
      error_source: response_mapping
      condition: final response mapping failed
      source_error_code: null
      target_error_code: RESPONSE_MAPPING_FAILED
      target_http_status: 500
      target_message: Failed to map final response.
      retryable: true
      fail_fast: true
      applies_to_steps:
        - STEP-012

  test_scenarios:
    - scenario_id: TC-DISCOUNT-001
      scenario_name: Valid Employee Discount Request
      scenario_type: positive
      description: Valid employee account request should execute employee discount path and return employee discount response.
      input:
        headers:
          requestorInfo-clientId: client-001
          requestorInfo-subclientId: subclient-001
          requestorInfo-traceId: trace-001
          requestorinfo-version: v1
          x-macys-apikey: test-api-key
          x-hmac-secret: test-secret
        path_params: {}
        query_params: {}
        body:
          account:
            encryptInfo:
              encryptedData: encrypted-value
              keyType: TOKEN
              keyName: key-name
              keyVersion: v1
              division: "12"
              storeNbr: "101"
            entryMode: TOKENIZED
          transaction:
            division: 12
            storeNbr: 101
            items:
              - sku: SKU123
                department: 10
                vendor: 20
                class: 30
                amount: 100.00
      mocks:
        external_services:
          SRV-INTERROGATE-ACCOUNT:
            status: 200
            body:
              accountInfo:
                internalAccountToken: token-123
                employeeFlag: true
        database:
          SQL-EMP-001:
            found: true
          SQL-EMP-002:
            found: true
            discountPercent: 20
          SQL-EXCL-001:
            found: false
      expected:
        http_status: 200
        response_body:
          discountType: Employee
          discountPercentApplied: 20
        executed_steps:
          - STEP-001
          - STEP-002
          - STEP-003
          - STEP-004
          - STEP-005
          - STEP-006
          - STEP-007
          - STEP-008
          - STEP-010
          - STEP-011
          - STEP-012
        skipped_steps:
          - STEP-009

    - scenario_id: TC-DISCOUNT-002
      scenario_name: Missing Required Header
      scenario_type: negative
      description: Missing client ID header should return validation error.
      input:
        headers:
          requestorInfo-subclientId: subclient-001
        path_params: {}
        query_params: {}
        body: {}
      mocks:
        external_services: {}
        database: {}
      expected:
        http_status: 400
        response_body:
          code: BAD_REQUEST
        executed_steps:
          - STEP-001
        skipped_steps:
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

    - scenario_id: TC-DISCOUNT-003
      scenario_name: Downstream Account Interrogation Failure
      scenario_type: failure
      description: Downstream account interrogation failure should return 502.
      input:
        headers:
          requestorInfo-clientId: client-001
          requestorInfo-subclientId: subclient-001
          x-macys-apikey: test-api-key
          x-hmac-secret: test-secret
        path_params: {}
        query_params: {}
        body:
          account:
            encryptInfo:
              encryptedData: encrypted-value
              keyType: TOKEN
            entryMode: TOKENIZED
          transaction:
            division: 12
            storeNbr: 101
            items:
              - sku: SKU123
                amount: 100.00
      mocks:
        external_services:
          SRV-INTERROGATE-ACCOUNT:
            status: 500
            body:
              code: INTERNAL_SERVER_ERROR
        database: {}
      expected:
        http_status: 502
        response_body:
          code: DOWNSTREAM_SERVICE_ERROR
        executed_steps:
          - STEP-001
          - STEP-002
          - STEP-003
          - STEP-004
        skipped_steps:
          - STEP-005
          - STEP-006
          - STEP-007
          - STEP-008
          - STEP-009
          - STEP-010
          - STEP-011
          - STEP-012

  source_traceability:
    workflow_source:
      requirement_doc: FEED Discount Requirement
      requirement_section: Evaluate Transaction Discount
      source_text_reference: Section 2.1
      confidence: high

    step_sources:
      - step_id: STEP-001
        requirement_section: Request Header Requirements
        source_text_reference: Section 2.2
        confidence: high

      - step_id: STEP-004
        requirement_section: Account Interrogation Integration
        source_text_reference: Section 3.1
        confidence: high

      - step_id: STEP-008
        requirement_section: Employee Discount Logic
        source_text_reference: Section 4.1
        confidence: high

      - step_id: STEP-009
        requirement_section: New Account Discount Logic
        source_text_reference: Section 4.2
        confidence: high

      - step_id: STEP-010
        requirement_section: Exclusion Rules
        source_text_reference: Section 4.3
        confidence: medium

    field_sources:
      - field_path: request.body.account.encryptInfo.keyType
        requirement_section: Account Encryption Input
        source_text_reference: Section 2.3
        confidence: high

      - field_path: request.body.account.entryMode
        requirement_section: Account Entry Mode Input
        source_text_reference: Section 2.3
        confidence: high

    business_rule_sources:
      - rule_id: BR-EMP-001
        requirement_section: Employee Eligibility
        source_text_reference: Section 4.1.1
        confidence: high

      - rule_id: BR-NA-001
        requirement_section: New Account Eligibility
        source_text_reference: Section 4.2.1
        confidence: high

  generator_readiness:
    node_red:
      status: mostly_ready
      missing_fields:
        - node_type_mapping
        - visual_layout_strategy
        - reusable_subflow_metadata
      notes: >
        Workflow steps, branching, mappings, services, and errors are defined.
        Node-specific metadata is still needed for direct Node-RED generation.

    excel:
      status: mostly_ready
      missing_fields:
        - exact_sheet_column_mapping
        - row_generation_rules
      notes: >
        API, service, business rule, query, and endpoint mapping data are available.
        Sheet-level formatting rules are still needed.

    go:
      status: partially_ready
      missing_fields:
        - request_dto_names
        - response_dto_names
        - service_method_names
        - repository_method_names
        - client_method_names
        - typed_error_definitions
      notes: >
        Workflow logic is clear, but implementation-level naming and package
        details are still needed before Go generation.

    documentation:
      status: ready
      missing_fields: []
      notes: >
        Schema contains enough information to generate human-readable workflow
        and API documentation.

    tests:
      status: mostly_ready
      missing_fields:
        - more boundary scenarios
        - more database no-record scenarios
        - more downstream timeout scenarios
      notes: >
        Initial test scenario structure exists, but more complete test coverage
        is required for production-grade generation.

    evaluation:
      status: partially_ready
      missing_fields:
        - complete source coverage scoring
        - requirement extraction confidence scoring
      notes: >
        Traceability is included, but evaluation scoring rules still need to be
        defined in future days.
```

---

# Notes About This Sample

This sample structure is intentionally detailed because the Discount API is a complex business workflow.

It demonstrates how Schema v2 can represent:

* API contract
* Request validation
* Workflow steps
* Branching
* External service integration
* Authentication
* Database query references
* Business rules
* Calculation rules
* Request mappings
* Response mappings
* Error mappings
* Test scenarios
* Source traceability
* Generator readiness

---

# How This Structure Supports Future Generators

## Node-RED Generator

The generator can use:

* `workflow_steps` to create ordered nodes
* `branching` to create conditional routing
* `external_services` to create HTTP request nodes
* `request_mappings` to create function or mapping nodes
* `response_mappings` to create response mapper nodes
* `error_mappings` to create error response nodes

## Excel Intake Generator

The generator can use:

* `api_contract` for the api_endpoints sheet
* `external_services` for the external_services sheet
* `business_rules` for the business_requirements sheet
* `database_references` for the queries sheet
* `workflow_steps` and mappings for the endpoint_service_mapping sheet

## Go Code Generator

The generator can use:

* `api_contract` for handler routing
* `field_validations` for validator logic
* `workflow_steps` for service orchestration
* `external_services` and `authentication` for client logic
* `database_references` for repository logic
* `business_rules` and `calculation_rules` for service logic
* `response_mappings` for mapper logic
* `error_mappings` for error handling

## Documentation Generator

The generator can use:

* `metadata` for summary
* `api_contract` for endpoint docs
* `field_validations` for request contract docs
* `workflow_steps` for workflow explanation
* `business_rules` for business logic docs
* `external_services` for integration docs
* `error_mappings` for error behavior docs

## Test Generator

The generator can use:

* `field_validations` for negative validation tests
* `branching` for path coverage tests
* `external_services` for mock downstream responses
* `database_references` for mock DB results
* `test_scenarios` for direct test creation
* `expected.executed_steps` for workflow path verification

---

# Simplified Structure for Wrapper APIs

Schema v2 also supports simple wrapper workflows.

A simple API like the Fabric API may use only these blocks:

```yaml
workflow:
  metadata:
  api_contract:
  field_validations:
  workflow_steps:
  branching:
  external_services:
  authentication:
  request_mappings:
  response_mappings:
  error_mappings:
  test_scenarios:
  source_traceability:
  generator_readiness:
```

It may not need:

```yaml
database_references:
business_rules:
calculation_rules:
```

This flexibility is important because not every enterprise API requires database access or calculation logic.

---

# Final Summary

This sample structure shows how Workflow Schema v2 can represent a complete enterprise API workflow in a structured, generator-friendly format.

Schema v2 is stronger than Schema v1 because it explicitly defines the areas needed by future generators.

The sample confirms that Schema v2 can support:

* Complex business orchestration APIs
* External service integration
* Database-backed rules
* Conditional branching
* Response and error mapping
* Test generation
* Traceability
* Generator readiness analysis

This completes Phase 4 of Day 10.
