# Day 14 — Phase 4: Manual Review Baseline

## Purpose

This document defines the manual review baseline for evaluating the Gap Analysis Framework.

The manual review baseline is the human-reviewed reference result used to compare against the AI-assisted gap analysis output.

The purpose of this baseline is to measure whether the framework can correctly detect gaps between:

1. Confluence requirement documents
2. Node-RED workflow implementations
3. Excel intake workbooks

The baseline acts as the ground truth for evaluation.

---

## Why a Manual Review Baseline Is Needed

A Gap Analysis Framework cannot be evaluated only by looking at its own output.

There must be a trusted reference to compare against.

A manual review baseline provides that reference.

It helps answer:

```text
What gaps should have been found?
Which detected gaps are correct?
Which detected gaps are false positives?
Which real gaps were missed?
Was the severity assigned correctly?
Was the recommendation useful?
```

Without a manual baseline, it would be difficult to know whether the framework is accurate.

---

## Definition

A manual review baseline is a gap report created by a human reviewer before or alongside the AI-assisted evaluation.

The reviewer manually compares:

```text
Confluence Requirement
        ↓
Node-RED Workflow
        ↓
Excel Intake Workbook
```

and records all expected gaps.

These expected gaps become the reference answer for evaluation.

---

## Role in the Evaluation Process

The manual review baseline is used to calculate important metrics such as:

```text
Gap Detection Accuracy
False Positive Rate
False Negative Rate
Severity Assignment Accuracy
Recommendation Usefulness Score
Manual Review Reduction
```

The AI-assisted output is compared against the manually prepared baseline.

---

## Manual Review Inputs

The human reviewer should review the same artifacts that the framework will review.

For each benchmark API, the reviewer should use:

```text
confluence_requirement.md
node_red_flow.json
excel_intake_summary.md
```

or their equivalent real project files.

The reviewer should not rely on memory or assumptions.

Every manually identified gap should be traceable to evidence from the artifacts.

---

## Manual Review Output

The manual reviewer should produce an expected gap report.

Recommended file name:

```text
expected_gaps.json
```

or:

```text
manual_baseline_gaps.md
```

Each expected gap should follow the same schema defined in Day 13.

Example:

```json
{
  "gap_id": "GAP-001",
  "gap_type": "Missing Header Gap",
  "gap_category_group": "Contract Gap",
  "source_of_truth": "Confluence",
  "compared_source": "Excel",
  "missing_from": "Excel",
  "affected_artifact": "api_endpoints",
  "affected_section": "required_headers",
  "affected_field": "x-macys-apikey",
  "severity": "High",
  "confidence": "High",
  "description": "The required header x-macys-apikey exists in Confluence but is missing from the Excel api_endpoints sheet.",
  "evidence": {
    "confluence": "x-macys-apikey is listed as a required request header.",
    "node_red": "Validate Headers node checks x-macys-apikey.",
    "excel": "api_endpoints required_headers does not include x-macys-apikey."
  },
  "recommendation": "Add x-macys-apikey to the required_headers field in the api_endpoints sheet.",
  "status": "Open"
}
```

---

## Manual Review Procedure

The manual review should follow a repeatable process.

---

# Step 1: Review the Confluence Requirement

The reviewer should first read the Confluence requirement carefully.

The reviewer should identify all requirement elements, including:

```text
Endpoint path
HTTP method
Headers
Request fields
Response fields
Validation rules
Enum values
Business rules
Downstream services
Database references
Error handling rules
Technology assumptions
```

The reviewer should treat Confluence as the primary source of truth unless the requirement is clearly incomplete or ambiguous.

---

# Step 2: Review the Node-RED Workflow

The reviewer should then review the Node-RED workflow.

The reviewer should check whether the workflow contains the required steps from Confluence.

Important workflow elements include:

```text
HTTP input
Header validation
Body validation
Enum validation
Request mapping
Business rule execution
Downstream service call
Database lookup
Response mapping
Error handling
HTTP response
```

The reviewer should record gaps if required workflow steps are missing or incorrectly modeled.

---

# Step 3: Review the Excel Intake Workbook

The reviewer should review the Excel intake workbook or workbook summary.

The reviewer should check whether required information is present in sheets such as:

```text
technical_requirements
api_endpoints
entities
external_services
business_requirements
endpoint_service_mapping
queries
```

The reviewer should record gaps if Excel is missing required generation input.

---

# Step 4: Compare Confluence to Node-RED

This comparison checks whether Node-RED correctly represents the original requirement.

The reviewer should ask:

```text
Did Node-RED capture the required endpoint?
Did Node-RED include required validation?
Did Node-RED include required downstream service calls?
Did Node-RED include required business logic?
Did Node-RED include required response mapping?
```

