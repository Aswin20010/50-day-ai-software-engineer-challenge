# Day 12 — Completion Report

## Theme

Requirement Extraction Evaluation Framework

---

## Purpose

Day 12 focused on defining how to evaluate the quality of extracted Workflow Schema v2 outputs.

Day 11 defined how raw requirements should be mapped into Workflow Schema v2.

Day 12 defined how to measure whether that mapping is complete, accurate, traceable, and ready for future generators.

No coding was performed on Day 12.

---

## Background

The 50-Day AI Software Engineer Challenge is building toward an AI-assisted enterprise API development pipeline.

The long-term goal is to convert enterprise requirement sources into structured engineering artifacts such as:

* Workflow Schema v2 documents
* Node-RED flows
* Excel intake files
* Go service code
* API documentation
* Test cases
* Evaluation reports

Before generated artifacts can be trusted, the project must first evaluate whether the extracted workflow schema correctly represents the original requirement.

Day 12 created the evaluation framework needed for that.

---

## Previous Work Recap

## Day 8

Day 8 created Workflow Schema v1.

Schema v1 introduced the first structured workflow representation layer.

It allowed API behavior to be described as workflow metadata, endpoint details, validation steps, service calls, database references, business rules, response mappings, and errors.

---

## Day 9

Day 9 validated Schema v1 against realistic APIs:

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

and

```text
POST /api/v1/fabrics
```

This validation showed that Schema v1 was useful, but it needed better support for:

* Branching
* Error mappings
* Authentication
* Database query references
* Calculation rules
* Test scenarios
* Source traceability
* Generator readiness

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

## Day 11

Day 11 defined how raw requirements map into Workflow Schema v2.

It created mapping rules for:

* API contract details
* Validation requirements
* Workflow steps
* External services
* Authentication
* Database references
* Business rules
* Calculation rules
* Request mappings
* Response mappings
* Error mappings

---

## Day 12 Focus

Day 12 answered the next important question:

```text
How do we evaluate whether the generated Workflow Schema v2 is good?
```

This matters because a generated schema can look detailed but still be wrong.

A strong evaluation framework helps identify:

* Missing schema blocks
* Incorrect extracted values
* Weak traceability
* Unsupported assumptions
* Generator readiness gaps

---

# Work Completed

The following files were created for Day 12:

```text
docs/day12/
├── extraction_evaluation_overview.md
├── schema_completeness_scoring.md
├── requirement_accuracy_scoring.md
├── traceability_scoring.md
├── generator_readiness_scoring.md
└── day12_completion_report.md
```

---

# Files Created

## 1. extraction_evaluation_overview.md

This file introduced the overall evaluation framework.

It explained why evaluation is required before generation.

It covered:

* Why extraction quality must be measured
* Why visual review alone is not enough
* What should be evaluated
* The difference between completeness, accuracy, traceability, and readiness
* How evaluation supports the research and paper goal
* How evaluation fits into the project metrics

The file defined four major evaluation dimensions:

```text
1. Schema Completeness
2. Requirement Accuracy
3. Source Traceability
4. Generator Readiness
```

---

## 2. schema_completeness_scoring.md

This file defined how to score whether a generated Workflow Schema v2 contains the expected structure.

It focused on the question:

```text
Did the generated schema include all required sections and fields?
```

It covered completeness scoring for:

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

It also introduced:

* Completeness score distribution
* Completeness status values
* Normalized scoring for non-applicable sections
* Completeness gap categories
* Completeness scorecard template

---

## 3. requirement_accuracy_scoring.md

This file defined how to score whether extracted schema values correctly match the original requirement source.

It focused on the question:

```text
Are the extracted values and meanings correct?
```

It covered accuracy scoring for:

* API contract accuracy
* Header accuracy
* Body field accuracy
* Validation accuracy
* Workflow step accuracy
* Branching accuracy
* External service accuracy
* Authentication accuracy
* Database reference accuracy
* Business rule accuracy
* Calculation rule accuracy
* Request mapping accuracy
* Response mapping accuracy
* Error mapping accuracy
* Test scenario accuracy

It also introduced:

* Accuracy score distribution
* Accuracy status values
* Accuracy scorecard template
* Accuracy gap categories
* Review checklist
* Example Fabric API and Discount API accuracy evaluations

---

## 4. traceability_scoring.md

This file defined how to score whether extracted schema items can be traced back to source requirement evidence.

It focused on the question:

```text
Can we prove where this extracted schema detail came from?
```

It covered traceability scoring for:

* Workflow source traceability
* API contract traceability
* Field validation traceability
* Workflow step traceability
* Branching traceability
* External service traceability
* Authentication traceability
* Database reference traceability
* Business rule traceability
* Calculation rule traceability
* Mapping traceability
* Error mapping traceability
* Confidence quality
* Inference transparency

It also introduced:

* Traceability score distribution
* Traceability status values
* Traceability scorecard template
* Traceability gap categories
* Confidence levels
* Inference handling

---

## 5. generator_readiness_scoring.md

This file defined how to score whether an extracted Workflow Schema v2 is ready for future artifact generators.

It focused on the question:

```text
Can this extracted schema be safely used by future generators?
```

It covered generator readiness scoring for:

* Node-RED flow generation
* Excel intake generation
* Go code generation
* Documentation generation
* Test case generation
* Evaluation script generation

It also defined:

* Readiness status values
* Generator readiness score distribution
* Generator readiness scorecard template
* Readiness gap categories
* Generator-specific review checklists

---

# Evaluation Dimensions Defined

Day 12 established the following four evaluation dimensions.

---

## 1. Schema Completeness

