# Day 12 — Extraction Evaluation Overview

## Purpose

This document defines the overview for evaluating requirement extraction quality.

Day 11 defined how raw API requirements should be mapped into Workflow Schema v2.

Day 12 focuses on how to evaluate whether that mapping was successful.

The goal is to create a clear evaluation framework that can measure whether a generated Workflow Schema v2 output correctly represents the original requirement source.

No coding is performed on Day 12.

---

## Background

The 50-Day AI Software Engineer Challenge is building an AI-assisted enterprise API development pipeline.

The pipeline is intended to convert raw requirement sources into structured artifacts such as:

* Workflow Schema v2 documents
* Node-RED flows
* Excel intake files
* Go service code
* Documentation
* Test cases
* Evaluation reports

Before any artifact generator can be trusted, the project must first validate the quality of the extracted workflow schema.

If the generated Schema v2 output is incomplete or inaccurate, every downstream artifact may also be incorrect.

Therefore, requirement extraction evaluation is a critical part of the project.

---

## Why Evaluation Is Needed

Requirement extraction is not only about producing an output.

It is about producing an output that is:

* Complete
* Accurate
* Traceable
* Consistent
* Generator-ready
* Reviewable by humans

A generated workflow schema may look correct visually, but still contain hidden problems.

Examples:

```text
The endpoint path may be correct, but required headers may be missing.

The validation rules may be present, but enum values may be incomplete.

The workflow steps may be listed, but branching conditions may be wrong.

The downstream service may be captured, but authentication rules may be missing.

The database query may be mentioned, but input and output mappings may be absent.

The business rule may be summarized, but the actual eligibility condition may be incorrect.

The response fields may be captured, but their source mappings may be missing.

The schema may be detailed, but not traceable back to the original requirement.
```

Because of these risks, extraction quality must be measured using a structured evaluation framework.

---

## Why Visual Review Alone Is Not Enough

A human reviewer can read a generated schema and say whether it seems reasonable.

However, visual review alone has limitations.

It may miss:

* Missing fields
* Incorrect source-to-target mappings
* Wrong conditions
* Missing branch paths
* Missing error behavior
* Missing test scenarios
* Incorrect assumptions
* Weak traceability
* Generator readiness gaps

Visual review is useful, but it is not enough for a repeatable research or engineering process.

The project needs a scoring framework that can evaluate extraction outputs consistently.

---

## Evaluation Objective

The objective of Day 12 is to define how extraction quality should be measured.

The evaluation framework should answer:

```text
Did the generated schema include all required sections?

Did it correctly capture the endpoint details?

Did it correctly capture headers and body fields?

Did it correctly capture validation rules?

Did it correctly capture workflow steps?

Did it correctly capture branching logic?

Did it correctly capture downstream services?

Did it correctly capture database references?

Did it correctly capture business rules?

Did it correctly capture request and response mappings?

Did it correctly capture error behavior?

Can each important extracted item be traced back to the source requirement?

Is the schema ready for future generators?
```

---

## What Is Being Evaluated

The main evaluation target is the generated Workflow Schema v2 document.

The evaluation compares:

```text
Original Requirement Source
        vs
Generated Workflow Schema v2
```

The original requirement source may include:

* Confluence requirement documents
* API contracts
* Request and response examples
* Node-RED prototype flows
* Existing workflow notes
* User-confirmed clarifications
* Excel intake expectations

The generated Workflow Schema v2 should be evaluated against these sources.

---

## Evaluation Dimensions

Day 12 defines four major evaluation dimensions.

```text
1. Schema Completeness
2. Requirement Accuracy
3. Source Traceability
4. Generator Readiness
```

Each dimension measures a different part of extraction quality.

---

# 1. Schema Completeness

## Definition

Schema completeness measures whether the generated Workflow Schema v2 contains the expected sections and fields.

It answers:

```text
Did the extraction produce the required schema structure?
```

## What It Checks

Schema completeness checks whether the output includes blocks such as:

* `metadata`
* `api_contract`
* `field_validations`
* `workflow_steps`
* `branching`
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
* `generator_readiness`

Not every API needs every block.

For example, a simple wrapper API may not need database references or calculation rules.

The scoring should consider whether a block is required for that workflow type.

---

## Example

A Fabric wrapper API may require:

```text
metadata
api_contract
field_validations
workflow_steps
external_services
request_mappings
response_mappings
error_mappings
source_traceability
generator_readiness
```

It may not require:

```text
database_references
calculation_rules
```

A Discount API may require almost every block because it has downstream services, database rules, branching, and calculations.

---

# 2. Requirement Accuracy

## Definition

Requirement accuracy measures whether the extracted schema details are correct when compared to the original requirement.

It answers:

```text
Did the extraction capture the requirement correctly?
```

## What It Checks

Requirement accuracy checks whether extracted details match the source requirement.

It evaluates:

* Endpoint path accuracy
* HTTP method accuracy
* Header accuracy
* Body field accuracy
* Validation accuracy
* Enum value accuracy
* Workflow step accuracy
* Branching condition accuracy
* External service accuracy
* Authentication accuracy
* Database query accuracy
* Business rule accuracy
* Calculation accuracy
* Request mapping accuracy
* Response mapping accuracy
* Error mapping accuracy

---

## Example

If the requirement says:

```text
Division-Id must be either 12 or 13.
```

The generated schema should not say:

```text
Division-Id must be either 11 or 12.
```

That would be an accuracy error.

If the requirement says:

```text
skuNumbers is an array of strings.
```

The generated schema should not treat it as a single string.

That would also be an accuracy error.

---

# 3. Source Traceability

## Definition

Source traceability measures whether extracted schema items can be traced back to the original requirement source.

It answers:

```text
Can we prove where this extracted detail came from?
```

## What It Checks

Traceability evaluates whether important schema elements include source references.

It checks:

* Requirement document name
* Requirement section
* Source text reference
* Step source
* Field source
* Business rule source
* Confidence level
* Notes for inferred details

Traceability is important because it helps humans review extraction quality.

It also supports research evaluation by showing how much of the generated schema is grounded in the original source.

---

## Example

A traceable field mapping may look like:

```yaml
source_traceability:
  field_sources:
    - field_path: request.headers.Division-Id
      requirement_section: Header Requirements
      source_text_reference: Section 2.2
      confidence: high
```

A weak traceability case may look like:

```yaml
source_traceability:
  field_sources:
    - field_path: request.headers.Division-Id
      requirement_section: unknown
      source_text_reference: inferred
      confidence: low
```

The second case may still be useful, but it should receive a lower score.

---

# 4. Generator Readiness

## Definition

Generator readiness measures whether the extracted schema contains enough structured information for future artifact generation.

It answers:

```text
Can this schema be safely used by future generators?
```

## What It Checks

Generator readiness evaluates whether the schema has enough detail for:

* Node-RED flow generation
* Excel intake generation
* Go code generation
* Documentation generation
* Test case generation
* Evaluation scripts

It checks whether required generator inputs are present.

For example:

## Node-RED Readiness

Needs:

* Ordered workflow steps
* Step types
* Branching paths
* Request mappings
* Response mappings
* Error paths
* External service calls

## Excel Readiness

Needs:

* API contract
* External services
* Business rules
* Database references
* Endpoint service mappings
* Queries
* Request and response mappings

## Go Readiness

Needs:

* Endpoint contract
* DTO fields
* Validation rules
* Service orchestration
* Client calls
* Repository references
* Mapper rules
* Error mappings
* Test scenarios

---

## Example

A schema may be complete enough for documentation generation but not ready for Go code generation.

Example:

```text
Documentation readiness: ready
Go readiness: partially_ready
```

This is acceptable as long as the readiness status clearly identifies missing fields.

---

# Evaluation Output

The evaluation process should eventually produce a scorecard.

A sample scorecard may include:

```text
Schema Completeness Score: 86%
Requirement Accuracy Score: 82%
Traceability Score: 78%
Generator Readiness Score: 70%
Overall Extraction Quality Score: 79%
```

The score should also include gap notes.

Example:

```text
Missing:
- HMAC authentication details
- New account cap calculation rule
- Database no-record behavior
- Negative test scenarios
```

---

# Recommended Scoring Scale

A simple scoring scale can be used initially.

```text
90–100% = Excellent
80–89%  = Good
70–79%  = Acceptable with review
60–69%  = Weak, needs correction
Below 60% = Not ready
```

This scale can be refined later.

---

# Evaluation Method

