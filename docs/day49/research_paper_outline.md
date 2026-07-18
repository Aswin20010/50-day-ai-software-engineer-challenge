# Day 49 – Phase 2

# Research Paper Structure and Detailed Outline

## Project

**AI-Assisted API Factory and Migration Analyzer**

## Working Paper Title

**AI-Assisted API Modernization: A Rule-Based Architecture for Requirement Extraction, API Generation, and Migration Analysis**

---

## 1. Phase 2 Objective

The objective of Day 49 Phase 2 is to create the complete structure and detailed section-by-section outline for the research paper.

This outline converts the research question and contribution defined in Phase 1 into a publication-ready paper plan. It identifies:

* The purpose of every section.
* The technical content to include.
* The project evidence required.
* The figures and tables to prepare.
* The evaluation results to collect.
* The relationship between the API Factory and API Migration Analyzer.
* The claims that must be supported through experiments.

This document will guide the full paper-writing workflow and prevent important technical or research details from being omitted.

---

## 2. Recommended Paper Format

The paper should follow a standard software-engineering research structure:

1. Title
2. Abstract
3. Keywords
4. Introduction
5. Background and Motivation
6. Related Work
7. Research Questions
8. Proposed Architecture
9. System Design and Implementation
10. Research Methodology
11. Experimental Evaluation
12. Results
13. Discussion
14. Threats to Validity
15. Limitations
16. Future Work
17. Conclusion
18. References
19. Appendices

The recommended initial target is:

* **Main paper:** 8–12 pages.
* **References:** 1–3 pages.
* **Appendices:** As required.
* **Target word count:** Approximately 6,000–8,500 words.

The final length must be adapted to the selected journal, workshop, or conference requirements.

---

# 3. Complete Research Paper Outline

## 3.1 Title

### Proposed Primary Title

**AI-Assisted API Modernization: A Rule-Based Architecture for Requirement Extraction, API Generation, and Migration Analysis**

### Alternative Title 1

**A Hybrid AI and Rule-Based Framework for Enterprise API Development and Modernization**

### Alternative Title 2

**From Requirements to Migration Plans: An Evidence-Driven AI Architecture for Enterprise APIs**

### Alternative Title 3

**Improving Enterprise API Modernization Through AI-Assisted Extraction and Deterministic Validation**

### Title Requirements

The title should communicate:

* The use of artificial intelligence.
* The deterministic or rule-based component.
* The enterprise API domain.
* The modernization or migration objective.
* The end-to-end nature of the workflow.

---

## 3.2 Abstract

### Purpose

The abstract must summarize the entire research paper in approximately 180–250 words.

### Content to Include

The abstract should contain:

1. The enterprise API modernization problem.
2. The limitations of manual analysis.
3. The risks of unrestricted AI-generated outputs.
4. The proposed hybrid AI and deterministic architecture.
5. The two implemented systems.
6. The evaluation methodology.
7. The major measurable results.
8. The primary research contribution.

### Planned Abstract Structure

```text
Context:
Enterprise API development and modernization require substantial manual
effort to interpret requirements, inspect existing implementations, identify
architecture gaps, and create migration plans.

Problem:
AI-assisted development can reduce this effort, but unrestricted model output
may be incomplete, non-deterministic, difficult to validate, and poorly
traceable to source evidence.

Method:
This paper proposes a hybrid architecture combining AI-assisted requirement
interpretation with structured schemas, deterministic normalization, rule-based
validation, source-code evidence extraction, target-architecture comparison,
and human review.

Implementation:
The architecture is evaluated through an API Factory and an API Migration
Analyzer.

Results:
The evaluation measures extraction accuracy, validation coverage, evidence
accuracy, migration-gap detection, traceability, consistency, and engineering
time reduction.

Contribution:
The proposed system demonstrates how generative AI can be integrated into a
governed and explainable enterprise API modernization workflow.
```

### Abstract Completion Dependency

The final abstract must be written after the evaluation results are available.

Do not include unverified performance numbers in the initial abstract draft.

---

## 3.3 Keywords

### Proposed Keywords

* Generative AI
* API modernization
* API migration
* Requirement extraction
* Software architecture
* Static analysis
* Rule-based validation
* Evidence-driven analysis
* Enterprise APIs
* Human-in-the-loop
* Software engineering automation
* Large language models

Use approximately five to eight keywords in the final submission.

---

## 3.4 Introduction

### Purpose

The introduction should explain the problem, its importance, the proposed solution, and the paper’s contributions.

### Section Structure

#### 3.4.1 Enterprise API Complexity

Discuss how enterprise API work requires engineers to understand:

* Business documentation.
* API contracts.
* Validation rules.
* Security requirements.
* Database behavior.
* External integrations.
* Legacy implementation details.
* Target architecture standards.
* Testing and operational requirements.

#### 3.4.2 Manual Development and Migration Challenges

Describe common manual activities:

* Reading requirements across multiple documents.
* Translating prose into API contracts.
* Detecting missing requirements.
* Inspecting legacy source code.
* Identifying undocumented behavior.
* Comparing implementation patterns.
* Preparing migration recommendations.
* Producing implementation task lists.
* Repeating analysis during requirement changes.

