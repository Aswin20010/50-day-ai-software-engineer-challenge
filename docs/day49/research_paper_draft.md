# Day 49 – Phase 3

# Research Methodology and Evaluation Plan

## Project

**AI-Assisted API Factory and Migration Analyzer**

## Research Paper Working Title

**AI-Assisted API Modernization: A Rule-Based Architecture for Requirement Extraction, API Generation, and Migration Analysis**

---

## 1. Phase 3 Objective

The objective of Day 49 Phase 3 is to define a repeatable and academically defensible methodology for evaluating the proposed hybrid API modernization architecture.

This phase defines:

* The research methodology.
* The experimental design.
* The evaluation datasets.
* The ground-truth preparation process.
* The baseline comparison approaches.
* The experiment execution procedure.
* The quantitative and qualitative metrics.
* The human-review process.
* The data-recording templates.
* The reproducibility requirements.
* The rules for interpreting and reporting results.

This phase defines the evaluation plan only. Any numerical values included as goals or thresholds must not be reported as achieved results until the experiments are executed.

---

# 2. Research Methodology

## 2.1 Selected Methodology

The research will use a combined:

1. **Design-science research methodology**
2. **Controlled comparative evaluation**
3. **Multiple-case study**
4. **Human expert review**

Design-science research is appropriate because the project creates and evaluates a technical artifact intended to solve a practical software-engineering problem.

The artifacts being evaluated are:

* The API Factory.
* The API Migration Analyzer.
* The structured requirement model.
* The deterministic validation pipeline.
* The evidence-extraction model.
* The target-architecture rule model.
* The gap and risk analysis process.
* The recommendation and migration-task generators.
* The browser-based human-review workflow.

---

## 2.2 Design-Science Research Cycle

The research follows the stages below.

### Stage 1 – Problem Identification

Identify challenges in enterprise API development and modernization:

* Manual requirement interpretation.
* Inconsistent API specifications.
* Missing validation rules.
* Poor traceability.
* Time-consuming legacy-code analysis.
* Subjective architecture assessments.
* Unstructured modernization recommendations.
* Risks from unrestricted AI-generated output.

### Stage 2 – Define Solution Objectives

The solution should:

* Extract structured API requirements.
* Validate AI-assisted output deterministically.
* Preserve traceability to source inputs.
* Analyze legacy API source artifacts.
* Compare observed capabilities with a target architecture.
* Detect gaps and risks consistently.
* Produce explainable readiness scores.
* Generate actionable recommendations and migration tasks.
* Preserve human review and approval.

### Stage 3 – Design and Development

Develop the two primary systems:

* API Factory.
* API Migration Analyzer.

Develop supporting artifacts:

* TypeScript domain models.
* Requirement schemas.
* Validation rules.
* Evidence models.
* Architecture criteria.
* Analysis rules.
* Scoring logic.
* Browser workflows.
* Automated validation scripts.

### Stage 4 – Demonstration

Demonstrate the system using multiple API cases representing different levels of complexity and modernization readiness.

### Stage 5 – Evaluation

Evaluate the artifacts using:

* Requirement extraction metrics.
* Validation defect-detection metrics.
* Artifact completeness.
* Gap-detection accuracy.
* Risk-detection accuracy.
* Evidence traceability.
* Repeated-run consistency.
* Human-review scores.
* Engineering time measurements.

### Stage 6 – Communication

Document:

* Architecture.
* Methodology.
* Results.
* Limitations.
* Threats to validity.
* Reproducibility instructions.
* Future research opportunities.

---

# 3. Evaluation Objectives

The evaluation should determine whether the proposed system improves the following qualities.

## 3.1 Accuracy

Determine whether the system correctly extracts and analyzes:

* Endpoints.
* Request and response fields.
* Headers.
* Validation rules.
* Business rules.
* Source capabilities.
* Architecture gaps.
* Technical risks.
* Recommendations.

## 3.2 Completeness

Determine whether the system identifies all expected requirements and modernization findings.

## 3.3 Validity

Determine whether deterministic controls identify malformed, unsupported, incomplete, or contradictory outputs.

## 3.4 Traceability

Determine whether findings and recommendations can be traced to:

* Requirement text.
* Source artifacts.
* Evidence identifiers.
* Architecture rules.
* Gap or risk identifiers.

## 3.5 Consistency

Determine whether repeated runs produce stable structured outputs and analysis results.

## 3.6 Efficiency

Determine whether the system reduces the time required for requirement structuring, codebase analysis, migration planning, and engineering review.

## 3.7 Actionability

Determine whether generated recommendations and migration tasks are sufficiently specific for implementation planning.

---

# 4. Research Questions and Evaluation Mapping

