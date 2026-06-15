# Day 15 — Phase 3: Manual vs AI-Assisted Comparison

## Purpose

This document defines how the research experiment will compare manual artifact review against AI-assisted gap analysis.

The goal is to measure whether the AI-assisted framework can reduce review effort while still maintaining strong accuracy, completeness, and code generation readiness.

The comparison will evaluate how well each approach identifies gaps between:

1. Confluence requirement documents
2. Node-RED workflow implementations
3. Excel intake workbooks

---

## Why This Comparison Is Needed

The project needs to prove that AI-assisted review provides measurable value.

It is not enough to say that AI can generate or review artifacts.

The project must show whether AI-assisted review performs better, faster, or more consistently than manual review.

This comparison helps answer:

```text
Does AI reduce manual review time?
Does AI detect the same important gaps as a human reviewer?
Does AI miss critical issues?
Does AI report too many false gaps?
Does AI provide useful recommendations?
Does AI help determine whether the Excel workbook is ready for Go code generation?
```

This makes the project measurable and research-ready.

---

## Comparison Objective

The main objective is to compare two review approaches:

```text
Approach 1: Manual Review
Approach 2: AI-Assisted Gap Analysis
```

The manual review acts as the baseline.

The AI-assisted review is measured against that baseline.

The experiment should evaluate whether the AI-assisted approach can achieve similar or better review quality while reducing the amount of human effort required.

---

# Approach 1 — Manual Review

## Definition

Manual review is the process where a human reviewer compares the Confluence requirement, Node-RED workflow, and Excel intake workbook manually.

The reviewer identifies expected gaps and records them in a structured baseline report.

---

## Manual Review Inputs

The manual reviewer will use the following artifacts:

```text
confluence_requirement.md
node_red_flow.json
excel_intake_summary.md
```

or equivalent real project files.

---

## Manual Review Tasks

The reviewer should manually check:

```text
Endpoint completeness
Header completeness
Request body completeness
Response body completeness
Validation completeness
Enum coverage
Business rule coverage
Downstream service coverage
Database query coverage
Workflow step coverage
Field mapping correctness
Response mapping correctness
Technology alignment
Error handling coverage
Unsupported assumptions
```

---

## Manual Review Output

The manual reviewer should produce:

```text
expected_gaps.json
manual_baseline_gaps.md
manual_review_time.json
```

The expected gaps file becomes the ground truth for evaluation.

---

## Manual Review Strengths

Manual review is useful because human reviewers can understand context, ambiguity, and business intent.

Strengths include:

* Strong contextual understanding
* Ability to interpret unclear requirements
* Ability to detect business logic inconsistencies
* Ability to identify undocumented assumptions
* Good judgment for severity assignment

---

## Manual Review Limitations

Manual review also has limitations.

These include:

* Time-consuming process
* Inconsistent results across reviewers
* Risk of missing repetitive details
* Difficulty comparing many fields across artifacts
* Hard to scale across many APIs
* Manual effort required for every review cycle

The AI-assisted framework is intended to reduce these limitations.

---

# Approach 2 — AI-Assisted Gap Analysis

## Definition

AI-assisted gap analysis is the process where the framework compares the same artifacts and generates a structured gap report.

The AI-assisted framework should identify missing, incorrect, inconsistent, unsupported, or ambiguous information.

---

## AI-Assisted Review Inputs

The AI-assisted review will use the same input artifacts as manual review:

```text
confluence_requirement.md
node_red_flow.json
excel_intake_summary.md
```

The same input artifacts must be used so that the comparison is fair.

---

## AI-Assisted Review Tasks

The framework should identify gaps such as:

```text
Missing Endpoint Gap
Missing Header Gap
Missing Request Field Gap
Missing Response Field Gap
Missing Validation Gap
Missing Enum Validation Gap
Missing Business Rule Gap
Missing Downstream Service Gap
Missing Database Query Gap
Missing Entity Gap
Incorrect Field Mapping Gap
Incorrect Response Mapping Gap
Incorrect Service Mapping Gap
Incorrect Data Type Gap
Incorrect Required or Optional Status Gap
Technology Mismatch Gap
Error Handling Gap
Extra Artifact Gap
Naming Mismatch Gap
Workflow Step Gap
```

---

## AI-Assisted Review Output

The AI-assisted framework should produce:

```text
detected_gaps.json
gap_analysis_report.md
evaluation_result.json
```

Each detected gap should follow the schema defined in Day 13.

Each gap should include:

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

## AI-Assisted Review Strengths

AI-assisted review can help by:

* Reviewing repetitive fields faster
* Comparing multiple artifacts consistently
* Producing structured gap reports
* Suggesting recommendations
* Supporting repeatable evaluation
* Reducing manual review time
* Helping identify code generation blockers early

