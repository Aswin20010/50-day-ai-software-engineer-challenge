# Day 18 — Dataset Review Process

## Purpose

This document defines the dataset review process for the workflow evaluation dataset in the 50-Day AI Software Engineer Challenge.

The purpose of the review process is to make sure each dataset item is clear, complete, sanitized, traceable, and suitable for evaluating AI-generated workflows.

The evaluation dataset will be used to compare:

```text
Source Requirement
        vs
AI-Generated Workflow
        vs
Ground Truth Workflow
```

Because this dataset supports research evaluation, every dataset item must be reviewed before it is used for formal scoring.

---

## Why a Dataset Review Process Is Needed

The evaluation dataset is only useful if the dataset items are reliable.

If the source requirement is unclear, incomplete, duplicated, or poorly structured, then the AI-generated workflow cannot be fairly evaluated.

A weak dataset item may cause incorrect evaluation results.

For example:

* If the source requirement does not clearly define headers, the AI may be unfairly marked wrong.
* If the ground truth workflow includes unsupported assumptions, the AI may be compared against an incorrect answer.
* If the sample contains confidential details, it may not be safe to commit to GitHub.
* If ambiguity is not marked, reviewers may treat guessed behavior as confirmed behavior.

The dataset review process prevents these issues.

---

## Review Process Overview

Each dataset item should go through the following review flow:

```text
Create Dataset Item Draft
        ↓
Review Source Requirement
        ↓
Review Requirement Category
        ↓
Review Sanitization
        ↓
Review Ground Truth Workflow
        ↓
Review Traceability
        ↓
Review Ambiguity Notes
        ↓
Review Validation Checklist Alignment
        ↓
Approve or Send Back for Update
```

Only approved dataset items should be used for final evaluation.

---

## Dataset Review Status Values

Each dataset item should have one of the following statuses:

| Status       | Meaning                                                  |
| ------------ | -------------------------------------------------------- |
| DRAFT        | Initial dataset item created but not reviewed            |
| NEEDS_UPDATE | Review found issues that must be corrected               |
| REVIEWED     | Dataset item has been reviewed but not formally approved |
| APPROVED     | Dataset item is ready for evaluation                     |
| RETIRED      | Dataset item should no longer be used                    |

---

## Review Roles

For this project, the review process can be performed by the project author.

However, the review should still be treated as a formal step.

Recommended roles:

| Role                | Responsibility                                           |
| ------------------- | -------------------------------------------------------- |
| Dataset Author      | Creates the source requirement sample                    |
| Ground Truth Author | Creates the expected workflow                            |
| Reviewer            | Checks clarity, correctness, traceability, and readiness |
| Approver            | Marks the dataset item as approved for evaluation        |

In this solo project, the same person may perform all roles, but the process should still be documented.

---

## Step 1 — Source Requirement Review

### Purpose

The first review step checks whether the source requirement is clear enough to be used as an evaluation input.

### Review Questions

| Check ID | Review Question                                        | Status |
| -------- | ------------------------------------------------------ | ------ |
| SRC-001  | Is the source requirement clearly written?             |        |
| SRC-002  | Is the API purpose understandable?                     |        |
| SRC-003  | Is the endpoint defined or reasonably inferable?       |        |
| SRC-004  | Are request headers listed when relevant?              |        |
| SRC-005  | Are request body fields listed when relevant?          |        |
| SRC-006  | Are validation rules included?                         |        |
| SRC-007  | Are business rules included, if applicable?            |        |
| SRC-008  | Are downstream services identified, if applicable?     |        |
| SRC-009  | Are database interactions identified, if applicable?   |        |
| SRC-010  | Are response expectations included?                    |        |
| SRC-011  | Are error scenarios included or marked as unspecified? |        |

### Decision

If the requirement is too vague and not intentionally part of the ambiguous requirement category, mark it as:

```text
NEEDS_UPDATE
```

---

## Step 2 — Requirement Category Review

### Purpose

This step checks whether the dataset item is assigned to the correct sample requirement category.

### Categories

The dataset item should belong to one of the categories defined in:

```text
docs/day18/sample_requirement_categories.md
```

Examples:

* Validation-Only API
* Simple Create API
* Database Lookup API
* Downstream REST Wrapper API
* Response Transformation API
* Business Rule Branching API
* Multi-Step Orchestration API
* Calculation-Based API
* Error-Handling-Heavy API
* Ambiguous Requirement API
* Existing System Modernization API
* Mixed Enterprise API

### Review Questions