| Research Question                    | Evaluation Focus                                     | Primary Metrics                                 |
| ------------------------------------ | ---------------------------------------------------- | ----------------------------------------------- |
| RQ1: Requirement extraction accuracy | Extracted structured requirements                    | Precision, recall and F1                        |
| RQ2: Deterministic validation value  | Invalid and incomplete output detection              | Defect-detection rate and validation coverage   |
| RQ3: API artifact generation         | Completeness and traceability of generated artifacts | Artifact completeness and traceability          |
| RQ4: Migration-analysis accuracy     | Gaps, risks and architecture findings                | Precision, recall and F1                        |
| RQ5: Explainability                  | Evidence-backed findings and reviewer experience     | Traceability coverage and reviewer score        |
| RQ6: Engineering efficiency          | Manual effort compared with assisted workflow        | Completion time, corrections and time reduction |

---

# 5. Experimental Approaches

The evaluation will compare three approaches where applicable.

## 5.1 Baseline A – Manual Engineering Analysis

An engineer manually reviews the requirement documentation or source artifacts.

The engineer produces:

* A structured API requirement specification.
* A list of endpoints.
* Request and response fields.
* Validation and business rules.
* Architecture findings.
* Technical risks.
* Migration recommendations.
* Migration tasks.

### Purpose

This baseline represents the existing human-driven process.

### Measurements

Record:

* Total completion time.
* Number of review passes.
* Number of corrections.
* Output completeness.
* Agreement with ground truth.

---

## 5.2 Baseline B – AI-Only Analysis

An AI model analyzes the same input without the proposed deterministic validation and evidence-rule pipeline.

The AI-only approach may produce:

* Structured requirements.
* Architecture observations.
* Risks.
* Recommendations.
* Migration tasks.

### Restrictions

The AI-only output must not receive:

* The deterministic validation result.
* The architecture rule result.
* The expected ground truth.
* Manual corrections before measurement.

### Purpose

This baseline isolates the effect of unrestricted AI generation.

### Measurements

Record:

* Precision and recall.
* Unsupported findings.
* Missing findings.
* Structural validity.
* Evidence quality.
* Repeated-run variance.
* Human correction effort.

---

## 5.3 Proposed Hybrid Approach

The proposed approach uses:

1. AI-assisted interpretation when enabled.
2. Structured domain schemas.
3. Deterministic normalization.
4. Schema validation.
5. Rule-based validation.
6. Source evidence extraction.
7. Target-architecture comparison.
8. Gap and risk rules.
9. Evidence-linked recommendations.
10. Human review.

### Purpose

This is the primary artifact being evaluated.

### Measurements

Use the same accuracy, time, consistency, traceability, and actionability metrics applied to the baselines.

---

# 6. Evaluation Dataset Design

## 6.1 Dataset Categories

The evaluation should include multiple cases rather than relying on a single API.

Recommended cases:

### Case C1 – Simple CRUD API

Characteristics:

* Three to five endpoints.
* Basic request validation.
* One database entity.
* Standard success and error responses.
* No external integrations.

Purpose:

* Validate basic requirement extraction.
* Validate endpoint detection.
* Establish a low-complexity baseline.

### Case C2 – Validation-Heavy API

Characteristics:

* Required and optional fields.
* String patterns.
* Numeric ranges.
* Conditional requirements.
* Cross-field validation.
* Multiple error responses.

Purpose:

* Evaluate validation-rule extraction.
* Evaluate missing-field and contradiction detection.

### Case C3 – Transactional Enterprise API

Characteristics:

* Handler, service and repository layers.
* Database transaction boundaries.
* Status transitions.
* Idempotency requirements.
* Multiple business rules.
* Structured error handling.

Purpose:

* Evaluate enterprise behavior and architecture analysis.
* Evaluate data-integrity and transaction findings.

### Case C4 – External Integration API

Characteristics:

* Downstream service client.
* Authentication headers.
* Timeout behavior.
* Retry or fallback requirements.
* External response mapping.
* Trace identifiers.

Purpose:

* Evaluate dependency and integration evidence.
* Evaluate resilience and observability findings.

### Case C5 – Legacy API

Characteristics:

* Mixed responsibilities.
* Limited layer separation.
* Inconsistent validation.
* Weak test coverage.
* Hard-coded configuration.
* Minimal structured logging.

Purpose:

* Evaluate gap and risk detection.
* Evaluate modernization recommendations.

### Case C6 – Partially Modernized API

Characteristics:

* Modern framework.
* Some layered architecture.
* Partial observability.
* Partial test coverage.
* Remaining legacy components.

Purpose:

* Evaluate partial rule outcomes.
* Evaluate whether the system avoids overstating deficiencies.

### Case C7 – Secure Layered API

Characteristics:

* Authentication.
* Authorization.
* Input validation.
* Secrets management.
* Structured logging.
* Automated tests.
* Database transaction management.

Purpose:

* Evaluate false-positive control.
* Verify that implemented capabilities are recognized.

### Case C8 – Incomplete Requirement Document

Characteristics:

* Missing response fields.
* Ambiguous business rules.
* Undefined error behavior.
* Conflicting terminology.

Purpose:

* Evaluate missing-information detection.
* Verify that the system does not invent unsupported requirements.

---

