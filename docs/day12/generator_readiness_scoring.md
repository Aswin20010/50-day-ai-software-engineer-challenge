# Day 12 — Generator Readiness Scoring

## Purpose

This document defines how to score generator readiness for a generated Workflow Schema v2 output.

Generator readiness measures whether the extracted schema contains enough structured information to support future artifact generation.

No coding is performed in this phase.

---

## Background

Day 10 created Workflow Schema v2.

Day 11 defined how raw requirements map into Workflow Schema v2.

Day 12 defines how to evaluate the quality of an extracted schema.

Previous Day 12 phases defined:

* Schema completeness scoring
* Requirement accuracy scoring
* Traceability scoring

Phase 5 focuses on generator readiness scoring.

Generator readiness is important because the final goal of the project is not only to create schemas.

The final goal is to use those schemas to generate artifacts such as:

* Node-RED flows
* Excel intake files
* Go service code
* Documentation
* Test cases
* Evaluation reports

---

## What Generator Readiness Means

Generator readiness answers the question:

```text
Can this extracted Workflow Schema v2 be safely used by future generators?
```

A schema may be complete and accurate, but still not ready for a specific generator.

For example:

```text
The workflow steps may be correct, but Node-RED node type mappings may be missing.

The API contract may be correct, but Go DTO names may be missing.

The business rules may be correct, but Excel sheet column mappings may be missing.

The response mappings may be correct, but test assertion details may be incomplete.
```

Generator readiness evaluates whether the schema has the right level of structure for each downstream artifact type.

---

## Generator Categories

Generator readiness should be scored for the following categories:

```text
1. Node-RED Flow Generator
2. Excel Intake Generator
3. Go Code Generator
4. Documentation Generator
5. Test Case Generator
6. Evaluation Script Generator
```

Each generator has different requirements.

A schema may be ready for documentation but only partially ready for Go code generation.

---

## Schema v2 Block Used

Generator readiness is captured in the Schema v2 block:

```yaml
generator_readiness:
  node_red:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string

  excel:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string

  go:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string

  documentation:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string

  tests:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string

  evaluation:
    status: ready | mostly_ready | partially_ready | not_ready
    missing_fields:
      - string
    notes: string
```

---

# Readiness Status Values

Use the following readiness status values.

## ready

Use when the schema contains all required information for that generator.

Example:

```text
documentation.status = ready
```

## mostly_ready

Use when the schema contains most required information, but minor generator-specific details are missing.

Example:

```text
node_red.status = mostly_ready
```

## partially_ready

Use when the schema contains useful information, but important generator-specific details are missing.

Example:

```text
go.status = partially_ready
```

## not_ready

Use when the schema is missing core information needed by that generator.

Example:

```text
tests.status = not_ready
```

---

# Recommended Scoring Scale

Use a 100-point scale.

```text
90–100 = Excellent
80–89  = Good
70–79  = Acceptable with review
60–69  = Weak, needs correction
Below 60 = Not ready
```

---

# Default Generator Readiness Score Distribution

| Generator Area              |  Points |
| --------------------------- | ------: |
| Node-RED Flow Readiness     |      20 |
| Excel Intake Readiness      |      20 |
| Go Code Readiness           |      20 |
| Documentation Readiness     |      15 |
| Test Case Readiness         |      15 |
| Evaluation Script Readiness |      10 |
| **Total**                   | **100** |

---

# Important Scoring Rule

Generator readiness should be scored based on the intended target generators.

If a workflow is not intended to generate Go code yet, Go readiness can be marked as:

```text
not_targeted_for_current_phase
```

However, for this 50-day challenge, the preferred approach is to score all six generator categories because the long-term goal includes all of them.

---

# 1. Node-RED Flow Readiness

## Purpose

Node-RED readiness measures whether the schema contains enough information to generate a Node-RED flow.

## Required Information

A Node-RED generator needs:

```text
API path
HTTP method
Ordered workflow steps
Step types
Step dependencies
Branching conditions
True and false paths
External service calls
Request mappings
Response mappings
Error mappings
Validation rules
Success and failure paths
```

## Helpful Future Metadata

Node-RED generation would also benefit from:

```text
node_type
node_label
node_position
wire_to
subflow_reference
function_template
environment_variable_mapping
```

These fields may not exist yet in Schema v2, so missing them should reduce readiness but not block early design readiness.

---

## Scoring

| Node-RED Readiness Quality                                      | Points |
| --------------------------------------------------------------- | -----: |
| All required flow generation inputs present                     |     20 |
| Most required inputs present, minor node metadata missing       |  16–19 |
| Workflow steps present but branching/mappings/errors incomplete |  10–15 |
| Only high-level workflow present                                |    5–9 |
| Not enough information to generate Node-RED flow                |    0–4 |

---

## Example Ready/Missing Fields

