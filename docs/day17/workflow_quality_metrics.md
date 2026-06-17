# Day 17 — Workflow Quality Metrics

## Purpose

This document defines the workflow quality metrics for the 50-Day AI Software Engineer Challenge.

The purpose of these metrics is to measure how well a generated workflow represents the original enterprise API requirement.

A workflow should not be judged only by whether it is formatted correctly. It should also be evaluated based on completeness, accuracy, traceability, correctness of logic, and readiness for downstream artifact generation.

These metrics help convert workflow review from a subjective judgment into a structured evaluation process.

---

## Why Workflow Quality Metrics Are Needed

The project aims to transform enterprise requirements into structured workflow artifacts.

However, workflow generation can fail in many ways.

A generated workflow may:

* miss required validation logic
* skip business rules
* use the wrong downstream service
* map the wrong response fields
* add unsupported assumptions
* ignore error handling
* fail to preserve traceability

Quality metrics help measure these issues clearly.

They also support the research paper by giving measurable evidence for how well the AI-driven pipeline performs.

---

## Metric Evaluation Scale

Each metric can be scored using a percentage scale.

| Score Range | Meaning                                                    |
| ----------- | ---------------------------------------------------------- |
| 90–100%     | Excellent quality; ready for downstream generation         |
| 75–89%      | Good quality; minor review needed                          |
| 60–74%      | Moderate quality; several corrections needed               |
| 40–59%      | Weak quality; major gaps exist                             |
| 0–39%       | Poor quality; workflow should be regenerated or redesigned |

---

## Overall Workflow Quality Score

The overall workflow quality score is calculated by combining multiple quality dimensions.

Recommended formula:

```text
Overall Workflow Quality Score =
Requirement Coverage Score
+ Validation Accuracy Score
+ Business Rule Coverage Score
+ Downstream Mapping Accuracy Score
+ Database Mapping Accuracy Score
+ Response Mapping Accuracy Score
+ Error Handling Completeness Score
+ Traceability Score
+ Hallucination Control Score
+ Artifact Readiness Score
```

Each metric can be scored individually and then averaged.

Example:

```text
Overall Score = Sum of all metric scores / Number of metrics
```

---

## 1. Requirement Coverage Score

### Definition

Requirement Coverage Score measures how much of the original source requirement is represented in the generated workflow.

This includes:

* endpoint purpose
* request headers
* request body fields
* business rules
* downstream calls
* database checks
* response structure
* error scenarios

### Formula

```text
Requirement Coverage Score =
(Number of requirement elements represented in workflow / Total number of requirement elements) × 100
```

### Example

If the source requirement has 20 important elements and the workflow captures 18:

```text
18 / 20 × 100 = 90%
```

### Interpretation

| Score     | Interpretation                                           |
| --------- | -------------------------------------------------------- |
| 90–100%   | Most requirement elements are captured                   |
| 75–89%    | Minor requirement gaps exist                             |
| 60–74%    | Several requirement elements are missing                 |
| Below 60% | Workflow does not sufficiently represent the requirement |

---

## 2. Step Completeness Score

### Definition

Step Completeness Score measures whether the workflow contains all required processing steps.

A complete workflow should usually include:

* request intake
* header validation
* request body validation
* precondition checks
* downstream service calls
* database interactions
* business rule execution
* response mapping
* error handling

### Formula

```text
Step Completeness Score =
(Number of required workflow steps present / Total required workflow steps) × 100
```

### Example

If 10 workflow steps are required and 8 are present:

```text
8 / 10 × 100 = 80%
```

### Interpretation

A low score means the workflow may not be detailed enough to support artifact generation.

---

## 3. Validation Accuracy Score

### Definition

Validation Accuracy Score measures whether the generated workflow correctly captures validation rules.

This includes validation for:

* required headers
* optional headers
* required body fields
* enum values
* data types
* null or blank fields
* format rules
* conditional validation

### Formula

```text
Validation Accuracy Score =
(Number of correctly represented validation rules / Total validation rules in source requirement) × 100
```

### Example

If the source has 12 validation rules and the workflow correctly captures 10:

```text
10 / 12 × 100 = 83.3%
```

### Interpretation

A low validation accuracy score indicates that generated code or tests may fail basic API contract expectations.

---

## 4. Business Rule Coverage Score

### Definition

Business Rule Coverage Score measures whether the workflow correctly represents business logic from the source requirement.

This includes:

* eligibility rules
* reject conditions
* branching conditions
* calculation rules
* ordering rules
* status checks
* special-case handling

### Formula

```text
Business Rule Coverage Score =
(Number of business rules represented / Total business rules in source requirement) × 100
```

### Example

If the source contains 8 business rules and the workflow captures 7:

```text
7 / 8 × 100 = 87.5%
```

### Interpretation

Business rule coverage is one of the most important metrics because enterprise APIs often depend heavily on business-specific logic.

