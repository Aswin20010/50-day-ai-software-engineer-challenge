# Day 10 — Workflow Schema v2 Generator Readiness

## Purpose

This document explains how Workflow Schema v2 supports future artifact generators.

The purpose of Schema v2 is not to generate artifacts immediately. Instead, it defines the structured information that future generators will need.

This file reviews how Schema v2 can support:

* Node-RED flow generation
* Excel intake generation
* Go code generation
* Documentation generation
* Test case generation
* Evaluation scripts

No coding is performed in this phase.

---

## Background

On Day 8, Workflow Schema v1 was created.

On Day 9, Schema v1 was validated using two APIs:

1. FEED Discount API
2. Customize IT Fabric API

The validation showed that Schema v1 was useful, but it needed stronger structure for reliable generation.

On Day 10, Workflow Schema v2 was designed to address those gaps.

Schema v2 adds formal blocks for:

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

These blocks make the workflow representation more useful for future generators.

---

# Generator Readiness Overview

Workflow Schema v2 is designed to act as a shared input contract for multiple generators.

The same workflow schema should eventually support different output artifacts.

```text
Requirement Document
        ↓
Requirement Extraction
        ↓
Workflow Schema v2
        ↓
Artifact Generators
        ↓
Node-RED Flow / Excel Intake / Go Code / Docs / Tests / Evaluation
```

Schema v2 does not replace generators.

Instead, it gives future generators a consistent and structured source of truth.

---

# 1. Node-RED Flow Generator Readiness

## Purpose of the Node-RED Generator

The Node-RED generator will convert a structured workflow into a Node-RED flow.

It should generate nodes for:

* HTTP input
* Request validation
* Request mapping
* External service calls
* Database calls
* Business rules
* Branching
* Response mapping
* Error handling
* HTTP response

---

## Schema v2 Blocks Used by Node-RED Generator

| Schema v2 Block     | How Node-RED Generator Uses It                        |
| ------------------- | ----------------------------------------------------- |
| metadata            | Creates tab name, flow label, and documentation notes |
| api_contract        | Creates HTTP In node path and method                  |
| field_validations   | Creates validation function nodes                     |
| workflow_steps      | Creates ordered workflow nodes                        |
| branching           | Creates switch nodes and conditional routes           |
| external_services   | Creates HTTP request or SOAP wrapper nodes            |
| authentication      | Adds required auth headers or signature logic         |
| database_references | Creates database query nodes or repository call nodes |
| business_rules      | Creates business rule function nodes                  |
| calculation_rules   | Creates calculation function nodes                    |
| request_mappings    | Creates mapping function nodes                        |
| response_mappings   | Creates response mapper nodes                         |
| error_mappings      | Creates error handling paths                          |
| test_scenarios      | Helps generate example inject/test nodes in future    |
| source_traceability | Adds comments showing source requirement references   |
| generator_readiness | Decides whether flow generation should proceed        |

---

## Node-RED Generator Required Fields

The Node-RED generator needs the following details:

```text
- API path
- HTTP method
- Ordered workflow steps
- Step types
- Step dependencies
- Branch conditions
- External service endpoints
- Request mappings
- Response mappings
- Error paths
- Validation rules
```

Schema v2 provides most of these fields.

---

## Node-RED Generator Remaining Gaps

Schema v2 is mostly ready for conceptual Node-RED generation, but direct Node-RED generation still needs additional metadata.

Missing or future fields include:

```text
- Node type mapping
- Node position/layout rules
- Node naming convention
- Wire generation strategy
- Reusable subflow metadata
- Node-RED function template references
- Environment variable mapping
```

---

## Node-RED Readiness Status

```text
Mostly Ready
```

## Reason

Schema v2 clearly defines workflow order, branching, mappings, external calls, and errors. However, visual Node-RED layout and exact node-type templates still need to be defined later.

---

# 2. Excel Intake Generator Readiness

## Purpose of the Excel Intake Generator

The Excel intake generator will convert a workflow schema into a structured workbook that can be consumed by a future Go Factory or artifact generation pipeline.

The workbook may include sheets such as:

* technical_requirements
* api_endpoints
* entities
* external_services
* business_requirements
* endpoint_service_mapping
* queries
* event_details, if needed

---

## Schema v2 Blocks Used by Excel Generator