## 6.2 Minimum Dataset Size

The preferred evaluation should contain:

* At least six requirement cases.
* At least four source-code analysis cases.
* At least two cases evaluated by all three approaches.
* At least one intentionally incomplete case.
* At least one well-architected case for false-positive analysis.

A smaller initial evaluation may be used for a prototype paper, but the limitation must be stated explicitly.

---

## 6.3 Dataset Independence

To reduce evaluation bias:

* Avoid testing only with examples used during development.
* Include unseen inputs.
* Include naming conventions different from the implementation examples.
* Include at least one case with intentionally misleading or ambiguous wording.
* Keep evaluation cases separate from unit-test fixtures where practical.

---

## 6.4 Dataset Anonymization

Before publication:

* Remove company names.
* Remove employee names.
* Remove internal URLs.
* Remove credentials and secrets.
* Replace account identifiers.
* Replace customer data.
* Replace proprietary system names.
* Remove confidential business thresholds where required.

Synthetic equivalents may be used while preserving the technical structure of the original case.

---

# 7. Ground-Truth Preparation

## 7.1 Purpose

Ground truth represents the expected correct output against which generated output will be measured.

Each case requires a manually verified reference specification.

---

## 7.2 Requirement Ground Truth

For each requirement case, document:

* Flow name.
* Endpoint name.
* HTTP method.
* Endpoint path.
* Required headers.
* Optional headers.
* Path parameters.
* Query parameters.
* Request fields.
* Response fields.
* Data types.
* Required indicators.
* Validation rules.
* Business rules.
* Error codes.
* Error messages.
* External integrations.
* Database interactions.
* Security requirements.
* Non-functional requirements.
* Explicitly missing or ambiguous information.

---

## 7.3 Migration Ground Truth

For each source-code case, document:

* Existing endpoints.
* Existing architectural layers.
* Existing validation behavior.
* Existing security controls.
* Existing data-access behavior.
* Existing logging and tracing.
* Existing tests.
* Existing configuration handling.
* Expected target capabilities.
* Confirmed gaps.
* Confirmed risks.
* Applicable recommendations.
* Non-applicable rules.

---

## 7.4 Ground-Truth Annotation Format

Use a structured representation.

```ts
export interface EvaluationGroundTruth {
  readonly caseId: string;
  readonly caseName: string;
  readonly expectedEndpoints: readonly ExpectedEndpoint[];
  readonly expectedRequirements: readonly ExpectedRequirement[];
  readonly expectedEvidence: readonly ExpectedEvidence[];
  readonly expectedGaps: readonly ExpectedGap[];
  readonly expectedRisks: readonly ExpectedRisk[];
  readonly expectedRecommendations: readonly ExpectedRecommendation[];
  readonly ambiguities: readonly GroundTruthAmbiguity[];
}
```

The exact implementation can differ, but the stored ground truth must remain machine-comparable where possible.

---

## 7.5 Review Process

Recommended review procedure:

1. Primary reviewer prepares the expected output.
2. Secondary reviewer independently checks the output.
3. Disagreements are recorded.
4. Reviewers resolve disagreements using the original source.
5. The final ground truth is versioned.
6. Post-experiment changes require a documented reason.

Where only one reviewer is available, that limitation must be documented.

---

## 7.6 Ground-Truth Freeze

Ground truth must be frozen before the main evaluation begins.

Record:

* Ground-truth version.
* Freeze date.
* Reviewer.
* Change history.
* Dataset version.

This prevents expected answers from being changed to match system output.

---

# 8. Experiment Variables

## 8.1 Independent Variable

The primary independent variable is the analysis approach:

* Manual.
* AI-only.
* Hybrid AI and deterministic.

Additional independent variables may include:

* Case complexity.
* Requirement quality.
* Target architecture.
* AI model.
* Prompt version.
* Rule-set version.

---

## 8.2 Dependent Variables

Measure:

* Precision.
* Recall.
* F1 score.
* Structural validity.
* Validation defect-detection rate.
* Traceability coverage.
* Execution time.
* Reviewer time.
* Correction count.
* Finding consistency.
* Recommendation actionability.
* Score variance.

---

## 8.3 Controlled Variables

Keep the following constant where possible:

* Input content.
* Input file order.
* Target architecture.
* Prompt template.
* Model settings.
* Schema version.
* Rule-set version.
* Hardware environment.
* Timing procedure.
* Reviewer rubric.

---

## 8.4 Confounding Variables

Potential confounding variables include:

* Engineer familiarity with the test case.
* AI-model updates.
* Prompt changes.
* Different reviewer skill levels.
* Varying case complexity.
* Cached model behavior.
* Machine load.
* Incomplete source artifacts.

These variables must be controlled or documented.

---

# 9. Experiment Execution Procedure

## 9.1 Preparation

For every evaluation run:

1. Assign a unique run identifier.
2. Record the case identifier.
3. Record the approach.
4. Record software and rule versions.
5. Record the input checksum.
6. Reset the application state.
7. Clear prior output.
8. Prepare the timing mechanism.
9. Confirm that ground truth is not exposed to the evaluated approach.