#### 3.4.3 Limitations of AI-Only Approaches

Explain that AI-generated software artifacts may contain:

* Hallucinated endpoints or fields.
* Missing validations.
* Unsupported technical assumptions.
* Inconsistent output structures.
* Non-reproducible recommendations.
* Poor evidence traceability.
* Difficulty proving correctness.

#### 3.4.4 Proposed Solution

Introduce the hybrid system that combines:

* AI-assisted extraction.
* Structured schemas.
* Deterministic normalization.
* Rule-based validation.
* Source evidence extraction.
* Target-architecture comparison.
* Explainable scoring.
* Human review.

#### 3.4.5 Research Objective

State the primary research objective:

> To evaluate whether a hybrid AI-assisted and rule-based architecture can improve the accuracy, traceability, consistency, and efficiency of enterprise API development and modernization.

#### 3.4.6 Contributions

Present the main contributions as a concise numbered list:

1. A unified architecture connecting requirement extraction, API artifact generation, and legacy API migration analysis.
2. A deterministic validation layer for controlling AI-assisted outputs.
3. An evidence-backed migration-analysis model.
4. An explainable architecture-readiness scoring process.
5. A structured method for converting findings into implementation-ready migration tasks.
6. An evaluation framework covering accuracy, traceability, consistency, and engineering effort.

#### 3.4.7 Paper Organization

Conclude the introduction with a paragraph describing the remaining sections.

---

## 3.5 Background and Motivation

### Purpose

This section should provide the technical concepts needed to understand the proposed system.

### 3.5.1 API Requirements Engineering

Explain:

* Functional API requirements.
* Non-functional requirements.
* Endpoint contracts.
* Data models.
* Validation rules.
* Error handling.
* Security requirements.
* Integration requirements.

### 3.5.2 API Modernization

Define API modernization as the process of transforming an existing API toward:

* Updated frameworks.
* Improved modularity.
* Cloud-native deployment.
* Better security.
* Improved observability.
* Automated testing.
* Standardized architecture.
* Maintainable data-access patterns.

### 3.5.3 Generative AI in Software Engineering

Discuss common applications:

* Requirement summarization.
* Code generation.
* Test generation.
* Documentation generation.
* Code explanation.
* Refactoring recommendations.
* Architecture assistance.

Also explain the reliability concerns associated with generated output.

### 3.5.4 Deterministic Validation

Explain the role of:

* Schemas.
* Enumerations.
* Required-field checks.
* Normalization rules.
* Duplicate detection.
* Cross-field validation.
* Rule engines.
* Reproducible scoring.

### 3.5.5 Static Evidence Extraction

Explain that source-code analysis can collect evidence such as:

* Routes.
* Handlers.
* Service methods.
* Database calls.
* Authentication controls.
* Validation logic.
* Logging.
* Tests.
* Configuration.
* External clients.

### 3.5.6 Human-in-the-Loop Governance

Explain why human review remains necessary for:

* Business-rule interpretation.
* Security approval.
* Architecture decisions.
* Compliance requirements.
* Production readiness.
* Acceptance of generated recommendations.

---

## 3.6 Related Work

### Purpose

This section should compare the proposed research with existing work and establish the research gap.

### Research Areas to Review

#### 3.6.1 AI-Assisted Requirement Engineering

Review research involving:

* Requirement classification.
* Requirement extraction.
* Natural-language-to-model transformation.
* Ambiguity detection.
* Requirement completeness analysis.

#### 3.6.2 AI-Assisted Code Generation

Review:

* Large language models for code generation.
* Prompt-based API creation.
* Test generation.
* Documentation generation.
* Reliability and hallucination studies.

#### 3.6.3 Model-Driven API Development

Review:

* OpenAPI-based generation.
* Schema-first development.
* Model-driven engineering.
* Low-code API platforms.
* Contract generation systems.

#### 3.6.4 Software Modernization and Migration Analysis

Review:

* Legacy modernization tools.
* Static code analysis.
* Architecture conformance checking.
* Technical-debt analysis.
* Cloud-readiness assessment.
* Automated refactoring systems.

#### 3.6.5 Explainable Software Engineering Tools

Review tools or studies that provide:

* Evidence-backed findings.
* Traceability matrices.
* Rule explanations.
* Architecture scores.
* Human verification workflows.

### Related-Work Comparison Table

Prepare a table with columns similar to:

| Approach            | Requirement Extraction | Code Generation | Legacy Analysis | Deterministic Validation | Evidence Traceability | Migration Tasks |
| ------------------- | ---------------------: | --------------: | --------------: | -----------------------: | --------------------: | --------------: |
| Existing approach A |                    Yes |              No |              No |                  Partial |                    No |              No |
| Existing approach B |                     No |             Yes |         Partial |                       No |               Partial |              No |
| Existing approach C |                     No |              No |             Yes |                      Yes |                   Yes |         Partial |
| Proposed system     |                    Yes |             Yes |             Yes |                      Yes |                   Yes |             Yes |

