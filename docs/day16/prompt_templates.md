# Day 16 — Phase 3: Prompt Templates

## Purpose

This document defines reusable prompt templates for the future AI-assisted enterprise API artifact generation and gap analysis system.

The purpose of prompt templates is to make AI outputs consistent, structured, reviewable, and repeatable.

The project should not depend on random one-time prompts.

Instead, each major pipeline step should have a clear reusable prompt template with:

* Agent role
* Input artifact
* Task objective
* Output format
* Guardrails
* Review behavior

---

## Why Prompt Templates Are Needed

Prompt templates are needed because enterprise API artifact generation requires precision.

Without templates, AI output may become inconsistent across runs.

For example, one run may extract all headers correctly, while another run may miss optional headers or invent unsupported fields.

Reusable templates help standardize how the AI performs tasks such as:

```text
Requirement extraction
Workflow modeling
Excel intake generation
Gap analysis
Evaluation scoring
Manual review support
Research reporting
```

Prompt templates also make the system easier to evaluate because the same prompt structure can be reused across benchmark APIs.

---

## Prompt Template Design Principles

All prompt templates should follow these principles:

```text
Use source artifacts as authority
Do not invent missing details
Separate confirmed facts from assumptions
Mark unclear items as Needs Manual Review
Use structured output
Include evidence when possible
Follow the assigned schema
Avoid unsupported technology assumptions
Preserve field names exactly when needed
Report gaps honestly
```

---

# Template 1 — Requirement Extraction Prompt

## Purpose

This prompt is used by the Requirement Extraction Agent.

It extracts structured API requirement details from a Confluence-style document.

---

## Prompt Template

```text
You are the Requirement Extraction Agent for an enterprise API artifact generation system.

Your task is to read the provided Confluence-style requirement and extract structured API requirement details.

Source of truth:
- Treat the provided Confluence requirement as the primary source of truth.
- Extract only details that are explicitly present or strongly supported by the source.
- Do not invent endpoints, headers, request fields, response fields, database tables, downstream services, or business rules.
- If something is missing or unclear, mark it as "Not Found" or "Needs Manual Review".

Input:
[PASTE_CONFLUENCE_REQUIREMENT_HERE]

Extract the following sections:

1. API Overview
2. Endpoint Details
3. Required Headers
4. Optional Headers
5. Request Body Fields
6. Response Body Fields
7. Validation Rules
8. Enum Values
9. Business Rules
10. Downstream Services
11. Database References
12. Error Handling Rules
13. Technology Assumptions
14. Open Questions
15. Extraction Confidence

Output format:
Return the result in Markdown using structured tables.

For each extracted item, include:
- Name
- Description
- Required or Optional
- Source Evidence
- Confidence
- Notes

Rules:
- Preserve exact field names from the source.
- Do not normalize names unless you also show the original name.
- If a field appears in a sample request but not in the description, mention that.
- If a business rule is inferred from process text, label it as inferred.
- If evidence is weak, set confidence to Medium or Low.
```

---

## Expected Output Structure

```markdown
# Requirement Extraction Output

## API Overview

| Field | Value |
|---|---|
| API Name | |
| Description | |
| Source Confidence | |

## Endpoint Details

| Endpoint ID | Method | Path | Operation | Evidence | Confidence |
|---|---|---|---|---|---|

## Required Headers

| Header | Description | Evidence | Confidence |
|---|---|---|---|

## Optional Headers

| Header | Description | Evidence | Confidence |
|---|---|---|---|

## Request Body Fields

| Field Path | Data Type | Required | Description | Evidence | Confidence |
|---|---|---|---|---|---|

## Response Body Fields

| Field Path | Data Type | Description | Evidence | Confidence |
|---|---|---|---|---|

## Validation Rules

| Rule ID | Field | Rule | Severity | Evidence | Confidence |
|---|---|---|---|---|---|

## Business Rules

| Rule ID | Description | Condition | Action | Evidence | Confidence |
|---|---|---|---|---|---|

## Downstream Services

| Service ID | Service Name | Method | Endpoint | Purpose | Evidence | Confidence |
|---|---|---|---|---|---|---|

## Database References

| Reference ID | Table or Entity | Operation | Purpose | Evidence | Confidence |
|---|---|---|---|---|---|

## Open Questions

| Question ID | Question | Reason |
|---|---|---|
```

---

# Template 2 — Workflow Modeling Prompt

## Purpose

This prompt is used by the Workflow Modeling Agent.

It converts extracted requirements into a step-by-step workflow representation.

---

## Prompt Template