---

## 9.2 Manual Baseline Procedure

1. Give the evaluator the source input.
2. Start the timer.
3. Allow the evaluator to inspect the provided artifacts.
4. Require the evaluator to complete the standardized output template.
5. Stop the timer when the output is submitted.
6. Compare the output with ground truth.
7. Record corrections and missing findings.

The evaluator should not use the proposed system during the manual baseline.

---

## 9.3 AI-Only Procedure

1. Provide the original input to the selected AI model.
2. Use a frozen prompt.
3. Record model and configuration.
4. Generate the output.
5. Do not apply deterministic correction.
6. Save the raw response.
7. Parse the response only for measurement where needed.
8. Compare the output with ground truth.
9. Repeat the run at least three times.

If parsing fails, record the failure rather than manually repairing it before measurement.

---

## 9.4 Hybrid Workflow Procedure

1. Load the same source input.
2. Start the timer.
3. Run AI-assisted extraction if enabled.
4. Store the raw extraction result.
5. Apply normalization.
6. Apply schema validation.
7. Record validation errors.
8. Apply rule-based validation.
9. Generate the structured specification.
10. Extract source evidence.
11. Run target-architecture analysis.
12. Generate gaps and risks.
13. Generate recommendations and migration tasks.
14. Record summary scores.
15. Stop the timer.
16. Save the full structured result.
17. Compare the output with ground truth.
18. Conduct human review.

---

## 9.5 Deterministic Rerun Procedure

To separate AI variability from downstream system consistency:

1. Save a valid structured extraction result.
2. Run only the deterministic stages multiple times.
3. Compare all outputs.
4. Confirm stable ordering or normalize ordering before comparison.
5. Record any unexpected variation as a defect.

Expected result:

* Equivalent inputs should produce equivalent deterministic outputs.

---

# 10. Repetition Strategy

## 10.1 AI-Only Runs

Run each AI-only case at least three times.

Purpose:

* Measure output variability.
* Detect hallucinated additions.
* Measure finding overlap.
* Calculate score variance where scores are requested.

---

## 10.2 Hybrid Runs

Run each hybrid case at least three times when AI extraction is involved.

Also perform deterministic reruns using a stored structured input.

---

## 10.3 Manual Runs

Manual analysis may be performed once per reviewer due to time cost.

Where multiple reviewers are available:

* Each reviewer should work independently.
* Reviewer identity should be anonymized in published results.
* Inter-reviewer agreement should be measured.

---

# 11. Quantitative Evaluation Metrics

## 11.1 True Positives, False Positives and False Negatives

For each output category:

* **True positive:** Expected item correctly identified.
* **False positive:** Item identified but not supported by ground truth.
* **False negative:** Expected item not identified.

True negatives are generally less useful where the set of all possible requirements or findings is undefined.

---

## 11.2 Precision

```text
Precision = TP / (TP + FP)
```

Precision measures how many generated items are correct.

Use for:

* Requirements.
* Evidence.
* Gaps.
* Risks.
* Recommendations.

---

## 11.3 Recall

```text
Recall = TP / (TP + FN)
```

Recall measures how many expected items were found.

---

## 11.4 F1 Score

```text
F1 = 2 × Precision × Recall / (Precision + Recall)
```

Use F1 when both false positives and missed findings matter.

---

## 11.5 Exact-Match Accuracy

Use exact matching for deterministic properties such as:

* HTTP method.
* Endpoint path.
* Required flag.
* Data type.
* Severity.
* Rule status.
* Evidence identifier.

```text
Exact-match accuracy =
exactly matched properties / total expected properties
```

---

## 11.6 Semantic Match

Textual descriptions may express the same meaning using different wording.

Use a structured reviewer judgment:

* Exact match.
* Semantically equivalent.
* Partially equivalent.
* Incorrect.
* Unsupported.

Do not use unrestricted automated semantic similarity as the only correctness measure.

---

## 11.7 Structural Validity Rate

```text
Structural validity rate =
outputs passing schema validation / total outputs
```

This metric is especially important for comparing AI-only output with the hybrid workflow.

---

## 11.8 Validation Defect-Detection Rate

Create intentionally invalid structured inputs containing known defects.

Possible injected defects:

* Missing endpoint path.
* Unsupported HTTP method.
* Duplicate requirement identifier.
* Invalid data type.
* Missing rule description.
* Invalid severity.
* Broken evidence reference.
* Recommendation without a linked finding.
* Migration task without acceptance criteria.

```text
Defect-detection rate =
detected injected defects / total injected defects
```

---

## 11.9 Validation False-Positive Rate

```text
Validation false-positive rate =
valid elements incorrectly rejected / total valid elements
```

A validator that rejects correct data too frequently is not useful even if it catches invalid data.

---

## 11.10 Traceability Coverage

```text
Traceability coverage =
supported findings with valid source links / total findings
```

