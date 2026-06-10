# Day 9 — Schema Validation Checklist

## Purpose

This document provides a checklist for validating the Day 8 workflow schema against sample API workflows.

The checklist is used to confirm whether the workflow schema can represent real API workflows in a structured, consistent, and generator-ready format.

This checklist will be applied to the sample workflows created on Day 9:

1. FEED Discount API
2. Customize IT Fabric API

No coding is performed in this phase.

---

## How to Use This Checklist

Use this checklist when reviewing any workflow represented using the Day 8 workflow schema.

For each section:

* Mark the item as complete if the schema can represent that information clearly.
* Mark the item as incomplete if the schema cannot represent that information.
* Add notes when a field is unclear, missing, duplicated, or difficult to map.
* Use the findings to update the schema gap analysis document.

---

# 1. Workflow Metadata Checklist

The workflow schema should capture high-level information about the API workflow.

| Validation Item                          | Status | Notes                                            |
| ---------------------------------------- | ------ | ------------------------------------------------ |
| Workflow ID is present                   | [ ]    |                                                  |
| Workflow name is present                 | [ ]    |                                                  |
| Workflow description is present          | [ ]    |                                                  |
| API endpoint is defined                  | [ ]    |                                                  |
| HTTP method is defined                   | [ ]    |                                                  |
| API version is defined                   | [ ]    |                                                  |
| Workflow type is defined                 | [ ]    | Example: service_wrapper, business_orchestration |
| Source domain is defined                 | [ ]    | Example: FEED Discount, Customize IT Fabric      |
| Schema validation status is defined      | [ ]    | Example: draft, reviewed, complete               |
| Source requirement reference is captured | [ ]    | Useful for traceability                          |
| Target generator type is captured        | [ ]    | Example: Node-RED, Excel, Go, docs, tests        |

---

# 2. API Endpoint Checklist

The schema should clearly describe the API endpoint that starts the workflow.

| Validation Item                      | Status | Notes                                   |
| ------------------------------------ | ------ | --------------------------------------- |
| Endpoint path is captured            | [ ]    |                                         |
| HTTP method is captured              | [ ]    |                                         |
| Controller or route name is captured | [ ]    | Optional but useful for code generation |
| API version is captured              | [ ]    |                                         |
| Request content type is captured     | [ ]    | Example: application/json               |
| Response content type is captured    | [ ]    | Example: application/json               |
| Success HTTP status is captured      | [ ]    |                                         |
| Error HTTP statuses are captured     | [ ]    |                                         |
| Endpoint purpose is described        | [ ]    |                                         |

---

# 3. Header Validation Checklist

The schema should represent required and optional request headers.

| Validation Item                            | Status | Notes                                            |
| ------------------------------------------ | ------ | ------------------------------------------------ |
| Required headers are listed                | [ ]    |                                                  |
| Optional headers are listed                | [ ]    |                                                  |
| Header names match source requirements     | [ ]    |                                                  |
| Header data types are defined              | [ ]    |                                                  |
| Header descriptions are provided           | [ ]    |                                                  |
| Header allowed values are captured         | [ ]    | Example: Division-Id allowed values              |
| Header format rules are captured           | [ ]    | Example: numeric, UUID, string                   |
| Missing header failure behavior is defined | [ ]    |                                                  |
| Invalid header failure behavior is defined | [ ]    |                                                  |
| Optional header pass-through is supported  | [ ]    | Example: Trace-Id passed downstream when present |

---

# 4. Request Body Checklist

The schema should describe request body fields clearly.

| Validation Item                                | Status | Notes                                  |
| ---------------------------------------------- | ------ | -------------------------------------- |
| Required body fields are listed                | [ ]    |                                        |
| Optional body fields are listed                | [ ]    |                                        |
| Nested body objects are supported              | [ ]    | Example: account.encryptInfo           |
| Array fields are supported                     | [ ]    | Example: skuNumbers, transaction items |
| Array item types are defined                   | [ ]    | Example: string array                  |
| Array item validation is supported             | [ ]    | Example: non-blank item validation     |
| Field data types are defined                   | [ ]    |                                        |
| Field descriptions are provided                | [ ]    |                                        |
| Default values are captured where applicable   | [ ]    |                                        |
| Missing body field failure behavior is defined | [ ]    |                                        |
| Invalid body field failure behavior is defined | [ ]    |                                        |