```text
You are the Workflow Modeling Agent for an enterprise API artifact generation system.

Your task is to convert structured API requirements into an ordered workflow representation.

Input:
[PASTE_REQUIREMENT_EXTRACTION_OUTPUT_HERE]

Source rules:
- Use only the requirement extraction output as input.
- Do not invent extra workflow steps.
- Do not add downstream calls, database queries, or validations unless they are supported by the input.
- If a required workflow step is unclear, mark it as "Needs Manual Review".

Create a workflow with the following details:

1. Workflow ID
2. Workflow Name
3. Endpoint Reference
4. Ordered Workflow Steps
5. Step Type
6. Input Data
7. Output Data
8. Decision Rules
9. Service Calls
10. Database Calls
11. Error Handling Paths
12. Response Mapping
13. Open Questions

Allowed step types:
- HTTP_INPUT
- HEADER_VALIDATION
- BODY_VALIDATION
- ENUM_VALIDATION
- TRANSFORMATION
- BUSINESS_RULE
- SERVICE_CALL
- DATABASE_QUERY
- RESPONSE_MAPPING
- ERROR_HANDLING
- HTTP_RESPONSE
- MANUAL_REVIEW

Output format:
Return the result in Markdown with a workflow table and a visual sequence.
```

---

## Expected Output Structure

````markdown
# Workflow Modeling Output

## Workflow Summary

| Field | Value |
|---|---|
| Workflow ID | |
| Workflow Name | |
| Endpoint | |
| Source Requirement | |

## Workflow Sequence

```text
HTTP Request Received
        ↓
Validate Headers
        ↓
Validate Request Body
        ↓
Apply Business Rules
        ↓
Invoke Downstream Service
        ↓
Map Response
        ↓
Return HTTP Response
````

## Workflow Steps

| Step ID | Step Name | Step Type | Input | Output | Description | Evidence | Confidence |
| ------- | --------- | --------- | ----- | ------ | ----------- | -------- | ---------- |

## Decision Points

| Decision ID | Condition | True Path | False Path | Evidence |
| ----------- | --------- | --------- | ---------- | -------- |

## Service Calls

| Step ID | Service Name | Method | Endpoint | Request Mapping | Response Mapping |
| ------- | ------------ | ------ | -------- | --------------- | ---------------- |

## Database Calls

| Step ID | Query Purpose | Table or Entity | Input Params | Output Fields |
| ------- | ------------- | --------------- | ------------ | ------------- |

## Error Handling Paths

| Error ID | Trigger | Response Behavior | Evidence |
| -------- | ------- | ----------------- | -------- |

## Open Questions

| Question ID | Question | Reason |
| ----------- | -------- | ------ |

````

---

# Template 3 — Excel Intake Generation Prompt

## Purpose

This prompt is used by the Excel Intake Generation Agent.

It converts requirements and workflow representation into Excel-ready intake content.

---

## Prompt Template

```text
You are the Excel Intake Generation Agent for a Go code generation factory.

Your task is to convert the structured requirement extraction and workflow representation into Excel-ready sheet content.

Inputs:
1. Requirement extraction output:
[PASTE_REQUIREMENT_EXTRACTION_OUTPUT_HERE]

2. Workflow modeling output:
[PASTE_WORKFLOW_MODELING_OUTPUT_HERE]

Target technology:
- Language: Go
- Database: PostgreSQL unless the source explicitly says otherwise
- Do not use SQLite unless the source explicitly requires it

Important rules:
- Do not invent headers, fields, services, entities, queries, or business rules.
- If a value is missing, mark it as "TBD" or "Needs Manual Review".
- Preserve exact API paths and field names.
- Include validation rules clearly.
- Include downstream service references only when supported by the source.
- Include database queries only when supported by the source.
- Separate confirmed information from assumptions.

Generate Excel-ready content for the following sheets:

1. technical_requirements
2. api_endpoints
3. entities
4. external_services
5. business_requirements
6. endpoint_service_mapping
7. queries

Output format:
Return each sheet as a Markdown table.
````

---

## Expected Output Structure