Schema completeness measures whether the extracted schema contains the required blocks and fields.

It checks structure.

Example question:

```text
Are field_validations, workflow_steps, response_mappings, error_mappings, source_traceability, and generator_readiness present?
```

Completeness does not check whether the values are correct.

It only checks whether required schema elements exist.

---

## 2. Requirement Accuracy

Requirement accuracy measures whether extracted values correctly match the original requirement.

It checks correctness.

Example question:

```text
Does the extracted Division-Id validation correctly say allowed values are 12 and 13?
```

Accuracy ensures that the schema preserves the meaning of the original requirement.

---

## 3. Source Traceability

Source traceability measures whether schema elements can be linked back to requirement evidence.

It checks grounding.

Example question:

```text
Can the employee discount eligibility rule be traced back to the requirement section where it was defined?
```

Traceability helps reviewers identify unsupported assumptions and inferred behavior.

---

## 4. Generator Readiness

Generator readiness measures whether the schema contains enough structured information for future artifact generation.

It checks usability.

Example question:

```text
Does the schema have enough workflow, mapping, service, and error details to generate a Node-RED flow or Excel intake file?
```

Readiness helps identify what is still missing before generation.

---

# Score Categories

Day 12 introduced a common scoring scale:

```text
90–100 = Excellent
80–89  = Good
70–79  = Acceptable with review
60–69  = Weak, needs correction
Below 60 = Not ready
```

This scale can be applied to:

* Schema completeness score
* Requirement accuracy score
* Traceability score
* Generator readiness score
* Overall extraction quality score

---

# Evaluation Scorecard Concept

A future evaluation report may produce a summary like this:

```text
Workflow Name: Evaluate Transaction Discount Workflow
Workflow ID: WF-DISCOUNT-001
Workflow Type: business_orchestration

Schema Completeness Score: 86%
Requirement Accuracy Score: 84%
Traceability Score: 78%
Generator Readiness Score: 75%

Overall Extraction Quality Score: 81%

Rating: Good

Key Gaps:
- Some calculation rules need stronger source traceability.
- Some database no-record behaviors are incomplete.
- Go generator readiness is only partially ready.
- More test scenarios are needed for branch and failure coverage.
```

---

# Overall Extraction Quality

Day 12 did not finalize a single formula, but it prepared the components needed for one.

A future overall quality score may combine:

```text
Schema Completeness
Requirement Accuracy
Source Traceability
Generator Readiness
```

A possible formula is:

```text
Overall Extraction Quality =
  (Schema Completeness * 0.25) +
  (Requirement Accuracy * 0.40) +
  (Source Traceability * 0.20) +
  (Generator Readiness * 0.15)
```

This weighting gives the highest importance to requirement accuracy.

The formula can be refined later during evaluation design.

---

# Why Accuracy Has the Highest Weight

Requirement accuracy should receive the highest weight because a complete schema is not useful if the extracted values are wrong.

For example:

```text
A schema may include all required blocks, but if the employee discount branch is reversed, the generated artifacts will be incorrect.
```

Therefore, accuracy should influence the overall score more than the other categories.

---

# Normalized Scoring

Day 12 also introduced normalized scoring.

This is important because not every workflow needs every Schema v2 block.

For example, a simple wrapper API may not need:

* Database references
* Business rules
* Calculation rules

These sections should be marked:

```text
not_applicable
```

Instead of being counted as missing.

The normalized scoring formula is:

```text
Score = (Earned Applicable Points / Total Applicable Points) * 100
```

---

# Example Workflow Types

Day 12 scoring supports multiple workflow types.

## Simple Wrapper API

Example:

```text
POST /api/v1/fabrics
```

Likely requires:

* API contract
* Field validations
* Workflow steps
* External service
* Request mappings
* Response mappings
* Error mappings
* Test scenarios
* Traceability
* Generator readiness

May not require:

* Database references
* Calculation rules

---

## Business Orchestration API

Example:

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

Likely requires:

* API contract
* Field validations
* Workflow steps
* Branching
* External service
* Authentication
* Database references
* Business rules
* Calculation rules
* Request mappings
* Response mappings
* Error mappings
* Test scenarios
* Traceability
* Generator readiness

---

# Day 12 Success Criteria Review

| Success Criteria                     | Status   |
| ------------------------------------ | -------- |
| Evaluation overview documented       | Complete |
| Schema completeness scoring defined  | Complete |
| Requirement accuracy scoring defined | Complete |
| Traceability scoring defined         | Complete |
| Generator readiness scoring defined  | Complete |
| Completion report created            | Complete |
| No coding performed                  | Complete |

---

# Final Folder Structure

At the end of Day 12, the folder should look like this:

```text
docs/day12/
├── extraction_evaluation_overview.md
├── schema_completeness_scoring.md
├── requirement_accuracy_scoring.md
├── traceability_scoring.md
├── generator_readiness_scoring.md
└── day12_completion_report.md
```

---

# Final Status

Day 12 is complete.

The project now has a clear framework for evaluating requirement extraction quality.

The evaluation framework can measure:

* Whether the generated schema is structurally complete
* Whether extracted values are accurate
* Whether schema elements are traceable to source requirements
* Whether the schema is ready for future generators

This makes the project more research-ready and engineering-ready.

---

# Recommended Next Step

Day 13 should apply the Day 12 evaluation framework to sample workflows.

The recommended Day 13 theme is:

```text
Sample Workflow Evaluation Using the Day 12 Framework
```

Day 13 should evaluate at least two sample workflows:

```text
1. Fabric API workflow
2. Discount API workflow
```

The goal of Day 13 is to test whether the evaluation framework is practical and whether the scorecards are easy to use.