---

# 5. Enum and Allowed Value Checklist

The schema should capture fields with restricted allowed values.

| Validation Item                             | Status | Notes    |
| ------------------------------------------- | ------ | -------- |
| Enum fields are identified                  | [ ]    |          |
| Allowed values are listed                   | [ ]    |          |
| Enum validation failure behavior is defined | [ ]    |          |
| Header allowed values are supported         | [ ]    |          |
| Body allowed values are supported           | [ ]    |          |
| Case-sensitivity rules are captured         | [ ]    | Optional |
| Unknown value behavior is captured          | [ ]    |          |

---

# 6. Workflow Step Checklist

Each workflow step should be represented clearly and consistently.

| Validation Item                                     | Status | Notes                                          |
| --------------------------------------------------- | ------ | ---------------------------------------------- |
| Each step has a unique step ID                      | [ ]    |                                                |
| Each step has a step name                           | [ ]    |                                                |
| Each step has a step type                           | [ ]    | Example: validation, mapping, external_service |
| Each step has a description                         | [ ]    |                                                |
| Step inputs are defined                             | [ ]    |                                                |
| Step outputs are defined                            | [ ]    |                                                |
| Step dependencies are defined                       | [ ]    |                                                |
| Success behavior is defined                         | [ ]    |                                                |
| Failure behavior is defined                         | [ ]    |                                                |
| Step execution condition is defined when needed     | [ ]    | Example: employee path only                    |
| Step produces reusable workflow context when needed | [ ]    | Example: discountDbContext                     |

---

# 7. Step Type Checklist

The schema should support the major step types required by enterprise API workflows.

| Step Type                | Supported | Notes |
| ------------------------ | --------- | ----- |
| Validation step          | [ ]       |       |
| Mapping step             | [ ]       |       |
| External service step    | [ ]       |       |
| Database query step      | [ ]       |       |
| Business rule step       | [ ]       |       |
| Calculation step         | [ ]       |       |
| Response mapping step    | [ ]       |       |
| Error mapping step       | [ ]       |       |
| Branching step           | [ ]       |       |
| Context preparation step | [ ]       |       |

---

# 8. Step Dependency Checklist

The schema should describe how workflow steps connect to each other.

| Validation Item                           | Status | Notes                                                        |
| ----------------------------------------- | ------ | ------------------------------------------------------------ |
| Sequential dependencies are represented   | [ ]    |                                                              |
| Steps can depend on previous step outputs | [ ]    |                                                              |
| Multiple dependencies are supported       | [ ]    | Example: exclusion step depends on employee/new account path |
| Conditional dependencies are supported    | [ ]    |                                                              |
| Failure path dependencies are supported   | [ ]    |                                                              |
| Terminal steps are clearly marked         | [ ]    | Example: final response return                               |
| Reusable intermediate outputs are named   | [ ]    |                                                              |

---

# 9. Conditional Branching Checklist

The schema should support branching logic.

| Validation Item                               | Status | Notes                                              |
| --------------------------------------------- | ------ | -------------------------------------------------- |
| Branch condition is captured                  | [ ]    |                                                    |
| True path is captured                         | [ ]    |                                                    |
| False path is captured                        | [ ]    |                                                    |
| Branch start step is captured                 | [ ]    |                                                    |
| Branch merge step is captured when applicable | [ ]    |                                                    |
| Branches without merge steps are supported    | [ ]    | Example: success/error branches returning directly |
| Branch-specific outputs are captured          | [ ]    |                                                    |
| Branch-specific failure behavior is captured  | [ ]    |                                                    |
| Multiple branch paths are supported           | [ ]    | Useful for future complex workflows                |

---

# 10. External Service Checklist

The schema should describe external REST or SOAP service calls.