| Schema v2 Block     | How Excel Generator Uses It                                          |
| ------------------- | -------------------------------------------------------------------- |
| metadata            | Populates technical requirement summary                              |
| api_contract        | Populates api_endpoints sheet                                        |
| field_validations   | Populates validation rules in api_endpoints or business_requirements |
| workflow_steps      | Populates endpoint_service_mapping                                   |
| branching           | Populates orchestration and conditional mapping rows                 |
| external_services   | Populates external_services sheet                                    |
| authentication      | Populates external service auth metadata                             |
| database_references | Populates queries sheet                                              |
| business_rules      | Populates business_requirements sheet                                |
| calculation_rules   | Populates business_requirements and endpoint_service_mapping         |
| request_mappings    | Populates endpoint_service_mapping mappings                          |
| response_mappings   | Populates response mapping details                                   |
| error_mappings      | Populates error handling rows                                        |
| source_traceability | Supports requirement-to-row traceability                             |
| generator_readiness | Indicates whether workbook generation is safe                        |

---

## Excel Generator Required Fields

The Excel generator needs:

```text
- Endpoint path and method
- Request headers
- Request body fields
- Validation rules
- External service details
- Business rules
- Query IDs
- Query purpose
- Query inputs and outputs
- Workflow step mappings
- Request and response mappings
- Error behavior
```

Schema v2 provides these details in structured blocks.

---

## Excel Generator Remaining Gaps

Direct Excel generation still needs sheet-specific mapping rules.

Missing or future fields include:

```text
- Exact sheet column mappings
- Required workbook tabs per workflow type
- Row ID generation rules
- Cross-sheet reference rules
- Column formatting rules
- Mandatory versus optional workbook columns
- Naming conventions for generated query rows
```

---

## Excel Generator Readiness Status

```text
Mostly Ready
```

## Reason

Schema v2 has enough business and technical metadata to populate most Excel sheets. The remaining work is to define workbook-specific formatting and sheet mapping rules.

---

# 3. Go Code Generator Readiness

## Purpose of the Go Code Generator

The Go code generator will convert workflow schema data into a Go service structure.

It may generate:

* Routes
* Handlers
* DTOs
* Validators
* Services
* Clients
* Repositories
* Mappers
* Error handling
* Tests

---

## Schema v2 Blocks Used by Go Generator

| Schema v2 Block     | How Go Generator Uses It                        |
| ------------------- | ----------------------------------------------- |
| metadata            | Defines service/module context                  |
| api_contract        | Generates route and handler metadata            |
| field_validations   | Generates request validation logic              |
| workflow_steps      | Generates service orchestration                 |
| branching           | Generates if/else or switch logic               |
| external_services   | Generates client definitions                    |
| authentication      | Generates auth/header/signature logic           |
| database_references | Generates repository method requirements        |
| business_rules      | Generates service rule logic                    |
| calculation_rules   | Generates calculation functions                 |
| request_mappings    | Generates mapper functions                      |
| response_mappings   | Generates response mappers                      |
| error_mappings      | Generates typed errors and error handlers       |
| test_scenarios      | Generates unit and integration test scaffolds   |
| source_traceability | Adds traceability comments or documentation     |
| generator_readiness | Determines whether Go generation should proceed |

---

## Go Generator Required Fields

The Go generator needs:

```text
- API route path
- HTTP method
- Request DTO fields
- Response DTO fields
- Header definitions
- Body field definitions
- Validation rules
- Service orchestration steps
- Branching logic
- External client metadata
- Repository query metadata
- Request mapper rules
- Response mapper rules
- Error mapping rules
- Test scenario definitions
```

Schema v2 provides the design-level version of these fields.

---

## Go Generator Remaining Gaps

Go generation requires more implementation-level details than Schema v2 currently provides.

Missing or future fields include:

```text
- Go package names
- Request DTO struct names
- Response DTO struct names
- Field JSON tags
- Validation tag strategy
- Handler function names
- Service interface names
- Service method names
- Repository interface names
- Repository method names
- Client interface names
- Client method names
- Typed error names
- Configuration struct names
- Environment variable names
- Logging and tracing conventions
```

---

## Go Generator Readiness Status

```text
Partially Ready
```

## Reason

Schema v2 is strong for workflow logic, but Go generation requires detailed implementation naming and package structure. Those should be designed later, closer to the coding phase.

---

# 4. Documentation Generator Readiness

## Purpose of the Documentation Generator

The documentation generator will convert workflow schema data into human-readable technical documents.

