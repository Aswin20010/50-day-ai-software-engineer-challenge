# Day 49 – Phase 5

# Research Paper Figures, Tables, Charts, and Publication Assets Plan

## Project

**AI-Assisted API Factory and Migration Analyzer**

## Research Paper Working Title

**AI-Assisted API Modernization: A Rule-Based Architecture for Requirement Extraction, API Generation, and Migration Analysis**

---

# 1. Phase 5 Objective

The objective of Day 49 Phase 5 is to define all visual and supporting publication assets required for the research paper.

This phase establishes:

* Architecture diagrams.
* Workflow figures.
* Domain-model diagrams.
* Evaluation-design diagrams.
* Result tables.
* Result charts.
* Browser screenshots.
* Code and evidence excerpts.
* Figure captions.
* Table captions.
* Asset naming conventions.
* Export and quality requirements.
* Publication-asset validation checklists.

The assets created from this plan should help readers understand the proposed system without requiring them to inspect the complete source code.

No placeholder values in this document should be reported as achieved experimental results.

---

# 2. Phase 5 Deliverables

The recommended publication asset structure is:

```text
docs/
└── day49/
    ├── research_question_and_contribution.md
    ├── research_paper_structure_and_outline.md
    ├── research_methodology.md
    ├── evaluation_dataset_and_templates.md
    ├── publication_assets_plan.md
    └── assets/
        ├── figures/
        ├── tables/
        ├── charts/
        ├── screenshots/
        ├── diagrams/
        └── source/
```

The primary Phase 5 document should be stored at:

```text
docs/day49/publication_assets_plan.md
```

---

# 3. Publication Asset Principles

All visual assets should follow these principles:

1. Every figure must support a specific research claim or explanation.
2. Every table must present information more clearly than prose.
3. Every chart must use actual evaluation data.
4. Screenshots must show relevant system behavior rather than decorative UI.
5. Sensitive or proprietary information must be removed.
6. Figures must remain readable when placed in a two-column paper.
7. Labels and terminology must remain consistent across all assets.
8. Results should not be visually exaggerated.
9. Captions must explain what readers should understand from the asset.
10. Each asset must be referenced from the paper text.

---

# 4. Required Figure Inventory

The paper should contain approximately six to eight primary figures.

| Figure   | Title                                            | Primary Section                 |
| -------- | ------------------------------------------------ | ------------------------------- |
| Figure 1 | End-to-End Hybrid API Modernization Architecture | Proposed Architecture           |
| Figure 2 | API Factory Requirement Processing Workflow      | API Factory Design              |
| Figure 3 | API Migration Analyzer Workflow                  | Migration Analyzer Design       |
| Figure 4 | Shared Research Domain Model                     | System Design                   |
| Figure 5 | Explainable Architecture Scoring Process         | Scoring Model                   |
| Figure 6 | Experimental Evaluation Design                   | Research Methodology            |
| Figure 7 | Human-in-the-Loop Review Workflow                | Governance or Discussion        |
| Figure 8 | Browser Analysis Results Interface               | Implementation or Demonstration |

When page limits are strict, Figures 7 and 8 may be moved to an appendix.

---

# 5. Figure 1 – End-to-End Hybrid API Modernization Architecture

## 5.1 Purpose

Figure 1 should communicate the complete research contribution in one diagram.

It should show how the two projects operate independently while sharing common structures and governance controls.

---

## 5.2 Required Components

The diagram should include:

### Requirement Input Path

* Unstructured requirements.
* AI-assisted extraction.
* Structured requirement model.
* Deterministic normalization.
* Schema and rule validation.
* Validated API specification.
* Generated API artifacts.

### Legacy Source Analysis Path

* Existing API source artifacts.
* Artifact normalization.
* Endpoint evidence extraction.
* Capability evidence extraction.
* Target architecture selection.
* Architecture rule evaluation.
* Gap and risk generation.
* Recommendation generation.
* Migration task generation.

### Shared Components

* Structured domain models.
* Deterministic validation.
* Evidence references.
* Explainable scoring.
* Human review.

---

## 5.3 Suggested Diagram

```text
                  ┌──────────────────────────┐
                  │ Unstructured API         │
                  │ Requirements             │
                  └────────────┬─────────────┘
                               │
                               v
                  ┌──────────────────────────┐
                  │ AI-Assisted Requirement  │
                  │ Extraction               │
                  └────────────┬─────────────┘
                               │
                               v
                  ┌──────────────────────────┐
                  │ Normalization, Schema,   │
                  │ and Rule Validation      │
                  └────────────┬─────────────┘
                               │
                               v
                  ┌──────────────────────────┐
                  │ Validated API            │
                  │ Specification            │
                  └────────────┬─────────────┘
                               │
                               v
                  ┌──────────────────────────┐
                  │ API Artifact Generation  │
                  └──────────────────────────┘


┌──────────────────────────┐
│ Existing API Source      │
│ Artifacts                │
└────────────┬─────────────┘
             │
             v
┌──────────────────────────┐
│ Endpoint and Capability  │
│ Evidence Extraction      │
└────────────┬─────────────┘
             │
             v
┌──────────────────────────┐       ┌──────────────────────────┐
│ Target Architecture      │──────>│ Rule-Based Architecture  │
│ Definition               │       │ Comparison               │
└──────────────────────────┘       └────────────┬─────────────┘
                                               │
                                               v
                                  ┌──────────────────────────┐
                                  │ Gaps, Risks, and         │
                                  │ Readiness Scores         │
                                  └────────────┬─────────────┘
                                               │
                                               v
                                  ┌──────────────────────────┐
                                  │ Recommendations and      │
                                  │ Migration Tasks          │
                                  └────────────┬─────────────┘
                                               │
                                               v
                                  ┌──────────────────────────┐
                                  │ Human Review and         │
                                  │ Approval                 │
                                  └──────────────────────────┘
```

The final publication diagram should visually connect shared models, traceability, and human review across both paths.

---

## 5.4 Planned Caption

> **Figure 1.** End-to-end hybrid architecture connecting AI-assisted requirement extraction, deterministic validation, API artifact generation, source-code evidence extraction, target-architecture comparison, and human-reviewed migration planning.

---

## 5.5 Recommended Asset Names

```text
figure-01-end-to-end-architecture.svg
figure-01-end-to-end-architecture.png
```

---

# 6. Figure 2 – API Factory Requirement Processing Workflow

## 6.1 Purpose

Figure 2 should explain the internal processing sequence of the API Factory.

---

## 6.2 Workflow Stages

The figure should show:

1. Requirement text or file input.
2. Input validation.
3. AI or deterministic extraction.
4. Raw extraction result.
5. Normalization.
6. Missing-field detection.
7. Rule construction.
8. Structured requirement model.
9. User review.
10. API artifact output.

---

## 6.3 Suggested Diagram