| Validation Item                                            | Status | Notes                         |
| ---------------------------------------------------------- | ------ | ----------------------------- |
| External service ID is present                             | [ ]    |                               |
| External service name is present                           | [ ]    |                               |
| Service purpose is described                               | [ ]    |                               |
| Service protocol is defined                                | [ ]    | Example: REST, SOAP           |
| Service method or operation is defined                     | [ ]    | Example: POST, SOAP operation |
| Endpoint reference is captured                             | [ ]    |                               |
| Request headers are mapped                                 | [ ]    |                               |
| Request body is mapped                                     | [ ]    |                               |
| Optional request fields are handled                        | [ ]    |                               |
| Authentication metadata is captured                        | [ ]    | Example: API key, HMAC        |
| Timeout behavior is captured                               | [ ]    |                               |
| Retry behavior is captured                                 | [ ]    |                               |
| Downstream success response is captured                    | [ ]    |                               |
| Downstream error response is captured                      | [ ]    |                               |
| Downstream response fields used by workflow are identified | [ ]    |                               |

---

# 11. Database Query Checklist

The schema should support database-backed workflows.

| Validation Item                                  | Status | Notes               |
| ------------------------------------------------ | ------ | ------------------- |
| Database type is captured                        | [ ]    | Example: PostgreSQL |
| Table name is captured                           | [ ]    |                     |
| Query ID is captured                             | [ ]    |                     |
| Query purpose is described                       | [ ]    |                     |
| Query inputs are listed                          | [ ]    |                     |
| Query outputs are listed                         | [ ]    |                     |
| Query is linked to workflow step                 | [ ]    |                     |
| No-record behavior is captured                   | [ ]    |                     |
| Query failure behavior is captured               | [ ]    |                     |
| Multiple query references per step are supported | [ ]    |                     |
| Query priority or order is captured when needed  | [ ]    |                     |

---

# 12. Business Rule Checklist

The schema should capture business logic clearly.

| Validation Item                                         | Status | Notes                                    |
| ------------------------------------------------------- | ------ | ---------------------------------------- |
| Business rule ID is present                             | [ ]    |                                          |
| Business rule name is present                           | [ ]    |                                          |
| Business rule category is defined                       | [ ]    | Example: validation, discount, exclusion |
| Business rule description is present                    | [ ]    |                                          |
| Business rule condition is captured                     | [ ]    |                                          |
| Business rule inputs are listed                         | [ ]    |                                          |
| Business rule outputs are listed                        | [ ]    |                                          |
| Business rule success behavior is captured              | [ ]    |                                          |
| Business rule failure behavior is captured              | [ ]    |                                          |
| Business rule is linked to workflow step                | [ ]    |                                          |
| Business rule is linked to query references when needed | [ ]    |                                          |

---

# 13. Calculation Checklist

The schema should represent calculation-oriented logic when required.

| Validation Item                             | Status | Notes                        |
| ------------------------------------------- | ------ | ---------------------------- |
| Calculation step is identified              | [ ]    |                              |
| Calculation inputs are listed               | [ ]    |                              |
| Calculation outputs are listed              | [ ]    |                              |
| Calculation rules are described             | [ ]    |                              |
| Percentage-based calculations are supported | [ ]    | Example: discount percentage |
| Cap amount calculations are supported       | [ ]    | Example: new account cap     |
| Item-level calculations are supported       | [ ]    |                              |
| Exclusion-aware calculations are supported  | [ ]    |                              |
| Calculation failure behavior is defined     | [ ]    |                              |

---

# 14. Response Mapping Checklist

The schema should capture final API response mapping.

| Validation Item                                     | Status | Notes |
| --------------------------------------------------- | ------ | ----- |
| Success response status is captured                 | [ ]    |       |
| Success response body structure is captured         | [ ]    |       |
| Source fields are mapped to target fields           | [ ]    |       |
| Response field data types are captured              | [ ]    |       |
| Optional response fields are supported              | [ ]    |       |
| Response transformation rules are captured          | [ ]    |       |
| Response mapping step is linked to workflow outputs | [ ]    |       |
| Response mapping failure behavior is defined        | [ ]    |       |
| Final response return step is clearly marked        | [ ]    |       |

