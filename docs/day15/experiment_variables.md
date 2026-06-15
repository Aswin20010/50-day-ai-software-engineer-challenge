# Day 15 — Phase 4: Experiment Variables

## Purpose

This document defines the experiment variables for the research evaluation.

The purpose of defining variables is to make the experiment structured, measurable, and repeatable.

The experiment evaluates whether an AI-assisted enterprise API artifact generation and gap analysis framework can improve the review process for API development artifacts.

The comparison focuses on:

```text
Manual Review
        vs
AI-Assisted Gap Analysis
```

The experiment uses benchmark API cases and compares results across Confluence requirements, Node-RED workflows, and Excel intake workbooks.

---

## Why Experiment Variables Are Needed

A research experiment must clearly define what is being changed, what is being measured, and what is being controlled.

Without variables, the evaluation may become unclear or inconsistent.

Experiment variables help answer:

```text
What input artifacts are used?
What review approach is being compared?
What outputs are measured?
What factors are kept the same?
What results prove whether the framework is useful?
```

Defining variables makes the experiment stronger and more research-ready.

---

# Variable Categories

The experiment variables are grouped into five categories:

```text
Independent Variables
Dependent Variables
Controlled Variables
Input Variables
Output Variables
```

Each category is defined below.

---

# 1. Independent Variables

## Definition

Independent variables are the factors that are changed or compared during the experiment.

In this project, the main independent variable is the review approach.

---

## Primary Independent Variable

```text
Review Approach
```

The experiment compares two review approaches:

```text
Manual Review
AI-Assisted Gap Analysis
```

---

## Manual Review

Manual review means a human reviewer manually compares the artifacts and identifies expected gaps.

The reviewer compares:

```text
Confluence Requirement
Node-RED Workflow
Excel Intake Workbook
```

The reviewer produces the manual baseline.

---

## AI-Assisted Gap Analysis

AI-assisted gap analysis means the framework reviews the same artifacts and generates a structured gap report.

The framework identifies:

```text
Missing information
Incorrect mappings
Validation gaps
Business rule gaps
Integration gaps
Technology mismatches
Unsupported assumptions
```

---

## Purpose of the Independent Variable

The goal is to measure whether changing the review approach from manual review to AI-assisted review improves productivity while maintaining review quality.

---

# 2. Dependent Variables

## Definition

Dependent variables are the results measured during the experiment.

These values change based on the review approach.

The experiment will measure whether AI-assisted review performs well compared with manual review.

---

## Main Dependent Variables

The main dependent variables are:

```text
Review time
Gap detection accuracy
False positive rate
False negative rate
Severity assignment accuracy
Recommendation usefulness
Requirement coverage
Workflow completeness
Excel completeness
Code generation readiness
Manual review reduction
```

Each dependent variable is explained below.

---

## Review Time

### Definition

Review time measures how long each review approach takes.

### Measurement

```text
Manual Review Time
AI-Assisted Review Time
```

### Purpose

This measures productivity improvement.

### Formula

```text
Manual Review Reduction =
(Manual Review Time - AI-Assisted Review Time) / Manual Review Time × 100
```

---

## Gap Detection Accuracy

### Definition

Gap detection accuracy measures how many expected gaps are correctly detected by the AI-assisted framework.

### Formula

```text
Gap Detection Accuracy =
True Positive Gaps / Total Expected Gaps × 100
```

### Purpose

This measures whether AI can find the same important gaps as the manual baseline.

---

## False Positive Rate

### Definition

False positive rate measures how many AI-reported gaps are not real issues.

### Formula

```text
False Positive Rate =
False Positive Gaps / Total Detected Gaps × 100
```

### Purpose

This measures whether the framework reports too many unnecessary gaps.

---

## False Negative Rate

### Definition

False negative rate measures how many real gaps were missed by the AI-assisted framework.

### Formula

```text
False Negative Rate =
Missed Gaps / Total Expected Gaps × 100
```

### Purpose

This measures whether important issues are being missed.

---

