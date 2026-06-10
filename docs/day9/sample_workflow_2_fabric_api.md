# Day 9 — Sample Workflow 2: Fabric API

## Purpose

This document validates the Day 8 workflow schema by applying it to a simpler wrapper-style API workflow.

The selected API is the Customize IT Fabric API:

```text
POST /api/v1/fabrics
```

This API was selected because it represents a lightweight service wrapper pattern. Unlike the Discount API, this workflow does not require complex database-backed business rules or multiple calculation branches. It mainly focuses on request validation, request mapping, downstream service invocation, response mapping, and error handling.

No coding is performed in this phase.

---

## API Overview

## API Name

Fabric Customization API

## Endpoint

```text
POST /api/v1/fabrics
```

## Purpose

The purpose of this API is to receive a list of SKU numbers and a fabric type identifier, validate the request, build a downstream service request, call the product customization fabric service, and return a mapped response to the client.

This workflow is useful for validating whether the schema can represent simple service wrapper APIs without forcing unnecessary complexity.

---

## Workflow Metadata

```yaml
workflow_id: WF-FABRIC-001
workflow_name: Fabric Customization Workflow
workflow_description: >
  Validates a fabric customization request, maps the request to the downstream
  product customization service, invokes the external service, maps success
  and error responses, and returns the final API response.
api_endpoint: /api/v1/fabrics
http_method: POST
api_version: v1
workflow_type: service_wrapper
source_domain: Customize IT Fabric
schema_validation_status: draft
```

---

## Request Contract Summary

## Required Headers

The schema should be able to capture the request headers needed by the Fabric API.

```yaml
headers:
  - name: Division-Id
    required: true
    type: integer
    description: Division identifier. Only supported divisions are allowed.

  - name: Trace-Id
    required: false
    type: string
    description: Optional correlation or trace identifier passed to the downstream service when present.
```

---

## Request Body Summary

```yaml
body:
  skuNumbers:
    required: true
    type: array
    item_type: string
    description: List of SKU numbers used for fabric customization lookup.

  fabricTypeId:
    required: true
    type: integer
    description: Fabric type identifier used by the downstream customization service.
```

---

## Validation Requirements

The schema should support the following validation rules:

```yaml
validations:
  - field: headers.Division-Id
    rule: required
    failure_status: 400
    failure_message: Division-Id header is required.

  - field: headers.Division-Id
    rule: allowed_values
    allowed_values:
      - 12
      - 13
    failure_status: 400
    failure_message: Division-Id must be one of the supported values.

  - field: body.skuNumbers
    rule: required
    failure_status: 400
    failure_message: skuNumbers is required.

  - field: body.skuNumbers
    rule: non_empty_array
    failure_status: 400
    failure_message: skuNumbers must contain at least one SKU number.

  - field: body.skuNumbers[]
    rule: non_blank_string
    failure_status: 400
    failure_message: Each SKU number must be a non-empty string.

  - field: body.fabricTypeId
    rule: required
    failure_status: 400
    failure_message: fabricTypeId is required.

  - field: body.fabricTypeId
    rule: integer
    failure_status: 400
    failure_message: fabricTypeId must be an integer.
```

---

## Workflow Step Summary

The Fabric API workflow can be represented as the following high-level sequence:

```text
STEP-001 Validate required headers
STEP-002 Validate Division-Id
STEP-003 Validate request body
STEP-004 Build downstream fabric request
STEP-005 Call fabric customization service
STEP-006 Validate downstream response
STEP-007 Map success response
STEP-008 Map error response
```

---

# Workflow Steps

## STEP-001 — Validate Required Headers

```yaml
step_id: STEP-001
step_name: Validate Required Headers
step_type: validation
description: >
  Validate that required request headers are present before processing
  the fabric customization request.
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

This step validates whether the workflow schema can represent simple header validation.

The schema should support:

* Required header names
* Optional header names
* Header data types
* Validation failure response
* Next step on success

---

## STEP-002 — Validate Division-Id

```yaml
step_id: STEP-002
step_name: Validate Division-Id
step_type: validation
description: >
  Validate that Division-Id is one of the supported values for the Fabric API.