---

## 5. Step Ordering Accuracy Score

### Definition

Step Ordering Accuracy Score measures whether the workflow steps are placed in the correct logical sequence.

Correct ordering is important because a workflow may contain the right steps but still produce wrong results if the steps are executed in the wrong order.

### Expected Order Example

```text
Receive Request
↓
Validate Headers
↓
Validate Request Body
↓
Apply Preconditions
↓
Call Downstream Services
↓
Perform Database Checks
↓
Apply Business Logic
↓
Map Response
↓
Return Success or Error
```

### Formula

```text
Step Ordering Accuracy Score =
(Number of correctly ordered step relationships / Total required step relationships) × 100
```

### Example

If there are 9 required ordering relationships and 8 are correct:

```text
8 / 9 × 100 = 88.9%
```

---

## 6. Downstream Mapping Accuracy Score

### Definition

Downstream Mapping Accuracy Score measures whether external service calls are represented correctly.

This includes:

* downstream service name
* endpoint URL or path
* HTTP method
* request headers
* request body mapping
* authentication or signature requirement
* response handling
* error handling

### Formula

```text
Downstream Mapping Accuracy Score =
(Number of correctly mapped downstream elements / Total required downstream elements) × 100
```

### Example

If a downstream service requires 10 mapping elements and the workflow captures 9:

```text
9 / 10 × 100 = 90%
```

### Interpretation

A low score means the generated artifact may call the wrong service, send the wrong payload, or mishandle downstream responses.

---

## 7. Database Mapping Accuracy Score

### Definition

Database Mapping Accuracy Score measures whether database-related requirements are correctly represented.

This includes:

* database type
* table name
* query purpose
* query type
* input parameters
* output fields
* no-record behavior
* database error handling

### Formula

```text
Database Mapping Accuracy Score =
(Number of correctly mapped database elements / Total required database elements) × 100
```

### Example

If there are 7 required database mapping elements and 6 are correct:

```text
6 / 7 × 100 = 85.7%
```

### Interpretation

A low database mapping score can result in generated code that queries the wrong table, uses wrong parameters, or mishandles query results.

---

## 8. Response Mapping Accuracy Score

### Definition

Response Mapping Accuracy Score measures whether the generated workflow correctly maps the final API response.

This includes:

* success response structure
* error response structure
* response field names
* response field data types
* downstream-to-API field mapping
* transformation logic
* default value handling

### Formula

```text
Response Mapping Accuracy Score =
(Number of correctly mapped response elements / Total response elements in requirement) × 100
```

### Example

If a response has 15 required fields and the workflow maps 13 correctly:

```text
13 / 15 × 100 = 86.7%
```

### Interpretation

A low response mapping score means the generated API may return an incorrect contract to the client.

---

## 9. Error Handling Completeness Score

### Definition

Error Handling Completeness Score measures whether the workflow handles required failure scenarios.

This includes:

* missing header errors
* invalid header errors
* missing request body field errors
* invalid request body field errors
* business rule failures
* downstream service failures
* database failures
* no-record scenarios
* unexpected errors

### Formula

```text
Error Handling Completeness Score =
(Number of represented error scenarios / Total required error scenarios) × 100
```

### Example

If the requirement has 9 error scenarios and the workflow captures 6:

```text
6 / 9 × 100 = 66.7%
```

### Interpretation

Low error handling completeness means the workflow may only represent the happy path and may not be production-ready.

---

## 10. Traceability Score

### Definition

Traceability Score measures how well workflow steps are linked back to the source requirement.

Each workflow step should ideally reference:

* source requirement ID
* business rule ID
* endpoint ID
* sample request reference
* sample response reference
* mapping rule reference

### Formula

```text
Traceability Score =
(Number of workflow steps with valid source references / Total workflow steps) × 100
```

### Example

If a workflow has 12 steps and 10 have traceability references:

```text
10 / 12 × 100 = 83.3%
```

### Interpretation

A low traceability score makes it difficult to prove that the workflow is based on the source requirement.

---

## 11. Hallucination Control Score

### Definition

Hallucination Control Score measures whether the workflow avoids adding unsupported logic.

Unsupported logic may include invented:

* headers
* request fields
* authorization rules
* downstream services
* database tables
* business rules
* response fields
* error behavior

Unlike other scores, this score is based on how many unsupported elements are absent.

### Formula

```text
Hallucination Control Score =
(1 - Number of unsupported workflow elements / Total workflow elements) × 100
```

### Example

If a workflow has 20 elements and 2 are unsupported:

```text
(1 - 2 / 20) × 100 = 90%
```

### Interpretation

A high score means the workflow stays close to the source requirement.

A low score means the workflow may contain assumptions or hallucinated logic.

---

## 12. Artifact Readiness Score

### Definition

Artifact Readiness Score measures whether the workflow contains enough structured detail to support downstream artifact generation.