## Severity Assignment Accuracy

### Definition

Severity assignment accuracy measures whether the AI assigns the same severity as the manual baseline for correctly detected gaps.

### Formula

```text
Severity Assignment Accuracy =
Correct Severity Assignments / Total True Positive Gaps × 100
```

### Purpose

This measures whether the framework prioritizes gaps correctly.

---

## Recommendation Usefulness

### Definition

Recommendation usefulness measures whether the AI-provided fix recommendation is clear and actionable.

### Scoring Scale

| Score | Meaning                              |
| ----- | ------------------------------------ |
| 5     | Very clear and directly actionable   |
| 4     | Mostly clear and useful              |
| 3     | Understandable but needs improvement |
| 2     | Vague or partially incorrect         |
| 1     | Not useful                           |

### Purpose

This measures whether the gap report can actually help engineers fix issues.

---

## Requirement Coverage

### Definition

Requirement coverage measures how much of the Confluence requirement is captured in the derived artifacts.

### Formula

```text
Requirement Coverage =
Captured Requirement Elements / Total Requirement Elements × 100
```

### Purpose

This measures how well the system preserves original requirements.

---

## Workflow Completeness

### Definition

Workflow completeness measures how well the Node-RED workflow captures the expected requirement steps.

### Formula

```text
Workflow Completeness =
Required Workflow Steps Present / Total Required Workflow Steps × 100
```

### Purpose

This measures the quality of workflow representation.

---

## Excel Completeness

### Definition

Excel completeness measures how complete the Excel intake workbook is for Go code generation.

### Formula

```text
Excel Completeness =
Completed Required Workbook Fields / Total Required Workbook Fields × 100
```

### Purpose

This measures whether the workbook is ready for the Go factory.

---

## Code Generation Readiness

### Definition

Code generation readiness measures whether the artifact set is ready to move toward future Go code generation.

### Purpose

This combines requirement coverage, workflow completeness, Excel completeness, gap quality, and artifact consistency into a final readiness score.

---

# 3. Controlled Variables

## Definition

Controlled variables are factors that should remain the same across manual review and AI-assisted review.

Keeping these variables constant makes the comparison fair.

---

## Controlled Variable 1: Input Artifacts

Both manual and AI-assisted review must use the same artifacts.

```text
confluence_requirement.md
node_red_flow.json
excel_intake_summary.md
```

The AI-assisted review should not receive extra hidden information.

---

## Controlled Variable 2: Experiment Cases

Both approaches should evaluate the same benchmark API cases.

Planned cases:

```text
API-001 — Interrogate Account API
API-002 — Evaluate Transaction Discount API
API-003 — Fabrics Wrapper API
API-004 — Clienteling Create Message API
```

---

## Controlled Variable 3: Gap Schema

Both manual and AI outputs should use the same gap schema.

The schema should include:

```text
gap_id
gap_type
gap_category_group
source_of_truth
compared_source
missing_from
affected_artifact
affected_section
affected_field
severity
confidence
description
evidence
recommendation
status
```

---

## Controlled Variable 4: Severity Levels

Both approaches should use the same severity levels:

```text
Critical
High
Medium
Low
Informational
```

Severity should follow the Day 13 severity rules.

---

## Controlled Variable 5: Evaluation Metrics

The same metrics should be used across all cases.

Examples:

```text
Gap Detection Accuracy
False Positive Rate
False Negative Rate
Severity Assignment Accuracy
Manual Review Reduction
Code Generation Readiness Score
```

---

## Controlled Variable 6: Matching Rules

The same matching rules should be used to compare manual and AI gaps.

Matching rules include:

```text
Exact match
Partial match
False positive
False negative
Severity match
Severity mismatch
```

---

## Controlled Variable 7: Benchmark Difficulty Level

Each benchmark case should keep its planned difficulty level.

Example:

```text
API-001 = Level 2
API-002 = Level 3
API-003 = Level 2
API-004 = Level 3
```

This helps compare results across API complexity levels.

---