inputs:
  - request.headers.Division-Id
outputs:
  - validatedDivisionId
depends_on:
  - STEP-001
validation_rule:
  field: Division-Id
  allowed_values:
    - 12
    - 13
success_behavior: Continue to STEP-003.
failure_behavior:
  error_code: INVALID_DIVISION
  http_status: 400
  message: Division-Id must be one of the supported values.
```

## Schema Validation Notes

This step validates whether the schema can represent allowed value validation for headers.

The schema should support:

* Header-level allowed values
* Numeric value validation
* Specific validation error messages
* Failure behavior

---

## STEP-003 — Validate Request Body

```yaml
step_id: STEP-003
step_name: Validate Request Body
step_type: validation
description: >
  Validate that skuNumbers and fabricTypeId are present and valid.
inputs:
  - request.body.skuNumbers
  - request.body.fabricTypeId
outputs:
  - validatedBody
depends_on:
  - STEP-002
validation_rules:
  - field: skuNumbers
    rule: required_non_empty_array
  - field: skuNumbers[]
    rule: non_blank_string
  - field: fabricTypeId
    rule: required_integer
success_behavior: Continue to STEP-004.
failure_behavior:
  error_code: BAD_REQUEST
  http_status: 400
  message: Request body is missing required fields or contains invalid values.
```

## Schema Validation Notes

This step validates whether the schema can represent basic body validation.

The schema should support:

* Required body fields
* Array validation
* Array item validation
* Integer validation
* Failure behavior

---

## STEP-004 — Build Downstream Fabric Request

```yaml
step_id: STEP-004
step_name: Build Downstream Fabric Request
step_type: mapping
description: >
  Map the public Fabric API request into the request format required by
  the downstream product customization fabric service.
inputs:
  - request.headers.Division-Id
  - request.headers.Trace-Id
  - request.body.skuNumbers
  - request.body.fabricTypeId
outputs:
  - downstreamFabricRequest
depends_on:
  - STEP-003
request_mapping:
  mappings:
    - source: request.body.skuNumbers
      target: downstreamRequest.customSkuNumbers
    - source: request.body.fabricTypeId
      target: downstreamRequest.fabricTypeId
    - source: request.headers.Division-Id
      target: downstreamHeaders.Division-Id
    - source: request.headers.Trace-Id
      target: downstreamHeaders.Trace-Id
      required: false
success_behavior: Continue to STEP-005.
failure_behavior:
  error_code: INTERNAL_MAPPING_ERROR
  http_status: 500
  message: Failed to build downstream fabric service request.
```

## Mapping Notes

The public request body uses:

```json
{
  "skuNumbers": ["SKU123", "SKU456"],
  "fabricTypeId": 3
}
```

The downstream request uses a service-specific field name for SKUs:

```json
{
  "customSkuNumbers": ["SKU123", "SKU456"],
  "fabricTypeId": 3
}
```

The schema should support this source-to-target field mapping clearly.

---

## STEP-005 — Call Fabric Customization Service

```yaml
step_id: STEP-005
step_name: Call Fabric Customization Service
step_type: external_service
description: >
  Invoke the downstream product customization fabric service using the
  mapped request.
inputs:
  - downstreamFabricRequest
outputs:
  - downstreamFabricResponse
depends_on:
  - STEP-004
external_service:
  service_id: SRV-FABRIC-CUSTOMIZATION
  service_name: Product Customization Fabric Service
  protocol: REST
  method: POST
  endpoint_reference: /productcustomization/v1/config/fabrics
  authentication:
    type: service_config
    description: Authentication and runtime headers are managed by service configuration.
  timeout_required: true
  retry_required: false