```yaml
generator_readiness:
  node_red:
    status: mostly_ready
    missing_fields:
      - node_type_mapping
      - visual_layout_strategy
      - reusable_subflow_metadata
    notes: >
      Workflow sequence, branching, mappings, and external calls are defined.
      Node-RED-specific visual metadata is still needed.
```

---

# 2. Excel Intake Readiness

## Purpose

Excel readiness measures whether the schema contains enough information to generate an intake workbook.

The workbook may include sheets such as:

```text
technical_requirements
api_endpoints
entities
external_services
business_requirements
endpoint_service_mapping
queries
event_details
```

## Required Information

An Excel generator needs:

```text
API contract details
Header and body validations
External services
Authentication details
Database references
Business rules
Calculation rules
Request mappings
Response mappings
Error mappings
Workflow step mappings
Source traceability
```

## Helpful Future Metadata

Excel generation would also benefit from:

```text
target_sheet
target_column
row_id
cross_sheet_reference
required_column
sheet_order
column_formatting_rules
```

---

## Scoring

| Excel Readiness Quality                                         | Points |
| --------------------------------------------------------------- | -----: |
| All required workbook information present                       |     20 |
| Most workbook information present, sheet-column rules missing   |  16–19 |
| Main sections present but query/rule/mapping details incomplete |  10–15 |
| Only API-level data present                                     |    5–9 |
| Not enough information to generate workbook                     |    0–4 |

---

## Example Ready/Missing Fields

```yaml
generator_readiness:
  excel:
    status: mostly_ready
    missing_fields:
      - exact_sheet_column_mapping
      - row_generation_rules
      - cross_sheet_reference_rules
    notes: >
      Schema contains endpoint, validation, service, query, rule, and mapping data.
      Excel-specific formatting and row mapping rules are still needed.
```

---

# 3. Go Code Readiness

## Purpose

Go readiness measures whether the schema contains enough information to generate Go service code.

Potential generated layers include:

```text
routes
handlers
DTOs
validators
services
clients
repositories
mappers
error handlers
tests
configuration
```

## Required Information

A Go code generator needs:

```text
API path and method
Request headers
Request body fields
Request DTO structure
Response DTO structure
Validation rules
Service orchestration steps
Branching logic
External client details
Authentication behavior
Database query references
Repository requirements
Business rules
Calculation rules
Request mapper rules
Response mapper rules
Error mapping rules
Test scenarios
```

## Helpful Future Metadata

Go code generation needs more implementation-specific details, such as:

```text
module_name
package_name
request_dto_name
response_dto_name
handler_name
service_interface_name
service_method_name
repository_interface_name
repository_method_name
client_interface_name
client_method_name
mapper_function_name
error_type_name
config_struct_name
environment_variable_name
logging_strategy
tracing_strategy
```

---

## Scoring

| Go Readiness Quality                                          | Points |
| ------------------------------------------------------------- | -----: |
| All design and implementation details present                 |     20 |
| Workflow logic complete, minor naming/config gaps             |  16–19 |
| Workflow logic clear, but package/DTO/interface names missing |  10–15 |
| API and workflow present, but implementation details weak     |    5–9 |
| Not enough information to generate Go code                    |    0–4 |

---

## Example Ready/Missing Fields

```yaml
generator_readiness:
  go:
    status: partially_ready
    missing_fields:
      - package_name
      - request_dto_name
      - response_dto_name
      - service_method_name
      - repository_method_name
      - client_method_name
      - typed_error_definitions
      - environment_variable_names
    notes: >
      Workflow logic is structured, but Go implementation naming and package-level
      details are not yet defined.
```

---

# 4. Documentation Readiness

## Purpose

Documentation readiness measures whether the schema contains enough information to generate human-readable technical documentation.

## Required Information

A documentation generator needs:

```text
workflow name
workflow purpose
API contract
headers
request body
validation rules
workflow steps
branching behavior
external services
database references
business rules
calculation rules
request mappings
response mappings
error mappings
test scenarios
source traceability
generator readiness notes
```

## Helpful Future Metadata

Documentation generation would also benefit from:

```text
document_type
audience
section_order
include_examples
include_diagrams
glossary_terms
diagram_hints
```

---

## Scoring

| Documentation Readiness Quality                                   | Points |
| ----------------------------------------------------------------- | -----: |
| Enough information for complete technical documentation           |     15 |
| Most documentation information present, examples/diagrams missing |  12–14 |
| API and workflow docs possible, but rules/mappings incomplete     |   8–11 |
| Only basic documentation possible                                 |    4–7 |
| Not enough information for documentation                          |    0–3 |

---

## Example Ready/Missing Fields

```yaml
generator_readiness:
  documentation:
    status: ready
    missing_fields:
      - optional_diagram_hints
      - glossary_terms
    notes: >
      Schema contains enough information to generate useful API and workflow
      documentation.
```