# 4. Input Variables

## Definition

Input variables are the artifacts and case details provided to the review process.

These are the materials that manual and AI-assisted review will analyze.

---

## Input Artifact 1: Confluence Requirement

### Purpose

The Confluence requirement acts as the primary source of truth.

### Contents

It may include:

```text
Endpoint path
HTTP method
Headers
Request body
Response body
Validation rules
Business rules
External services
Database references
Error handling expectations
Technology assumptions
```

---

## Input Artifact 2: Node-RED Workflow

### Purpose

The Node-RED workflow represents the workflow implementation or prototype.

### Contents

It may include:

```text
HTTP input node
Validation nodes
Transformation nodes
Downstream service nodes
Database nodes
Business rule nodes
Response mapping nodes
Error handling nodes
HTTP response node
```

---

## Input Artifact 3: Excel Intake Workbook

### Purpose

The Excel intake workbook represents the structured input for the Go code generation factory.

### Contents

It may include sheets such as:

```text
technical_requirements
api_endpoints
entities
external_services
business_requirements
endpoint_service_mapping
queries
```

---

## Input Artifact 4: Manual Baseline

### Purpose

The manual baseline is the expected answer used to evaluate AI output.

### Contents

It includes:

```text
expected_gaps.json
manual_baseline_gaps.md
manual_review_time.json
```

---

## Input Artifact 5: AI-Assisted Gap Report

### Purpose

The AI-assisted gap report is the framework output.

### Contents

It includes:

```text
detected_gaps.json
gap_analysis_report.md
ai_review_time.json
```

---

# 5. Output Variables

## Definition

Output variables are the final results produced by the experiment.

These values are used to evaluate performance.

---

## Output Variable 1: Evaluation Result

The evaluation result contains the calculated metrics for each benchmark case.

Example:

```json
{
  "benchmark_id": "api_001_interrogate_account",
  "gap_detection_accuracy": "87.5%",
  "false_positive_rate": "22.2%",
  "false_negative_rate": "12.5%",
  "manual_review_reduction": "71.4%"
}
```

---

## Output Variable 2: Final Readiness Status

The final readiness status indicates whether the artifact set can move toward code generation review.

Possible values:

```text
Ready for Code Generation Review
Ready with Minor Cleanup
Needs Review
Blocked
Not Ready
```

---

## Output Variable 3: Gap Comparison Summary

The gap comparison summary shows how manual and AI outputs matched.

It should include:

```text
Expected gaps
Detected gaps
True positives
False positives
False negatives
Partial matches
Severity matches
Severity mismatches
```

---

## Output Variable 4: Research Summary Table

The research summary table combines results across all experiment cases.

Example:

| Case ID | Accuracy | FP Rate | FN Rate | Review Reduction | Readiness |
| ------- | -------: | ------: | ------: | ---------------: | --------- |
| API-001 |    87.5% |   22.2% |   12.5% |            71.4% | Good      |
| API-002 |    83.3% |   18.0% |   16.7% |            68.0% | Moderate  |
| API-003 |    85.7% |   14.2% |   14.3% |            72.0% | Good      |
| API-004 |    90.0% |   12.0% |   10.0% |            70.5% | Excellent |

---

# Experiment Variable Table

| Variable Type | Variable                     | Description                                |
| ------------- | ---------------------------- | ------------------------------------------ |
| Independent   | Review Approach              | Manual review vs AI-assisted review        |
| Dependent     | Review Time                  | Time required to complete review           |
| Dependent     | Gap Detection Accuracy       | Percentage of expected gaps found          |
| Dependent     | False Positive Rate          | Percentage of AI gaps that are incorrect   |
| Dependent     | False Negative Rate          | Percentage of expected gaps missed         |
| Dependent     | Severity Assignment Accuracy | Correctness of severity labels             |
| Dependent     | Recommendation Usefulness    | Actionability of recommendations           |
| Dependent     | Requirement Coverage         | Requirement elements captured              |
| Dependent     | Excel Completeness           | Workbook completeness for Go factory       |
| Controlled    | Input Artifacts              | Same Confluence, Node-RED, and Excel files |
| Controlled    | Gap Schema                   | Same gap schema for both approaches        |
| Controlled    | Severity Levels              | Same severity classification rules         |
| Controlled    | Metrics                      | Same formulas across all cases             |
| Input         | Confluence Requirement       | Source-of-truth requirement artifact       |
| Input         | Node-RED Workflow            | Workflow representation artifact           |
| Input         | Excel Intake Workbook        | Code generation intake artifact            |
| Output        | Evaluation Result            | Final metrics and comparison results       |
| Output        | Readiness Status             | Code generation readiness decision         |