### Research Gap Statement

Conclude with a clear gap:

> Existing solutions generally focus on requirement analysis, code generation, or legacy modernization as separate activities. Few approaches combine AI-assisted interpretation, deterministic validation, source evidence, target-architecture comparison, explainable scoring, and migration-task generation in a unified workflow.

---

## 3.7 Research Questions and Hypotheses

### Purpose

This section formally presents the research questions and hypotheses defined in Phase 1.

### Primary Research Question

> How effectively can a hybrid AI-assisted and rule-based system transform unstructured API requirements and legacy API evidence into validated implementation specifications and actionable modernization recommendations?

### Secondary Research Questions

#### RQ1

How accurately can the system extract structured API requirements from unstructured or semi-structured documentation?

#### RQ2

To what extent do deterministic schema and rule validation reduce incomplete, inconsistent, or unsupported generated requirements?

#### RQ3

Can validated requirement models consistently produce implementation-ready API artifacts with traceability to source requirements?

#### RQ4

How accurately can the migration analyzer identify architecture gaps, technical risks, and modernization requirements?

#### RQ5

Does evidence-backed analysis improve the explainability and reviewability of modernization recommendations?

#### RQ6

How much engineering effort can the proposed workflow reduce compared with manual API analysis?

### Hypotheses

Include the primary hypothesis and supporting hypotheses H1 through H5 from Phase 1.

### Research Question Mapping Table

| Research Question | System Component             | Primary Metric                           |
| ----------------- | ---------------------------- | ---------------------------------------- |
| RQ1               | Requirement extractor        | Precision and recall                     |
| RQ2               | Schema and rule validator    | Validation coverage and defect detection |
| RQ3               | API Factory                  | Artifact completeness and traceability   |
| RQ4               | Migration Analyzer           | Gap and risk detection accuracy          |
| RQ5               | Evidence viewer and findings | Traceability and reviewer confidence     |
| RQ6               | End-to-end workflow          | Time and effort reduction                |

---

## 3.8 Proposed System Architecture

### Purpose

This section describes the complete architecture at a conceptual level.

### 3.8.1 Architectural Principles

The architecture should follow these principles:

* AI output is treated as a proposal, not final truth.
* All critical artifacts use structured data models.
* Deterministic validation follows AI extraction.
* Every major finding should have evidence.
* Scoring must be explainable.
* Workflow stages should remain independently testable.
* Human approval should be available at critical checkpoints.

### 3.8.2 End-to-End Workflow

```text
Unstructured Requirements
          |
          v
AI-Assisted Extraction
          |
          v
Structured Requirement Model
          |
          v
Normalization and Schema Validation
          |
          v
Validated API Specification
          |
          +-----------------------------+
          |                             |
          v                             v
API Artifact Generation        Existing API Project
                                        |
                                        v
                                Artifact Collection
                                        |
                                        v
                                 Evidence Extraction
                                        |
                                        v
                            Target Architecture Comparison
                                        |
                                        v
                              Gaps, Risks, and Scores
                                        |
                                        v
                       Recommendations and Migration Tasks
                                        |
                                        v
                             Human Review and Approval
```

### 3.8.3 Major Architectural Layers

#### Input Layer

Accepts:

* Requirement text.
* Requirement documents.
* Source artifacts.
* Target architecture selection.
* Analysis configuration.

#### AI Interpretation Layer

Responsible for:

* Requirement interpretation.
* Candidate field extraction.
* Candidate rule extraction.
* Requirement classification.
* Suggested mappings.

#### Structured Domain Layer

Contains models for:

* Requirements.
* Endpoints.
* Fields.
* Rules.
* Evidence.
* Gaps.
* Risks.
* Recommendations.
* Tasks.
* Scores.

#### Deterministic Validation Layer

Performs:

* Required-field validation.
* Type validation.
* Enumeration validation.
* Cross-field validation.
* Duplicate removal.
* Missing-field detection.
* Unsupported-value detection.

#### Evidence Extraction Layer

Collects:

* Endpoint evidence.
* Capability evidence.
* Validation evidence.
* Security evidence.
* Data-access evidence.
* Testing evidence.
* Observability evidence.

#### Analysis Layer

Performs:

* Rule evaluation.
* Target-architecture comparison.
* Gap detection.
* Risk detection.
* Severity assignment.
* Readiness scoring.

#### Recommendation Layer

Generates:

* Technical recommendations.
* Migration priorities.
* Implementation tasks.
* Acceptance criteria.
* Dependency ordering.

#### Presentation Layer

Displays:

* Summary scorecards.
* Rule results.
* Gaps.
* Risks.
* Recommendations.
* Tasks.
* Evidence.
* Loading and error states.

### 3.8.4 Architecture Figure

Prepare a publication-quality architecture diagram showing:

* Both project workflows.
* Shared domain models.
* Validation layers.
* Human-review checkpoints.
* Output artifacts.

---

## 3.9 API Factory Design

### Purpose

This section explains Project 1 in technical detail.

### 3.9.1 Requirement Input