| Check ID | Review Question                                             | Status |
| -------- | ----------------------------------------------------------- | ------ |
| CAT-001  | Is the selected category correct?                           |        |
| CAT-002  | Does the category match the requirement complexity?         |        |
| CAT-003  | Does the item test a useful workflow generation capability? |        |
| CAT-004  | Does the item avoid duplicating another dataset sample?     |        |
| CAT-005  | Is the complexity level marked correctly?                   |        |

### Decision

If the category is wrong, update the category before approval.

---

## Step 3 — Sanitization Review

### Purpose

This step checks whether the dataset item is safe to store in the public project repository.

The dataset should preserve enterprise API patterns without exposing confidential details.

### Sanitization Checklist

| Check ID | Review Question                                                   | Status |
| -------- | ----------------------------------------------------------------- | ------ |
| SAN-001  | Are real internal URLs removed or replaced?                       |        |
| SAN-002  | Are API keys, tokens, and secrets removed?                        |        |
| SAN-003  | Are customer details removed?                                     |        |
| SAN-004  | Are employee details removed?                                     |        |
| SAN-005  | Are proprietary business values removed or generalized if needed? |        |
| SAN-006  | Are real production identifiers removed?                          |        |
| SAN-007  | Are sensitive database values removed?                            |        |
| SAN-008  | Does the sample still preserve the original workflow pattern?     |        |

### Decision

If sensitive information exists, mark the dataset item as:

```text
NEEDS_UPDATE
```

The item should not be committed until sanitized.

---

## Step 4 — Ground Truth Workflow Review

### Purpose

This step checks whether the ground truth workflow correctly represents the source requirement.

The ground truth workflow is the expected answer used to evaluate AI-generated workflows.

### Review Questions

| Check ID | Review Question                                              | Status |
| -------- | ------------------------------------------------------------ | ------ |
| GT-001   | Does the ground truth workflow match the source requirement? |        |
| GT-002   | Are endpoint details correct?                                |        |
| GT-003   | Are required headers captured?                               |        |
| GT-004   | Are optional headers captured separately?                    |        |
| GT-005   | Are request body fields captured correctly?                  |        |
| GT-006   | Are validation rules represented?                            |        |
| GT-007   | Are business rules represented?                              |        |
| GT-008   | Are downstream calls represented, if applicable?             |        |
| GT-009   | Are database interactions represented, if applicable?        |        |
| GT-010   | Is workflow step order correct?                              |        |
| GT-011   | Is response mapping defined?                                 |        |
| GT-012   | Is error handling defined?                                   |        |
| GT-013   | Does the workflow avoid unsupported assumptions?             |        |
| GT-014   | Is the workflow structured enough for evaluation?            |        |

### Decision

If the ground truth workflow is incomplete or unsupported by the requirement, mark the item as:

```text
NEEDS_UPDATE
```

---

## Step 5 — Traceability Review

### Purpose

This step verifies that each important ground truth workflow element can be traced back to the source requirement.

Traceability prevents unsupported logic from entering the evaluation reference.

### Traceability Checklist

| Check ID  | Review Question                                            | Status |
| --------- | ---------------------------------------------------------- | ------ |
| TRACE-001 | Does the dataset item reference the source requirement?    |        |
| TRACE-002 | Does each validation step have a source reference?         |        |
| TRACE-003 | Does each business rule have a source reference?           |        |
| TRACE-004 | Does each downstream service call have a source reference? |        |
| TRACE-005 | Does each database interaction have a source reference?    |        |
| TRACE-006 | Does each response mapping have a source reference?        |        |
| TRACE-007 | Are unsupported assumptions flagged?                       |        |
| TRACE-008 | Are ambiguous areas linked to ambiguity notes?             |        |

### Example Traceability Matrix

| Ground Truth Element          | Source Requirement Reference | Status |
| ----------------------------- | ---------------------------- | ------ |
| Validate Client-Id            | Header section               | PASS   |
| Validate messageSeverity enum | Request body section         | PASS   |
| Check overlap before insert   | Business rule BR-001         | PASS   |
| Insert into APP_MSGS          | Database section             | PASS   |
| Return generated id           | Response section             | PASS   |

---

## Step 6 — Ambiguity Review

### Purpose

This step checks whether unclear requirement areas are documented instead of guessed.

Ambiguity should be handled explicitly.

### Review Questions

| Check ID | Review Question                                          | Status |
| -------- | -------------------------------------------------------- | ------ |
| AMB-001  | Are unclear requirement areas identified?                |        |
| AMB-002  | Are ambiguous headers marked?                            |        |
| AMB-003  | Are ambiguous body fields marked?                        |        |
| AMB-004  | Are ambiguous response mappings marked?                  |        |
| AMB-005  | Are ambiguous error scenarios marked?                    |        |
| AMB-006  | Are assumptions separated from confirmed requirements?   |        |
| AMB-007  | Does the ground truth avoid guessing ambiguous behavior? |        |