```markdown
# Excel Intake Generation Output

## Sheet: technical_requirements

| Field | Value | Source Evidence | Confidence |
|---|---|---|---|

## Sheet: api_endpoints

| endpoint_id | method | path | operation_name | required_headers | optional_headers | request_body | response_body | validation_rules | notes |
|---|---|---|---|---|---|---|---|---|---|

## Sheet: entities

| entity_id | entity_name | table_name | fields | primary_key | purpose | source_evidence | notes |
|---|---|---|---|---|---|---|---|

## Sheet: external_services

| service_id | service_name | service_type | method | endpoint | request_headers | request_body | response_body | error_handling | notes |
|---|---|---|---|---|---|---|---|---|---|

## Sheet: business_requirements

| business_rule_id | rule_name | description | condition | action | acceptance_criteria | severity | source_evidence |
|---|---|---|---|---|---|---|---|

## Sheet: endpoint_service_mapping

| mapping_id | endpoint_id | sequence | step_name | step_type | input_mapping | output_mapping | failure_behavior | references |
|---|---|---:|---|---|---|---|---|---|

## Sheet: queries

| query_id | query_name | query_type | table_name | purpose | input_params | output_fields | sql_or_description | source_evidence |
|---|---|---|---|---|---|---|---|---|

## Open Questions

| Question ID | Sheet | Field | Question | Reason |
|---|---|---|---|---|
```

---

# Template 4 — Gap Analysis Prompt

## Purpose

This prompt is used by the Gap Analysis Agent.

It compares Confluence, Node-RED, and Excel artifacts to identify gaps.

---

## Prompt Template

```text
You are the Gap Analysis Agent for an enterprise API artifact generation system.

Your task is to compare the following artifacts and detect missing, incorrect, inconsistent, unsupported, or ambiguous information.

Inputs:

1. Confluence requirement extraction:
[PASTE_CONFLUENCE_EXTRACTION_HERE]

2. Node-RED workflow representation:
[PASTE_NODE_RED_OR_WORKFLOW_OUTPUT_HERE]

3. Excel intake workbook summary:
[PASTE_EXCEL_INTAKE_OUTPUT_HERE]

Source priority:
1. Confluence is the primary source of truth.
2. Node-RED is the workflow representation.
3. Excel is the code generation intake artifact.

Compare the artifacts in these directions:
- Confluence to Node-RED
- Confluence to Excel
- Node-RED to Excel
- Excel to Confluence
- Node-RED to Confluence

Use these gap categories:
- Missing Endpoint Gap
- Missing Header Gap
- Missing Request Field Gap
- Missing Response Field Gap
- Missing Validation Gap
- Missing Enum Validation Gap
- Missing Business Rule Gap
- Missing Downstream Service Gap
- Missing Database Query Gap
- Missing Entity Gap
- Incorrect Field Mapping Gap
- Incorrect Response Mapping Gap
- Incorrect Service Mapping Gap
- Incorrect Data Type Gap
- Incorrect Required or Optional Status Gap
- Technology Mismatch Gap
- Error Handling Gap
- Extra Artifact Gap
- Naming Mismatch Gap
- Workflow Step Gap

Severity levels:
- Critical
- High
- Medium
- Low
- Informational

Rules:
- Do not invent gaps without evidence.
- Do not treat naming differences as gaps if a mapping is documented.
- Mark ambiguous items as Informational or Needs Manual Review.
- Include evidence from each relevant artifact.
- Provide a specific recommendation for every confirmed gap.
- Assign confidence as High, Medium, or Low.

Output:
Return a structured Markdown gap report and a JSON-style gap list.
```

---

## Expected Output Structure

````markdown
# Gap Analysis Report

## Summary

| Metric | Count |
|---|---:|
| Total Gaps | |
| Critical | |
| High | |
| Medium | |
| Low | |
| Informational | |

## Gap List

| Gap ID | Gap Type | Missing From | Affected Artifact | Affected Field | Severity | Confidence |
|---|---|---|---|---|---|---|

## Detailed Gaps

### GAP-001

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
  "description": "The required header x-macys-apikey exists in Confluence but is missing from Excel.",
  "evidence": {
    "confluence": "x-macys-apikey is listed as a required header.",
    "node_red": "Header validation includes x-macys-apikey.",
    "excel": "api_endpoints required_headers does not include x-macys-apikey."
  },
  "recommendation": "Add x-macys-apikey to api_endpoints.required_headers and validation_rules.",
  "status": "Open"
}
````

## Open Questions

| Question ID | Question | Related Artifact | Reason |
| ----------- | -------- | ---------------- | ------ |

````

---

# Template 5 — Evaluation Scoring Prompt

## Purpose

This prompt is used by the Evaluation Agent.

It compares AI-detected gaps against a manual baseline and calculates evaluation metrics.

---

## Prompt Template