---

# 15. Error Handling Checklist

The schema should standardize workflow error behavior.

| Validation Item                                   | Status | Notes |
| ------------------------------------------------- | ------ | ----- |
| Validation errors are captured                    | [ ]    |       |
| External service errors are captured              | [ ]    |       |
| Database errors are captured                      | [ ]    |       |
| Business rule failures are captured               | [ ]    |       |
| Calculation errors are captured                   | [ ]    |       |
| Mapping errors are captured                       | [ ]    |       |
| Unexpected errors are captured                    | [ ]    |       |
| HTTP status is mapped for each error              | [ ]    |       |
| Error code is mapped for each error               | [ ]    |       |
| Error message is mapped for each error            | [ ]    |       |
| Retryable flag is captured                        | [ ]    |       |
| Fail-fast behavior is captured                    | [ ]    |       |
| Downstream errors can be preserved or transformed | [ ]    |       |

---

# 16. Intermediate Context Checklist

The schema should support intermediate workflow context objects.

| Validation Item                                 | Status | Notes                      |
| ----------------------------------------------- | ------ | -------------------------- |
| Intermediate context objects are supported      | [ ]    | Example: discountDbContext |
| Context inputs are defined                      | [ ]    |                            |
| Context outputs are defined                     | [ ]    |                            |
| Context is reusable across later steps          | [ ]    |                            |
| Context is linked to source request fields      | [ ]    |                            |
| Context is linked to downstream response fields | [ ]    |                            |
| Context failure behavior is defined             | [ ]    |                            |

---

# 17. Generator Readiness Checklist

The schema should support future artifact generation.

## Node-RED Generator

| Validation Item                                    | Status | Notes |
| -------------------------------------------------- | ------ | ----- |
| HTTP In node information is available              | [ ]    |       |
| Validation node information is available           | [ ]    |       |
| Mapping node information is available              | [ ]    |       |
| External service node information is available     | [ ]    |       |
| Database node information is available when needed | [ ]    |       |
| Business rule node information is available        | [ ]    |       |
| Branching node information is available            | [ ]    |       |
| HTTP Response node information is available        | [ ]    |       |
| Error handling node information is available       | [ ]    |       |

## Excel Intake Generator

| Validation Item                                         | Status | Notes |
| ------------------------------------------------------- | ------ | ----- |
| api_endpoints sheet information is available            | [ ]    |       |
| external_services sheet information is available        | [ ]    |       |
| business_requirements sheet information is available    | [ ]    |       |
| endpoint_service_mapping sheet information is available | [ ]    |       |
| queries sheet information is available when needed      | [ ]    |       |
| entities sheet information is available when needed     | [ ]    |       |
| Sheet-level references are consistent                   | [ ]    |       |

## Go Code Generator

| Validation Item                                    | Status | Notes |
| -------------------------------------------------- | ------ | ----- |
| Handler metadata is available                      | [ ]    |       |
| DTO structure is available                         | [ ]    |       |
| Validation rules are available                     | [ ]    |       |
| Service orchestration steps are available          | [ ]    |       |
| Client metadata is available                       | [ ]    |       |
| Repository/query metadata is available when needed | [ ]    |       |
| Mapper rules are available                         | [ ]    |       |
| Error handling rules are available                 | [ ]    |       |
| Branching logic is available                       | [ ]    |       |

## Documentation Generator

| Validation Item                                | Status | Notes |
| ---------------------------------------------- | ------ | ----- |
| API summary can be generated                   | [ ]    |       |
| Request contract can be generated              | [ ]    |       |
| Response contract can be generated             | [ ]    |       |
| Validation rules can be documented             | [ ]    |       |
| Business rules can be documented               | [ ]    |       |
| External services can be documented            | [ ]    |       |
| Database queries can be documented when needed | [ ]    |       |
| Error behavior can be documented               | [ ]    |       |

## Test Case Generator

