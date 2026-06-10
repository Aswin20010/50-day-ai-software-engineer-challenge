# Day 9 — Workflow Schema Validation Plan

## Purpose

This document defines the validation plan for the workflow schema created on Day 8 of the 50-Day AI Software Engineer Challenge.

The workflow schema from Day 8 describes how API requirements can be stored as structured workflow steps. Day 9 validates whether that schema is practical, complete, and usable for future artifact generation.

No coding is performed on Day 9.

---

## Background

On Day 8, a workflow schema was defined to act as a contract between the requirement extraction phase and future generation phases.

The schema is expected to support multiple downstream generators, including:

* Node-RED flow generators
* Excel intake generators
* Go code generators
* Documentation generators
* Test case generators
* Evaluation scripts

Before using the schema for generation, it must be validated against realistic API workflows.

Day 9 focuses on applying the schema to sample APIs and identifying whether the schema can represent real enterprise API behavior.

---

## Validation Objective

The objective of schema validation is to confirm that the workflow schema can represent API workflows clearly and consistently.

The validation should answer the following questions:

1. Can the schema describe workflow-level metadata?
2. Can the schema represent API endpoint details?
3. Can the schema capture required request headers?
4. Can the schema capture request body fields?
5. Can the schema represent validation rules?
6. Can the schema represent business rules?
7. Can the schema model external service calls?
8. Can the schema describe database query usage?
9. Can the schema represent conditional workflow paths?
10. Can the schema describe final response mapping?
11. Can the schema describe error handling?
12. Can the schema support future artifact generators?

---

## APIs Selected for Validation

Two sample APIs will be used to validate the schema.

---

## Sample API 1 — Discount API

### Endpoint

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

### Reason for Selection

The Discount API represents a complex enterprise workflow.

It includes:

* Header validation
* Request body validation
* External account interrogation
* Database lookups
* Employee discount rules
* New account discount rules
* Exclusion checks
* Conditional branching
* Discount calculation
* Final response mapping

This API is useful for testing whether the schema can handle complex business logic and multi-step workflow orchestration.

---

## Sample API 2 — Fabric API

### Endpoint

```text
POST /api/v1/fabrics
```

### Reason for Selection

The Fabric API represents a simpler wrapper-style API.

It includes:

* Header validation
* Request body validation
* Downstream request mapping
* External REST service call
* Success response mapping
* Error response mapping

This API is useful for testing whether the schema can also represent simple service wrapper workflows without unnecessary complexity.

---

## Validation Scope

The validation will focus only on schema representation.

The following activities are in scope:

* Reviewing whether required workflow fields are present
* Mapping sample API behavior into workflow steps
* Checking whether step inputs and outputs are clear
* Checking whether dependencies between steps are clear
* Checking whether external services can be represented
* Checking whether database queries can be referenced
* Checking whether response mapping is clear
* Identifying schema gaps and improvement areas

---

## Out of Scope

The following activities are not part of Day 9:

* Writing implementation code
* Generating Node-RED flows
* Generating Go code
* Creating Excel files
* Running automated tests
* Building a working API
* Connecting to databases
* Calling external services

Day 9 is limited to documentation, schema validation, and design review.

---

## Validation Method

The workflow schema will be validated using a manual documentation-based approach.

The process will follow these steps:

1. Select realistic sample APIs.
2. Break each API into workflow steps.
3. Represent each step using the Day 8 workflow schema structure.
4. Check whether the schema can capture all required information.
5. Mark fields that are missing, unclear, or difficult to represent.
6. Compare both sample workflows.
7. Identify common schema strengths.
8. Identify schema gaps.
9. Recommend improvements for future days.

---

## Validation Criteria

The schema will be considered valid if it can represent the following areas.

---

## 1. Workflow Metadata

The schema should capture high-level workflow information.

Required metadata includes:

* Workflow ID
* Workflow name
* Workflow description
* API endpoint
* HTTP method
* API version
* Source requirement reference
* Target generator type
* Status

Example:

```text
workflow_id: WF-DISCOUNT-001
workflow_name: Evaluate Transaction Discount Workflow
endpoint: /api/v1/discount/evaluateTransactionDiscount
method: POST
version: v1
status: draft
```

---

## 2. Request Input Definition

The schema should capture request inputs clearly.

This includes:

* Headers
* Path parameters
* Query parameters
* Request body fields
* Required fields
* Optional fields
* Data types
* Allowed values
* Default values, if applicable

Example validation questions:

* Are required headers represented?
* Are required body fields represented?
* Are enum values represented?
* Are numeric fields represented?
* Are optional fields clearly marked?

---

## 3. Validation Rules

The schema should represent request validation rules.

Validation rules may include:

* Mandatory field checks
* Data type checks
* Enum validation
* Numeric validation
* String format validation
* Date/time validation
* Conditional validation

Example validation steps:

```text
Validate required headers
Validate request body
Validate enum fields
Validate numeric fields
Validate timestamp fields
```

The schema should show:

* What is being validated
* Validation condition
* Failure behavior
* Error response mapping

---

## 4. Workflow Steps

The schema should represent the API workflow as a sequence of steps.

Each step should include:

* Step ID
* Step name
* Step type
* Description
* Inputs
* Outputs
* Dependencies
* Success behavior
* Failure behavior

Example step types:

```text
validation
mapping
external_service
database_query
business_rule
calculation
response_mapping
error_mapping
```

Each step should be clear enough for future generators to understand what action is expected.

---

## 5. Step Dependencies

The schema should define how steps connect to each other.

This includes:

* Sequential dependencies
* Conditional dependencies
* Branching logic
* Merge points
* Failure paths

Example:

```text
STEP-003 depends on STEP-002
STEP-005 runs only if employeeFlag is true
STEP-006 runs only if employeeFlag is false
STEP-009 merges discount calculation results
```

The validation should check whether conditional paths can be represented without ambiguity.

---

## 6. External Service Calls

The schema should support external REST or SOAP service calls.

For each external service step, the schema should capture:

* Service ID
* Service name
* Protocol
* Endpoint
* HTTP method or SOAP operation
* Request headers
* Request body mapping
* Authentication requirements
* Response mapping
* Timeout behavior
* Retry behavior
* Error handling

Example external service:

```text
service_id: SRV-INTERROGATE-ACCOUNT
service_name: Interrogate Account Service
protocol: REST
method: POST
purpose: Resolve account information before discount calculation
```

---

## 7. Database Query Usage

The schema should represent database-related steps.

For each database step, the schema should capture:

* Database type
* Table name
* Query ID
* Query purpose
* Query inputs
* Query outputs
* Success behavior
* No-record behavior
* Failure behavior

Example:

```text
query_id: SQL-DISCOUNT-001
purpose: Check employee discount profile by division, department, vendor, and class
inputs: division, department, vendor, class
outputs: discountProfileName
```

The schema should not require actual SQL implementation at this stage, but it should provide enough metadata for future SQL generation.

---

## 8. Business Rule Representation

The schema should capture business rules in a structured way.

Business rules may include:

* Eligibility checks
* Exclusion checks
* Discount percentage rules
* Cap amount rules
* Date-based rules
* Priority rules
* Conditional rules

Each business rule should include:

* Business rule ID
* Business rule name
* Description
* Condition
* Inputs
* Outputs
* Success behavior
* Failure behavior

Example:

```text
business_rule_id: BR-DISCOUNT-001
name: Employee Discount Eligibility Check
condition: employeeFlag is true and account is eligible
output: employeeDiscountEligible
```

---

## 9. Conditional Branching

The schema should support branching workflows.

This is especially important for the Discount API.

Examples:

```text
If account is employee account, execute employee discount path.
If account is new account, execute new account discount path.
If exclusion exists, skip discount.
If no exclusion exists, continue discount calculation.
```

The schema should make branch logic readable and generator-friendly.

Required branching details:

* Branch condition
* True path
* False path
* Next step after branch
* Merge point after branch

---

## 10. Response Mapping

The schema should describe how final responses are created.

Response mapping should include:

* Source fields
* Target response fields
* HTTP status code
* Success response format
* Error response format
* Default values, if allowed
* Transformation rules

Example:

```text
source: calculatedDiscount.discountAmount
target: response.discountAmount
```

The schema should make it possible for future generators to build response DTOs and mapping logic.

---

## 11. Error Handling

The schema should represent error behavior consistently.

Error handling should include:

* Validation errors
* External service errors
* Database errors
* Business rule failures
* Mapping errors
* Unexpected system errors

Each error mapping should capture:

* Error source
* Error condition
* Internal error code
* External error message
* HTTP status code
* Retryable flag
* Fail-fast behavior

Example:

```text
error_source: validation
condition: missing required header
http_status: 400
error_code: BAD_REQUEST
message: Required header is missing
```

---

## 12. Generator Readiness

The schema should be evaluated for future generator compatibility.

The validation should check whether the schema can support:

### Node-RED Flow Generation

The schema should define enough information to create:

* HTTP In node
* Validation nodes
* Mapping nodes
* External service nodes
* Database nodes
* Business rule nodes
* HTTP Response node

### Excel Intake Generation

The schema should define enough information to populate:

* api_endpoints
* entities
* external_services
* business_requirements
* endpoint_service_mapping
* queries

### Go Code Generation

The schema should define enough information to create:

* Handler layer
* DTO layer
* Validation layer
* Service layer
* Repository layer
* Client layer
* Mapper layer
* Error handling layer

### Documentation Generation

The schema should define enough information to generate:

* API summary
* Request contract
* Response contract
* Business rule documentation
* External service documentation
* Database query documentation

### Test Case Generation

The schema should define enough information to generate:

* Positive test cases
* Negative validation tests
* Business rule tests
* External service failure tests
* Database failure tests
* Response mapping tests

---

## Expected Validation Outputs

Day 9 validation should produce the following outputs:

```text
docs/day9/
├── workflow_schema_validation_plan.md
├── sample_workflow_1_discount_api.md
├── sample_workflow_2_fabric_api.md
├── schema_validation_checklist.md
├── schema_gap_analysis.md
└── day9_completion_report.md
```

This file represents the validation plan.

The remaining files will apply this plan to sample workflows and document the results.

---

## Success Criteria

Day 9 will be considered successful if:

* The schema validation plan is documented.
* At least two sample APIs are selected for validation.
* The validation criteria are clearly defined.
* The schema is checked against both a complex workflow and a simple workflow.
* Schema strengths are identified.
* Schema gaps are identified.
* Future improvement recommendations are documented.
* No implementation or coding is performed.

---

## Risks

The following risks should be considered during validation:

1. The schema may be too generic and not detailed enough for generators.
2. The schema may not represent conditional branching clearly.
3. Error handling may be inconsistent across APIs.
4. External service authentication details may be missing.
5. Database query references may not be strongly linked to workflow steps.
6. Test generation may require additional metadata not included in the original schema.

---

## Mitigation Plan

To reduce these risks:

* Use both complex and simple API examples.
* Record every field that is difficult to represent.
* Keep a separate gap analysis document.
* Avoid changing the schema too early without evidence.
* Use the validation checklist consistently.
* Focus on future generator readiness.

---

## Final Notes

The purpose of this validation plan is not to prove that the schema is perfect.

The purpose is to discover whether the schema is strong enough to support realistic enterprise API workflows and identify what must be improved before implementation begins.

Day 9 acts as an important checkpoint before moving toward artifact generation design in future days.