```text
You are the Evaluation Agent for a research experiment.

Your task is to compare the manual baseline gap report against the AI-assisted detected gap report.

Inputs:

1. Manual baseline expected gaps:
[PASTE_EXPECTED_GAPS_HERE]

2. AI-assisted detected gaps:
[PASTE_DETECTED_GAPS_HERE]

3. Manual review time:
[PASTE_MANUAL_REVIEW_TIME_HERE]

4. AI-assisted review time:
[PASTE_AI_REVIEW_TIME_HERE]

Matching rules:
- Exact Match: same issue, same affected field, same affected artifact.
- Partial Match: same issue but wording or section differs slightly.
- False Positive: AI reports a gap not present in manual baseline and not valid.
- False Negative: manual baseline contains a gap AI missed.
- Severity Match: AI severity matches manual severity.
- Severity Mismatch: AI detected the gap but assigned different severity.

Calculate:
1. True positives
2. Partial matches
3. False positives
4. False negatives
5. Severity matches
6. Severity mismatches
7. Gap detection accuracy
8. False positive rate
9. False negative rate
10. Severity assignment accuracy
11. Manual review reduction
12. Recommendation usefulness score if recommendation ratings are provided

Rules:
- Do not hide false positives or false negatives.
- Do not change formulas.
- Mark ambiguous matches as Needs Manual Review.
- Separate measured results from assumptions.

Output:
Return evaluation_result.json style output and a Markdown summary table.
````

---

## Expected Output Structure

````markdown
# Evaluation Scoring Output

## Gap Comparison Summary

| Metric | Value |
|---|---:|
| Expected Gaps | |
| AI-Detected Gaps | |
| True Positives | |
| Partial Matches | |
| False Positives | |
| False Negatives | |
| Severity Matches | |
| Severity Mismatches | |

## Calculated Metrics

| Metric | Formula | Result |
|---|---|---:|
| Gap Detection Accuracy | True Positives / Expected Gaps × 100 | |
| False Positive Rate | False Positives / AI-Detected Gaps × 100 | |
| False Negative Rate | False Negatives / Expected Gaps × 100 | |
| Severity Assignment Accuracy | Severity Matches / True Positives × 100 | |
| Manual Review Reduction | (Manual Time - AI Time) / Manual Time × 100 | |

## JSON Result

```json
{
  "benchmark_id": "",
  "expected_gaps": 0,
  "detected_gaps": 0,
  "true_positives": 0,
  "partial_matches": 0,
  "false_positives": 0,
  "false_negatives": 0,
  "severity_matches": 0,
  "severity_mismatches": 0,
  "gap_detection_accuracy": 0,
  "false_positive_rate": 0,
  "false_negative_rate": 0,
  "severity_assignment_accuracy": 0,
  "manual_review_reduction": 0
}
````

````

---

# Template 6 — Reviewer Agent Prompt

## Purpose

This prompt is used by the Reviewer Agent.

It reviews generated outputs and highlights risks, open questions, and next actions for human review.

---

## Prompt Template

```text
You are the Reviewer Agent for an AI-assisted enterprise API artifact generation system.

Your task is to review the generated outputs and identify whether the artifact set is ready for the next stage.

Inputs:
1. Requirement extraction output:
[PASTE_REQUIREMENT_EXTRACTION_OUTPUT_HERE]

2. Workflow modeling output:
[PASTE_WORKFLOW_OUTPUT_HERE]

3. Excel intake output:
[PASTE_EXCEL_INTAKE_OUTPUT_HERE]

4. Gap analysis report:
[PASTE_GAP_ANALYSIS_REPORT_HERE]

5. Evaluation result:
[PASTE_EVALUATION_RESULT_HERE]

Review focus:
- Critical gaps
- High gaps
- Low-confidence findings
- Unsupported assumptions
- Missing source evidence
- Technology mismatches
- Code generation blockers
- Open questions

Rules:
- Do not approve artifacts with unresolved Critical gaps.
- Do not ignore High gaps affecting contract, validation, integration, or business logic.
- Do not treat AI output as final authority.
- Clearly separate Ready, Needs Review, Blocked, and Not Ready.
- Provide exact next actions.

Output:
Return a review summary, risk list, open questions, and approval recommendation.
````

---

## Expected Output Structure

```markdown
# Reviewer Agent Output

## Review Status

| Field | Value |
|---|---|
| Status | Ready / Needs Review / Blocked / Not Ready |
| Reason | |
| Blocking Gap Count | |
| Critical Gap Count | |
| High Gap Count | |

## Key Risks

| Risk ID | Severity | Description | Required Action |
|---|---|---|---|

## Open Questions

| Question ID | Question | Artifact | Reason |
|---|---|---|---|

## Required Actions

| Action ID | Action | Owner | Priority |
|---|---|---|---|

## Approval Recommendation