```text
Requirement Input
       |
       v
Input Validation
       |
       v
Requirement Extraction
       |
       v
Raw Extracted Data
       |
       v
Normalization
       |
       +--------------------+
       |                    |
       v                    v
Missing-Field Detection   Rule Construction
       |                    |
       +----------+---------+
                  |
                  v
       Structured Requirement Model
                  |
                  v
          Browser-Based Review
                  |
                  v
         API Specification Output
```

---

## 6.4 Important Callouts

Add visual annotations for:

* Raw AI output is not treated as final.
* Missing information is preserved.
* Normalization is deterministic.
* Review happens before implementation use.

---

## 6.5 Planned Caption

> **Figure 2.** API Factory workflow showing how unstructured inputs are extracted, normalized, validated, converted into structured requirements, and presented for human review.

---

## 6.6 Recommended Asset Names

```text
figure-02-api-factory-workflow.svg
figure-02-api-factory-workflow.png
```

---

# 7. Figure 3 – API Migration Analyzer Workflow

## 7.1 Purpose

Figure 3 should explain the complete source-analysis pipeline.

---

## 7.2 Required Stages

Include:

1. Sample or uploaded source project.
2. Source artifact collection.
3. Artifact normalization.
4. Endpoint evidence extraction.
5. Capability evidence extraction.
6. Evidence deduplication.
7. Target architecture selection.
8. Architecture rule evaluation.
9. Gap detection.
10. Risk detection.
11. Recommendation generation.
12. Migration-task generation.
13. Summary score generation.
14. Evidence-backed browser presentation.

---

## 7.3 Suggested Diagram

```text
Source Project
      |
      v
Artifact Collection
      |
      v
Artifact Normalization
      |
      +-----------------------------+
      |                             |
      v                             v
Endpoint Evidence             Capability Evidence
      |                             |
      +--------------+--------------+
                     |
                     v
          Evidence Deduplication
                     |
                     v
         Target Architecture Rules
                     |
                     v
             Rule Evaluation
                     |
          +----------+-----------+
          |                      |
          v                      v
     Migration Gaps        Technical Risks
          |                      |
          +----------+-----------+
                     |
                     v
         Recommendations and Tasks
                     |
                     v
        Scores, Findings, and Evidence
```

---

## 7.4 Planned Caption

> **Figure 3.** API Migration Analyzer workflow from source artifact ingestion through evidence extraction, architecture-rule evaluation, gap and risk detection, and migration-task generation.

---

## 7.5 Recommended Asset Names

```text
figure-03-migration-analyzer-workflow.svg
figure-03-migration-analyzer-workflow.png
```

---

# 8. Figure 4 – Shared Research Domain Model

## 8.1 Purpose

Figure 4 should show how the main structured entities are related.

---

## 8.2 Required Entities

Include:

* Requirement document.
* Endpoint.
* Field.
* Validation rule.
* Business rule.
* Source artifact.
* Evidence.
* Target architecture.
* Architecture rule.
* Rule result.
* Migration gap.
* Technical risk.
* Recommendation.
* Migration task.
* Summary score.

---

## 8.3 Suggested Relationships

```text
Requirement Document
        |
        | contains
        v
     Endpoint
      /     \
     v       v
  Field     Rule


Source Artifact
        |
        | produces
        v
     Evidence
        |
        | evaluated against
        v
Architecture Rule
        |
        | produces
        v
    Rule Result
      /      \
     v        v
Migration Gap  Technical Risk
      \        /
       v      v
     Recommendation
           |
           v
     Migration Task
           |
           v
      Summary Score
```

---

## 8.4 Relationship Requirements

The final diagram should make clear that:

* Evidence supports rule results.
* Gaps are produced from unmet architecture expectations.
* Risks may be linked to one or more gaps.
* Recommendations address gaps and risks.
* Tasks operationalize recommendations.
* Scores are derived from rule outcomes rather than directly from AI text.

---

## 8.5 Planned Caption

> **Figure 4.** Shared domain model linking requirements, source evidence, architecture rules, findings, recommendations, migration tasks, and explainable readiness scores.

---

## 8.6 Recommended Asset Names

```text
figure-04-shared-domain-model.svg
figure-04-shared-domain-model.png
```

---

# 9. Figure 5 – Explainable Architecture Scoring Process

## 9.1 Purpose

Figure 5 should demonstrate that the readiness score is calculated through explicit rules and evidence.

---

## 9.2 Required Process

```text
Source Evidence
      |
      v
Architecture Rule
      |
      v
Rule Outcome
Passed / Partial / Failed / N/A / Insufficient Evidence
      |
      v
Rule Weight and Earned Credit
      |
      v
Category Score
      |
      v
Overall Readiness Score
      |
      v
Linked Gaps, Risks, and Recommendations
```

---

## 9.3 Supporting Formula

The paper may display the category score as:

```text
Category score =
sum of earned weighted credit /
sum of applicable rule weights × 100
```

The overall score may be displayed as:

```text
Overall readiness score =
sum of category score × category weight /
sum of category weights
```

---

## 9.4 Visual Requirements

The diagram should show that readers can trace an overall score backward to:

* Category scores.
* Rule outcomes.
* Rule weights.
* Supporting evidence.

---

## 9.5 Planned Caption

> **Figure 5.** Explainable scoring process in which source evidence determines architecture-rule outcomes, weighted category scores, overall readiness, and linked remediation findings.

---

## 9.6 Recommended Asset Names

```text
figure-05-explainable-scoring.svg
figure-05-explainable-scoring.png
```

---

# 10. Figure 6 – Experimental Evaluation Design

## 10.1 Purpose

Figure 6 should summarize the evaluation methodology.

---

## 10.2 Required Evaluation Paths

Include:

### Shared Inputs

* Requirement cases.
* Source-project cases.
* Frozen ground truth.

### Compared Approaches

* Manual baseline.
* AI-only baseline.
* Hybrid approach.

### Evaluation Outputs

* Accuracy.
* Structural validity.
* Traceability.
* Consistency.
* Completion time.
* Correction effort.
* Human-review scores.

---

## 10.3 Suggested Diagram

```text
Evaluation Dataset
       |
       +------------------+------------------+
       |                  |                  |
       v                  v                  v
Manual Analysis      AI-Only Analysis   Hybrid Analysis
       |                  |                  |
       +------------------+------------------+
                          |
                          v
                   Generated Outputs
                          |
                          v
                   Frozen Ground Truth
                          |
                          v
                  Automated Comparison
                          |
                          v
                    Human Review
                          |
                          v
       Accuracy, Traceability, Time, Consistency,
            Corrections, and Confidence Metrics
```

---

## 10.4 Planned Caption

> **Figure 6.** Experimental design comparing manual, AI-only, and hybrid analysis using shared inputs, frozen ground truth, automated metrics, and human review.

---

## 10.5 Recommended Asset Names

```text
figure-06-experimental-design.svg
figure-06-experimental-design.png
```

---

# 11. Figure 7 – Human-in-the-Loop Review Workflow