---

## AI-Assisted Review Limitations

AI-assisted review may still have limitations.

These include:

* Possible false positives
* Possible false negatives
* Difficulty understanding ambiguous business language
* Risk of over-classifying minor naming differences
* Need for human confirmation on critical issues
* Dependence on artifact quality and prompt design

Because of these limitations, the AI-assisted framework should support human review rather than fully replace it.

---

# Comparison Process

The comparison process should follow a repeatable sequence.

---

## Step 1: Prepare Input Artifacts

For each experiment case, prepare the required artifacts.

Example:

```text
benchmarks/api_001_interrogate_account/
├── confluence_requirement.md
├── node_red_flow.json
├── excel_intake_summary.md
└── benchmark_notes.md
```

The same artifacts should be used for both manual and AI-assisted review.

---

## Step 2: Perform Manual Review

A human reviewer compares the artifacts and creates the expected gap baseline.

Output files:

```text
expected_gaps.json
manual_baseline_gaps.md
manual_review_time.json
```

The reviewer should record review start time, end time, total minutes, and expected gaps found.

---

## Step 3: Perform AI-Assisted Gap Analysis

The AI-assisted framework reviews the same artifacts and produces detected gaps.

Output files:

```text
detected_gaps.json
gap_analysis_report.md
ai_review_time.json
```

The AI-assisted review time should also be recorded.

---

## Step 4: Compare Manual and AI Outputs

The expected gaps from manual review should be compared with the detected gaps from AI-assisted review.

The comparison should identify:

```text
True positives
False positives
False negatives
Severity matches
Severity mismatches
Recommendation quality
```

---

## Step 5: Calculate Metrics

The comparison should calculate metrics such as:

```text
Gap Detection Accuracy
False Positive Rate
False Negative Rate
Severity Assignment Accuracy
Recommendation Usefulness Score
Manual Review Reduction
Code Generation Readiness Score
```

---

## Step 6: Produce Evaluation Result

The final comparison result should be stored as:

```text
evaluation_result.json
evaluation_summary.md
```

This result will later support research reporting.

---

# Matching Rules

To compare manual and AI gap reports, clear matching rules are needed.

---

## Exact Match

An exact match occurs when the manual and AI reports identify the same issue with the same affected field and artifact.

Example:

```text
Manual: x-macys-apikey missing from Excel api_endpoints.required_headers.
AI: x-macys-apikey missing from Excel api_endpoints.required_headers.
```

Result:

```text
True Positive
```

---

## Partial Match

A partial match occurs when the AI detects the correct issue but uses slightly different wording or a broader affected section.

Example:

```text
Manual: entryMode enum validation missing from Excel api_endpoints.validation_rules.
AI: entryMode validation incomplete in Excel.
```

Result:

```text
True Positive with partial match
```

Partial matches should be accepted if the issue and affected artifact are clearly the same.

---

## False Positive

A false positive occurs when AI reports a gap that is not present in the manual baseline and is not a valid additional issue.

Example:

```text
AI reports Division-id vs division_id as a gap, but the artifacts include a clear mapping showing they are equivalent.
```

Result:

```text
False Positive
```

---

## False Negative

A false negative occurs when the manual baseline contains a gap that the AI-assisted framework missed.

Example:

```text
Manual baseline identifies missing HMAC authorization mapping.
AI output does not report this issue.
```

Result:

```text
False Negative
```

---

## Severity Match

A severity match occurs when AI assigns the same severity as the manual baseline.

Example:

```text
Manual Severity: High
AI Severity: High
```

Result:

```text
Severity Match
```

---

## Severity Mismatch

A severity mismatch occurs when AI detects the correct gap but assigns a different severity.

Example:

```text
Manual Severity: Critical
AI Severity: Medium
```

Result:

```text
Severity Mismatch
```

The gap is still counted as detected, but severity assignment accuracy should be reduced.

---

# Metrics Used in the Comparison

## 1. Gap Detection Accuracy

```text
Gap Detection Accuracy = True Positive Gaps / Total Expected Gaps × 100
```

This measures how many expected gaps were found by the AI-assisted framework.

---

## 2. False Positive Rate

```text
False Positive Rate = False Positive Gaps / Total Detected Gaps × 100
```

This measures how many AI-reported gaps were not real issues.

---

## 3. False Negative Rate

```text
False Negative Rate = Missed Gaps / Total Expected Gaps × 100
```

This measures how many manual baseline gaps were missed by AI.

---

## 4. Severity Assignment Accuracy

```text
Severity Assignment Accuracy = Correct Severity Assignments / Total True Positive Gaps × 100
```

This measures whether AI prioritized detected gaps correctly.

---

## 5. Manual Review Reduction