A valid link must resolve to:

* Source requirement.
* Source artifact.
* Evidence item.
* Architecture rule.

---

## 11.11 Evidence Accuracy

```text
Evidence accuracy =
correct evidence references / total evidence references
```

An evidence reference is correct only when the cited artifact supports the finding.

---

## 11.12 Recommendation Coverage

```text
Recommendation coverage =
actionable confirmed gaps with recommendations / total actionable confirmed gaps
```

Not every observation requires a recommendation. Non-actionable items should be marked accordingly.

---

## 11.13 Migration Task Completeness

Evaluate whether each task contains:

* Title.
* Description.
* Priority.
* Affected component.
* Linked finding.
* Acceptance criteria.

```text
Task completeness =
present required task attributes / total required task attributes
```

---

## 11.14 Repeated-Run Finding Overlap

For two runs, use Jaccard similarity:

```text
Jaccard similarity =
size of intersection / size of union
```

Apply to normalized sets of:

* Requirements.
* Gaps.
* Risks.
* Recommendations.

---

## 11.15 Score Variance

When the system produces a readiness score, calculate:

* Mean.
* Minimum.
* Maximum.
* Standard deviation.
* Range.

High score variation for the same input indicates instability.

---

## 11.16 Completion Time

Record time separately for:

* Input preparation.
* Requirement extraction.
* Validation.
* Migration analysis.
* Review.
* Correction.
* Final output preparation.

Avoid combining user reading time and system execution time without explaining the measurement.

---

## 11.17 Time Reduction

```text
Time reduction percentage =
(Manual time - Assisted time) / Manual time × 100
```

Report both the absolute and percentage difference.

---

## 11.18 Correction Rate

```text
Correction rate =
items requiring correction / total generated items
```

Corrections include:

* Deletion.
* Addition.
* Reclassification.
* Rewording required to fix technical meaning.
* Evidence-link correction.

Cosmetic edits should be recorded separately.

---

# 12. Qualitative Evaluation

## 12.1 Human Review Dimensions

Reviewers should evaluate outputs based on:

1. Correctness.
2. Completeness.
3. Explainability.
4. Evidence quality.
5. Actionability.
6. Priority clarity.
7. Ease of review.
8. Overall confidence.

---

## 12.2 Likert Scale

Use a five-point scale:

| Score | Interpretation                                             |
| ----: | ---------------------------------------------------------- |
|     1 | Strongly disagree or unusable                              |
|     2 | Mostly incorrect or difficult to use                       |
|     3 | Partially useful but requires major review                 |
|     4 | Useful with minor corrections                              |
|     5 | Highly useful and implementation-ready after normal review |

---

## 12.3 Reviewer Questionnaire

Suggested statements:

* The extracted requirements accurately represent the source.
* The output captures the important requirements.
* Unsupported assumptions are clearly avoided or identified.
* Architecture findings are supported by evidence.
* The readiness score is understandable.
* Recommendations address the identified findings.
* Migration tasks are sufficiently specific for planning.
* The output is easier to verify than an unsupported AI response.
* The workflow reduces manual analysis effort.
* I would use this output as a starting point for an engineering review.

---

## 12.4 Reviewer Comments

For every score below four, request a brief reason.

Suggested comment categories:

* Incorrect.
* Missing.
* Unsupported.
* Too generic.
* Wrong priority.
* Insufficient evidence.
* Duplicate.
* Not actionable.
* Difficult to understand.

---

# 13. Inter-Reviewer Agreement

Where at least two reviewers are available, measure agreement.

## 13.1 Categorical Findings

For correct, partial or incorrect classifications, calculate:

* Percentage agreement.
* Cohen’s kappa where appropriate.

## 13.2 Rating Agreement

For Likert-scale evaluations, report:

* Mean rating.
* Median rating.
* Rating range.
* Intraclass correlation if the reviewer count and dataset support it.

Do not overstate statistical conclusions when the reviewer sample is small.

---

# 14. Architecture Scoring Evaluation

## 14.1 Rule Outcome Categories

Each target-architecture rule should produce one of:

* Passed.
* Partially passed.
* Failed.
* Not applicable.
* Insufficient evidence.

---

## 14.2 Rule Weighting

Suggested default weighting:

| Severity      | Weight |
| ------------- | -----: |
| Critical      |      5 |
| High          |      4 |
| Medium        |      3 |
| Low           |      2 |
| Informational |      1 |

The final system may use a different model, but the rationale must be documented.

---

## 14.3 Outcome Credit

Suggested credit:

| Outcome               |                     Credit |
| --------------------- | -------------------------: |
| Passed                |                        1.0 |
| Partially passed      |                        0.5 |
| Failed                |                        0.0 |
| Insufficient evidence | 0.0 or reported separately |
| Not applicable        |                   Excluded |

The treatment of insufficient evidence must be chosen before evaluation and applied consistently.

---

## 14.4 Category Score

```text
Category score =
sum of earned weighted credit /
sum of applicable rule weights × 100
```

