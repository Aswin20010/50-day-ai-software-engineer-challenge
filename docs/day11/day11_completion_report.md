# Day 11 — Completion Report

## Theme

Requirement-to-Workflow Mapping Rules

---

## Purpose

Day 11 focused on defining how raw API requirements should be mapped into Workflow Schema v2.

The goal was to create clear mapping rules so that future requirement extraction agents can convert unstructured or semi-structured requirement sources into a structured, reviewable, and generator-ready workflow format.

No coding was performed on Day 11.

---

## Background

The 50-Day AI Software Engineer Challenge is building toward an AI-assisted enterprise API development pipeline.

The long-term goal is to support generation of:

* Node-RED flows
* Excel intake files
* Go service code
* API documentation
* Test cases
* Evaluation reports

Before those artifacts can be generated, raw requirements must first be converted into a structured workflow representation.

Workflow Schema v2, created on Day 10, provides that structure.

Day 11 defined how raw requirement details should be mapped into that schema.

---

## Previous Work Recap

## Day 8

Day 8 created Workflow Schema v1.

Schema v1 introduced the idea of representing API behavior as structured workflow steps.

It provided the first formal bridge between requirement extraction and future artifact generation.

---

## Day 9

Day 9 validated Schema v1 using two sample APIs:

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

and

```text
POST /api/v1/fabrics
```

This validation showed that Schema v1 was useful, but it needed better support for branching, errors, authentication, query references, calculations, test scenarios, and traceability.

---

## Day 10

Day 10 refined Schema v1 into Workflow Schema v2.

Schema v2 added formal blocks for:

* Metadata
* API contract
* Field validations
* Workflow steps
* Branching
* External services
* Authentication
* Database references
* Business rules
* Calculation rules
* Request mappings
* Response mappings
* Error mappings
* Test scenarios
* Source traceability
* Generator readiness

---

## Day 11 Focus

Day 11 answered the next key question:

```text
How do raw API requirements map into Workflow Schema v2?
```

This is important because future generators should not work directly from raw requirement documents.

Instead, requirements should first be converted into Schema v2.

The Schema v2 representation can then act as the source of truth for downstream generators.

---

# Work Completed

The following files were created for Day 11:

```text
docs/day11/
├── requirement_to_workflow_mapping_overview.md
├── api_contract_mapping_rules.md
├── validation_mapping_rules.md
├── workflow_step_mapping_rules.md
├── service_query_business_rule_mapping.md
└── day11_completion_report.md
```

---

# Files Created

## 1. requirement_to_workflow_mapping_overview.md

This file explained the overall purpose of requirement-to-workflow mapping.

It covered:

* What raw requirements look like
* Why direct artifact generation from raw requirements is risky
* Why Workflow Schema v2 should be used as an intermediate contract
* How mapping rules reduce ambiguity
* How mapping rules support future agents and generators
* The priority order of requirement sources
* How confidence levels should be assigned
* How ambiguity should be handled

The file established the core mapping philosophy:

```text
Raw requirements should first become Schema v2 before becoming generated artifacts.
```

---

## 2. api_contract_mapping_rules.md

This file defined how API-level details should map into Schema v2.

It covered mapping rules for:

* Endpoint path
* HTTP method
* Endpoint ID
* API version
* Operation name
* Summary
* Content type
* Response type
* Controller name
* Handler name
* Tags
* Headers
* Body fields
* Path parameters
* Query parameters
* Success status
* Error status
* Success response body
* Error response body
* Request examples
* Response examples
* Source traceability

The main Schema v2 blocks used were:

* `api_contract`
* `field_validations`
* `response_mappings`
* `error_mappings`
* `test_scenarios`
* `source_traceability`

---

## 3. validation_mapping_rules.md

This file defined how validation requirements should map into Schema v2.

It covered mapping rules for:

* Required fields
* Optional fields
* Header validation
* Body field validation
* Path parameter validation
* Query parameter validation
* Data type validation
* String validation
* Numeric validation
* Enum validation
* Array validation
* Nested object validation
* Date and timestamp validation
* Conditional validation
* Validation workflow steps
* Validation error mapping
* Validation test scenarios
* Validation source traceability

The main Schema v2 blocks used were:

* `field_validations`
* `workflow_steps`
* `error_mappings`
* `test_scenarios`
* `source_traceability`

---

## 4. workflow_step_mapping_rules.md

This file defined how requirement actions should become workflow steps.

It covered step mapping for:

* Validation steps
* Mapping steps
* External service steps
* Database query steps
* Business rule steps
* Calculation steps
* Response mapping steps
* Error mapping steps
* Branching steps
* Context preparation steps

It also defined rules for:

* Step ordering
* Step ID naming
* Step names
* Step inputs
* Step outputs
* Step dependencies
* Step conditions
* Success behavior
* Failure behavior

The main Schema v2 block used was:

* `workflow_steps`

Supporting blocks included:

* `branching`
* `request_mappings`
* `external_services`
* `database_references`
* `business_rules`
* `calculation_rules`
* `response_mappings`
* `error_mappings`
* `source_traceability`

---

## 5. service_query_business_rule_mapping.md

This file defined how deeper workflow logic should map into Schema v2.

It covered mapping rules for:

* External services
* Authentication
* Database queries
* Business rules
* Calculation rules
* Request mappings
* Response mappings
* Error mappings
* Test scenarios
* Source traceability

This file explained how requirement statements such as:

```text
Call the downstream service.
```

```text
Lookup discount profile from the database.
```

```text
Apply employee discount eligibility.
```

```text
Map skuNumbers to customSkuNumbers.
```

should be converted into structured Schema v2 blocks.

The main Schema v2 blocks used were:

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

---

# Mapping Rules Defined

Day 11 defined the following major mapping categories.

---

## 1. API Contract Mapping

Raw endpoint details should map into `api_contract`.

Examples:

| Requirement Detail | Schema v2 Target             |
| ------------------ | ---------------------------- |
| Endpoint path      | api_contract.path            |
| HTTP method        | api_contract.method          |
| API version        | api_contract.api_version     |
| Operation name     | api_contract.operation_name  |
| Content type       | api_contract.content_type    |
| Response type      | api_contract.response_type   |
| Controller name    | api_contract.controller_name |
| Handler name       | api_contract.handler_name    |

---

## 2. Field Validation Mapping

Validation requirements should map into `field_validations`.

Examples:

| Requirement Detail     | Schema v2 Target                 |
| ---------------------- | -------------------------------- |
| Required header        | field_validations                |
| Optional header        | field_validations                |
| Required body field    | field_validations                |
| Enum values            | field_validations.allowed_values |
| Array non-empty rule   | field_validations.min_items      |
| String non-blank rule  | field_validations.allow_blank    |
| Date format            | field_validations.format         |
| Conditional validation | field_validations.condition      |

---

## 3. Workflow Step Mapping

Requirement actions should map into `workflow_steps`.

Examples:

| Requirement Signal       | Step Type           |
| ------------------------ | ------------------- |
| Validate request         | validation          |
| Build downstream request | mapping             |
| Call external API        | external_service    |
| Query database           | database_query      |
| Check eligibility        | business_rule       |
| Calculate discount       | calculation         |
| Return response          | response_mapping    |
| Handle error             | error_mapping       |
| If/else condition        | branching           |
| Prepare context          | context_preparation |

---

## 4. External Service Mapping

Downstream service requirements should map into `external_services`.

Examples:

| Requirement Detail      | Schema v2 Target                     |
| ----------------------- | ------------------------------------ |
| Downstream service name | external_services.service_name       |
| Downstream protocol     | external_services.service_type       |
| Downstream endpoint     | external_services.endpoint_reference |
| HTTP method             | external_services.method             |
| SOAP operation          | external_services.operation_name     |
| Request source          | external_services.request_source     |
| Response usage          | external_services.response_used_by   |

---

## 5. Authentication Mapping

Authentication requirements should map into `authentication`.

Examples:

| Requirement Detail   | Schema v2 Target                       |
| -------------------- | -------------------------------------- |
| API key              | authentication.api_key                 |
| HMAC                 | authentication.signature               |
| Authorization header | authentication.signature.header_name   |
| Signature format     | authentication.signature.format        |
| Signature algorithm  | authentication.signature.algorithm     |
| Secret source        | authentication.signature.secret_source |

---

## 6. Database Query Mapping

Database requirements should map into `database_references`.

Examples:

| Requirement Detail | Schema v2 Target                       |
| ------------------ | -------------------------------------- |
| Table name         | database_references.table_names        |
| Query purpose      | database_references.purpose            |
| Query inputs       | database_references.inputs             |
| Query outputs      | database_references.outputs            |
| No-record behavior | database_references.no_record_behavior |
| Failure behavior   | database_references.failure_behavior   |

---

## 7. Business Rule Mapping

Business logic should map into `business_rules`.

Examples:

| Requirement Detail | Schema v2 Target         |
| ------------------ | ------------------------ |
| Eligibility rule   | business_rules           |
| Exclusion rule     | business_rules           |
| Cap rule           | business_rules           |
| Employee rule      | business_rules           |
| New account rule   | business_rules           |
| Condition          | business_rules.condition |
| Inputs             | business_rules.inputs    |
| Outputs            | business_rules.outputs   |

---

## 8. Calculation Rule Mapping

Calculation behavior should map into `calculation_rules`.

Examples:

| Requirement Detail     | Schema v2 Target                    |
| ---------------------- | ----------------------------------- |
| Discount percentage    | calculation_rules                   |
| Cap amount             | calculation_rules                   |
| Item-level calculation | calculation_rules.item_level        |
| Rounding behavior      | calculation_rules.rounding_behavior |
| Calculation input      | calculation_rules.inputs            |
| Calculation output     | calculation_rules.output            |