## 11.1 Purpose

Figure 7 should explain where human decisions occur and why the proposed system does not replace engineering judgment.

---

## 11.2 Review Checkpoints

Include:

* Review extracted requirements.
* Resolve missing information.
* Resolve ambiguities.
* Verify source evidence.
* Accept or reject findings.
* Adjust severity or priority.
* Approve recommendations.
* Approve migration tasks.

---

## 11.3 Suggested Diagram

```text
AI or Rule Output
       |
       v
Structured Review Interface
       |
       +--------------------+
       |                    |
       v                    v
Accept Supported Item   Reject or Correct Item
       |                    |
       +---------+----------+
                 |
                 v
          Approved Artifact
                 |
                 v
   API Implementation or Migration Planning
```

---

## 11.4 Planned Caption

> **Figure 7.** Human-in-the-loop governance workflow for correcting extracted requirements, validating evidence, reviewing findings, and approving implementation artifacts.

---

## 11.5 Recommended Asset Names

```text
figure-07-human-review-workflow.svg
figure-07-human-review-workflow.png
```

---

# 12. Figure 8 – Browser Analysis Results Interface

## 12.1 Purpose

Figure 8 should demonstrate that the architecture was implemented as a usable system.

---

## 12.2 Screenshot Content

The selected screenshot should display:

* Selected sample project.
* Selected target architecture.
* Overall readiness score.
* Summary scorecards.
* Gap count.
* Risk count.
* Recommendation count.
* Migration tasks.
* Evidence viewer.

Use a screenshot with meaningful populated results rather than an empty state.

---

## 12.3 Screenshot Requirements

Before capture:

* Use anonymized sample data.
* Hide local usernames and private paths.
* Hide API keys and environment values.
* Use a standard browser width.
* Ensure text is legible.
* Avoid browser extensions or unrelated tabs.
* Use consistent zoom.
* Remove debug overlays.
* Capture only the relevant application window.

---

## 12.4 Planned Caption

> **Figure 8.** Browser-based migration-analysis interface presenting readiness scorecards, architecture findings, risks, recommendations, migration tasks, and source evidence.

---

## 12.5 Recommended Asset Names

```text
figure-08-browser-results-interface.png
figure-08-browser-results-interface-source.png
```

---

# 13. Optional Figure 9 – Evidence-to-Recommendation Traceability

## 13.1 Purpose

This figure may be included when traceability is a central paper claim.

---

## 13.2 Suggested Chain

```text
Source Artifact
      |
      v
Extracted Evidence
      |
      v
Failed Architecture Rule
      |
      v
Migration Gap
      |
      v
Technical Risk
      |
      v
Recommendation
      |
      v
Migration Task
```

---

## 13.3 Planned Caption

> **Figure 9.** Traceability chain connecting a source artifact to extracted evidence, architecture-rule failure, migration finding, recommendation, and implementation task.

This figure may be moved to an appendix if page space is limited.

---

# 14. Required Table Inventory

The paper should prepare approximately 10–15 tables, although only the most important should remain in the main paper.

| Table    | Title                                     |
| -------- | ----------------------------------------- |
| Table 1  | Research Questions and Evaluation Metrics |
| Table 2  | Related-Work Capability Comparison        |
| Table 3  | Proposed Architecture Components          |
| Table 4  | Requirement Model Summary                 |
| Table 5  | Target Architecture Rule Categories       |
| Table 6  | Evaluation Dataset Summary                |
| Table 7  | Requirement Extraction Results            |
| Table 8  | Validation Results                        |
| Table 9  | Migration Gap and Risk Results            |
| Table 10 | Traceability Results                      |
| Table 11 | Repeated-Run Consistency                  |
| Table 12 | Workflow Timing Results                   |
| Table 13 | Human Reviewer Results                    |
| Table 14 | Research Question Summary                 |
| Table 15 | Threats to Validity and Mitigations       |

Tables containing extensive case-level details may be moved to appendices.

---

# 15. Table 1 – Research Questions and Evaluation Metrics

## Planned Structure

| Research Question | Evaluated Component        | Dataset Cases                  | Metrics                                                   |
| ----------------- | -------------------------- | ------------------------------ | --------------------------------------------------------- |
| RQ1               | Requirement extraction     | C1, C2, C8                     | Precision, recall, F1                                     |
| RQ2               | Schema and rule validation | C2 and injected-defect dataset | Defect-detection rate, false-positive rate                |
| RQ3               | API Factory outputs        | C1, C2, C3                     | Completeness, structural validity, traceability           |
| RQ4               | Migration Analyzer         | C3–C7                          | Gap and risk precision, recall, F1                        |
| RQ5               | Evidence-backed analysis   | C4–C7                          | Traceability coverage, evidence accuracy, reviewer rating |
| RQ6               | End-to-end workflow        | All selected cases             | Completion time, review time, correction effort           |

### Planned Caption

> **Table 1.** Mapping of research questions to evaluated system components, dataset cases, and primary metrics.

---

# 16. Table 2 – Related-Work Capability Comparison

## Planned Structure

| Approach                     | Requirement Extraction | API Generation | Legacy Analysis | Deterministic Validation | Evidence Traceability | Migration Tasks |
| ---------------------------- | ---------------------: | -------------: | --------------: | -----------------------: | --------------------: | --------------: |
| Requirement-engineering tool |                    Yes |        Partial |              No |                      Yes |               Partial |              No |
| AI code generator            |                Partial |            Yes |         Partial |                       No |                    No |              No |
| Static-analysis tool         |                     No |             No |             Yes |                      Yes |                   Yes |         Partial |
| Modernization platform       |                Partial |        Partial |             Yes |                  Partial |               Partial |             Yes |
| Proposed system              |                    Yes |            Yes |             Yes |                      Yes |                   Yes |             Yes |

The actual named approaches must be populated only after reviewing and citing relevant research.

### Planned Caption

> **Table 2.** Capability comparison between the proposed system and representative requirement-engineering, code-generation, static-analysis, and modernization approaches.

---

# 17. Table 3 – Proposed Architecture Components

## Planned Structure

| Component                | Input                            | Processing                         | Output                     |
| ------------------------ | -------------------------------- | ---------------------------------- | -------------------------- |
| Requirement extractor    | Requirement text                 | Candidate extraction               | Raw requirement model      |
| Normalizer               | Raw extraction                   | Canonicalization and deduplication | Normalized requirements    |
| Schema validator         | Normalized requirements          | Structural checks                  | Errors and validated model |
| Evidence extractor       | Source artifacts                 | Endpoint and capability analysis   | Evidence collection        |
| Rule evaluator           | Evidence and target architecture | Deterministic comparison           | Rule results               |
| Finding generator        | Rule results                     | Gap and risk creation              | Findings                   |
| Recommendation generator | Findings                         | Remediation planning               | Recommendations            |
| Task generator           | Recommendations                  | Implementation breakdown           | Migration tasks            |

### Planned Caption