success_behavior: Continue to STEP-006.
failure_behavior:
  error_code: DOWNSTREAM_SERVICE_ERROR
  http_status: 502
  message: Fabric customization service failed.
```

## Schema Validation Notes

This step validates whether the schema can represent a simple external REST service call.

The schema should support:

* Service ID
* Service name
* Protocol
* HTTP method
* Endpoint reference
* Request input
* Response output
* Timeout behavior
* Downstream error behavior

---

## STEP-006 — Validate Downstream Response

```yaml
step_id: STEP-006
step_name: Validate Downstream Response
step_type: validation
description: >
  Validate whether the downstream fabric customization service returned
  a success response or an error response.
inputs:
  - downstreamFabricResponse
outputs:
  - responseClassification
depends_on:
  - STEP-005
branching:
  condition: downstreamFabricResponse.statusCode is success
  true_path: STEP-007
  false_path: STEP-008
success_behavior: Route to success response mapping or error response mapping.
failure_behavior:
  error_code: DOWNSTREAM_RESPONSE_VALIDATION_FAILED
  http_status: 502
  message: Unable to classify downstream fabric response.
```

## Schema Validation Notes

This step validates whether the schema can represent simple success/error branching.

The schema should support:

* Response status classification
* Success path
* Error path
* Branching after external service response

---

## STEP-007 — Map Success Response

```yaml
step_id: STEP-007
step_name: Map Success Response
step_type: response_mapping
description: >
  Map the downstream successful fabric response into the public API response.
inputs:
  - downstreamFabricResponse
outputs:
  - finalApiResponse
depends_on:
  - STEP-006
condition: responseClassification == success
response_mapping:
  http_status: 200
  mappings:
    - source: downstreamFabricResponse.body
      target: response.body
success_behavior: Return final API success response.
failure_behavior:
  error_code: RESPONSE_MAPPING_FAILED
  http_status: 500
  message: Failed to map fabric success response.
```

## Schema Validation Notes

This step validates whether the schema can represent straightforward success response mapping.

The schema should support:

* Downstream response source
* Public API response target
* HTTP status
* Mapping failure behavior

---

## STEP-008 — Map Error Response

```yaml
step_id: STEP-008
step_name: Map Error Response
step_type: error_mapping
description: >
  Map downstream fabric service errors into standardized public API errors.
inputs:
  - downstreamFabricResponse
outputs:
  - errorApiResponse
depends_on:
  - STEP-006
condition: responseClassification == error
error_mapping:
  mappings:
    - source: downstreamFabricResponse.statusCode
      target: response.httpStatus
    - source: downstreamFabricResponse.errorCode
      target: response.code
    - source: downstreamFabricResponse.errorDesc
      target: response.message
  default_http_status: 400
success_behavior: Return final API error response.
failure_behavior:
  error_code: ERROR_MAPPING_FAILED
  http_status: 500
  message: Failed to map fabric error response.
```

## Error Mapping Notes

An example downstream error may contain:

```json
{
  "errorCode": "NO_VALID_FABRIC_TYPE_FOUND",
  "errorDesc": "fabricTypeId is inactive or invalid for the requested SKU combination."
}
```

The public API should preserve the meaningful downstream error information while returning it in a consistent structure.

---

# External Service References

## SRV-FABRIC-CUSTOMIZATION

```yaml
service_id: SRV-FABRIC-CUSTOMIZATION
service_name: Product Customization Fabric Service
service_type: REST
purpose: Return fabric configuration details for the provided SKU numbers and fabric type.
method: POST
endpoint_reference: /productcustomization/v1/config/fabrics
request_source:
  - request.headers.Division-Id
  - request.headers.Trace-Id
  - request.body.skuNumbers
  - request.body.fabricTypeId
request_mapping:
  - source: request.body.skuNumbers
    target: downstreamRequest.customSkuNumbers
  - source: request.body.fabricTypeId
    target: downstreamRequest.fabricTypeId