### Example Ambiguity Note

```text
The requirement says "standard headers are required" but does not list the exact header names.
This should be marked as ambiguous.
The ground truth workflow should not invent a header list unless the project defines a standard header profile separately.
```

### Decision

If ambiguity is guessed instead of documented, mark the item as:

```text
NEEDS_UPDATE
```

---

## Step 7 — Validation Checklist Alignment

### Purpose

This step checks whether the dataset item can be evaluated using the Day 17 validation checklist.

The dataset item should support review across the relevant validation areas.

### Review Questions

| Check ID | Review Question                                                | Status |
| -------- | -------------------------------------------------------------- | ------ |
| VAL-001  | Can endpoint validation be performed?                          |        |
| VAL-002  | Can header validation be performed?                            |        |
| VAL-003  | Can request body validation be performed?                      |        |
| VAL-004  | Can business rule validation be performed, if applicable?      |        |
| VAL-005  | Can downstream mapping validation be performed, if applicable? |        |
| VAL-006  | Can database mapping validation be performed, if applicable?   |        |
| VAL-007  | Can response mapping validation be performed?                  |        |
| VAL-008  | Can error handling validation be performed?                    |        |
| VAL-009  | Can traceability validation be performed?                      |        |
| VAL-010  | Can hallucination detection be performed?                      |        |
| VAL-011  | Can artifact readiness be assessed?                            |        |

### Decision

If the dataset item cannot be evaluated using the validation checklist, it needs more structure.

---

## Step 8 — Quality Metric Readiness Review

### Purpose

This step checks whether the dataset item supports scoring using the workflow quality metrics.

### Review Questions

| Check ID | Review Question                                                     | Status |
| -------- | ------------------------------------------------------------------- | ------ |
| MET-001  | Can Requirement Coverage Score be calculated?                       |        |
| MET-002  | Can Step Completeness Score be calculated?                          |        |
| MET-003  | Can Validation Accuracy Score be calculated?                        |        |
| MET-004  | Can Business Rule Coverage Score be calculated?                     |        |
| MET-005  | Can Step Ordering Accuracy Score be calculated?                     |        |
| MET-006  | Can Downstream Mapping Accuracy Score be calculated, if applicable? |        |
| MET-007  | Can Database Mapping Accuracy Score be calculated, if applicable?   |        |
| MET-008  | Can Response Mapping Accuracy Score be calculated?                  |        |
| MET-009  | Can Error Handling Completeness Score be calculated?                |        |
| MET-010  | Can Traceability Score be calculated?                               |        |
| MET-011  | Can Hallucination Control Score be calculated?                      |        |
| MET-012  | Can Artifact Readiness Score be calculated?                         |        |

---

## Step 9 — Duplicate and Coverage Review

### Purpose

This step ensures that the dataset remains balanced and does not contain unnecessary duplicates.

### Review Questions

| Check ID | Review Question                                        | Status |
| -------- | ------------------------------------------------------ | ------ |
| DUP-001  | Does this sample duplicate another dataset item?       |        |
| DUP-002  | Does this sample test a unique capability?             |        |
| DUP-003  | Does this sample improve category coverage?            |        |
| DUP-004  | Does this sample improve complexity coverage?          |        |
| DUP-005  | Is this sample needed for the current dataset version? |        |

### Decision

If a sample duplicates another without adding evaluation value, it should be revised or removed.

---

## Step 10 — Final Approval Review

### Purpose

The final approval step determines whether the dataset item is ready for evaluation.

### Final Approval Checklist

| Check ID | Review Question                                               | Status |
| -------- | ------------------------------------------------------------- | ------ |
| APR-001  | Source requirement is clear enough or ambiguity is documented |        |
| APR-002  | Requirement category is correct                               |        |
| APR-003  | Dataset item is sanitized                                     |        |
| APR-004  | Ground truth workflow is complete                             |        |
| APR-005  | Traceability is documented                                    |        |
| APR-006  | Ambiguity notes are documented                                |        |
| APR-007  | Validation checklist can be applied                           |        |
| APR-008  | Quality metrics can be calculated                             |        |
| APR-009  | Dataset item is not an unnecessary duplicate                  |        |
| APR-010  | Dataset item is ready for research evaluation                 |        |

### Approval Decision

Use one of the following:

```text
APPROVED
NEEDS_UPDATE
RETIRED
```

Only `APPROVED` items should be used for formal evaluation.

---

## Review Report Template

Each dataset item should have a review report.

Recommended location:

```text
datasets/workflow_evaluation/review_notes/
```

Recommended file naming:

```text
review_REQ-WF-001.md
review_REQ-WF-002.md
review_REQ-WF-003.md
```