> **Table 3.** Primary components of the proposed hybrid architecture and their inputs, processing responsibilities, and outputs.

---

# 18. Table 4 – Requirement Model Summary

## Planned Structure

| Entity               | Important Attributes          | Relationship                   |
| -------------------- | ----------------------------- | ------------------------------ |
| Requirement document | Name, source, ambiguities     | Contains endpoints             |
| Endpoint             | Method, path, description     | Contains fields and rules      |
| Field                | Name, type, required          | Belongs to request or response |
| Rule                 | Category, description, target | Applies to endpoint or field   |
| Missing information  | Category, description         | Requires human resolution      |

### Planned Caption

> **Table 4.** Simplified structured model used to represent extracted and normalized API requirements.

---

# 19. Table 5 – Target Architecture Rule Categories

## Planned Structure

| Category       | Example Expectation                        | Example Evidence                     |
| -------------- | ------------------------------------------ | ------------------------------------ |
| Layering       | Handler, service and repository separation | Separate source modules              |
| Validation     | Central request validation                 | Schema or validation middleware      |
| Security       | Authentication and authorization           | Middleware and policy checks         |
| Data access    | Controlled repository transactions         | Transaction wrapper                  |
| Error handling | Standardized error mapping                 | Central error handler                |
| Observability  | Structured logging and tracing             | Logger and trace propagation         |
| Testing        | Unit and integration tests                 | Test artifacts                       |
| Configuration  | Externalized environment configuration     | Configuration module                 |
| Resilience     | Timeout and retry controls                 | External-client policy               |
| Deployment     | Health and build readiness                 | Health endpoint and deployment files |

### Planned Caption

> **Table 5.** Target-architecture rule categories, expected implementation characteristics, and representative source evidence.

---

# 20. Table 6 – Evaluation Dataset Summary

## Planned Structure

| Case | Type                    | Endpoints | Rule Complexity | Expected Gaps | Primary Evaluation            |
| ---- | ----------------------- | --------: | --------------- | ------------: | ----------------------------- |
| C1   | Simple CRUD             |       TBD | Low             |           TBD | Basic extraction              |
| C2   | Validation-heavy        |       TBD | High            |           TBD | Rule extraction               |
| C3   | Transactional           |       TBD | High            |           TBD | Transaction analysis          |
| C4   | External integration    |       TBD | High            |           TBD | Resilience analysis           |
| C5   | Legacy API              |       TBD | Medium          |           TBD | Gap and risk detection        |
| C6   | Partially modernized    |       TBD | Medium          |           TBD | Partial compliance            |
| C7   | Secure layered API      |       TBD | Medium          |           TBD | False-positive control        |
| C8   | Incomplete requirements |       TBD | Ambiguous       |           N/A | Missing-information detection |

### Planned Caption

> **Table 6.** Evaluation cases designed to test extraction, validation, migration analysis, false-positive control, and ambiguity handling.

---

# 21. Table 7 – Requirement Extraction Results

## Planned Structure

| Case | Approach |  TP |  FP |  FN | Precision | Recall |  F1 |
| ---- | -------- | --: | --: | --: | --------: | -----: | --: |
| C1   | AI-only  | TBD | TBD | TBD |       TBD |    TBD | TBD |
| C1   | Hybrid   | TBD | TBD | TBD |       TBD |    TBD | TBD |
| C2   | AI-only  | TBD | TBD | TBD |       TBD |    TBD | TBD |
| C2   | Hybrid   | TBD | TBD | TBD |       TBD |    TBD | TBD |

### Planned Caption

> **Table 7.** Requirement extraction performance for AI-only and hybrid approaches compared with frozen ground truth.

---

# 22. Table 8 – Deterministic Validation Results

## Planned Structure

| Defect Type                 | Injected | Detected | Missed | Detection Rate |
| --------------------------- | -------: | -------: | -----: | -------------: |
| Missing required field      |      TBD |      TBD |    TBD |            TBD |
| Unsupported HTTP method     |      TBD |      TBD |    TBD |            TBD |
| Duplicate identifier        |      TBD |      TBD |    TBD |            TBD |
| Broken evidence reference   |      TBD |      TBD |    TBD |            TBD |
| Invalid recommendation link |      TBD |      TBD |    TBD |            TBD |
| Incomplete migration task   |      TBD |      TBD |    TBD |            TBD |

Include a separate valid-input false-positive result.

### Planned Caption

> **Table 8.** Deterministic validation performance on intentionally injected structural and relationship defects.

---

# 23. Table 9 – Migration Gap and Risk Results

## Planned Structure

| Case | Approach | Gap Precision | Gap Recall | Gap F1 | Risk Precision | Risk Recall | Risk F1 |
| ---- | -------- | ------------: | ---------: | -----: | -------------: | ----------: | ------: |
| C5   | AI-only  |           TBD |        TBD |    TBD |            TBD |         TBD |     TBD |
| C5   | Hybrid   |           TBD |        TBD |    TBD |            TBD |         TBD |     TBD |
| C6   | AI-only  |           TBD |        TBD |    TBD |            TBD |         TBD |     TBD |
| C6   | Hybrid   |           TBD |        TBD |    TBD |            TBD |         TBD |     TBD |
| C7   | AI-only  |           TBD |        TBD |    TBD |            TBD |         TBD |     TBD |
| C7   | Hybrid   |           TBD |        TBD |    TBD |            TBD |         TBD |     TBD |

### Planned Caption

> **Table 9.** Architecture-gap and technical-risk detection results across legacy, partially modernized, and well-architected API cases.

---

# 24. Table 10 – Traceability Results

## Planned Structure

| Case | Approach | Findings | Findings with Evidence | Supporting Links | Invalid Links | Coverage |
| ---- | -------- | -------: | ---------------------: | ---------------: | ------------: | -------: |
| C4   | AI-only  |      TBD |                    TBD |              TBD |           TBD |      TBD |
| C4   | Hybrid   |      TBD |                    TBD |              TBD |           TBD |      TBD |
| C5   | AI-only  |      TBD |                    TBD |              TBD |           TBD |      TBD |
| C5   | Hybrid   |      TBD |                    TBD |              TBD |           TBD |      TBD |

### Planned Caption

> **Table 10.** Traceability coverage and evidence-link accuracy for generated architecture findings.

---

# 25. Table 11 – Repeated-Run Consistency

## Planned Structure

| Case | Approach | Endpoint Overlap | Gap Overlap | Risk Overlap | Recommendation Overlap | Score Range |
| ---- | -------- | ---------------: | ----------: | -----------: | ---------------------: | ----------: |
| C1   | AI-only  |              TBD |         N/A |          N/A |                    TBD |         TBD |
| C1   | Hybrid   |              TBD |         N/A |          N/A |                    TBD |         TBD |
| C5   | AI-only  |              TBD |         TBD |          TBD |                    TBD |         TBD |
| C5   | Hybrid   |              TBD |         TBD |          TBD |                    TBD |         TBD |