| Validation Item                                     | Status | Notes |
| --------------------------------------------------- | ------ | ----- |
| Positive test scenarios are captured                | [ ]    |       |
| Negative validation scenarios are captured          | [ ]    |       |
| Enum validation test scenarios are captured         | [ ]    |       |
| Boundary test scenarios are captured                | [ ]    |       |
| External service failure scenarios are captured     | [ ]    |       |
| Database failure scenarios are captured when needed | [ ]    |       |
| Business rule test scenarios are captured           | [ ]    |       |
| Expected HTTP status is captured                    | [ ]    |       |
| Expected response body is captured                  | [ ]    |       |
| Mock downstream response requirements are captured  | [ ]    |       |

---

# 18. Traceability Checklist

The schema should support traceability from requirement to workflow to generated artifact.

| Validation Item                                   | Status | Notes |
| ------------------------------------------------- | ------ | ----- |
| Workflow links back to source requirement         | [ ]    |       |
| Step links back to requirement section            | [ ]    |       |
| Business rule links back to requirement section   | [ ]    |       |
| Query links back to business rule                 | [ ]    |       |
| External service links back to requirement source | [ ]    |       |
| Response field links back to source mapping       | [ ]    |       |
| Error mapping links back to requirement source    | [ ]    |       |

---

# 19. Completeness Checklist

The schema should be complete enough to represent the workflow end-to-end.

| Validation Item                                    | Status | Notes |
| -------------------------------------------------- | ------ | ----- |
| Workflow has a clear start step                    | [ ]    |       |
| Workflow has a clear success end step              | [ ]    |       |
| Workflow has clear error end steps                 | [ ]    |       |
| All required inputs are captured                   | [ ]    |       |
| All expected outputs are captured                  | [ ]    |       |
| All external dependencies are captured             | [ ]    |       |
| All database dependencies are captured when needed | [ ]    |       |
| All business rules are captured                    | [ ]    |       |
| All response mappings are captured                 | [ ]    |       |
| All known error paths are captured                 | [ ]    |       |

---

# 20. Flexibility Checklist

The schema should support both complex orchestration APIs and simple wrapper APIs.

| Validation Item                                        | Status | Notes                 |
| ------------------------------------------------------ | ------ | --------------------- |
| Supports complex workflows                             | [ ]    | Example: Discount API |
| Supports simple wrapper workflows                      | [ ]    | Example: Fabric API   |
| Allows workflows without database steps                | [ ]    |                       |
| Allows workflows without calculation steps             | [ ]    |                       |
| Allows workflows with external service only            | [ ]    |                       |
| Allows workflows with multiple external services       | [ ]    |                       |
| Allows workflows with conditional branching            | [ ]    |                       |
| Allows workflows with direct response branches         | [ ]    |                       |
| Allows optional metadata without forcing unused fields | [ ]    |                       |

---

# 21. Review Summary Template

Use the following template after applying this checklist to a workflow.

```markdown
## Workflow Review Summary

### Workflow Name

### Endpoint

### Review Date

### Overall Status

- [ ] Ready
- [ ] Mostly ready
- [ ] Partially ready
- [ ] Not ready

### Strengths

- 

### Gaps

- 

### Required Schema Improvements

- 

### Generator Readiness

| Generator | Status | Notes |
|---|---|---|
| Node-RED |  |  |
| Excel Intake |  |  |
| Go Code |  |  |
| Documentation |  |  |
| Test Cases |  |  |

### Final Notes

```

---

# Checklist Application Notes

When applying this checklist to the Day 9 sample APIs:

## Discount API

The Discount API should be used to stress-test the schema for:

* Complex business orchestration
* External service authentication
* Database query references
* Employee discount branching
* New account discount branching
* Exclusion rules
* Discount calculations
* Complex response mapping

## Fabric API

The Fabric API should be used to test the schema for:

* Simple wrapper workflows
* Header validation
* Body validation
* Array item validation
* Optional header pass-through
* External service request mapping
* Success and error response mapping
* Direct response branches

---

# Final Status

This checklist provides a reusable validation tool for reviewing workflow schema completeness.

It will be used to support the Day 9 schema gap analysis and future workflow schema improvements.