The workflow should be usable for generating:

* Node-RED flows
* Excel intake files
* Go code later in the roadmap
* test cases
* documentation
* gap analysis reports

### Formula

```text
Artifact Readiness Score =
(Number of artifact-ready workflow sections / Total expected artifact sections) × 100
```

### Example

If the workflow supports 5 out of 6 artifact generation needs:

```text
5 / 6 × 100 = 83.3%
```

### Interpretation

A low score means the workflow may be understandable to a human but not structured enough for automation.

---

## Recommended Metric Weights

Not all metrics have equal importance.

For this project, the recommended weighting is:

| Metric                            | Weight |
| --------------------------------- | -----: |
| Requirement Coverage Score        |    15% |
| Business Rule Coverage Score      |    15% |
| Validation Accuracy Score         |    10% |
| Step Completeness Score           |    10% |
| Step Ordering Accuracy Score      |    10% |
| Downstream Mapping Accuracy Score |    10% |
| Database Mapping Accuracy Score   |    10% |
| Response Mapping Accuracy Score   |    10% |
| Error Handling Completeness Score |     5% |
| Traceability Score                |     3% |
| Hallucination Control Score       |     1% |
| Artifact Readiness Score          |     1% |

Total:

```text
100%
```

---

## Weighted Score Formula

The weighted workflow quality score can be calculated as:

```text
Weighted Workflow Quality Score =
(Requirement Coverage × 0.15)
+ (Business Rule Coverage × 0.15)
+ (Validation Accuracy × 0.10)
+ (Step Completeness × 0.10)
+ (Step Ordering Accuracy × 0.10)
+ (Downstream Mapping Accuracy × 0.10)
+ (Database Mapping Accuracy × 0.10)
+ (Response Mapping Accuracy × 0.10)
+ (Error Handling Completeness × 0.05)
+ (Traceability × 0.03)
+ (Hallucination Control × 0.01)
+ (Artifact Readiness × 0.01)
```

---

## Example Quality Evaluation

### Example Input

```text
Workflow ID: WF-001
Endpoint: POST /api/v1/discount/interrogateAccount
Source Requirement: Account status check using encrypted account data
```

### Example Scores

| Metric                      | Score |
| --------------------------- | ----: |
| Requirement Coverage        |    90 |
| Business Rule Coverage      |    85 |
| Validation Accuracy         |    80 |
| Step Completeness           |    88 |
| Step Ordering Accuracy      |    90 |
| Downstream Mapping Accuracy |    92 |
| Database Mapping Accuracy   |    75 |
| Response Mapping Accuracy   |    86 |
| Error Handling Completeness |    70 |
| Traceability                |    82 |
| Hallucination Control       |    95 |
| Artifact Readiness          |    88 |

### Example Weighted Result

```text
Weighted Workflow Quality Score: 85.45%
```

### Interpretation

```text
Decision: PASS WITH WARNINGS
Reason:
The workflow has strong coverage and downstream mapping, but error handling and database mapping need review before artifact generation.
```

---

## Quality Decision Thresholds

| Final Score | Decision           |
| ----------- | ------------------ |
| 90–100%     | PASS               |
| 75–89%      | PASS WITH WARNINGS |
| 60–74%      | NEEDS REVIEW       |
| Below 60%   | FAIL               |

---

## How Metrics Support Research Evaluation

These metrics help evaluate the AI Software Engineer system in a measurable way.

They support the research paper by allowing comparison between:

* manually created workflows
* AI-generated workflows
* AI-generated workflows after validation
* corrected final workflows

The metrics also help measure improvement over time.

For example:

```text
Initial AI Workflow Quality: 68%
Validated AI Workflow Quality: 86%
Human-Corrected Workflow Quality: 95%
```

This can show whether the validation framework improves the reliability of AI-generated workflow artifacts.

---

## How Metrics Support the 50-Day Challenge

The workflow quality metrics support the broader project goals by creating a measurable evaluation layer.

They help answer:

* Did the AI capture the requirement correctly?
* Did the workflow preserve business logic?
* Is the workflow ready for Excel generation?
* Is the workflow ready for Node-RED generation?
* Is the workflow ready for future Go code generation?
* Did validation reduce hallucinated logic?
* Can workflow quality be compared across APIs?

These answers are important for both the technical project and the research paper.

---

## Summary

Workflow quality metrics provide a structured way to measure generated workflow correctness.

The key metrics are:

* requirement coverage
* step completeness
* validation accuracy
* business rule coverage
* step ordering accuracy
* downstream mapping accuracy
* database mapping accuracy
* response mapping accuracy
* error handling completeness
* traceability
* hallucination control
* artifact readiness

Together, these metrics make workflow validation measurable, repeatable, and useful for research evaluation.

Day 17 Phase 3 establishes the scoring framework that will later help prove whether AI-generated workflows are reliable enough to support enterprise API artifact generation.