The evaluation should follow this process:

```text
1. Identify the original requirement source.
2. Identify the generated Workflow Schema v2 output.
3. Check required schema sections.
4. Compare extracted details against source requirements.
5. Check traceability for important fields, steps, and rules.
6. Check generator readiness.
7. Assign category scores.
8. Identify missing or incorrect items.
9. Produce an evaluation summary.
```

---

# Evaluation Categories

The Day 12 evaluation framework will be divided into the following files:

```text
docs/day12/
├── extraction_evaluation_overview.md
├── schema_completeness_scoring.md
├── requirement_accuracy_scoring.md
├── traceability_scoring.md
├── generator_readiness_scoring.md
└── day12_completion_report.md
```

Each file focuses on one part of the framework.

---

# Research Importance

This evaluation framework supports the research and paper goal of the project.

The research goal is not only to build a tool.

The goal is to show that AI-assisted requirement extraction can improve enterprise API development.

To make that claim credible, the project needs measurable evidence.

Possible research metrics include:

* Requirement extraction completeness
* Workflow extraction accuracy
* Artifact generation readiness
* Gap detection accuracy
* Human correction effort
* Time reduction
* Traceability coverage

The evaluation framework defined on Day 12 will make those measurements possible later.

---

# Relationship to Project Metrics

The project already has target metrics such as:

```text
Requirement extraction accuracy >= 85%
Flow generation accuracy >= 80%
Excel generation accuracy >= 90%
Gap detection accuracy >= 90%
Time reduction >= 70%
```

Day 12 begins converting these high-level goals into specific evaluation categories.

For example:

```text
Requirement extraction accuracy
```

can be measured through:

* Schema completeness
* Requirement accuracy
* Traceability

```text
Flow generation readiness
```

can be measured through:

* Workflow step completeness
* Branching completeness
* Node-RED readiness

```text
Excel generation readiness
```

can be measured through:

* API contract completeness
* External service completeness
* Business rule completeness
* Database reference completeness

---

# Evaluation Review Roles

In a future implementation, evaluation may include multiple review roles.

## 1. Human Reviewer

Checks whether the schema correctly represents the original requirement.

## 2. Automated Evaluator

Checks whether required schema fields are present.

## 3. Generator Readiness Checker

Checks whether the schema contains enough data for target artifact generation.

## 4. Gap Analyzer

Identifies missing or ambiguous information.

For Day 12, these roles are documented conceptually only.

No evaluator is implemented yet.

---

# Example Evaluation Scenario

## Source Requirement

```text
The Fabric API accepts POST /api/v1/fabrics. Division-Id is required and must be 12 or 13. The body must contain skuNumbers as a non-empty array of strings and fabricTypeId as an integer. The API calls Product Customization Fabric service and maps the downstream response.
```

## Generated Schema Findings

Assume the generated schema contains:

```text
- Correct endpoint path
- Correct HTTP method
- Correct Division-Id validation
- Correct fabricTypeId validation
- Missing skuNumbers item-level validation
- External service captured
- Missing downstream error mapping
- Source traceability present for endpoint and headers only
```

## Evaluation Summary

```text
Schema Completeness: Good
Requirement Accuracy: Acceptable with review
Traceability: Weak
Generator Readiness: Partially ready
```

## Gap Notes

```text
- Add skuNumbers[] item-level validation.
- Add downstream service error mapping.
- Add source traceability for body fields and external service.
```

This example shows why evaluation is useful before generation.

---

# What Day 12 Does Not Do

Day 12 does not:

* Implement an evaluator
* Write scoring scripts
* Parse documents automatically
* Generate workflow schemas
* Generate Node-RED flows
* Generate Excel files
* Generate Go code
* Run automated tests

Day 12 only defines the evaluation framework.

Implementation will happen in a later phase of the 50-day roadmap.

---

# Final Summary

Day 12 introduces the evaluation framework for requirement extraction.

The main idea is that a generated Workflow Schema v2 should be judged by four major dimensions:

```text
1. Schema Completeness
2. Requirement Accuracy
3. Source Traceability
4. Generator Readiness
```

This framework is important because it makes the AI-assisted pipeline measurable.

It also supports the research goal by giving the project a way to prove improvement instead of only demonstrating generation.

This completes Phase 1 of Day 12.