### Planned Caption

> **Table 11.** Repeated-run consistency measured through normalized finding overlap and readiness-score variation.

---

# 26. Table 12 – Workflow Timing Results

## Planned Structure

| Case | Approach | Preparation | Execution | Review | Correction | Total |
| ---- | -------- | ----------: | --------: | -----: | ---------: | ----: |
| C1   | Manual   |         TBD |       N/A |    TBD |        TBD |   TBD |
| C1   | AI-only  |         TBD |       TBD |    TBD |        TBD |   TBD |
| C1   | Hybrid   |         TBD |       TBD |    TBD |        TBD |   TBD |

### Planned Caption

> **Table 12.** End-to-end workflow time separated into preparation, system execution, human review, and correction.

---

# 27. Table 13 – Human Reviewer Results

## Planned Structure

| Approach | Correctness | Completeness | Explainability | Evidence Quality | Actionability | Confidence |
| -------- | ----------: | -----------: | -------------: | ---------------: | ------------: | ---------: |
| Manual   |         TBD |          TBD |            TBD |              TBD |           TBD |        TBD |
| AI-only  |         TBD |          TBD |            TBD |              TBD |           TBD |        TBD |
| Hybrid   |         TBD |          TBD |            TBD |              TBD |           TBD |        TBD |

Report mean and, where useful, median and range.

### Planned Caption

> **Table 13.** Human reviewer ratings for correctness, completeness, explainability, evidence quality, actionability, and confidence.

---

# 28. Table 14 – Research Question Summary

## Planned Structure

| Research Question | Main Result | Interpretation | Limitation |
| ----------------- | ----------- | -------------- | ---------- |
| RQ1               | TBD         | TBD            | TBD        |
| RQ2               | TBD         | TBD            | TBD        |
| RQ3               | TBD         | TBD            | TBD        |
| RQ4               | TBD         | TBD            | TBD        |
| RQ5               | TBD         | TBD            | TBD        |
| RQ6               | TBD         | TBD            | TBD        |

### Planned Caption

> **Table 14.** Summary of experimental findings and interpretations for each research question.

---

# 29. Table 15 – Threats to Validity

## Planned Structure

| Threat                          | Type                | Potential Impact                 | Mitigation                            |
| ------------------------------- | ------------------- | -------------------------------- | ------------------------------------- |
| Small dataset                   | External validity   | Limits generalization            | Use diverse cases and state scope     |
| Researcher-created ground truth | Internal validity   | May introduce bias               | Secondary review and freeze           |
| Prompt tuning                   | Internal validity   | May favor known cases            | Frozen prompts and unseen cases       |
| Limited reviewers               | Conclusion validity | Weakens rating reliability       | Report sample size and agreement      |
| Architecture-specific rules     | Construct validity  | Scores may reflect one viewpoint | Publish rules and allow customization |

### Planned Caption

> **Table 15.** Principal threats to validity and mitigation measures used in the evaluation.

---

# 30. Required Chart Inventory

Charts should be generated only after actual evaluation data is available.

Recommended charts:

1. Requirement extraction precision and recall.
2. Gap and risk detection performance.
3. Structural-validity comparison.
4. Traceability coverage.
5. Workflow completion time.
6. Human-review ratings.
7. Repeated-run consistency.
8. Readiness score versus expert score.
9. False-positive findings by case.
10. Validation defect-detection results.

---

# 31. Chart 1 – Requirement Extraction Performance

## Recommended Type

Grouped bar chart.

## X-Axis

Evaluation case.

## Y-Axis

Metric value from 0 to 1 or 0% to 100%.

## Series

* AI-only precision.
* AI-only recall.
* Hybrid precision.
* Hybrid recall.

## Purpose

Show whether deterministic structure and validation improve requirement extraction quality.

## Recommended Asset Name

```text
chart-01-requirement-extraction-performance.svg
```

---

# 32. Chart 2 – Migration Analysis Performance

## Recommended Type

Grouped bar chart or separate gap and risk charts.

## Metrics

* Gap F1.
* Risk F1.

## Series

* AI-only.
* Hybrid.

## Cases

* C5.
* C6.
* C7.

## Purpose

Compare modernization-analysis performance across systems with different readiness levels.

## Recommended Asset Name

```text
chart-02-migration-analysis-performance.svg
```

---

# 33. Chart 3 – Structural Validity

## Recommended Type

Bar chart.

## Categories

* Manual.
* AI-only.
* Hybrid.

## Metric

Percentage of outputs passing the required structure.

## Important Interpretation

Manual outputs should be considered structurally valid only when submitted using the standardized output template.

## Recommended Asset Name

```text
chart-03-structural-validity.svg
```

---

# 34. Chart 4 – Traceability Coverage

## Recommended Type

Grouped bar chart.

## Metric

Percentage of findings with valid supporting evidence.

## Series

* AI-only.
* Hybrid.

## Purpose

Support the paper’s explainability claim.

## Recommended Asset Name

```text
chart-04-traceability-coverage.svg
```

---

# 35. Chart 5 – End-to-End Workflow Time

## Recommended Type

Stacked bar chart.

## Stacks

* Input preparation.
* System execution.
* Human review.
* Human correction.

## Categories

* Manual.
* AI-only.
* Hybrid.

## Purpose

Avoid misleading claims based only on machine execution speed.

## Recommended Asset Name

```text
chart-05-workflow-time.svg
```

---

# 36. Chart 6 – Human Reviewer Ratings

## Recommended Type

Grouped bar chart.

## Dimensions

* Correctness.
* Completeness.
* Explainability.
* Actionability.
* Confidence.

## Series

* Manual.
* AI-only.
* Hybrid.

## Recommended Asset Name

```text
chart-06-human-review-ratings.svg
```

---

# 37. Chart 7 – Repeated-Run Consistency

## Recommended Type

Box plot or bar chart with error bars.

## Metrics

* Finding overlap.
* Score variance.
* Recommendation overlap.

## Purpose

Show whether the hybrid workflow provides more stable downstream output.

## Recommended Asset Name

```text
chart-07-run-consistency.svg
```

---

# 38. Chart 8 – System Score Versus Expert Score

## Recommended Type

Scatter plot.

## X-Axis

Expert-assigned readiness score.

## Y-Axis

System readiness score.

## Supporting Element

A diagonal perfect-agreement reference line.

## Purpose

Evaluate whether the explainable scoring model aligns with expert assessment.

## Recommended Asset Name

```text
chart-08-system-vs-expert-score.svg
```

---

# 39. Chart 9 – False Positives by Case

## Recommended Type

Bar chart.

## X-Axis

Case.

## Y-Axis

Number of unsupported findings.

## Series

* AI-only.
* Hybrid.

## Purpose

Show whether deterministic and evidence-based controls reduce unsupported findings.

## Recommended Asset Name

```text
chart-09-false-positives.svg
```

---

# 40. Chart 10 – Validator Defect Detection