Template:

```text
# Dataset Review Report — REQ-WF-001

## Metadata

Dataset Item ID:
API Name:
Requirement Category:
Complexity Level:
Reviewer:
Review Date:
Review Status:

---

## Source Requirement Review

Status:
Notes:

---

## Category Review

Status:
Notes:

---

## Sanitization Review

Status:
Notes:

---

## Ground Truth Workflow Review

Status:
Notes:

---

## Traceability Review

Status:
Notes:

---

## Ambiguity Review

Status:
Notes:

---

## Validation Checklist Alignment

Status:
Notes:

---

## Quality Metric Readiness

Status:
Notes:

---

## Duplicate and Coverage Review

Status:
Notes:

---

## Final Decision

Decision:

## Required Updates

List any required updates here.

## Approval Notes

List final approval notes here.
```

---

## Example Review Report

```text
# Dataset Review Report — REQ-WF-001

## Metadata

Dataset Item ID: REQ-WF-001
API Name: Create Application Message API
Requirement Category: Simple Create API
Complexity Level: MEDIUM
Reviewer: Aswin
Review Date: 2026-06-18
Review Status: APPROVED

---

## Source Requirement Review

Status: PASS
Notes:
The source requirement clearly defines endpoint behavior, required fields, validation rules, database insert behavior, and response mapping.

---

## Category Review

Status: PASS
Notes:
The sample is correctly categorized as Simple Create API with business validation.

---

## Sanitization Review

Status: PASS
Notes:
No confidential URLs, secrets, customer data, or internal-only identifiers are present.

---

## Ground Truth Workflow Review

Status: PASS
Notes:
Ground truth workflow correctly captures validation, overlap check, database insert, and response mapping.

---

## Traceability Review

Status: PASS
Notes:
Each workflow step maps back to the source requirement sections.

---

## Ambiguity Review

Status: PASS
Notes:
No major ambiguity identified.

---

## Validation Checklist Alignment

Status: PASS
Notes:
The dataset item supports endpoint, header, body, business rule, database, response, error, and traceability validation.

---

## Quality Metric Readiness

Status: PASS
Notes:
All relevant workflow quality metrics can be calculated.

---

## Duplicate and Coverage Review

Status: PASS
Notes:
This sample is useful because it tests create API behavior with database validation.

---

## Final Decision

Decision: APPROVED

## Required Updates

None.

## Approval Notes

This dataset item is ready for evaluation.
```

---

## Common Review Failures

The following issues should prevent approval:

| Issue                                      | Action                      |
| ------------------------------------------ | --------------------------- |
| Source requirement is too vague            | Mark NEEDS_UPDATE           |
| Ground truth includes unsupported logic    | Mark NEEDS_UPDATE           |
| Confidential data is present               | Mark NEEDS_UPDATE           |
| Required workflow steps are missing        | Mark NEEDS_UPDATE           |
| Ambiguity is guessed instead of documented | Mark NEEDS_UPDATE           |
| Dataset item duplicates another sample     | Revise or retire            |
| Quality metrics cannot be calculated       | Add more structure          |
| Traceability is missing                    | Add traceability references |

---

## Dataset Approval Criteria

A dataset item can be approved only when:

* the source requirement is clear enough
* requirement category is correct
* complexity level is correct
* sample is sanitized
* ground truth workflow is complete
* traceability is documented
* ambiguity is marked
* validation checklist can be applied
* quality metrics can be calculated
* sample adds value to the dataset
* sample is ready for research evaluation

---

## How This Review Process Supports the Project

This review process improves the reliability of the evaluation dataset.

It ensures that the dataset can fairly test the AI-generated workflows.

It also helps make the research results more credible because each evaluation item is reviewed before use.

A reviewed dataset supports stronger claims such as:

```text
The AI-generated workflows were evaluated against approved ground truth workflows from a curated enterprise API dataset.
```

This makes the project more research-ready.

---

## Relationship to Day 17 Validation Framework

The dataset review process connects directly to the Day 17 validation framework.

Day 17 defined how to validate generated workflows.

Day 18 defines how to review the dataset used for that validation.

Together, they create a complete evaluation foundation:

```text
Day 17:
How to validate generated workflows.

Day 18:
How to create and review the dataset used for validation.
```

---

## Summary

The dataset review process ensures that each evaluation dataset item is reliable, safe, and useful.

The review process checks:

* source requirement clarity
* requirement category
* sanitization
* ground truth workflow correctness
* traceability
* ambiguity handling
* validation checklist alignment
* quality metric readiness
* dataset coverage
* final approval status

Day 18 Phase 4 establishes the review process needed to make the workflow evaluation dataset trustworthy and research-ready.