It should generate:

* API overview
* Request contract
* Response contract
* Validation rules
* Workflow explanation
* Business rule documentation
* External service documentation
* Database query documentation
* Error behavior documentation
* Test scenario documentation

---

## Schema v2 Blocks Used by Documentation Generator

| Schema v2 Block     | How Documentation Generator Uses It                 |
| ------------------- | --------------------------------------------------- |
| metadata            | Creates document title and summary                  |
| api_contract        | Documents endpoint, method, content type, operation |
| field_validations   | Documents request rules                             |
| workflow_steps      | Documents workflow sequence                         |
| branching           | Documents decision paths                            |
| external_services   | Documents downstream integrations                   |
| authentication      | Documents security/auth behavior                    |
| database_references | Documents database usage                            |
| business_rules      | Documents business logic                            |
| calculation_rules   | Documents calculation behavior                      |
| request_mappings    | Documents transformation logic                      |
| response_mappings   | Documents response structure                        |
| error_mappings      | Documents error behavior                            |
| test_scenarios      | Documents validation scenarios                      |
| source_traceability | Documents requirement source links                  |
| generator_readiness | Documents artifact readiness                        |

---

## Documentation Generator Required Fields

The documentation generator needs:

```text
- Workflow name
- Workflow purpose
- Endpoint path and method
- Request fields
- Response fields
- Validation rules
- Workflow steps
- Business rules
- External dependencies
- Error behavior
```

Schema v2 provides these fields clearly.

---

## Documentation Generator Remaining Gaps

The documentation generator may still need optional presentation metadata.

Missing or future fields include:

```text
- Documentation template type
- Audience type
- Diagram generation hints
- Section ordering preferences
- Example request and response payloads
- Glossary terms
```

---

## Documentation Generator Readiness Status

```text
Ready
```

## Reason

Schema v2 is already strong enough to generate useful human-readable documentation.

---

# 5. Test Case Generator Readiness

## Purpose of the Test Case Generator

The test case generator will convert workflow schema data into test cases.

It should support:

* Positive tests
* Negative validation tests
* Boundary tests
* Downstream failure tests
* Database failure tests
* Business rule path tests
* Response mapping tests

---

## Schema v2 Blocks Used by Test Generator

| Schema v2 Block     | How Test Generator Uses It                    |
| ------------------- | --------------------------------------------- |
| api_contract        | Determines endpoint, method, and content type |
| field_validations   | Creates negative validation tests             |
| workflow_steps      | Verifies expected execution path              |
| branching           | Creates path coverage tests                   |
| external_services   | Defines downstream mocks                      |
| database_references | Defines database mocks                        |
| business_rules      | Creates business rule tests                   |
| calculation_rules   | Creates calculation tests                     |
| response_mappings   | Creates expected response checks              |
| error_mappings      | Creates expected error checks                 |
| test_scenarios      | Provides direct test case definitions         |
| source_traceability | Links tests to source requirements            |
| generator_readiness | Determines if test generation is safe         |

---

## Test Generator Required Fields

The test generator needs:

```text
- Test scenario ID
- Scenario name
- Scenario type
- Input headers
- Input body
- Mock external service responses
- Mock database responses
- Expected HTTP status
- Expected response body
- Expected executed steps
- Expected skipped steps
```

Schema v2 introduces these fields through the `test_scenarios` block.

---

## Test Generator Remaining Gaps

Test generation still needs more detailed conventions.

Missing or future fields include:

```text
- Testing framework target
- Unit versus integration test type
- Mocking framework conventions
- Assertion style
- Boundary value generation rules
- Test data generation rules
- Database fixture rules
- Downstream mock server strategy
```

---

## Test Generator Readiness Status

```text
Mostly Ready
```

## Reason

Schema v2 introduces structured test scenarios, which was missing in Schema v1. More framework-specific details are still needed later.

---

# 6. Evaluation Script Readiness

## Purpose of Evaluation Scripts

Evaluation scripts will measure how well the AI-assisted pipeline performs.

They may evaluate:

* Requirement extraction accuracy
* Workflow completeness
* Schema field coverage
* Generator readiness
* Gap detection accuracy
* Artifact consistency
* Time reduction
* Human correction effort

---

## Schema v2 Blocks Used by Evaluation Scripts