Example gap:

```text
Confluence requires enum validation for entryMode, but Node-RED does not include an enum validation step.
```

---

# Step 5: Compare Confluence to Excel

This comparison checks whether the Excel workbook correctly captures the original requirement.

The reviewer should ask:

```text
Did Excel include all required headers?
Did Excel include all request and response fields?
Did Excel include all validation rules?
Did Excel include all business rules?
Did Excel include downstream services and queries?
Did Excel use the correct target technology?
```

Example gap:

```text
Confluence says the target database is PostgreSQL, but Excel refers to SQLite.
```

---

# Step 6: Compare Node-RED to Excel

This comparison checks whether the Excel workbook captures workflow logic.

The reviewer should ask:

```text
Did Excel include the same workflow steps as Node-RED?
Did Excel capture service invocation order?
Did Excel capture mapping logic?
Did Excel capture validation steps?
Did Excel capture database lookup steps?
```

Example gap:

```text
Node-RED contains a Validate Headers step, but Excel endpoint_service_mapping does not include header validation in the workflow sequence.
```

---

# Step 7: Identify Extra or Unsupported Artifacts

The reviewer should also check for unsupported additions.

These are cases where Node-RED or Excel includes something not found in Confluence.

Example:

```text
Excel includes Kafka publishing, but Confluence does not mention Kafka.
```

This should be recorded as an extra artifact gap or unsupported assumption.

---

# Step 8: Assign Severity

Each manually identified gap should receive a severity level.

Allowed severity values are:

```text
Critical
High
Medium
Low
Informational
```

Severity should follow the rules defined in:

```text
docs/day13/gap_severity_levels.md
```

Critical and High gaps should be used for issues that can affect code generation, business logic, required validation, or integration behavior.

---

# Step 9: Add Evidence

Every manual gap should include evidence.

Evidence should explain what each artifact contains.

Example:

```json
{
  "confluence": "entryMode must be one of the allowed enum values.",
  "node_red": "No enum validation node exists.",
  "excel": "api_endpoints validation_rules only says entryMode is required."
}
```

Evidence improves transparency and helps compare manual and AI-assisted output.

---

# Step 10: Add Recommendation

Every manually identified gap should include a recommendation.

The recommendation should be specific and actionable.

Weak recommendation:

```text
Fix Excel.
```

Strong recommendation:

```text
Add the full entryMode enum list to the api_endpoints validation_rules column and reference it in endpoint_service_mapping before downstream service invocation.
```

---

## Manual Review Timing

Manual review time should be measured because one of the project goals is to show reduction in review effort.

The reviewer should record:

```text
Start time
End time
Total review time
Number of artifacts reviewed
Number of gaps found
Number of pages or rows reviewed
```

Example:

```json
{
  "benchmark_id": "api_001_interrogate_account",
  "manual_review_start_time": "10:00 AM",
  "manual_review_end_time": "11:45 AM",
  "manual_review_time_minutes": 105,
  "artifacts_reviewed": 3,
  "expected_gaps_found": 8
}
```

---

## Manual Review Time Formula

Manual review time is calculated as:

```text
Manual Review Time = Review End Time - Review Start Time
```

Example:

```text
Manual Review Time = 11:45 AM - 10:00 AM = 105 minutes
```

This value will later be compared against AI-assisted review time.

---

## Manual vs AI-Assisted Review Comparison

Once the AI-assisted framework produces its own gap report, the two outputs should be compared.

The comparison should identify:

```text
True Positives
False Positives
False Negatives
Severity Matches
Severity Mismatches
Recommendation Quality
```

---

## True Positive

A true positive occurs when the AI-assisted framework detects a gap that also exists in the manual baseline.

Example:

```text
Manual baseline says x-macys-apikey is missing from Excel.
Framework also reports x-macys-apikey missing from Excel.
```

This is a correct detection.

---

## False Positive

A false positive occurs when the framework reports a gap that does not exist in the manual baseline.

Example:

```text
Framework reports Division-id naming mismatch as a gap.
Manual baseline says Division-id and division_id are already mapped correctly.
```

This is an incorrect detection.

---

## False Negative

A false negative occurs when the manual baseline contains a gap that the framework failed to detect.

Example:

```text
Manual baseline says entryMode enum validation is missing.
Framework does not report this gap.
```

This is a missed issue.

---

## Severity Match

A severity match occurs when the framework assigns the same severity as the manual baseline.

Example:

```text
Manual baseline severity: High
Framework severity: High
```

This is correct severity assignment.

---

## Severity Mismatch

A severity mismatch occurs when the framework detects the correct gap but assigns the wrong severity.