response_used_by:
  - STEP-006
  - STEP-007
  - STEP-008
error_behavior:
  timeout: Return downstream service error.
  invalid_fabric_type: Return mapped downstream validation error.
  invalid_response: Return downstream invalid response error.
```

---

# Business Rules Required by Workflow

The Fabric API uses simple validation and wrapper rules rather than deep business calculation logic.

```yaml
business_rules:
  - rule_id: BR-FABRIC-001
    name: Division Validation
    category: validation
    description: Division-Id must be present and must be one of the supported divisions.

  - rule_id: BR-FABRIC-002
    name: SKU Number Validation
    category: validation
    description: skuNumbers must be present, must be an array, and must contain at least one non-blank string.

  - rule_id: BR-FABRIC-003
    name: Fabric Type Validation
    category: validation
    description: fabricTypeId must be present and must be an integer.

  - rule_id: BR-FABRIC-004
    name: Downstream Request Mapping
    category: mapping
    description: Public skuNumbers must be mapped to downstream customSkuNumbers.

  - rule_id: BR-FABRIC-005
    name: Downstream Response Mapping
    category: response_mapping
    description: Downstream success and error responses must be mapped to public API responses.
```

---

# Conditional Branching Representation

The Fabric API does not have complex business branching like the Discount API.

However, it still has a simple response classification branch after the downstream service call.

## Branch 1 — Success Response Path

```yaml
branch_id: BRANCH-FABRIC-001
condition: downstreamFabricResponse.statusCode is success
path_name: Fabric Success Response Path
start_step: STEP-007
merge_step: none
```

## Branch 2 — Error Response Path

```yaml
branch_id: BRANCH-FABRIC-002
condition: downstreamFabricResponse.statusCode is error
path_name: Fabric Error Response Path
start_step: STEP-008
merge_step: none
```

## Branching Validation Result

This sample confirms that even simple wrapper APIs need lightweight branching support.

The schema should support:

* Success path
* Error path
* No merge step when the branch directly returns a response
* Branch condition based on downstream status

---

# Response Mapping Representation

## Success Response Concept

```yaml
success_response:
  http_status: 200
  body:
    source: downstreamFabricResponse.body
    target: response.body
```

## Error Response Concept

```yaml
error_response:
  http_status:
    source: downstreamFabricResponse.statusCode
    default: 400
  body:
    code:
      source: downstreamFabricResponse.errorCode
      type: string
    message:
      source: downstreamFabricResponse.errorDesc
      type: string