| Schema v2 Block     | How Evaluation Scripts Use It              |
| ------------------- | ------------------------------------------ |
| metadata            | Identifies workflow and version            |
| source_traceability | Measures source requirement coverage       |
| field_validations   | Measures validation extraction accuracy    |
| workflow_steps      | Measures workflow completeness             |
| branching           | Measures branching accuracy                |
| external_services   | Measures integration extraction accuracy   |
| database_references | Measures DB/query extraction accuracy      |
| business_rules      | Measures business rule extraction accuracy |
| response_mappings   | Measures response mapping completeness     |
| error_mappings      | Measures error handling completeness       |
| test_scenarios      | Measures testing readiness                 |
| generator_readiness | Measures artifact generation readiness     |

---

## Evaluation Script Required Fields

Evaluation scripts need:

```text
- Source requirement references
- Extracted workflow elements
- Confidence values
- Expected fields
- Generated fields
- Missing fields
- Readiness status
- Gap categories
```

Schema v2 supports these concepts partially through source traceability and generator readiness.

---

## Evaluation Script Remaining Gaps

Evaluation needs scoring logic that is not yet defined.

Missing or future fields include:

```text
- Coverage scoring rules
- Accuracy scoring rules
- Confidence scoring rules
- Manual review status
- Human correction count
- Time-to-complete metrics
- Artifact quality rubric
- Baseline comparison fields
```

---

## Evaluation Script Readiness Status

```text
Partially Ready
```

## Reason

Schema v2 includes traceability and readiness metadata, but future days must define evaluation scoring methods and metrics.

---

# Generator Readiness Summary Table

| Generator               | Current Readiness | Main Strength                                          | Main Missing Area                          |
| ----------------------- | ----------------- | ------------------------------------------------------ | ------------------------------------------ |
| Node-RED Flow Generator | Mostly Ready      | Workflow steps and branching are structured            | Node type and layout metadata              |
| Excel Intake Generator  | Mostly Ready      | Endpoint, service, query, and rule data are structured | Sheet-column mapping rules                 |
| Go Code Generator       | Partially Ready   | Workflow logic is clear                                | Package, DTO, interface, and method naming |
| Documentation Generator | Ready             | Human-readable metadata is strong                      | Optional formatting/template hints         |
| Test Case Generator     | Mostly Ready      | Test scenarios are now structured                      | Framework-specific test conventions        |
| Evaluation Scripts      | Partially Ready   | Traceability and readiness exist                       | Scoring methods and metrics                |

---

# Recommended Future Enhancements

The following enhancements should be considered in future days.

## 1. Node-RED-Specific Metadata

Add fields such as:

```text
node_type
node_label
node_group
node_position
wire_to
subflow_reference
```

## 2. Excel Sheet Mapping Metadata

Add fields such as:

```text
target_sheet
target_column
row_id
cross_sheet_reference
required_column
```

## 3. Go Implementation Metadata

Add fields such as:

```text
package_name
struct_name
interface_name
method_name
repository_method
client_method
mapper_function
error_type
```

## 4. Documentation Template Metadata

Add fields such as:

```text
document_type
audience
section_order
include_examples
include_diagrams
```

## 5. Test Framework Metadata

Add fields such as:

```text
test_type
test_framework
mock_strategy
fixture_source
assertion_style
```

## 6. Evaluation Metrics Metadata

Add fields such as:

```text
coverage_score
accuracy_score
confidence_score
manual_review_required
human_correction_count
time_saved_minutes
```

---

# Final Assessment

Workflow Schema v2 is a major improvement over Schema v1.

It is now strong enough to support planning for future artifact generators.

The schema is especially strong for:

* Documentation generation
* Excel intake planning
* Node-RED flow planning
* Test scenario planning

It is partially ready for:

* Go code generation
* Evaluation scripts

The next major work is not to code immediately, but to continue refining generator-specific requirements so that future implementation days can start with a clear contract.

---

# Final Summary

Schema v2 gives the project a stronger bridge between extracted API requirements and future generated artifacts.

It provides structured information for:

* What the API does
* What fields it accepts
* How validation works
* What steps run
* Which branches exist
* Which downstream services are called
* Which database queries are needed
* Which business rules apply
* How calculations are performed
* How responses are mapped
* How errors are handled
* How tests should be created
* How source requirements are traced
* How ready each generator is

This confirms that Workflow Schema v2 is a suitable foundation for the next stage of the 50-Day AI Software Engineer Challenge.