---

# Expected Target Values

The experiment will use the following target values.

| Metric                          |          Target |
| ------------------------------- | --------------: |
| Requirement Coverage            |   85% or higher |
| Workflow Completeness Score     |   80% or higher |
| Excel Completeness Score        |   90% or higher |
| Gap Detection Accuracy          |   85% or higher |
| False Positive Rate             |    15% or lower |
| False Negative Rate             |    15% or lower |
| Severity Assignment Accuracy    |   80% or higher |
| Recommendation Usefulness Score | 4.0/5 or higher |
| Manual Review Reduction         |   70% or higher |
| Code Generation Readiness Score |   85% or higher |

---

# Variable Control Rules

## Rule 1: Use the Same Artifacts for Both Approaches

Manual review and AI-assisted review must analyze the same source files.

This makes the comparison fair.

---

## Rule 2: Use the Same Metric Formulas

All metrics should use the formulas defined in Day 14.

This keeps evaluation consistent.

---

## Rule 3: Use the Same Gap Categories

Both manual and AI gap reports should use the same gap categories from Day 13.

This makes matching easier.

---

## Rule 4: Track Review Time Separately

Manual review time and AI-assisted review time should be recorded separately.

This is required to calculate manual review reduction.

---

## Rule 5: Keep Ambiguous Cases Separate

Ambiguous requirement cases should be marked as informational or low-confidence.

They should not unfairly increase false positive or false negative counts.

---

## Rule 6: Do Not Change the Benchmark During Evaluation

Once an experiment case begins, its artifacts should not be changed until that evaluation is completed.

This prevents unstable results.

---

# Threats to Validity

The experiment should also recognize possible threats to validity.

---

## Threat 1: Manual Baseline Bias

The manual reviewer may miss gaps or assign severity incorrectly.

Mitigation:

```text
Use a structured review checklist.
Use the same schema for every baseline.
Review Critical and High gaps carefully.
```

---

## Threat 2: Prompt Sensitivity

AI output may change depending on prompt wording.

Mitigation:

```text
Use a consistent prompt template.
Use the same gap schema.
Use benchmark examples repeatedly.
```

---

## Threat 3: Small Dataset Size

Four API cases may not represent all enterprise API patterns.

Mitigation:

```text
Start with four cases.
Expand later with SOAP, event-driven, and multi-endpoint APIs.
```

---

## Threat 4: Artifact Quality

Poorly written Confluence or Excel artifacts may affect both manual and AI review.

Mitigation:

```text
Keep source artifacts structured.
Mark ambiguous requirements clearly.
Add benchmark notes when needed.
```

---

## Threat 5: Overfitting to Known APIs

The framework may perform well only on familiar APIs.

Mitigation:

```text
Add new benchmark cases later.
Include unfamiliar API patterns.
Test on APIs with different structures.
```

---

# Summary

Experiment variables define what is changed, measured, controlled, provided, and produced in the research evaluation.

The main independent variable is the review approach:

```text
Manual Review vs AI-Assisted Gap Analysis
```

The main dependent variables are review time, gap detection accuracy, false positives, false negatives, severity accuracy, recommendation usefulness, artifact completeness, and code generation readiness.

Controlled variables ensure that both approaches are compared fairly.

By defining variables clearly, the project becomes more structured, measurable, and research-ready.