Describe:

* Supported input form.
* Text or file ingestion.
* Initial validation.
* User workflow.

### 3.9.2 Requirement Extraction

Describe extraction of:

* Flow name.
* Endpoints.
* Request fields.
* Response fields.
* Headers.
* Validations.
* Business rules.
* Dependencies.
* Missing fields.

### 3.9.3 Requirement Normalization

Explain normalization of:

* HTTP methods.
* Field names.
* Data types.
* Required flags.
* Rule formats.
* Endpoint paths.
* Duplicates.
* Empty values.

### 3.9.4 Requirement Schema

Document the primary TypeScript models and their relationships.

Include a simplified schema figure or table.

### 3.9.5 Rule Construction

Explain how normalized extraction results become structured rules.

Rule categories may include:

* Required-field rules.
* Format rules.
* Range rules.
* Conditional rules.
* Business rules.
* Security rules.
* Error-handling rules.

### 3.9.6 Workflow State Management

Describe:

* Input state.
* Loading state.
* Success state.
* Error state.
* Reset behavior.
* Reusable workflow hooks.
* State-refactor decisions.

### 3.9.7 Generated Outputs

Explain what the API Factory produces:

* Structured requirement models.
* Technical specifications.
* Workflow definitions.
* Validation rules.
* Test ideas.
* Implementation guidance.

### 3.9.8 API Factory Validation

Document:

* Unit tests.
* Hook tests.
* Lint validation.
* Production build validation.
* Browser workflow validation.

---

## 3.10 API Migration Analyzer Design

### Purpose

This section explains Project 2 in technical detail.

### 3.10.1 Source Artifact Model

Describe supported source artifacts:

* Route files.
* Handler files.
* Service files.
* Repository files.
* Models.
* Configuration.
* Test files.
* Documentation.

### 3.10.2 Target Architecture Model

Describe target architecture definitions and expected characteristics.

Example categories:

* Layering.
* Validation.
* Security.
* Data access.
* Error handling.
* Observability.
* Testing.
* Configuration.
* Resilience.
* Deployment readiness.

### 3.10.3 Endpoint Evidence Extraction

Explain how the analyzer identifies:

* Routes.
* HTTP methods.
* Endpoint paths.
* Handler functions.
* Request and response handling.
* Validation behavior.
* Service dependencies.

### 3.10.4 Capability Evidence Extraction

Explain extraction of capabilities such as:

* Authentication.
* Authorization.
* Logging.
* Tracing.
* Database transactions.
* Retry behavior.
* Error handling.
* Testing.
* Configuration management.

### 3.10.5 Evidence Normalization and Deduplication

Describe:

* Evidence identifiers.
* Source references.
* Duplicate detection.
* Evidence confidence.
* Category normalization.

### 3.10.6 Migration Gap Detection

Explain how expected target capabilities are compared against observed evidence.

A gap should include:

* Identifier.
* Category.
* Description.
* Severity.
* Expected capability.
* Observed evidence.
* Source references.
* Remediation guidance.

### 3.10.7 Risk Detection

Explain risk categories such as:

* Security risk.
* Data-integrity risk.
* Operational risk.
* Migration-complexity risk.
* Test-coverage risk.
* Integration risk.
* Maintainability risk.

### 3.10.8 Recommendation Generation

Explain how gaps and risks are transformed into recommendations.

### 3.10.9 Migration Task Generation

Each task should include:

* Task title.
* Technical description.
* Priority.
* Dependencies.
* Affected components.
* Acceptance criteria.
* Linked findings.
* Linked evidence.

### 3.10.10 Analysis Orchestrator

Explain the orchestration sequence:

1. Validate analysis request.
2. Load target architecture.
3. Normalize source artifacts.
4. Extract endpoint evidence.
5. Extract capability evidence.
6. Evaluate architecture rules.
7. Generate gaps.
8. Generate risks.
9. Deduplicate findings.
10. Generate recommendations.
11. Generate migration tasks.
12. Calculate summary scores.
13. Return the analysis result.

### 3.10.11 Browser Workflow

Describe:

* Sample project selection.
* Target architecture selection.
* Analysis request form.
* Execution state.
* Error handling.
* Summary scorecards.
* Rule results.
* Gaps and risks.
* Recommendations.
* Migration tasks.
* Evidence viewer.
* Responsive layout.

---

## 3.11 Explainable Scoring Model

### Purpose

This section defines how architecture and migration readiness scores are calculated.

### 3.11.1 Scoring Categories

Potential score categories:

* Endpoint coverage.
* Validation readiness.
* Security readiness.
* Architecture alignment.
* Data-access readiness.
* Error-handling readiness.
* Observability readiness.
* Testing readiness.
* Deployment readiness.
* Overall modernization readiness.

### 3.11.2 Rule Scoring

Each rule may produce:

* Passed.
* Partially passed.
* Failed.
* Not applicable.
* Insufficient evidence.

### 3.11.3 Example Weighting Model

| Severity or Rule Type | Suggested Weight |
| --------------------- | ---------------: |
| Critical              |                5 |
| High                  |                4 |
| Medium                |                3 |
| Low                   |                2 |
| Informational         |                1 |