## Recommended Type

Bar chart.

## X-Axis

Injected defect type.

## Y-Axis

Detection rate.

## Purpose

Show the strengths and limitations of deterministic validation.

## Recommended Asset Name

```text
chart-10-validation-defect-detection.svg
```

---

# 41. Chart Construction Rules

All charts should follow these rules:

* Use zero-based axes for bars unless a non-zero baseline is clearly justified.
* Do not use three-dimensional charts.
* Do not truncate axes to exaggerate small differences.
* Show sample size where possible.
* Display error bars when reporting averages across repeated runs.
* Use the same approach order across charts.
* Use consistent terminology.
* Include units in axis labels.
* Avoid excessive gridlines.
* Ensure grayscale readability.
* Use direct labels or a clear legend.
* Do not plot target thresholds as achieved values.
* Preserve the underlying CSV used to generate each chart.

---

# 42. Result Data Sources

Each chart must be generated from a versioned result file.

Recommended mapping:

| Chart    | Source File                                     |
| -------- | ----------------------------------------------- |
| Chart 1  | `results/accuracy/accuracy-results.csv`         |
| Chart 2  | `results/accuracy/accuracy-results.csv`         |
| Chart 3  | `results/accuracy/structural-validity.csv`      |
| Chart 4  | `results/traceability/traceability-results.csv` |
| Chart 5  | `results/timing/timing-records.csv`             |
| Chart 6  | `results/human-review/review-results.csv`       |
| Chart 7  | `results/consistency/consistency-results.csv`   |
| Chart 8  | `results/scoring/scoring-results.csv`           |
| Chart 9  | `results/accuracy/accuracy-results.csv`         |
| Chart 10 | `results/validation/defect-detection.csv`       |

---

# 43. Browser Screenshot Inventory

The research repository should capture the following screenshots.

## Screenshot 1 – API Factory Input State

Show:

* Requirement input.
* File-selection area.
* Extraction configuration.
* Submit action.

Recommended name:

```text
screenshot-01-api-factory-input.png
```

---

## Screenshot 2 – API Factory Extraction Result

Show:

* Extracted flow name.
* Structured endpoint details.
* Validation rules.
* Missing fields.
* Normalized output.

Recommended name:

```text
screenshot-02-api-factory-results.png
```

---

## Screenshot 3 – API Factory Error State

Show a meaningful validation or extraction error.

Recommended name:

```text
screenshot-03-api-factory-error.png
```

---

## Screenshot 4 – Migration Analyzer Request Form

Show:

* Sample project selection.
* Target architecture selection.
* Analysis controls.

Recommended name:

```text
screenshot-04-migration-analyzer-form.png
```

---

## Screenshot 5 – Migration Analyzer Summary

Show:

* Overall score.
* Category scorecards.
* Gap count.
* Risk count.
* Recommendation count.

Recommended name:

```text
screenshot-05-migration-summary.png
```

---

## Screenshot 6 – Gap and Risk Results

Show:

* Severity.
* Description.
* Expected capability.
* Observed behavior.
* Linked evidence.

Recommended name:

```text
screenshot-06-gaps-and-risks.png
```

---

## Screenshot 7 – Migration Recommendations and Tasks

Show:

* Priority.
* Recommendation.
* Task title.
* Acceptance criteria.
* Dependencies.

Recommended name:

```text
screenshot-07-recommendations-and-tasks.png
```

---

## Screenshot 8 – Evidence Viewer

Show:

* Evidence category.
* Artifact path.
* Relevant excerpt.
* Linked rule or finding.

Recommended name:

```text
screenshot-08-evidence-viewer.png
```

---

## Screenshot 9 – Responsive Layout

Show the analysis interface at a narrower viewport.

Recommended name:

```text
screenshot-09-responsive-layout.png
```

This screenshot is better suited to project documentation or an appendix than the main research paper.

---

# 44. Screenshot Capture Checklist

Before capturing each screenshot:

* [ ] Use only anonymized data.
* [ ] Remove confidential project names.
* [ ] Hide local user paths.
* [ ] Hide browser bookmarks if sensitive.
* [ ] Hide environment variables.
* [ ] Hide API keys and credentials.
* [ ] Use meaningful populated data.
* [ ] Remove debug console overlays.
* [ ] Confirm the interface is not clipped.
* [ ] Confirm all relevant labels are readable.
* [ ] Use consistent browser zoom.
* [ ] Record the viewport dimensions.
* [ ] Record the application commit hash.

---

# 45. Code Excerpt Plan

The paper may include a limited number of short code excerpts.

Recommended excerpts:

1. Structured requirement model.
2. Architecture rule definition.
3. Evidence object.
4. Gap-generation rule.
5. Category score calculation.
6. Migration-task structure.

Code excerpts should:

* Be short.
* Support a technical explanation.
* Exclude unrelated implementation details.
* Avoid confidential identifiers.
* Include syntax highlighting in the final paper format.
* Not replace architecture explanations.

Long implementations should remain in the repository or appendix.

---

# 46. Example Requirement Model Excerpt

```ts
export interface ExtractedEndpoint {
  readonly id: string;
  readonly name: string;
  readonly method: HttpMethod;
  readonly path: string;
  readonly requestFields: readonly ExtractedField[];
  readonly responseFields: readonly ExtractedField[];
  readonly rules: readonly ExtractedRule[];
}
```

Purpose:

* Demonstrate that AI output is converted into a strongly typed model.

---

# 47. Example Evidence Model Excerpt

```ts
export interface MigrationEvidence {
  readonly id: string;
  readonly category: EvidenceCategory;
  readonly artifactId: string;
  readonly sourceLocation: string;
  readonly description: string;
  readonly confidence: EvidenceConfidence;
}
```

Purpose:

* Demonstrate how findings retain links to source artifacts.

---

# 48. Example Architecture Rule Excerpt

```ts
export interface ArchitectureRule {
  readonly id: string;
  readonly category: ArchitectureCategory;
  readonly title: string;
  readonly description: string;
  readonly severity: RuleSeverity;
  readonly weight: number;
  readonly evidenceRequirements: readonly string[];
}
```

Purpose:

* Demonstrate that architecture expectations and scoring weights are explicit.

---

# 49. Example Migration Task Excerpt

```ts
export interface MigrationTask {
  readonly id: string;
  readonly title: string;
  readonly description: string;
  readonly priority: MigrationPriority;
  readonly affectedArtifacts: readonly string[];
  readonly linkedFindingIds: readonly string[];
  readonly dependencies: readonly string[];
  readonly acceptanceCriteria: readonly string[];
}
```

Purpose:

* Demonstrate how recommendations are translated into actionable engineering work.

---

# 50. Asset Source Files

Preserve editable source files for every publication asset.

Recommended structure:

```text
docs/day49/assets/source/
├── diagrams/
│   ├── figure-01-end-to-end-architecture.drawio
│   ├── figure-02-api-factory-workflow.drawio
│   ├── figure-03-migration-analyzer-workflow.drawio
│   └── figure-04-shared-domain-model.drawio
├── chart-scripts/
│   ├── chart-01-requirement-extraction.mjs
│   ├── chart-02-migration-analysis.mjs
│   └── chart-05-workflow-time.mjs
└── data/
    ├── accuracy-results.csv
    ├── timing-records.csv
    └── review-results.csv
```

Source files must remain version controlled where licensing and repository size permit.

---

# 51. Asset Naming Convention

Use:

```text
<asset-type>-<two-digit-number>-<short-description>.<extension>
```

Examples:

```text
figure-01-end-to-end-architecture.svg
table-07-requirement-extraction-results.csv
chart-05-workflow-time.svg
screenshot-08-evidence-viewer.png
```

Avoid names such as:

```text
final.png
final-new.png
final-actual.png
diagram2-latest.png
```

---

# 52. Figure and Table Caption Rules

Every caption should:

* Be understandable without reading the full section.
* State what the asset shows.
* Define unusual abbreviations.
* Avoid claims not visible in the asset.
* Indicate whether values are averages.
* Indicate the number of runs when relevant.
* State whether error bars show standard deviation, confidence interval, or range.

Example:

> **Figure 10.** Mean requirement extraction F1 score across three repeated runs per case. Error bars represent one standard deviation.

---

# 53. Accessibility Requirements

Publication assets should remain accessible.

Requirements:

* Do not rely only on color.
* Use patterns, labels, or marker shapes where needed.
* Maintain sufficient contrast.
* Use legible font sizes.
* Provide descriptive captions.
* Avoid dense text inside diagrams.
* Define abbreviations.
* Use readable line thickness.
* Test figures in grayscale.
* Include alternative text where the publication platform supports it.

---

# 54. Image and Export Requirements

Preferred formats:

* SVG or PDF for diagrams and charts.
* PNG for browser screenshots.
* CSV for underlying chart data.
* Editable source format for diagrams.

Recommended raster export:

* Minimum 300 DPI for print.
* Avoid unnecessary compression.
* Preserve original screenshot resolution.
* Do not enlarge low-resolution images.

Recommended width:

* Single-column figure: approximately 3.3–3.5 inches.
* Double-column figure: approximately 7–7.2 inches.

Exact requirements depend on the selected venue.

---

# 55. Typography Rules

Use consistent typography across all assets.

Requirements:

* Use one font family for diagrams and charts.
* Use the same capitalization style.
* Use a minimum readable size after paper scaling.
* Avoid excessive bold text.
* Use monospace only for paths, identifiers, and code.
* Keep architecture component labels concise.
* Do not place complete paragraphs inside figures.

---

# 56. Color and Visual Semantics

Use consistent semantic meaning.

Suggested categories:

* Input or source information.
* AI-assisted processing.
* Deterministic processing.
* Human review.
* Valid or passed result.
* Partial result.
* Failed result.
* Risk or warning.
* Output artifact.

The exact colors should be selected later, but the same semantic category must use the same visual treatment throughout.

A grayscale-safe alternative must remain understandable.

---

# 57. Publication Asset Mapping

Each asset must map to a paper section.

| Asset           | Paper Section                |
| --------------- | ---------------------------- |
| Figure 1        | Proposed Architecture        |
| Figure 2        | API Factory Design           |
| Figure 3        | Migration Analyzer Design    |
| Figure 4        | Domain Model                 |
| Figure 5        | Explainable Scoring          |
| Figure 6        | Research Methodology         |
| Figure 7        | Human-in-the-Loop Governance |
| Figure 8        | Implementation Demonstration |
| Tables 7–13     | Results                      |
| Charts 1–10     | Results and Discussion       |
| Screenshots 1–9 | Implementation or Appendix   |

---

# 58. Main Paper Versus Appendix

## Recommended Main Paper Assets

Include:

* Figure 1 – End-to-End Architecture.
* Figure 2 – API Factory Workflow.
* Figure 3 – Migration Analyzer Workflow.
* Figure 5 – Explainable Scoring.
* Figure 6 – Experimental Design.
* Table 1 – Research Questions and Metrics.
* Table 6 – Dataset Summary.
* Table 7 – Extraction Results.
* Table 9 – Migration Results.
* Table 10 – Traceability Results.
* Table 12 – Timing Results.
* One or two result charts.

## Recommended Appendix Assets

Move to appendix when necessary:

* Full domain model.
* Human-review workflow.
* Additional browser screenshots.
* Per-case result tables.
* Full validation results.
* Repeated-run pairwise comparisons.
* Reviewer questionnaire.
* Complete architecture-rule inventory.

---

# 59. Result Visualization Selection Rules

A metric should not appear in both a detailed table and a chart unless each serves a different purpose.

Use:

* Tables for exact values.
* Charts for comparisons and patterns.
* Figures for architecture and processes.
* Screenshots for implementation evidence.
* Code excerpts for data structures or algorithms.

Example:

* Table 7 gives exact precision, recall, and F1.
* Chart 1 highlights the difference between AI-only and hybrid approaches.

---

# 60. Evidence Screenshot Annotation

When annotating screenshots:

* Use numbered callouts.
* Keep labels outside important content.
* Include a legend beneath the image.
* Do not cover values or findings.
* Preserve an unannotated source image.
* Avoid excessive arrows.

Example callouts:

1. Overall readiness score.
2. Failed architecture rules.
3. High-severity migration risks.
4. Evidence reference.
5. Generated migration task.

---

# 61. Reproducible Chart Generation

Every chart should have:

* A source data file.
* A chart-generation script.
* A generated vector asset.
* A generation command.
* A brief README entry.

Example command:

```bash
npm run research:charts
```

Only add this command after the implementation exists.

The chart-generation process should not depend on manual spreadsheet editing for final values.

---

# 62. Publication Asset README

Recommended file:

```text
docs/day49/assets/README.md
```

Suggested content:

```md
# Research Publication Assets

## Figures

List each figure, source file, exported file and paper section.

## Charts

List each chart, source dataset, script and generation command.

## Screenshots

List each screenshot, application version, viewport and dataset case.

## Tables

List each table and its generated or manually verified source.

## Validation

Record final checks for readability, anonymization and citation.
```

---

# 63. Asset Metadata Template

Each major asset may have a metadata file.

Example:

```json
{
  "assetId": "FIGURE-01",
  "title": "End-to-End Hybrid API Modernization Architecture",
  "sourceFile": "source/diagrams/figure-01-end-to-end-architecture.drawio",
  "exportFiles": [
    "figures/figure-01-end-to-end-architecture.svg",
    "figures/figure-01-end-to-end-architecture.png"
  ],
  "paperSection": "Proposed Architecture",
  "datasetVersion": null,
  "generatedAt": "TBD",
  "reviewStatus": "draft",
  "containsSensitiveData": false
}
```

---

# 64. Asset Review Workflow