---

# 5. Test Case Readiness

## Purpose

Test readiness measures whether the schema contains enough information to generate test cases.

Generated tests may include:

```text
positive tests
negative validation tests
boundary tests
branch path tests
downstream failure tests
database no-record tests
database failure tests
calculation tests
response mapping tests
error mapping tests
```

## Required Information

A test generator needs:

```text
API path and method
request input examples
field validations
expected validation failures
workflow steps
branching paths
external service mocks
database mocks
business rule scenarios
calculation scenarios
expected HTTP status
expected response body
expected executed steps
expected skipped steps
```

## Helpful Future Metadata

Test generation would also benefit from:

```text
test_framework
unit_or_integration
mock_strategy
fixture_source
assertion_style
boundary_value_rules
test_data_generation_rules
database_fixture_rules
mock_server_strategy
```

---

## Scoring

| Test Readiness Quality                                                          | Points |
| ------------------------------------------------------------------------------- | -----: |
| Enough information for positive, negative, branch, service, DB, and error tests |     15 |
| Most test paths present, some edge cases missing                                |  12–14 |
| Basic positive and negative tests possible                                      |   8–11 |
| Only minimal test generation possible                                           |    4–7 |
| Not enough information to generate tests                                        |    0–3 |

---

## Example Ready/Missing Fields

```yaml
generator_readiness:
  tests:
    status: mostly_ready
    missing_fields:
      - more_boundary_scenarios
      - test_framework
      - mock_strategy
      - database_fixture_rules
    notes: >
      Schema contains test scenarios and expected paths, but framework-specific
      test generation details are still needed.
```

---

# 6. Evaluation Script Readiness

## Purpose

Evaluation readiness measures whether the schema contains enough information to support automated or semi-automated evaluation.

Evaluation scripts may measure:

```text
schema completeness
requirement accuracy
source traceability
generator readiness
gap detection
human correction effort
time reduction
artifact quality
```

## Required Information

An evaluation generator needs:

```text
workflow metadata
source traceability
schema completeness indicators
required vs optional blocks
confidence levels
gap notes
generator readiness statuses
missing fields
test scenarios
expected outputs
```

## Helpful Future Metadata

Evaluation scripts would also benefit from:

```text
coverage_score
accuracy_score
traceability_score
readiness_score
manual_review_status
human_correction_count
time_saved_minutes
baseline_time_minutes
artifact_quality_score
evaluation_rubric_version
```

---

## Scoring

| Evaluation Readiness Quality                                  | Points |
| ------------------------------------------------------------- | -----: |
| Enough information for complete evaluation scorecard          |     10 |
| Most evaluation information present, scoring metadata missing |    8–9 |
| Basic evaluation possible, but metrics incomplete             |    5–7 |
| Only manual review possible                                   |    2–4 |
| Not enough information for evaluation                         |    0–1 |

---

## Example Ready/Missing Fields

```yaml
generator_readiness:
  evaluation:
    status: partially_ready
    missing_fields:
      - scoring_rubric_version
      - human_correction_count
      - baseline_time_minutes
      - time_saved_minutes
      - artifact_quality_score
    notes: >
      Schema supports completeness, traceability, and readiness review, but
      research metric fields still need to be defined.
```

---

# Generator Readiness Scorecard Template

Use this template to evaluate generator readiness.

```text
Workflow Name:
Workflow ID:
Workflow Type:
Requirement Source:
Evaluator:
Evaluation Date:

Generator Readiness Scorecard
-------------------------------------------------
Node-RED Flow Readiness:      __ / 20
Excel Intake Readiness:       __ / 20
Go Code Readiness:            __ / 20
Documentation Readiness:      __ / 15
Test Case Readiness:          __ / 15
Evaluation Script Readiness:  __ / 10
-------------------------------------------------
Total:                        __ / 100

Readiness Rating:
Ready Generators:
Partially Ready Generators:
Not Ready Generators:
Missing Fields:
Reviewer Notes:
```

---

# Example Generator Readiness Evaluation — Fabric API

## Workflow Type

```text
simple_wrapper
```

## Expected Generator Needs

The Fabric API should be easier to generate because it is mainly a wrapper workflow.

It requires:

```text
validation
request mapping
external service call
response mapping
error mapping
test scenarios
```

## Example Score

```text
Node-RED Flow Readiness:      17 / 20
Excel Intake Readiness:       18 / 20
Go Code Readiness:            12 / 20
Documentation Readiness:      14 / 15
Test Case Readiness:          11 / 15
Evaluation Script Readiness:  7 / 10
-------------------------------------------------
Total:                        79 / 100
```

## Rating

```text
Acceptable with review
```

## Gap Notes