```

---

# Schema Validation Findings from Fabric API

## Strengths Confirmed

The workflow schema can represent:

* API-level metadata
* Required header validation
* Optional trace header handling
* Required body field validation
* Array validation
* Integer validation
* Request mapping
* External REST service invocation
* Success response mapping
* Error response mapping
* Simple response branching

---

## Gaps Identified

The Fabric API reveals several schema improvement areas.

## Gap 1 — Array Item Validation Should Be Explicit

The `skuNumbers` field is an array of strings.

The schema should support validation at both the array level and item level.

Required support:

* Array is required
* Array must be non-empty
* Each item must be a string
* Each item must be non-blank

## Gap 2 — Optional Header Pass-Through Should Be Supported

The `Trace-Id` header is optional but should be passed to the downstream service when present.

The schema should support optional pass-through mapping.

## Gap 3 — Wrapper APIs Need Lightweight Service Mapping

Not every API needs complex business rules or database queries.

The schema should allow a simple service-wrapper workflow without forcing unused database or calculation sections.

## Gap 4 — Downstream Error Preservation Should Be Standardized

The downstream service may return specific error codes and descriptions.

The schema should define how much of the downstream error is preserved versus transformed.

## Gap 5 — Success and Error Branches May Return Directly

The Fabric API has response branches that directly return to the client.

The schema should support branches with no merge step.

---

# Generator Readiness Validation

## Node-RED Generator Readiness

The workflow schema contains enough information to generate the following Node-RED nodes conceptually:

* HTTP In node
* Header validation node
* Division validation node
* Body validation node
* Request mapping node
* REST client node
* Response classification node
* Success response mapper node
* Error response mapper node
* HTTP Response node

Status:

```text
Mostly ready
```

Reason:

The workflow is simple and sequential. The schema is mostly sufficient, but array item validation and optional header pass-through should be represented more clearly.

---

## Excel Intake Generator Readiness

The workflow schema contains enough information to populate these Excel sheets conceptually:

* api_endpoints
* external_services
* business_requirements
* endpoint_service_mapping

The workflow does not require database tables or query rows unless future requirements add persistence.

Status:

```text
Ready with minor improvements
```

Reason:

The API is a wrapper flow, so it maps naturally to endpoint, service, business rule, and endpoint-service mapping sheets.

---

## Go Code Generator Readiness

The workflow schema contains enough information to generate conceptual Go layers:

* Handler
* DTO
* Validator
* Service
* Client
* Mapper
* Error handler

Status:

```text
Mostly ready
```

Reason:

The workflow is simple enough for code generation if request DTOs, response DTOs, and error structures are standardized.

---

## Documentation Generator Readiness

The workflow schema contains enough information to generate API documentation.

Status:

```text
Ready
```

Reason:

The schema includes endpoint, request contract, validation rules, downstream service details, response mapping, and error behavior.

---

## Test Case Generator Readiness

The workflow schema contains partial information for test generation.

Status:

```text
Partially ready
```

Reason:

The schema can support obvious tests, but it should explicitly capture test scenarios.

Recommended test scenarios:

```yaml
test_scenarios:
  - scenario_id: TC-FABRIC-001
    name: Valid fabric request
    expected_status: 200

  - scenario_id: TC-FABRIC-002
    name: Missing Division-Id header
    expected_status: 400

  - scenario_id: TC-FABRIC-003
    name: Unsupported Division-Id
    expected_status: 400

  - scenario_id: TC-FABRIC-004
    name: Missing skuNumbers
    expected_status: 400

  - scenario_id: TC-FABRIC-005
    name: Empty skuNumbers array
    expected_status: 400

  - scenario_id: TC-FABRIC-006
    name: Missing fabricTypeId
    expected_status: 400

  - scenario_id: TC-FABRIC-007
    name: Invalid downstream fabric type
    expected_status: 400

  - scenario_id: TC-FABRIC-008
    name: Downstream service unavailable
    expected_status: 502
```

---

# Comparison with Discount API Workflow

The Fabric API validates a different workflow shape than the Discount API.

## Discount API

```text
Complex orchestration
External service call
Database lookups
Business branching
Eligibility checks
Exclusion checks
Discount calculation
Complex response mapping
```

## Fabric API

```text
Simple wrapper
Header validation
Body validation
Request mapping
External service call
Success/error response mapping
No database
No internal calculation
```

Together, both APIs prove that the schema must support both complex business orchestration and simple wrapper-style APIs.

---

# Validation Conclusion

The Fabric API confirms that the Day 8 workflow schema can represent simple service wrapper workflows.

The schema successfully captures request validation, optional header pass-through, downstream request mapping, external REST service invocation, success response mapping, and error response mapping.

This sample also shows that the schema should remain flexible. It should not force every workflow to include database steps, complex business rules, or calculations.

The most important improvements identified from this sample are:

1. Add explicit array item validation support.
2. Add optional header pass-through mapping support.
3. Allow lightweight wrapper workflows without database sections.
4. Standardize downstream error preservation.
5. Support response branches that return directly without a merge step.

This sample confirms that the Day 8 schema is useful for simpler APIs, but it should be refined so that wrapper-style APIs can be represented cleanly and without unnecessary fields.