Every asset should pass through:

1. Technical review.
2. Research-claim review.
3. Readability review.
4. Confidentiality review.
5. Caption review.
6. Venue-format review.
7. Final export review.

Review status values:

* Draft.
* Technical review complete.
* Research review complete.
* Publication ready.
* Archived.

---

# 65. Figure Review Checklist

For each figure:

* [ ] The figure supports a paper section.
* [ ] The figure number is correct.
* [ ] The title is accurate.
* [ ] All labels are readable.
* [ ] Terminology matches the paper.
* [ ] Arrows indicate the correct direction.
* [ ] AI and deterministic stages are distinguishable.
* [ ] Human-review points are shown where relevant.
* [ ] No unsupported claim is embedded.
* [ ] No confidential data appears.
* [ ] The figure remains readable in grayscale.
* [ ] The caption explains the figure.
* [ ] The editable source is preserved.
* [ ] SVG or PDF export is available.
* [ ] PNG fallback is available where needed.

---

# 66. Table Review Checklist

For each table:

* [ ] The table number is correct.
* [ ] The caption is accurate.
* [ ] Column names are defined.
* [ ] Units are included.
* [ ] Decimal precision is consistent.
* [ ] Percentages use the same scale.
* [ ] Missing values are marked clearly.
* [ ] Targets are not mixed with achieved results.
* [ ] Failed runs are represented.
* [ ] Sample sizes are included where necessary.
* [ ] Values match source files.
* [ ] Totals and averages are recalculated.
* [ ] The table fits the publication layout.

---

# 67. Chart Review Checklist

For each chart:

* [ ] The chart uses actual result data.
* [ ] The data source is versioned.
* [ ] The chart-generation script is saved.
* [ ] Axis labels are present.
* [ ] Units are shown.
* [ ] The baseline is not misleading.
* [ ] Error bars are defined.
* [ ] Sample size is stated.
* [ ] Colors or patterns are consistent.
* [ ] The chart is readable in grayscale.
* [ ] Values match the result tables.
* [ ] The chart does not hide failed runs.
* [ ] The caption states what is being compared.
* [ ] Vector export is available.

---

# 68. Screenshot Review Checklist

For each screenshot:

* [ ] The application state is meaningful.
* [ ] The screenshot uses anonymized data.
* [ ] No local username is visible.
* [ ] No credentials are visible.
* [ ] No confidential paths are visible.
* [ ] The viewport is recorded.
* [ ] The application version is recorded.
* [ ] Text is readable.
* [ ] The image is not unnecessarily cropped.
* [ ] An original unannotated image is preserved.
* [ ] The paper caption explains the screenshot.
* [ ] The screenshot is referenced from the paper or appendix.

---

# 69. Required Phase 5 Files

The planned Phase 5 file structure is:

```text
docs/day49/
├── publication_assets_plan.md
└── assets/
    ├── README.md
    ├── asset-manifest.json
    ├── figures/
    │   ├── figure-01-end-to-end-architecture.svg
    │   ├── figure-02-api-factory-workflow.svg
    │   ├── figure-03-migration-analyzer-workflow.svg
    │   ├── figure-04-shared-domain-model.svg
    │   ├── figure-05-explainable-scoring.svg
    │   └── figure-06-experimental-design.svg
    ├── charts/
    │   ├── chart-01-requirement-extraction-performance.svg
    │   ├── chart-02-migration-analysis-performance.svg
    │   ├── chart-04-traceability-coverage.svg
    │   └── chart-05-workflow-time.svg
    ├── screenshots/
    ├── tables/
    └── source/
```

The listed generated assets remain planned until they are actually created.

---

# 70. Recommended Asset Manifest

```json
{
  "version": "1.0.0",
  "figures": [
    {
      "id": "FIGURE-01",
      "title": "End-to-End Hybrid API Modernization Architecture",
      "status": "planned",
      "paperSection": "Proposed Architecture"
    },
    {
      "id": "FIGURE-02",
      "title": "API Factory Requirement Processing Workflow",
      "status": "planned",
      "paperSection": "API Factory Design"
    },
    {
      "id": "FIGURE-03",
      "title": "API Migration Analyzer Workflow",
      "status": "planned",
      "paperSection": "Migration Analyzer Design"
    }
  ],
  "charts": [],
  "screenshots": [],
  "tables": []
}
```

---

# 71. Asset Validation Commands Plan

Potential commands:

```bash
npm run research:validate-assets
npm run research:generate-charts
npm run research:check-image-sizes
npm run research:check-missing-assets
```

These commands should be documented only after they are implemented.

Potential validations:

* Required asset exists.
* Required source file exists.
* Minimum image dimensions.
* Allowed file format.
* Asset manifest consistency.
* Duplicate numbering.
* Missing caption.
* Missing source data.
* Unexpected large file size.

---

# 72. Publication Asset Completion Criteria

Phase 5 planning is complete when:

* The required figure list is defined.
* The required table list is defined.
* The chart plan is defined.
* The screenshot plan is defined.
* Asset captions are drafted.
* Asset names are standardized.
* Source-file requirements are defined.
* Accessibility rules are defined.
* Review checklists are defined.
* Main-paper and appendix assets are separated.

Actual asset creation will require:

* Final application screenshots.
* Final evaluation data.
* Final paper layout.
* Final venue requirements.

---

# 73. Phase 5 Completion Checklist

* [x] Defined publication asset principles.
* [x] Defined the primary figure inventory.
* [x] Planned the end-to-end architecture figure.
* [x] Planned the API Factory workflow figure.
* [x] Planned the Migration Analyzer workflow figure.
* [x] Planned the shared domain-model figure.
* [x] Planned the explainable-scoring figure.
* [x] Planned the experimental-design figure.
* [x] Planned the human-review figure.
* [x] Planned the implementation screenshot figure.
* [x] Defined the required table inventory.
* [x] Created result-table templates.
* [x] Defined the chart inventory.
* [x] Defined chart construction rules.
* [x] Defined chart source files.
* [x] Defined the browser screenshot inventory.
* [x] Defined screenshot capture requirements.
* [x] Defined the code excerpt plan.
* [x] Defined source-file preservation.
* [x] Defined asset naming conventions.
* [x] Defined caption rules.
* [x] Defined accessibility requirements.
* [x] Defined export and resolution requirements.
* [x] Defined visual consistency rules.
* [x] Mapped assets to paper sections.
* [x] Separated main-paper and appendix assets.
* [x] Defined reproducible chart generation.
* [x] Defined asset metadata.
* [x] Defined asset review workflows.
* [x] Defined figure, table, chart, and screenshot checklists.

---

# 74. Phase 5 Completion Status

**Status:** Completed

**Recommended file:**

```text
docs/day49/publication_assets_plan.md
```

**Next Phase:**

Day 49 Phase 6 will define the related-work research strategy, literature search queries, paper-selection criteria, citation tracking, source-quality controls, and bibliography preparation plan.