Example:

```text
Manual baseline severity: Critical
Framework severity: Medium
```

This means the gap was found but not prioritized correctly.

---

## Baseline Review Template

The following template can be used for each benchmark.

```markdown
# Manual Review Baseline — [Benchmark Name]

## Benchmark ID

[API-001]

## Artifact Reviewed

- Confluence requirement: [file name]
- Node-RED workflow: [file name]
- Excel intake workbook: [file name]

## Manual Review Time

- Start time:
- End time:
- Total minutes:

## Expected Gap Summary

| Gap ID | Gap Type | Missing From | Affected Artifact | Severity |
|---|---|---|---|---|
| GAP-001 | Missing Header Gap | Excel | api_endpoints | High |
| GAP-002 | Missing Enum Validation Gap | Excel | api_endpoints | High |

## Detailed Expected Gaps

### GAP-001

**Gap Type:** Missing Header Gap

**Source of Truth:** Confluence

**Missing From:** Excel

**Affected Field:** x-macys-apikey

**Severity:** High

**Description:** The required header x-macys-apikey exists in Confluence but is missing from Excel.

**Evidence:**

- Confluence: x-macys-apikey is required.
- Node-RED: Header validation checks x-macys-apikey.
- Excel: required_headers does not include x-macys-apikey.

**Recommendation:** Add x-macys-apikey to api_endpoints.required_headers.
```

---

## Baseline Quality Rules

The manual baseline should follow these quality rules.

---

## Rule 1: Be Evidence-Based

Every gap must be supported by evidence from the artifacts.

Do not record gaps based only on memory or assumptions.

---

## Rule 2: Use the Same Schema

Manual gaps should use the same schema as AI-detected gaps.

This makes comparison easier.

---

## Rule 3: Avoid Over-Reporting

Do not classify minor naming differences as gaps if a clear mapping exists.

Example:

```text
Division-id and division_id should not be treated as a serious gap if the mapping is documented.
```

---

## Rule 4: Mark Ambiguity Clearly

If the requirement is unclear, mark the issue as Informational or Low Confidence.

Do not treat ambiguous wording as a confirmed Critical gap.

---

## Rule 5: Keep Recommendations Actionable

Each recommendation should explain exactly what needs to be changed.

---

## Rule 6: Record Review Time Honestly

Manual review timing should be recorded accurately.

This is important for measuring manual review reduction.

---

## Baseline Output Files

For each benchmark, the manual review baseline can produce:

```text
manual_baseline_gaps.md
expected_gaps.json
manual_review_notes.md
manual_review_time.json
```

Recommended benchmark folder structure:

```text
benchmarks/
└── api_001_interrogate_account/
    ├── confluence_requirement.md
    ├── node_red_flow.json
    ├── excel_intake_summary.md
    ├── expected_gaps.json
    ├── manual_baseline_gaps.md
    └── manual_review_time.json
```

---

## How the Baseline Supports Metrics

The manual baseline supports the following calculations:

| Metric                       | Baseline Role                                     |
| ---------------------------- | ------------------------------------------------- |
| Gap Detection Accuracy       | Provides total expected gaps                      |
| False Positive Rate          | Helps identify incorrect framework gaps           |
| False Negative Rate          | Helps identify missed expected gaps               |
| Severity Assignment Accuracy | Provides expected severity                        |
| Recommendation Usefulness    | Provides comparison against reviewer expectations |
| Manual Review Reduction      | Provides manual review time                       |
| Code Generation Readiness    | Identifies unresolved blocking gaps               |

---

## Example Manual Baseline Summary

```json
{
  "benchmark_id": "api_001_interrogate_account",
  "manual_review_time_minutes": 105,
  "total_expected_gaps": 8,
  "critical_gaps": 1,
  "high_gaps": 4,
  "medium_gaps": 2,
  "low_gaps": 1,
  "informational_gaps": 0,
  "pipeline_status": "Blocked",
  "blocking_reason": "One critical gap and four high gaps must be resolved before code generation."
}
```

---

## Success Criteria

The manual review baseline is successful if it:

```text
Identifies expected gaps clearly
Uses the standard gap schema
Includes evidence for every gap
Assigns severity consistently
Provides actionable recommendations
Records manual review time
Supports metric calculation
Can be compared against AI-assisted output
```

---

## Summary

The manual review baseline is the human-created reference used to evaluate the Gap Analysis Framework.

It defines the expected gaps for each benchmark API and provides the ground truth for calculating accuracy, false positives, false negatives, severity correctness, recommendation usefulness, and review time reduction.

This baseline is essential because it allows the project to prove whether AI-assisted gap analysis is accurate, measurable, and useful for enterprise API development.