---

## 14.5 Overall Readiness Score

```text
Overall readiness score =
sum of category score × category weight /
sum of category weights
```

---

## 14.6 Scoring Accuracy

Compare the system score against an expert-assigned score or readiness band.

Suggested readiness bands:

|  Score | Readiness                          |
| -----: | ---------------------------------- |
| 90–100 | High readiness                     |
|  75–89 | Moderate-to-high readiness         |
|  60–74 | Moderate readiness                 |
|  40–59 | Low readiness                      |
|   0–39 | Significant modernization required |

These bands are evaluation conventions, not universal industry standards.

---

## 14.7 Scoring Error

```text
Absolute scoring error =
absolute value of system score - expert score
```

Report mean absolute error across cases where expert scores are available.

---

# 15. Test Cases for Deterministic Validation

## 15.1 Requirement Model Tests

Validate detection of:

* Missing flow name.
* Empty endpoint list.
* Missing endpoint identifier.
* Invalid HTTP method.
* Invalid path.
* Duplicate endpoint identifier.
* Duplicate field identifier.
* Invalid required flag.
* Unsupported field type.
* Missing rule description.
* Rule with invalid target field.

---

## 15.2 Evidence Model Tests

Validate detection of:

* Missing artifact identifier.
* Missing source location.
* Empty evidence text.
* Invalid evidence category.
* Duplicate evidence.
* Evidence linked to an unknown artifact.

---

## 15.3 Finding Model Tests

Validate detection of:

* Gap without an architecture rule.
* Risk without severity.
* Finding without evidence when evidence is required.
* Duplicate gap.
* Duplicate risk.
* Invalid score contribution.

---

## 15.4 Recommendation and Task Tests

Validate detection of:

* Recommendation without linked findings.
* Migration task without recommendation.
* Missing priority.
* Missing affected component.
* Missing acceptance criteria.
* Circular task dependency.
* Unknown dependency identifier.

---

# 16. Evaluation Result Recording

## 16.1 Run Metadata

Store the following for every run:

```ts
export interface EvaluationRunMetadata {
  readonly runId: string;
  readonly caseId: string;
  readonly approach: "manual" | "ai-only" | "hybrid";
  readonly startedAt: string;
  readonly completedAt: string;
  readonly durationMs: number;
  readonly datasetVersion: string;
  readonly groundTruthVersion: string;
  readonly promptVersion?: string;
  readonly modelName?: string;
  readonly modelConfiguration?: string;
  readonly schemaVersion: string;
  readonly ruleSetVersion: string;
  readonly targetArchitectureId?: string;
  readonly inputChecksum: string;
}
```

---

## 16.2 Accuracy Result

```ts
export interface EvaluationAccuracyResult {
  readonly category: string;
  readonly truePositives: number;
  readonly falsePositives: number;
  readonly falseNegatives: number;
  readonly precision: number;
  readonly recall: number;
  readonly f1Score: number;
}
```

---

## 16.3 Review Result

```ts
export interface EvaluationReviewResult {
  readonly runId: string;
  readonly reviewerId: string;
  readonly correctness: number;
  readonly completeness: number;
  readonly explainability: number;
  readonly evidenceQuality: number;
  readonly actionability: number;
  readonly confidence: number;
  readonly reviewDurationMs: number;
  readonly correctionCount: number;
  readonly comments: readonly string[];
}
```

---

# 17. Recommended Evaluation Folder Structure

```text
evaluation/
├── README.md
├── datasets/
│   ├── requirements/
│   ├── source-projects/
│   └── manifests/
├── ground-truth/
│   ├── requirements/
│   ├── migration-analysis/
│   └── versions/
├── prompts/
│   ├── ai-only/
│   └── hybrid-extraction/
├── runs/
│   ├── manual/
│   ├── ai-only/
│   └── hybrid/
├── reviews/
├── results/
│   ├── accuracy/
│   ├── consistency/
│   ├── timing/
│   ├── traceability/
│   └── human-review/
└── scripts/
    ├── calculate-metrics.mjs
    ├── compare-runs.mjs
    ├── validate-ground-truth.mjs
    └── generate-results-summary.mjs
```

---

# 18. Experiment Manifest

Each case should have a manifest.

```json
{
  "caseId": "C1",
  "caseName": "Simple CRUD API",
  "description": "Basic API with standard validation and database access.",
  "requirementInput": "datasets/requirements/c1.md",
  "sourceArtifacts": "datasets/source-projects/c1/",
  "groundTruth": "ground-truth/c1.json",
  "targetArchitectureId": "layered-enterprise-api",
  "enabledApproaches": [
    "manual",
    "ai-only",
    "hybrid"
  ],
  "repetitions": {
    "manual": 1,
    "ai-only": 3,
    "hybrid": 3,
    "deterministicRerun": 3
  }
}
```

---

# 19. Reproducibility Requirements

The research repository should preserve:

* Dataset version.
* Ground-truth version.
* Source-artifact checksum.
* Prompt text.
* Prompt version.
* Model name.
* Model configuration.
* Schema version.
* Rule-set version.
* Target-architecture version.
* Dependency lockfile.
* Operating environment.
* Commands used.
* Raw output.
* Normalized output.
* Validation output.
* Final analysis result.

---

## 19.1 Recommended Commands

The final repository should provide commands similar to:

```bash
npm run lint
npm run test
npm run build
npm run validate:phase2
npm run evaluation:validate
npm run evaluation:run
npm run evaluation:metrics
npm run evaluation:summary
```

Only include commands that actually exist in the final repository.

---

## 19.2 Randomness and AI Configuration

Where supported, record:

* Temperature.
* Top-p.
* Seed.
* Maximum output tokens.
* System prompt.
* User prompt.
* Tool availability.

Model output may remain non-deterministic even with controlled settings. This must be acknowledged.

---

# 20. Statistical Analysis Plan

## 20.1 Descriptive Statistics

For each approach, report:

* Mean.
* Median.
* Minimum.
* Maximum.
* Standard deviation.
* Count.

Apply these to:

* Precision.
* Recall.
* F1.
* Completion time.
* Review time.
* Correction count.
* Reviewer ratings.
* Score variance.

---

## 20.2 Comparative Analysis

Compare:

* Manual versus AI-only.
* Manual versus hybrid.
* AI-only versus hybrid.

Primary comparisons:

* Accuracy.
* Structural validity.
* Traceability.
* Consistency.
* Completion time.
* Correction effort.

---

## 20.3 Significance Testing

Formal significance testing should be used only when the sample size is sufficient.

Potential tests:

* Paired t-test for approximately normal paired measurements.
* Wilcoxon signed-rank test for non-normal paired measurements.
* McNemar’s test for paired categorical outcomes.

For a small prototype evaluation, emphasize descriptive results and effect sizes rather than unsupported significance claims.

---

## 20.4 Effect Size

Where comparisons are valid, report effect size.

Potential measures:

* Cohen’s d.
* Rank-biserial correlation.
* Absolute percentage-point improvement.
* Relative time reduction.

---

# 21. Result Interpretation Rules

## 21.1 Accuracy Claims

Do not state that the system is accurate based only on one successful example.

Report:

* Number of cases.
* Total evaluated items.
* False positives.
* False negatives.
* Variation across cases.

---

## 21.2 Efficiency Claims

Separate:

* Machine execution time.
* Human input-preparation time.
* Human review time.
* Human correction time.

A fast machine result is not necessarily an efficient workflow if it requires extensive correction.

---

## 21.3 Traceability Claims

A generated evidence identifier does not automatically prove traceability.

The reference must:

* Resolve successfully.
* Point to the correct artifact.
* Support the associated claim.

---

## 21.4 Recommendation Claims

Recommendations should be classified as:

* Correct and actionable.
* Correct but generic.
* Partially correct.
* Incorrect.
* Unsupported.
* Not applicable.

---

## 21.5 Failed Runs

Failed runs must remain in the evaluation record.

Examples:

* Schema parse failure.
* Missing required output.
* Runtime exception.
* Timeout.
* Empty result.
* Invalid evidence reference.

Do not silently discard failed runs.

---

# 22. Ethical and Confidentiality Considerations

The evaluation must not expose:

* Personally identifiable information.
* Customer information.
* Authentication credentials.
* API secrets.
* Proprietary source code without permission.
* Confidential architecture details.
* Internal URLs.
* Production data.

Synthetic or anonymized cases should be used for publication.

AI services must not receive confidential code or documentation unless organizational policy explicitly permits it.

---

# 23. Threats to Methodology

## 23.1 Development Bias

The system creator may understand the test cases better than independent users.

Mitigation:

* Use unseen evaluation cases.
* Use independent reviewers where possible.
* Freeze ground truth before execution.

## 23.2 Dataset Bias

The dataset may favor the architecture implemented by the system.

Mitigation:

* Include diverse architectures.
* Include a well-designed source system.
* Include unsupported or ambiguous cases.

## 23.3 Prompt Bias

The prompt may be optimized for known cases.

Mitigation:

* Freeze the prompt.
* Use unseen cases.
* Report prompt changes.

## 23.4 Reviewer Bias

Reviewers may prefer the proposed approach.

Mitigation:

* Anonymize outputs where practical.
* Use a fixed rubric.
* Review approaches in randomized order.

## 23.5 Timing Bias

Manual analysis time may vary with experience.

Mitigation:

* Record reviewer experience.
* Use the same output template.
* Separate reading, execution and correction time.

---

# 24. Success Criteria

The initial target criteria are:

| Evaluation Area                       |                      Target |
| ------------------------------------- | --------------------------: |
| Requirement extraction precision      |                At least 85% |
| Requirement extraction recall         |                At least 85% |
| Structural validity for hybrid output |                At least 95% |
| Injected-defect detection             |                At least 90% |
| Endpoint evidence accuracy            |                At least 90% |
| Gap-detection precision               |                At least 85% |
| Gap-detection recall                  |                At least 80% |
| Risk-detection precision              |                At least 80% |
| Traceability coverage                 | 100% for published findings |
| Key-module test coverage              |                At least 80% |
| Workflow completion rate              |                At least 95% |
| Manual time reduction                 |                At least 50% |

These are target thresholds and must not be reported as achieved until supported by completed experiments.

A result below a target does not invalidate the research. It should be analyzed and reported honestly.

---

# 25. Required Evaluation Outputs

At the end of the evaluation, prepare:

1. Dataset summary.
2. Ground-truth summary.
3. Per-run metadata.
4. Requirement extraction metrics.
5. Validation metrics.
6. Evidence metrics.
7. Gap metrics.
8. Risk metrics.
9. Traceability metrics.
10. Repeated-run consistency metrics.
11. Timing comparison.
12. Human-review results.
13. Failure analysis.
14. Threats-to-validity analysis.
15. Research-question answers.

---

# 26. Recommended Output Tables

## Table A – Dataset Summary

| Case | Type             | Endpoints | Expected Rules | Expected Gaps | Complexity |
| ---- | ---------------- | --------: | -------------: | ------------: | ---------- |
| C1   | Simple CRUD      |       TBD |            TBD |           TBD | Low        |
| C2   | Validation-heavy |       TBD |            TBD |           TBD | Medium     |
| C3   | Transactional    |       TBD |            TBD |           TBD | High       |

## Table B – Requirement Extraction

| Case | Approach | Precision | Recall |  F1 | Structurally Valid |
| ---- | -------- | --------: | -----: | --: | -----------------: |
| C1   | AI-only  |       TBD |    TBD | TBD |                TBD |
| C1   | Hybrid   |       TBD |    TBD | TBD |                TBD |

## Table C – Migration Analysis

| Case | Approach | Gap Precision | Gap Recall | Risk Precision | Risk Recall |
| ---- | -------- | ------------: | ---------: | -------------: | ----------: |
| C5   | AI-only  |           TBD |        TBD |            TBD |         TBD |
| C5   | Hybrid   |           TBD |        TBD |            TBD |         TBD |

## Table D – Traceability

| Case | Findings | Valid Evidence Links | Invalid Links | Coverage |
| ---- | -------: | -------------------: | ------------: | -------: |
| C5   |      TBD |                  TBD |           TBD |      TBD |

## Table E – Efficiency

| Case | Manual Time | AI-Only Time | Hybrid Time | Hybrid Review Time |
| ---- | ----------: | -----------: | ----------: | -----------------: |
| C1   |         TBD |          TBD |         TBD |                TBD |

## Table F – Human Review

| Approach | Correctness | Completeness | Explainability | Actionability | Confidence |
| -------- | ----------: | -----------: | -------------: | ------------: | ---------: |
| Manual   |         TBD |          TBD |            TBD |           TBD |        TBD |
| AI-only  |         TBD |          TBD |            TBD |           TBD |        TBD |
| Hybrid   |         TBD |          TBD |            TBD |           TBD |        TBD |

---

# 27. Phase 3 Deliverables

Phase 3 produces the following finalized plans:

* Research methodology.
* Dataset design.
* Ground-truth process.
* Baseline definitions.
* Experimental procedure.
* Repetition strategy.
* Quantitative metrics.
* Human-review rubric.
* Architecture-scoring evaluation.
* Reproducibility requirements.
* Statistical-analysis plan.
* Result-interpretation rules.
* Success criteria.

---

# 28. Recommended File Location

```text
docs/day49/research_methodology.md
```

---

# 29. Phase 3 Completion Checklist

* [x] Selected the research methodology.
* [x] Defined the design-science stages.
* [x] Defined evaluation objectives.
* [x] Mapped research questions to measurements.
* [x] Defined manual, AI-only and hybrid approaches.
* [x] Designed the evaluation cases.
* [x] Defined dataset-independence requirements.
* [x] Defined the ground-truth process.
* [x] Defined experiment variables.
* [x] Defined the execution procedures.
* [x] Defined the repetition strategy.
* [x] Defined quantitative metrics.
* [x] Defined qualitative metrics.
* [x] Defined the reviewer rubric.
* [x] Defined scoring evaluation.
* [x] Defined validation defect-injection cases.
* [x] Defined result-recording models.
* [x] Defined the evaluation folder structure.
* [x] Defined reproducibility requirements.
* [x] Defined the statistical-analysis plan.
* [x] Defined interpretation and claim-control rules.
* [x] Defined ethical and confidentiality requirements.
* [x] Defined initial success criteria.

---

# 30. Phase 3 Completion Status

**Status:** Completed

**Recommended file:**

```text
docs/day49/research_methodology.md
```

**Next Phase:**

Day 49 Phase 4 will define the evaluation dataset cases, ground-truth templates, experiment manifests, reviewer worksheets, and results-collection files required to execute this methodology.