### 3.11.4 Example Category Score

```text
category score =
earned rule weight / applicable rule weight × 100
```

### 3.11.5 Overall Score

The overall score may be calculated as a weighted average of category scores.

The paper must clearly state:

* Which categories are weighted.
* Why the weights were selected.
* How missing evidence is treated.
* How not-applicable rules are excluded.
* How partial results affect the score.

### 3.11.6 Explainability Requirement

Every score must be traceable to:

* Individual rules.
* Rule outcomes.
* Evidence.
* Assigned weights.
* Gap and risk results.

The scoring model should not be presented as universally valid unless it has been sufficiently validated across multiple systems.

---

## 3.12 Research Methodology

### Purpose

This section explains how the research is evaluated.

### 3.12.1 Research Method

Use a design-science and experimental case-study approach.

The research process includes:

1. Identify the enterprise API modernization problem.
2. Define system requirements.
3. Design the hybrid architecture.
4. Implement the API Factory.
5. Implement the Migration Analyzer.
6. Construct evaluation datasets.
7. Establish expert-reviewed expected outputs.
8. Run the systems.
9. Measure results.
10. Analyze findings and limitations.

### 3.12.2 Evaluation Datasets

Prepare multiple requirement and source-project cases.

Suggested dataset categories:

* Simple CRUD API.
* API with complex validation.
* API with database transactions.
* API with external service integrations.
* API with authentication and authorization.
* Layered enterprise API.
* Poorly structured legacy API.
* Partially modernized API.

### 3.12.3 Ground-Truth Preparation

For each case, manually document:

* Expected endpoints.
* Expected request and response fields.
* Expected validation rules.
* Expected business rules.
* Expected source capabilities.
* Expected architecture gaps.
* Expected technical risks.
* Expected recommendations.

Ground truth should be reviewed by at least one experienced software engineer where possible.

### 3.12.4 Baseline Approaches

Compare the proposed system with:

#### Baseline A – Manual Analysis

An engineer manually produces:

* Requirement specification.
* Architecture findings.
* Migration recommendations.

#### Baseline B – AI-Only Analysis

An AI model produces outputs without deterministic schema validation or evidence-based rule evaluation.

#### Proposed Approach

AI-assisted extraction followed by deterministic validation and evidence-backed analysis.

### 3.12.5 Experiment Execution

For each case:

1. Provide the same source requirements to each relevant approach.
2. Record generated outputs.
3. Compare outputs with ground truth.
4. Record completion time.
5. Count missing or incorrect results.
6. Measure repeatability across multiple runs.
7. Conduct human review.
8. Record reviewer effort and confidence.

### 3.12.6 Repetition

Run AI-supported experiments multiple times where practical to measure output stability.

Suggested minimum:

* Three runs per input for AI-only analysis.
* Three runs per input for the hybrid workflow where AI extraction is enabled.
* One deterministic rerun using stored extracted input to verify stable downstream analysis.

---

## 3.13 Evaluation Metrics

### 3.13.1 Requirement Extraction Precision

```text
precision =
correctly extracted requirements / all extracted requirements
```

### 3.13.2 Requirement Extraction Recall

```text
recall =
correctly extracted requirements / all expected requirements
```

### 3.13.3 F1 Score

```text
F1 =
2 × precision × recall / (precision + recall)
```

### 3.13.4 Validation Defect Detection Rate

```text
defect detection rate =
detected invalid outputs / total intentionally invalid outputs
```

### 3.13.5 Gap Detection Accuracy

Measure correct, missed, and false-positive migration gaps.

### 3.13.6 Risk Detection Accuracy

Measure correct, missed, and false-positive risks.

### 3.13.7 Traceability Coverage

```text
traceability coverage =
findings with valid evidence links / total findings
```

### 3.13.8 Recommendation Actionability

Human reviewers score recommendations based on:

* Specificity.
* Technical correctness.
* Evidence support.
* Implementation readiness.
* Priority clarity.

Use a five-point Likert scale if appropriate.

### 3.13.9 Output Consistency

Compare repeated results using:

* Exact-match rate for deterministic fields.
* Semantic similarity for text findings.
* Score variance.
* Finding overlap.
* Recommendation overlap.

### 3.13.10 Time Reduction

```text
time reduction =
(manual time - automated time) / manual time × 100
```

### 3.13.11 Reviewer Effort

Record:

* Review duration.
* Number of corrections.
* Number of rejected findings.
* Number of accepted findings.
* Number of missing findings added manually.

---

## 3.14 Experimental Evaluation

### Purpose

This section presents the actual evaluation setup.

### 3.14.1 Hardware and Software Environment

Document:

* Operating system.
* Node.js version.
* TypeScript version.
* Next.js version.
* Vitest version.
* ESLint version.
* Browser version.
* AI model and configuration, if used.
* Machine hardware.
* Execution settings.

### 3.14.2 Test Cases

Prepare a table:

| Case ID | System Type              | Endpoints | Rules | External Integrations | Expected Gaps |
| ------- | ------------------------ | --------: | ----: | --------------------: | ------------: |
| C1      | Simple API               |       TBD |   TBD |                   TBD |           TBD |
| C2      | Validation-heavy API     |       TBD |   TBD |                   TBD |           TBD |
| C3      | Layered enterprise API   |       TBD |   TBD |                   TBD |           TBD |
| C4      | Legacy API               |       TBD |   TBD |                   TBD |           TBD |
| C5      | Partially modernized API |       TBD |   TBD |                   TBD |           TBD |

### 3.14.3 Experiment Configuration

Document:

* Input formats.
* Prompt version.
* Schema version.
* Rule-set version.
* Target architecture.
* Scoring weights.
* Number of repetitions.
* Time-measurement method.

### 3.14.4 Automated Validation

Include results from:

* Unit tests.
* Integration tests.
* Lint checks.
* Production builds.
* Browser workflow validation.

### 3.14.5 Human Evaluation

Describe:

* Number of reviewers.
* Reviewer experience.
* Review rubric.
* Scoring method.
* Conflict-resolution process.

---

## 3.15 Results

### Purpose

This section reports measured results without overstating conclusions.

### 3.15.1 Requirement Extraction Results

Prepare:

| Case | Precision | Recall |  F1 | Missing Fields | Incorrect Fields |
| ---- | --------: | -----: | --: | -------------: | ---------------: |
| C1   |       TBD |    TBD | TBD |            TBD |              TBD |
| C2   |       TBD |    TBD | TBD |            TBD |              TBD |

### 3.15.2 Validation Results

Report:

* Invalid structures detected.
* Missing fields detected.
* Duplicate rules removed.
* Unsupported values rejected.
* AI-output corrections required.

### 3.15.3 Migration Gap Results

Prepare:

| Case | Expected Gaps | Correct | Missed | False Positives | Accuracy |
| ---- | ------------: | ------: | -----: | --------------: | -------: |
| C1   |           TBD |     TBD |    TBD |             TBD |      TBD |
| C2   |           TBD |     TBD |    TBD |             TBD |      TBD |

### 3.15.4 Risk Detection Results

Use a similar table for technical risks.

### 3.15.5 Traceability Results

Report:

* Findings with evidence.
* Findings without evidence.
* Invalid evidence links.
* Reviewer verification success.

### 3.15.6 Consistency Results

Report:

* Finding overlap across runs.
* Score variance.
* Recommendation overlap.
* Deterministic rerun stability.

### 3.15.7 Efficiency Results

Prepare:

| Activity                | Manual Time | Proposed Workflow Time | Reduction |
| ----------------------- | ----------: | ---------------------: | --------: |
| Requirement structuring |         TBD |                    TBD |       TBD |
| Legacy analysis         |         TBD |                    TBD |       TBD |
| Gap documentation       |         TBD |                    TBD |       TBD |
| Task preparation        |         TBD |                    TBD |       TBD |

### 3.15.8 Reviewer Evaluation

Report average reviewer scores for:

* Correctness.
* Explainability.
* Actionability.
* Confidence.
* Ease of verification.

---

## 3.16 Discussion

### Purpose

This section interprets the results and answers the research questions.

### 3.16.1 Answer to RQ1

Discuss the system’s ability to extract structured requirements.

Cover:

* Strong extraction categories.
* Weak extraction categories.
* Ambiguous language.
* Missing context.
* False assumptions.

### 3.16.2 Answer to RQ2

Discuss the value of deterministic validation.

Explain which errors were prevented or detected after AI extraction.

### 3.16.3 Answer to RQ3

Discuss whether validated requirements produced implementation-ready artifacts.

### 3.16.4 Answer to RQ4

Discuss migration-gap and risk-detection accuracy.

Identify categories that were reliably detected and categories requiring more advanced analysis.

### 3.16.5 Answer to RQ5

Discuss whether evidence improved reviewer confidence and verification speed.

### 3.16.6 Answer to RQ6

Discuss measured time reduction and where manual work was still necessary.

### 3.16.7 Hybrid Versus AI-Only Workflow

Compare:

* Accuracy.
* Structure.
* Consistency.
* Traceability.
* Review effort.
* Flexibility.
* Processing cost.

### 3.16.8 Engineering Implications

Explain how the architecture could be used by:

* API platform teams.
* Enterprise architects.
* Modernization teams.
* Consulting teams.
* Software-development organizations.
* Regulated enterprises.

### 3.16.9 Research Implications

Discuss how the work contributes to:

* AI governance in software engineering.
* Explainable developer tools.
* Hybrid symbolic and generative systems.
* Evidence-based architecture analysis.

---

## 3.17 Threats to Validity

### 3.17.1 Internal Validity

Potential threats:

* Incorrect ground truth.
* Researcher bias.
* Prompt tuning based on test cases.
* Inconsistent manual timing.
* Rule-set implementation defects.

### 3.17.2 External Validity

Potential threats:

* Small number of projects.
* Limited programming-language support.
* Limited framework diversity.
* Enterprise APIs may differ across organizations.
* Results may not generalize to all architectures.