```text
Manual Review Reduction =
(Manual Review Time - AI-Assisted Review Time) / Manual Review Time × 100
```

This measures time saved by AI-assisted review.

---

## 6. Recommendation Usefulness Score

Recommendations should be reviewed on a 1 to 5 scale.

| Score | Meaning                              |
| ----- | ------------------------------------ |
| 5     | Very clear and directly actionable   |
| 4     | Mostly clear and useful              |
| 3     | Understandable but needs improvement |
| 2     | Vague or partially incorrect         |
| 1     | Not useful                           |

---

# Example Comparison Result

```json
{
  "benchmark_id": "api_001_interrogate_account",
  "manual_expected_gaps": 8,
  "ai_detected_gaps": 9,
  "true_positives": 7,
  "partial_matches": 1,
  "false_positives": 2,
  "false_negatives": 1,
  "severity_matches": 6,
  "severity_mismatches": 1,
  "gap_detection_accuracy": "87.5%",
  "false_positive_rate": "22.2%",
  "false_negative_rate": "12.5%",
  "severity_assignment_accuracy": "85.7%",
  "manual_review_time_minutes": 105,
  "ai_assisted_review_time_minutes": 30,
  "manual_review_reduction": "71.4%",
  "recommendation_usefulness_score": "4.2/5"
}
```

---

# Comparison Summary Table

For research reporting, results can be shown in table format.

| Benchmark ID | Expected Gaps | AI Detected | True Positives | False Positives | False Negatives | Accuracy | Review Reduction |
| ------------ | ------------: | ----------: | -------------: | --------------: | --------------: | -------: | ---------------: |
| API-001      |             8 |           9 |              7 |               2 |               1 |    87.5% |            71.4% |
| API-002      |            12 |          13 |             10 |               3 |               2 |    83.3% |            68.0% |
| API-003      |             7 |           7 |              6 |               1 |               1 |    85.7% |            72.0% |
| API-004      |            10 |          11 |              9 |               2 |               1 |    90.0% |            70.5% |

---

# Fairness Rules

The comparison should follow fairness rules so the experiment is valid.

---

## Rule 1: Same Inputs

Manual review and AI-assisted review must use the same artifacts.

Do not give AI extra information that the manual reviewer did not have.

---

## Rule 2: Same Gap Schema

Manual and AI outputs should use the same gap schema.

This makes comparison easier and more consistent.

---

## Rule 3: Manual Baseline Comes First

The manual baseline should be created before evaluating the AI output.

This reduces bias.

---

## Rule 4: Allow Partial Matches

AI wording may differ from manual wording.

A match should be accepted if the same gap is detected with the same affected artifact and field.

---

## Rule 5: Track Ambiguous Cases Separately

If a case is unclear, it should be marked as ambiguous or informational.

Ambiguous cases should not be used unfairly to inflate false positives or false negatives.

---

## Rule 6: Do Not Treat AI as Final Authority

AI-assisted review should support human reviewers.

Critical and High gaps should still be reviewed by a human before final approval.

---

# Expected Outcome

The manual vs AI-assisted comparison should show whether the framework can:

```text
Detect most expected gaps
Avoid excessive false positives
Avoid missing important gaps
Assign useful severity levels
Provide actionable recommendations
Reduce review time
Improve code generation readiness
```

The expected target outcomes are:

| Metric                          | Target          |
| ------------------------------- | --------------- |
| Gap Detection Accuracy          | 85% or higher   |
| False Positive Rate             | 15% or lower    |
| False Negative Rate             | 15% or lower    |
| Severity Assignment Accuracy    | 80% or higher   |
| Recommendation Usefulness Score | 4.0/5 or higher |
| Manual Review Reduction         | 70% or higher   |

---

# Research Value

This comparison is important for the research direction:

```text
Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis
```

It provides evidence for whether the framework improves the development process.

The comparison can support research claims such as:

```text
AI-assisted review reduced manual review effort by 70%.
AI-assisted gap analysis achieved 85%+ detection accuracy.
AI-assisted review helped identify code generation blockers before implementation.
```

These claims should only be made after the experiment produces actual measured results.

---

## What This Phase Does Not Include

This phase does not include:

* Running the comparison
* Creating actual benchmark outputs
* Calculating live results
* Implementing automation
* Writing parsing logic
* Generating Go code
* Finalizing research claims

This phase only defines how manual and AI-assisted review will be compared.

---

## Summary

Manual vs AI-assisted comparison is the core evaluation method for the research experiment.

Manual review provides the baseline.

AI-assisted review provides the framework output.

By comparing both outputs, the project can measure accuracy, missed gaps, false positives, severity quality, recommendation usefulness, and review-time reduction.

This comparison makes the project measurable, research-ready, and stronger for technical storytelling.