[Clear recommendation here]
```

---

# Template 7 — Research Reporting Prompt

## Purpose

This prompt is used by the Research Reporting Agent.

It converts evaluation results into research-ready, Medium-ready, LinkedIn-ready, and resume-ready summaries.

---

## Prompt Template

```text
You are the Research Reporting Agent for the 50-Day AI Software Engineer Challenge.

Your task is to convert experiment results into clear research and portfolio summaries.

Inputs:
1. Evaluation results:
[PASTE_EVALUATION_RESULTS_HERE]

2. Benchmark case descriptions:
[PASTE_BENCHMARK_CASES_HERE]

3. Metrics and targets:
[PASTE_METRIC_TARGETS_HERE]

Rules:
- Do not overclaim.
- Separate planned targets from measured results.
- Report dataset size.
- Include limitations.
- Do not say AI replaces human review.
- Use accurate wording such as "AI-assisted review supports human reviewers".
- Only mention achieved metrics if they were actually measured.

Generate:
1. Research paper result summary
2. Medium article result section
3. LinkedIn update
4. Resume bullet
5. Limitations section
```

---

## Expected Output Structure

```markdown
# Research Reporting Output

## Research Paper Summary

[Summary]

## Medium Article Summary

[Summary]

## LinkedIn Update

[Summary]

## Resume Bullet

[Bullet]

## Limitations

[Limitations]
```

---

# Template 8 — Open Questions Extraction Prompt

## Purpose

This prompt identifies unclear, missing, or ambiguous details that need human confirmation.

---

## Prompt Template

```text
You are the Open Questions Extraction Agent.

Your task is to identify unclear, missing, or ambiguous details from the provided artifact.

Input:
[PASTE_ARTIFACT_HERE]

Identify open questions related to:

- Endpoint path
- HTTP method
- Required headers
- Optional headers
- Request fields
- Response fields
- Validation rules
- Business rules
- Downstream services
- Database queries
- Technology assumptions
- Error handling
- Security behavior

Rules:
- Do not answer the question yourself unless the source provides evidence.
- Do not invent missing details.
- Mark each question with priority.
- Explain why the question matters.
- Suggest which artifact or person should clarify it.

Output:
Return a Markdown table.
```

---

## Expected Output Structure

```markdown
# Open Questions

| Question ID | Area | Question | Why It Matters | Priority | Suggested Owner |
|---|---|---|---|---|---|
```

---

# Prompt Template Usage Rules

## Rule 1: Use the Correct Template for the Correct Task

Do not use the Excel generation prompt for gap analysis.

Do not use the gap analysis prompt for evaluation scoring.

Each template has a specific purpose.

---

## Rule 2: Keep Inputs Explicit

Each prompt should clearly identify the input artifacts.

The AI should not rely on memory from previous unrelated tasks.

---

## Rule 3: Preserve Source Names

Field names, endpoint paths, headers, and service names should be preserved exactly unless normalization is explicitly requested.

---

## Rule 4: Mark Missing Information

If information is missing, the output should say:

```text
Not Found
Missing
Needs Manual Review
TBD
```

It should not silently guess.

---

## Rule 5: Include Evidence

For requirement extraction, gap analysis, and review prompts, include source evidence whenever possible.

---

## Rule 6: Separate Planned Targets from Measured Results

Research reporting prompts must not claim experiment results before the experiment is run.

---

# Prompt Template Summary

| Template                       | Agent                         | Main Output                               |
| ------------------------------ | ----------------------------- | ----------------------------------------- |
| Requirement Extraction Prompt  | Requirement Extraction Agent  | Structured requirement extraction         |
| Workflow Modeling Prompt       | Workflow Modeling Agent       | Workflow representation                   |
| Excel Intake Generation Prompt | Excel Intake Generation Agent | Excel-ready sheet content                 |
| Gap Analysis Prompt            | Gap Analysis Agent            | Gap report                                |
| Evaluation Scoring Prompt      | Evaluation Agent              | Evaluation metrics                        |
| Reviewer Agent Prompt          | Reviewer Agent                | Review status and risks                   |
| Research Reporting Prompt      | Research Reporting Agent      | Paper, Medium, LinkedIn, resume summaries |
| Open Questions Prompt          | Reviewer or Extraction Agent  | Clarification questions                   |

---

## Summary

This document defines reusable prompt templates for the future AI-assisted enterprise API artifact generation and gap analysis system.

The templates cover requirement extraction, workflow modeling, Excel intake generation, gap analysis, evaluation scoring, review support, research reporting, and open question extraction.

These templates improve consistency, reduce unsupported assumptions, support measurable evaluation, and prepare the project for future implementation after the no-code planning phase.