### 3.17.3 Construct Validity

Potential threats:

* Accuracy metrics may not fully represent usefulness.
* Score weights may be subjective.
* Recommendation actionability may depend on reviewer experience.
* Time reduction may vary based on engineer familiarity.

### 3.17.4 Conclusion Validity

Potential threats:

* Insufficient experiment repetitions.
* Small reviewer sample.
* High variance in AI output.
* Limited statistical power.

### Mitigation Strategies

Include:

* Multiple test cases.
* Multiple runs.
* Clear scoring rubrics.
* Versioned prompts and rules.
* Expert review.
* Transparent reporting.
* Separation of observed results from assumptions.

---

## 3.18 Limitations

Document current limitations honestly.

### Technical Limitations

* Source analysis may rely on supplied artifacts rather than a complete repository parser.
* Dynamic runtime behavior may not be observable.
* Reflection or generated routes may be missed.
* Business intent may not be inferable from code alone.
* AI extraction may still require correction.
* Scoring weights may require domain-specific tuning.
* Target architecture definitions may be subjective.
* Cross-language support may be limited.
* Security findings are not a replacement for penetration testing.
* Generated migration tasks still require engineering review.

### Research Limitations

* Limited dataset size.
* Limited reviewer population.
* Project examples may reflect the creator’s architectural assumptions.
* Evaluation may not include production deployment.
* Cost and latency may not be comprehensively measured.

---

## 3.19 Future Work

Potential future extensions:

1. Repository-wide source-code ingestion.
2. AST-based analysis for multiple languages.
3. Runtime telemetry integration.
4. OpenAPI comparison.
5. Database-schema analysis.
6. Dependency-graph extraction.
7. Automated code transformation.
8. Migration pull-request generation.
9. Security scanning integration.
10. Cloud-readiness rules.
11. Organization-specific architecture policies.
12. Feedback-based rule refinement.
13. Retrieval-augmented requirement extraction.
14. Confidence scoring for AI-generated fields.
15. Multi-agent review workflows.
16. Continuous architecture-conformance monitoring.
17. Integration with CI/CD pipelines.
18. Automated regression-test generation.
19. Cost-aware migration planning.
20. Longitudinal evaluation on production modernization programs.

---

## 3.20 Conclusion

### Purpose

The conclusion should summarize:

* The problem.
* The proposed architecture.
* The two implemented systems.
* The evaluation.
* The principal results.
* The contribution.
* The future direction.

### Planned Conclusion Structure

1. Restate the challenge of enterprise API modernization.
2. Summarize the hybrid architecture.
3. Highlight the importance of deterministic validation.
4. Highlight evidence-backed analysis.
5. Report the main measured outcomes.
6. State where human review remains necessary.
7. Close with the broader significance of governed AI-assisted software engineering.

Do not introduce new experiments or claims in the conclusion.

---

## 3.21 References

### Reference Categories

The bibliography should include research on:

* Large language models for software engineering.
* Requirement engineering.
* Natural-language requirement extraction.
* Code generation.
* Hallucination and reliability.
* Static code analysis.
* Software architecture conformance.
* Legacy-system modernization.
* Model-driven engineering.
* Explainable AI.
* Human-in-the-loop systems.
* API design and OpenAPI.
* Design-science research methodology.

### Reference Requirements

* Prefer peer-reviewed papers.
* Use recent research where relevant.
* Include foundational work where necessary.
* Avoid relying primarily on blogs or vendor marketing.
* Use a consistent citation style.
* Verify every citation before submission.
* Do not include references that were not read or used.

The exact references will be collected and verified in a later phase.

---

## 3.22 Appendices

### Appendix A – Requirement Schema

Include major TypeScript interfaces or simplified models.

### Appendix B – Target Architecture Definition

Include:

* Architecture categories.
* Rules.
* Weights.
* Expected capabilities.

### Appendix C – Evaluation Dataset

Include anonymized:

* Requirement samples.
* Source-artifact descriptions.
* Ground-truth findings.

### Appendix D – Human Review Rubric

Include reviewer questions and scoring guidance.

### Appendix E – Additional Results

Include:

* Full test outputs.
* Per-case findings.
* Additional charts.
* Repeated-run results.

### Appendix F – Reproducibility Information

Include:

* Repository structure.
* Installation commands.
* Test commands.
* Build commands.
* Experiment configuration.
* Prompt version.
* Schema version.
* Rule-set version.

---

# 4. Required Figures

The following figures should be prepared for the paper.

## Figure 1 – End-to-End Research Architecture

Show:

* Requirement input.
* AI extraction.
* Deterministic validation.
* API Factory.
* Source artifacts.
* Evidence extraction.
* Migration analysis.
* Recommendations.
* Human review.

## Figure 2 – API Factory Workflow

Show:

* Requirement upload.
* Extraction.
* Normalization.
* Missing-field detection.
* Rule construction.
* Structured output.
* Browser review.

## Figure 3 – Migration Analyzer Workflow

Show:

* Source artifact ingestion.
* Endpoint evidence.
* Capability evidence.
* Target architecture.
* Rule evaluation.
* Findings.
* Scores.
* Recommendations.
* Tasks.

## Figure 4 – Shared Domain Model

Show relationships among:

* Requirements.
* Endpoints.
* Rules.
* Evidence.
* Architecture rules.
* Gaps.
* Risks.
* Recommendations.
* Tasks.

## Figure 5 – Explainable Scoring Process

Show:

```text
Evidence
   |
   v
Rule Evaluation
   |
   v
Pass / Partial / Fail
   |
   v
Weighted Category Score
   |
   v
Overall Readiness Score
   |
   v
Linked Findings and Recommendations
```

## Figure 6 – Experimental Evaluation Design

Show:

* Manual baseline.
* AI-only baseline.
* Hybrid approach.
* Ground truth.
* Metrics.
* Human review.

---

# 5. Required Tables

Prepare the following tables.

1. Research questions and metrics.
2. Related-work comparison.
3. Architecture components.
4. Requirement schema summary.
5. Target architecture rule categories.
6. Evaluation cases.
7. Requirement extraction results.
8. Validation results.
9. Gap-detection results.
10. Risk-detection results.
11. Traceability results.
12. Consistency results.
13. Time-reduction results.
14. Human-review results.
15. Threats and mitigations.

---

# 6. Evidence to Collect From the Projects

## API Factory Evidence

Collect:

* Final project structure.
* Requirement models.
* Extraction workflow.
* Normalization helpers.
* Rule builders.
* Missing-field logic.
* Custom hook design.
* Unit-test results.
* Lint result.
* Production build result.
* Browser screenshots.

## Migration Analyzer Evidence

Collect:

* Source artifact models.
* Target architecture models.
* Evidence helper implementations.
* Endpoint extraction logic.
* Capability extraction logic.
* Gap generation.
* Risk generation.
* Recommendation generation.
* Migration task generation.
* Orchestrator workflow.
* Validation scripts.
* Unit-test results.
* Browser screenshots.

## Research Evaluation Evidence

Collect:

* Input requirements.
* Ground-truth annotations.
* Generated outputs.
* False positives.
* False negatives.
* Execution times.
* Repeated-run outputs.
* Reviewer scores.
* Reviewer corrections.

---

# 7. Recommended Repository Documentation Structure

```text
docs/
└── day49/
    ├── research_question_and_contribution.md
    ├── research_paper_structure_and_outline.md
    ├── research_methodology.md
    ├── evaluation_dataset_plan.md
    ├── metrics_and_scoring_plan.md
    ├── figures_and_tables_plan.md
    ├── related_work_search_plan.md
    └── day49_completion_report.md
```

### Phase 2 File

```text
docs/day49/research_paper_structure_and_outline.md
```

---

# 8. Recommended Paper-Writing Order

The paper should not be written strictly from the first section to the last.

Use the following order:

1. Research questions.
2. Proposed architecture.
3. System design and implementation.
4. Research methodology.
5. Evaluation dataset.
6. Results.
7. Discussion.
8. Threats to validity.
9. Limitations.
10. Related work.
11. Introduction.
12. Conclusion.
13. Abstract.
14. Title and keywords.
15. Final references and appendices.

Writing the abstract last ensures that it reflects the actual measured results.

---

# 9. Claim-Control Rules

To keep the paper academically credible:

* Do not claim production readiness unless it was evaluated.
* Do not claim universal generalizability.
* Do not present target metrics as achieved results.
* Do not describe test coverage as system correctness.
* Do not describe generated recommendations as guaranteed correct.
* Do not claim AI eliminated human review.
* Do not use unsupported percentage improvements.
* Separate observed results from inferred benefits.
* Report failed cases and false positives.
* State when a conclusion is based on a limited dataset.
* Preserve version information for prompts, schemas, and rules.

---

# 10. Phase 2 Completion Checklist

* [x] Defined the recommended paper format.
* [x] Defined the complete section structure.
* [x] Created the introduction outline.
* [x] Created the background outline.
* [x] Created the related-work outline.
* [x] Mapped research questions to metrics.
* [x] Defined the architecture section.
* [x] Defined the API Factory design section.
* [x] Defined the Migration Analyzer design section.
* [x] Defined the scoring-model section.
* [x] Defined the research methodology.
* [x] Defined the evaluation metrics.
* [x] Defined the experimental evaluation.
* [x] Defined the results section.
* [x] Defined the discussion section.
* [x] Defined threats to validity.
* [x] Defined limitations and future work.
* [x] Defined required figures.
* [x] Defined required tables.
* [x] Defined project evidence to collect.
* [x] Defined the recommended writing order.
* [x] Defined claim-control rules.

---

# 11. Phase 2 Completion Status

**Status:** Completed

**Recommended file location:**

```text
docs/day49/research_paper_structure_and_outline.md
```

**Next Phase:**

Day 49 Phase 3 will define the research methodology, evaluation datasets, experiment procedure, ground-truth process, baseline comparisons, and measurement strategy.