```text
- Go DTO names are not defined.
- Node-RED visual layout metadata is missing.
- Test framework and mock strategy are not defined.
- Evaluation metrics are not fully specified.
```

---

# Example Generator Readiness Evaluation — Discount API

## Workflow Type

```text
business_orchestration
```

## Expected Generator Needs

The Discount API requires more detailed generator inputs because it includes:

```text
validation
external service call
HMAC authentication
database references
employee/new account branching
business rules
exclusion rules
calculation rules
response mappings
error mappings
test scenarios
```

## Example Score

```text
Node-RED Flow Readiness:      16 / 20
Excel Intake Readiness:       17 / 20
Go Code Readiness:            11 / 20
Documentation Readiness:      14 / 15
Test Case Readiness:          10 / 15
Evaluation Script Readiness:  7 / 10
-------------------------------------------------
Total:                        75 / 100
```

## Rating

```text
Acceptable with review
```

## Gap Notes

```text
- Go package and interface naming are missing.
- Repository method names are missing.
- Some calculation test cases are not fully defined.
- Node-RED subflow strategy is missing.
- Evaluation score metadata is still incomplete.
```

---

# Readiness Gap Categories

When points are lost, classify the reason.

Recommended categories:

```text
missing_node_red_metadata
missing_excel_sheet_mapping
missing_go_naming
missing_dto_definition
missing_repository_definition
missing_client_definition
missing_mapper_definition
missing_error_type_definition
missing_test_framework
missing_mock_strategy
missing_boundary_tests
missing_evaluation_metrics
missing_source_coverage_metrics
missing_artifact_quality_rubric
```

---

# Generator Readiness Review Checklist

Use this checklist during generator readiness review.

## Node-RED

```text
- Are workflow steps ordered?
- Are step types defined?
- Are dependencies defined?
- Are branch paths defined?
- Are external service steps defined?
- Are request mappings defined?
- Are response mappings defined?
- Are error mappings defined?
- Are validation rules defined?
- Are missing Node-RED-specific fields listed?
```

## Excel

```text
- Is api_contract complete enough for api_endpoints?
- Are field validations available?
- Are external services available?
- Are database references available when needed?
- Are business rules available?
- Are request/response mappings available?
- Are error mappings available?
- Are missing Excel-specific fields listed?
```

## Go

```text
- Is route information available?
- Are request and response fields available?
- Are validation rules available?
- Are service orchestration steps available?
- Are client calls defined?
- Are repository references defined?
- Are business rules structured?
- Are calculation rules structured?
- Are mapper rules structured?
- Are error mappings structured?
- Are missing Go-specific fields listed?
```

## Documentation

```text
- Is workflow purpose clear?
- Is API contract clear?
- Are request and response details available?
- Are validation rules available?
- Are workflow steps available?
- Are business rules documented?
- Are external services documented?
- Are errors documented?
```

## Tests

```text
- Are positive scenarios available?
- Are negative validation scenarios available?
- Are branch scenarios available?
- Are downstream failure scenarios available?
- Are database scenarios available when applicable?
- Are calculation scenarios available when applicable?
- Are expected outputs defined?
- Are missing test-specific fields listed?
```

## Evaluation

```text
- Is source traceability available?
- Are confidence levels available?
- Are generator readiness statuses available?
- Are missing fields recorded?
- Are test scenarios available?
- Are scoring gaps captured?
- Are missing evaluation metrics listed?
```

---

# Common Mistakes to Avoid

## Mistake 1 — Treating Documentation Readiness as Full Project Readiness

A schema may be ready for documentation but not ready for Go code generation.

Each generator should be scored separately.

---

## Mistake 2 — Saying Ready Without Listing Missing Fields

If a generator is not fully ready, missing fields should be listed clearly.

Example:

```text
Go readiness is partially_ready because DTO names and repository method names are missing.
```

---

## Mistake 3 — Ignoring Generator-Specific Needs

Different generators need different metadata.

Node-RED needs flow and node structure.

Excel needs sheet and row mapping.

Go needs package, DTO, interface, and method details.

Tests need scenarios, mocks, and expected outputs.

---

## Mistake 4 — Penalizing Early Design Too Harshly

During early documentation days, some generator-specific details are expected to be missing.

The goal is to identify gaps, not to fail the schema too early.

---

## Mistake 5 — Not Updating Readiness After Future Refinement

Generator readiness should improve as future days add more detailed generator-specific specifications.

---

# Final Summary

Generator readiness scoring measures whether a generated Workflow Schema v2 contains enough structured information for future artifact generation.

It evaluates readiness for:

* Node-RED flow generation
* Excel intake generation
* Go code generation
* Documentation generation
* Test case generation
* Evaluation script generation

This scoring category is important because the project goal is not only to extract requirements, but to use those extracted workflows to generate useful engineering artifacts.

This completes Phase 5 of Day 12.