---

## 9. Request Mapping

Source-to-target input transformations should map into `request_mappings`.

Examples:

| Requirement Detail       | Schema v2 Target                           |
| ------------------------ | ------------------------------------------ |
| Copy request field       | request_mappings                           |
| Rename field             | request_mappings                           |
| Build downstream payload | request_mappings                           |
| Optional pass-through    | request_mappings.pass_through_when_present |
| Omit if missing          | request_mappings.omit_when_missing         |

---

## 10. Response Mapping

Output transformations should map into `response_mappings`.

Examples:

| Requirement Detail     | Schema v2 Target                 |
| ---------------------- | -------------------------------- |
| Success response field | response_mappings                |
| Source field           | response_mappings.source         |
| Target field           | response_mappings.target         |
| HTTP status            | response_mappings.http_status    |
| Transformation         | response_mappings.transformation |

---

## 11. Error Mapping

Failure behavior should map into `error_mappings`.

Examples:

| Requirement Detail    | Schema v2 Target                  |
| --------------------- | --------------------------------- |
| Validation failure    | error_mappings                    |
| Downstream failure    | error_mappings                    |
| Database failure      | error_mappings                    |
| Business rule failure | error_mappings                    |
| Target HTTP status    | error_mappings.target_http_status |
| Target error code     | error_mappings.target_error_code  |
| Retryable flag        | error_mappings.retryable          |
| Fail-fast flag        | error_mappings.fail_fast          |

---

## 12. Test Scenario Mapping

Important behavior should map into `test_scenarios`.

Examples:

| Requirement Detail     | Schema v2 Target                       |
| ---------------------- | -------------------------------------- |
| Valid request          | positive test_scenario                 |
| Missing required field | negative test_scenario                 |
| Invalid enum           | negative test_scenario                 |
| Downstream failure     | failure test_scenario                  |
| Database no record     | boundary or business test_scenario     |
| Branch path coverage   | test_scenarios.expected.executed_steps |

---

## 13. Source Traceability Mapping

Source evidence should map into `source_traceability`.

Examples:

| Requirement Detail   | Schema v2 Target                                    |
| -------------------- | --------------------------------------------------- |
| Requirement document | source_traceability.workflow_source.requirement_doc |
| Requirement section  | source_traceability.requirement_section             |
| Source reference     | source_traceability.source_text_reference           |
| Confidence           | source_traceability.confidence                      |

---

# Mapping Priority Order

Day 11 confirmed the recommended source priority order:

```text
1. Official requirement document
2. API contract or confirmed request/response examples
3. User-confirmed clarification
4. Existing prototype behavior
5. Node-RED flow behavior
6. Excel intake expectation
7. Reasonable assumption marked with low confidence
```

The mapping process should avoid silent assumptions.

When a detail is inferred, it should be marked with medium or low confidence.

---

# Confidence Levels

The following confidence levels should be used:

```text
high
medium
low
```

## High Confidence

Use when the requirement explicitly states the detail.

## Medium Confidence

Use when the detail is supported by a prototype, example, or related document but not clearly stated in the primary requirement.

## Low Confidence

Use when the detail is inferred and needs review.

---

# Ambiguity Handling

When a requirement is unclear, the mapping process should:

1. Capture the best available interpretation.
2. Mark it with medium or low confidence.
3. Add a traceability note.
4. Add the issue to a future review or gap list.
5. Avoid hiding assumptions inside generated artifacts.

---

# Day 11 Success Criteria Review

| Success Criteria                                  | Status   |
| ------------------------------------------------- | -------- |
| Mapping overview documented                       | Complete |
| API contract mapping rules defined                | Complete |
| Validation mapping rules defined                  | Complete |
| Workflow step mapping rules defined               | Complete |
| Service, query, and business rule mapping defined | Complete |
| Completion report created                         | Complete |
| No coding performed                               | Complete |

---

# Final Folder Structure

At the end of Day 11, the folder should look like this:

```text
docs/day11/
├── requirement_to_workflow_mapping_overview.md
├── api_contract_mapping_rules.md
├── validation_mapping_rules.md
├── workflow_step_mapping_rules.md
├── service_query_business_rule_mapping.md
└── day11_completion_report.md
```

---

# Final Status

Day 11 is complete.

The project now has a clear set of rules for mapping raw API requirements into Workflow Schema v2.

These rules make the future extraction process more consistent, reviewable, and generator-ready.

The project now has three important layers:

```text
Raw Requirements
        ↓
Requirement-to-Workflow Mapping Rules
        ↓
Workflow Schema v2
        ↓
Future Artifact Generators
```

This structure reduces ambiguity and prepares the project for future automation.

---

# Recommended Next Step

Day 12 should build on these mapping rules by defining a requirement extraction evaluation framework.

The recommended Day 12 theme is:

```text
Requirement Extraction Evaluation Framework
```

Day 12 should define how to measure whether extracted Schema v2 workflows correctly represent the original requirements.
