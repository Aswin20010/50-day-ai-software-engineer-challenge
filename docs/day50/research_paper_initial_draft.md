# Day 50 – Phase 5

# Initial Research Paper Draft

## Working Title

# AI-Assisted API Modernization: A Rule-Based Architecture for Requirement Extraction, API Generation, and Migration Analysis

## Author

**Aswin Tony Kanikairaj**

## Document Status

**Initial manuscript draft**

This document combines the research planning completed during Day 49 with the implemented API Factory and API Migration Analyzer projects.

The following components remain pending:

* Complete literature review.
* Final citations.
* Frozen evaluation dataset.
* Ground-truth review.
* Experiment execution.
* Human reviewer evaluation.
* Statistical analysis.
* Final figures and charts.
* Venue-specific formatting.
* Final abstract and conclusion.

Placeholder text is marked with `TBD`, `[Citation Required]`, or `[Results Pending]`.

---

# Abstract

Enterprise API engineering involves two related challenges. First, teams must transform incomplete and unstructured business requirements into implementation-ready API specifications. Second, organizations must assess existing APIs, identify architectural deficiencies, and create actionable modernization plans. These activities are frequently performed through separate manual workflows, making them difficult to standardize, reproduce, and audit. Large language models can assist with interpreting unstructured information, but unconstrained generated output may include unsupported assumptions, incomplete structures, and inconsistent recommendations.

This paper proposes a hybrid AI-assisted and rule-based architecture for API development and modernization. The architecture contains two connected systems. The API Factory converts unstructured requirement inputs into structured endpoint, field, validation, business-rule, error, and missing-information models. The API Migration Analyzer processes existing API source artifacts, extracts endpoint and capability evidence, evaluates configurable target-architecture rules, and generates evidence-linked gaps, risks, recommendations, migration tasks, and readiness summaries. The proposed workflow separates probabilistic interpretation from deterministic normalization, validation, rule evaluation, scoring, and traceability.

The research methodology compares manual engineering analysis, AI-only analysis, and the proposed hybrid approach using multiple API cases. Planned measurements include precision, recall, F1 score, structural validity, validation defect-detection rate, evidence accuracy, traceability coverage, repeated-run consistency, correction effort, human reviewer confidence, and total workflow time. The current work establishes the system architecture, prototype implementation, evaluation methodology, dataset design, and publication plan. Empirical results remain pending and will be added after execution of the controlled evaluation.

**Keywords:** API modernization, requirements engineering, generative AI, large language models, static analysis, architecture conformance, software migration, explainable AI, developer tools, human-in-the-loop systems

---

# 1. Introduction

Application programming interfaces are central to modern enterprise systems. They connect user applications, internal platforms, data services, external providers, and operational workflows. Despite their importance, the engineering work surrounding APIs often begins with incomplete information and continues through manual architecture analysis.

For a new API, requirements may be distributed across business documents, tickets, examples, emails, architecture notes, and conversations. Engineers must determine the required endpoints, HTTP methods, paths, headers, request fields, response fields, validations, business rules, errors, security requirements, and downstream dependencies. Missing or contradictory requirements must also be identified before implementation begins.

For an existing API, modernization requires a different but related form of analysis. Engineers must inspect source artifacts, recover endpoint and capability information, compare the implementation with a desired architecture, identify technical risks, and convert findings into an actionable migration backlog. This process can require extensive manual review and may produce inconsistent results across reviewers.

Generative artificial intelligence offers potential assistance for both workflows. Large language models can interpret natural-language requirements, summarize source artifacts, generate structured drafts, and propose implementation changes. However, generated outputs may vary across runs, omit critical constraints, invent unsupported behavior, or produce recommendations that are difficult to trace to evidence. Prior research has identified both the potential and reliability limitations of generative AI in software-engineering tasks. [Citation Required]

Traditional software-engineering techniques provide stronger control through schemas, static analysis, model-driven development, validation rules, and architecture-conformance checking. These techniques are reproducible and explainable, but they may be less effective when the initial input is ambiguous natural language or heterogeneous legacy documentation. [Citation Required]

This work investigates a hybrid approach. Artificial intelligence is used where interpretation of unstructured input is valuable, while deterministic models, validators, architecture rules, evidence links, scoring functions, and human review govern downstream decisions.

The proposed architecture contains two connected projects:

1. **AI-Assisted API Factory:** Converts unstructured API requirements into structured, normalized, validated, and reviewable specifications.
2. **API Migration Analyzer:** Evaluates existing API artifacts against configurable target architectures and generates evidence-backed modernization findings and tasks.

The central research question is:

> How effectively can a hybrid AI-assisted and rule-based system transform unstructured API requirements and legacy API evidence into validated implementation specifications and actionable modernization recommendations?

The primary contributions of this work are:

1. A unified architecture connecting requirement interpretation, API specification, source evidence, architecture assessment, and migration planning.
2. A typed intermediate representation for requirements and modernization artifacts.
3. A deterministic validation and architecture-rule pipeline surrounding AI-assisted interpretation.
4. An evidence model linking source artifacts to rules, gaps, risks, recommendations, and migration tasks.
5. An explainable scoring model based on explicit rule outcomes and weights.
6. A human-in-the-loop browser workflow for reviewing generated artifacts.
7. An evaluation methodology comparing manual, AI-only, and hybrid approaches.

This initial manuscript describes the problem, architecture, prototype implementation, and evaluation design. The empirical evaluation remains pending.

---

# 2. Background and Motivation

## 2.1 Enterprise API Requirement Engineering

API requirements are rarely delivered as complete formal specifications. A requirement may describe the intended business flow without defining:

* The final HTTP method.
* The endpoint path.
* Required headers.
* Field types.
* Required and optional fields.
* Conditional validation.
* Error responses.
* Transaction boundaries.
* Security behavior.
* External-service failure handling.

Engineers must interpret the available information and convert it into a technical contract.

This conversion is vulnerable to several problems:

* Requirements may be incomplete.
* Different statements may conflict.
* Important behavior may be implied but not explicit.
* Field names may be inconsistent.
* Examples may not match written rules.
* Engineers may make undocumented assumptions.
* Different reviewers may produce different interpretations.

A structured requirement model can reduce ambiguity by representing endpoints, fields, validations, business rules, errors, integrations, and missing information explicitly.

---

## 2.2 API Implementation and Artifact Generation

Once requirements are structured, they can support:

* OpenAPI generation.
* Request and response models.
* Validation schemas.
* Server scaffolding.
* Test cases.
* Documentation.
* Implementation planning.

However, code or contract generation is only as reliable as the structured input. When generated requirements contain unsupported assumptions, those assumptions may propagate into code, tests, and documentation.

The proposed workflow therefore treats extracted requirements as candidate data that must be normalized, validated, and reviewed.

---

## 2.3 Legacy API Modernization

Existing APIs may contain architecture and maintainability issues such as:

* Business logic inside HTTP handlers.
* Direct database access from routes.
* Hard-coded configuration.
* Inconsistent validation.
* Inconsistent error responses.
* Limited automated testing.
* Missing transaction management.
* Tight coupling to external services.
* Weak observability.
* Missing trace propagation.
* Duplicate implementations.
* Undocumented dependencies.

Modernization requires more than identifying individual code smells. Engineers must determine:

* What capabilities already exist.
* Which target capabilities are missing.
* Which deficiencies create meaningful risk.
* Which changes should be prioritized.
* How recommendations should be divided into implementable tasks.
* What evidence supports each conclusion.

A migration recommendation without evidence can be difficult to verify. A readiness score without explicit scoring rules can be difficult to trust. The Migration Analyzer addresses these concerns through structured source artifacts, evidence extraction, architecture rules, finding relationships, and explainable scoring.

---

## 2.4 AI-Only Workflow Limitations

A purely generative workflow may produce useful drafts, but it introduces several concerns:

* Output structure may vary.
* Required fields may be missing.
* Unsupported endpoints or rules may be invented.
* Similar findings may be duplicated.
* Severity may be inconsistent.
* Evidence references may be absent or invalid.
* Recommendations may not address the actual finding.
* Migration tasks may lack dependencies or acceptance criteria.
* Results may change across repeated runs.

These issues do not imply that generative AI is unusable. Instead, they motivate a design in which AI assistance is constrained by deterministic software-engineering mechanisms.

---

## 2.5 Hybrid AI and Deterministic Engineering

The proposed architecture divides work according to the strengths of each approach.

### AI-Assisted Processing

Useful for:

* Interpreting natural language.
* Identifying candidate entities.
* Summarizing requirement context.
* Mapping inconsistent terminology.
* Proposing candidate structured data.

### Deterministic Processing

Useful for:

* Schema validation.
* Field normalization.
* Identifier generation.
* Deduplication.
* Rule evaluation.
* Scoring.
* Relationship validation.
* Traceability checks.
* Output consistency.

### Human Review

Required for:

* Resolving ambiguity.
* Approving assumptions.
* Adjusting severity.
* Confirming business intent.
* Validating evidence.
* Approving recommendations.
* Accepting migration tasks.

---

# 3. Related Work

The final related-work section will synthesize research across eight areas.

## 3.1 AI-Assisted Requirements Engineering

Prior research has applied natural-language processing, machine learning, and generative models to:

* Requirement classification.
* Entity extraction.
* Functional and non-functional requirement identification.
* Ambiguity detection.
* Conflict detection.
* Traceability.
* Requirement generation.

The final review will examine whether existing approaches:

* Produce structured API-specific models.
* Detect missing information.
* Preserve source traceability.
* Apply deterministic validation.
* Connect requirement extraction to modernization analysis.

**Status:** Literature search pending.

**Required citations:** TBD.

---

## 3.2 Large Language Models for Software Engineering

Large language models have been studied for:

* Code generation.
* Code completion.
* Documentation.
* Test generation.
* Program repair.
* Code explanation.
* Refactoring.
* Architecture assistance.

The final section will examine findings related to:

* Correctness.
* Hallucination.
* Security.
* Reproducibility.
* Prompt sensitivity.
* Developer productivity.
* Human trust.
* Review effort.

**Status:** Literature search pending.

---

## 3.3 AI-Assisted API and Code Generation

Relevant work includes:

* Natural-language-to-OpenAPI generation.
* API scaffold generation.
* Contract generation.
* Microservice generation.
* Test generation.
* Documentation generation.

The comparison will evaluate whether prior systems include:

* Structured intermediate models.
* Validation.
* Missing-information handling.
* Source traceability.
* Enterprise business rules.
* Human approval.

**Status:** Literature search pending.

---

## 3.4 Model-Driven and Schema-First API Engineering

Model-driven and contract-first methods provide deterministic foundations for:

* Interface definition.
* Constraint representation.
* Code generation.
* Documentation generation.
* Validation.
* Model-to-code traceability.

The proposed system builds upon this principle by introducing structured intermediate representations between unstructured inputs and implementation artifacts.

**Status:** Literature search pending.

---

## 3.5 Static Analysis and Architecture Conformance

Architecture recovery and conformance checking analyze source structure, dependencies, layers, and rules. Relevant research will be reviewed for:

* Source representations.
* Language support.
* Architecture reconstruction.
* Rule definition.
* Evidence reporting.
* False-positive handling.
* Remediation generation.

The proposed Migration Analyzer uses structured source artifacts rather than complete AST-based repository analysis in its current prototype.

**Status:** Literature search pending.

---

## 3.6 Legacy Modernization and Migration Planning

Prior modernization research includes:

* Monolith decomposition.
* Cloud-readiness assessment.
* Technical-debt analysis.
* Migration pattern recommendation.
* Legacy architecture recovery.
* Risk assessment.
* Migration planning.

The final comparison will determine whether these approaches generate traceable migration tasks from explicit architecture-rule findings.

**Status:** Literature search pending.

---

## 3.7 Explainability and Evidence-Backed Tools

Explainability in developer tools may include:

* Source-code references.
* Evidence excerpts.
* Rule descriptions.
* Confidence values.
* Correction workflows.
* Traceable recommendations.

The proposed architecture treats evidence as a first-class domain object and preserves stable identifiers through the complete analysis chain.

**Status:** Literature search pending.

---

## 3.8 Human-in-the-Loop Software Engineering

Human-AI collaboration research examines:

* Review workflows.
* Correction behavior.
* Trust.
* Cognitive load.
* Approval.
* Accountability.
* Mixed-initiative systems.

The proposed system positions AI and automation as decision support. Human review remains necessary before requirements or migration plans are accepted.

**Status:** Literature search pending.

---

## 3.9 Provisional Research Gap

Existing research appears to address requirement extraction, code generation, model-driven API development, static architecture analysis, and legacy modernization as related but frequently separate problems.

The provisional gap is the limited integration of:

* Unstructured requirement interpretation.
* Deterministic requirement validation.
* API artifact preparation.
* Source-code evidence extraction.
* Configurable target-architecture comparison.
* Explainable readiness scoring.
* Gap and risk generation.
* Migration-task generation.
* Human review.

This gap statement must be revised after the systematic literature review.

---

# 4. Research Questions and Hypotheses

## 4.1 Primary Research Question

> How effectively can a hybrid AI-assisted and rule-based system transform unstructured API requirements and legacy API evidence into validated implementation specifications and actionable modernization recommendations?

---

## 4.2 Secondary Research Questions

### RQ1 – Requirement Extraction

How accurately does the hybrid system extract endpoints, fields, validations, business rules, errors, and missing information from unstructured API requirements?

### RQ2 – Deterministic Validation

How effectively do schemas and deterministic rules detect structural defects, unsupported relationships, and incomplete AI-generated artifacts?

### RQ3 – API Specification Quality

How complete, structurally valid, and traceable are the API Factory outputs compared with manual and AI-only approaches?

### RQ4 – Migration Finding Accuracy

How accurately does the Migration Analyzer identify architecture gaps and technical risks compared with expert-defined ground truth?

### RQ5 – Explainability

Does source-evidence traceability improve the reviewability and reviewer confidence of generated migration findings?

### RQ6 – Engineering Efficiency

How does the hybrid workflow affect end-to-end preparation, review, correction, and total workflow time compared with manual and AI-only analysis?

---

## 4.3 Primary Hypothesis

> A hybrid workflow combining AI-assisted interpretation with deterministic schemas, validation rules, evidence extraction, and architecture evaluation will produce more structurally valid, traceable, and consistent API engineering outputs than an AI-only workflow while reducing total manual effort compared with fully manual analysis.

---

## 4.4 Supporting Hypotheses

### H1

The hybrid approach will achieve higher structural-validity rates than the AI-only baseline.

### H2

Deterministic validation will detect most intentionally injected structural and relationship defects.

### H3

Evidence-linked migration findings will receive higher explainability and reviewer-confidence ratings than findings without source traceability.

### H4

The hybrid approach will produce fewer unsupported gaps and risks than the AI-only baseline.

### H5

The hybrid workflow will reduce total completion time compared with manual engineering analysis while requiring less correction than AI-only analysis.

These hypotheses remain untested.

---

# 5. Proposed System Architecture

## 5.1 Architectural Overview

The architecture contains two primary processing paths.

### Path A – Requirement to API Specification

```text
Unstructured Requirements
          |
          v
AI-Assisted or Deterministic Extraction
          |
          v
Raw Requirement Candidates
          |
          v
Normalization and Deduplication
          |
          v
Schema and Rule Validation
          |
          v
Structured Requirement Model
          |
          v
Human Review
          |
          v
Implementation-Oriented API Artifacts
```

### Path B – Source Artifacts to Migration Plan

```text
Existing API Source Artifacts
          |
          v
Artifact Normalization
          |
          v
Endpoint and Capability Evidence
          |
          v
Target Architecture Rule Evaluation
          |
          v
Rule Results
          |
     +----+----+
     |         |
     v         v
   Gaps      Risks
     \         /
      +---+---+
          |
          v
Recommendations
          |
          v
Migration Tasks
          |
          v
Readiness Summary and Human Review
```

---

## 5.2 Design Principles

The architecture is guided by six principles.

### Structured Intermediate Representations

Unstructured or generated content is converted into typed domain objects before downstream use.

### Deterministic Downstream Decisions

Normalization, validation, rule evaluation, scoring, and relationship checks use deterministic logic.

### Evidence Before Findings

Migration findings originate from extracted evidence and explicit architecture expectations.

### Stable Traceability

Stable identifiers connect artifacts, evidence, rules, findings, recommendations, and tasks.

### Human Approval

Generated artifacts require review before implementation or migration decisions.

### Configurability

Target architectures, rules, weights, evidence requirements, and thresholds can be adjusted for different organizational contexts.

---

# 6. API Factory Design

## 6.1 Purpose

The API Factory transforms unstructured API descriptions into structured and reviewable requirement artifacts.

The current prototype supports a browser workflow that:

* Accepts requirement input.
* Executes extraction.
* Stores raw extraction output.
* Normalizes the output.
* Detects missing fields.
* Constructs validation and business rules.
* Presents results.
* Supports reset and retry behavior.

---

## 6.2 Core Requirement Model

A simplified endpoint model is:

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

Additional models represent:

* Headers.
* Path parameters.
* Query parameters.
* Error responses.
* Dependencies.
* Security requirements.
* Missing information.
* Ambiguities.
* Extraction metadata.

---

## 6.3 Requirement Input

The input may contain:

* Flow description.
* Endpoint details.
* Request examples.
* Response examples.
* Validation statements.
* Business rules.
* Error scenarios.
* Security requirements.
* External integration behavior.

The input is not assumed to be complete.

---

## 6.4 Extraction

The extraction stage produces candidate structured data.

Depending on configuration, extraction may use:

* Deterministic sample processing.
* AI-assisted interpretation.
* A combination of explicit parsing and generative extraction.

The system should preserve the raw extraction result for auditability.

---

## 6.5 Normalization

Normalization converts inconsistent representations into canonical values.

Examples include:

* Uppercasing HTTP methods.
* Normalizing endpoint paths.
* Trimming field names.
* Normalizing field types.
* Removing duplicate rules.
* Converting severity or category names.
* Assigning stable identifiers.

Normalization must not silently add unsupported business behavior.

---

## 6.6 Missing-Information Detection

The system records unresolved requirements such as:

* Missing HTTP method.
* Missing path.
* Unknown required field.
* Undefined success response.
* Missing error behavior.
* Missing security requirements.
* Contradictory status definitions.

This behavior is central to reducing unsupported AI assumptions.

---

## 6.7 Rule Construction

Candidate statements are transformed into structured rules.

Rule categories may include:

* Required-field validation.
* Format validation.
* Range validation.
* Conditional validation.
* Business eligibility.
* Status transition.
* Error behavior.
* Integration behavior.
* Security behavior.

---

## 6.8 Workflow State

The browser workflow distinguishes:

* Idle.
* Ready.
* Executing.
* Successful.
* Failed.
* Reset.

The extraction workflow was refactored into reusable hooks and helpers so that page components remain focused on presentation.

---

## 6.9 Human Review

The user can inspect:

* Extracted flow.
* Endpoints.
* Fields.
* Rules.
* Errors.
* Missing information.
* Normalized values.

The current prototype supports review but does not yet provide a complete collaborative approval or version-history system.

---

# 7. API Migration Analyzer Design

## 7.1 Purpose

The API Migration Analyzer evaluates existing API artifacts against a selected target architecture.

It produces:

* Endpoint evidence.
* Capability evidence.
* Architecture-rule results.
* Migration gaps.
* Technical risks.
* Recommendations.
* Migration tasks.
* Summary scores.
* Evidence references.

---

## 7.2 Source Artifact Model

A source artifact represents a unit of implementation evidence.

It may contain:

* Artifact identifier.
* File path.
* Artifact type.
* Language.
* Content.
* Metadata.
* Endpoint hints.
* Capability hints.

The prototype uses supplied or synthetic artifacts rather than automatically cloning and parsing arbitrary repositories.

---

## 7.3 Evidence Extraction

### Endpoint Evidence

Endpoint evidence may identify:

* HTTP method.
* Route path.
* Handler.
* Request validation.
* Response behavior.
* External calls.
* Repository usage.

### Capability Evidence

Capability evidence may identify:

* Layer separation.
* Validation.
* Authentication.
* Authorization.
* Configuration.
* Logging.
* Tracing.
* Transactions.
* Testing.
* Resilience.
* Health checks.
* Documentation.

A simplified evidence interface is:

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

---

## 7.4 Evidence Deduplication

Multiple artifacts may support the same capability. The analyzer normalizes and deduplicates evidence before rule evaluation.

Deduplication helps prevent:

* Inflated evidence counts.
* Duplicate findings.
* Repeated recommendations.
* Inconsistent scoring.

---

## 7.5 Target Architecture Model

A target architecture defines expected capabilities.

A simplified rule structure is:

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

Target architecture categories may include:

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

---

## 7.6 Rule Evaluation

Each architecture rule receives an outcome such as:

* Passed.
* Partially passed.
* Failed.
* Not applicable.
* Insufficient evidence.

The exact supported values must match the final codebase.

Rule evaluation should preserve:

* Rule identifier.
* Outcome.
* Earned credit.
* Supporting evidence.
* Missing evidence.
* Explanation.

---

## 7.7 Gap Generation

A migration gap represents a difference between the observed system and the selected target architecture.

A gap may contain:

* Category.
* Severity.
* Expected capability.
* Observed implementation.
* Supporting evidence.
* Linked architecture rules.
* Affected artifacts.

---

## 7.8 Risk Generation

A technical risk describes the potential consequence of a gap or observed weakness.

Examples include:

* Data-integrity risk.
* Security risk.
* Maintainability risk.
* Regression risk.
* Availability risk.
* Observability risk.
* Configuration risk.
* Migration-complexity risk.

Risks should link to supporting evidence or gaps.

---

## 7.9 Recommendation Generation

Recommendations translate findings into technical remediation guidance.

A recommendation should include:

* Priority.
* Description.
* Rationale.
* Linked findings.
* Affected areas.
* Expected outcome.

Recommendations should not be generic statements unrelated to the observed project.

---

## 7.10 Migration Task Generation

Migration tasks convert recommendations into implementation-oriented work.

A simplified task model is:

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

Tasks may include:

* Service-layer extraction.
* Repository-layer extraction.
* Centralized validation.
* Error-handler implementation.
* Configuration externalization.
* Logging and tracing.
* Transaction handling.
* Unit-test creation.
* Integration-test creation.
* Documentation.

---

## 7.11 Browser Workspace

The browser interface includes:

* Sample project selection.
* Target architecture selection.
* Form validation.
* Execution state.
* Error state.
* Summary scorecards.
* Rule results.
* Gaps.
* Risks.
* Recommendations.
* Tasks.
* Evidence viewer.
* Responsive layout.

The UI is separated from the core analysis modules so that the analysis logic can be tested independently.

---

# 8. Explainable Architecture Scoring

## 8.1 Purpose

The scoring system summarizes architecture readiness without hiding the underlying evidence and rules.

A score should not be interpreted independently from:

* Rule outcomes.
* Rule weights.
* Category scores.
* Gaps.
* Risks.
* Evidence.

---

## 8.2 Rule Credits

Example outcome credits may be:

| Outcome               |       Credit |
| --------------------- | -----------: |
| Passed                |         1.00 |
| Partial               |         0.50 |
| Failed                |         0.00 |
| Not applicable        |     Excluded |
| Insufficient evidence | Configurable |

The exact credits must be frozen before evaluation.

---

## 8.3 Category Score

A category score is calculated as:

```text
Category Score =
Sum of Earned Weighted Credit
÷
Sum of Applicable Rule Weights
× 100
```

---

## 8.4 Overall Score

The overall readiness score is calculated as:

```text
Overall Score =
Sum of Category Score × Category Weight
÷
Sum of Category Weights
```

---

## 8.5 Readiness Classification

Potential readiness bands are:

|  Score | Classification                     |
| -----: | ---------------------------------- |
| 85–100 | High readiness                     |
|  70–84 | Moderate readiness                 |
|  50–69 | Significant modernization required |
|   0–49 | Major modernization required       |

These bands are provisional and must be validated with expert review.

---

## 8.6 Explainability

A user should be able to trace:

```text
Overall Score
      |
      v
Category Score
      |
      v
Rule Outcome
      |
      v
Architecture Rule
      |
      v
Supporting Evidence
      |
      v
Source Artifact
```

This traceability distinguishes the scoring system from an unexplained generated score.

---

# 9. Research Methodology

## 9.1 Methodological Approach

The research uses:

* Design-science research.
* Controlled comparative evaluation.
* Multiple-case study.
* Human expert review.

The design-science component evaluates the utility of the created artifacts. The comparative component measures differences among manual, AI-only, and hybrid approaches.

---

## 9.2 Compared Approaches

### Manual Baseline

An engineer analyzes the requirement or source artifacts using the same output template.

### AI-Only Baseline

A language model receives the input and output schema but does not use the deterministic rule engine or hybrid correction pipeline.

### Hybrid Approach

The proposed system uses:

* AI-assisted interpretation where applicable.
* Structured models.
* Normalization.
* Schema validation.
* Deterministic architecture rules.
* Evidence linking.
* Scoring.
* Human review.

---

## 9.3 Evaluation Cases

Eight cases are planned.

| Case | Name                              | Primary Purpose        |
| ---- | --------------------------------- | ---------------------- |
| C1   | Simple Product API                | Basic extraction       |
| C2   | Customer Validation API           | Conditional validation |
| C3   | Wallet Transaction API            | Transaction behavior   |
| C4   | Identity Integration API          | External integration   |
| C5   | Legacy Order API                  | Modernization gaps     |
| C6   | Partially Modernized Discount API | Partial compliance     |
| C7   | Secure Layered Account API        | False-positive control |
| C8   | Incomplete Shipment Requirement   | Ambiguity handling     |

---

## 9.4 Minimum Evaluation

The minimum viable study includes:

* C1.
* C2.
* C5.
* C7.
* C8.

Planned executions:

* Five manual runs.
* Fifteen AI-only runs.
* Fifteen hybrid runs.
* Six deterministic reruns.

Planned total:

```text
41 executions
```

This number is a study plan, not a completed experiment count.

---

## 9.5 Ground Truth

Ground truth will be prepared before the final experiments.

Ground truth includes:

* Expected endpoints.
* Expected fields.
* Expected rules.
* Expected errors.
* Expected missing information.
* Expected source evidence.
* Expected rule outcomes.
* Expected gaps.
* Expected risks.
* Expected recommendations.
* Non-applicable rules.

Ground truth should be reviewed and frozen before execution.

---

## 9.6 Controlled Variables

Control where practical:

* Dataset version.
* Prompt version.
* Output schema.
* Model.
* Model parameters.
* Rule-set version.
* Target architecture.
* Comparison logic.
* Reviewer instructions.
* Execution environment.

---

## 9.7 Repetition

AI-only and hybrid experiments should be repeated to measure output consistency.

Each run should record:

* Run identifier.
* Start and completion time.
* Model configuration.
* Input checksum.
* Raw output.
* Normalized output.
* Validation result.
* Comparison result.
* Failure state.

---

# 10. Evaluation Metrics

## 10.1 Precision

```text
Precision = True Positives ÷ (True Positives + False Positives)
```

Precision measures how many generated findings are correct.

---

## 10.2 Recall

```text
Recall = True Positives ÷ (True Positives + False Negatives)
```

Recall measures how many expected findings were generated.

---

## 10.3 F1 Score

```text
F1 = 2 × Precision × Recall ÷ (Precision + Recall)
```

---

## 10.4 Structural Validity

Structural validity measures whether output:

* Parses correctly.
* Matches the required schema.
* Contains valid identifiers.
* Contains valid relationships.
* Uses supported values.
* Contains required properties.

---

## 10.5 Defect-Detection Rate

Intentional defects include:

* Missing required field.
* Unsupported HTTP method.
* Duplicate identifier.
* Unknown rule target.
* Broken evidence link.
* Recommendation without finding.
* Task without acceptance criteria.
* Circular dependency.

---

## 10.6 Traceability Coverage

```text
Traceability Coverage =
Findings with Valid Evidence Links
÷
Total Findings
```

---

## 10.7 Evidence Accuracy

```text
Evidence Accuracy =
Supporting Evidence Links
÷
Reviewed Evidence Links
```

---

## 10.8 Repeated-Run Consistency

Use normalized overlap metrics such as Jaccard similarity for:

* Endpoints.
* Requirements.
* Gaps.
* Risks.
* Recommendations.

Also measure score variance.

---

## 10.9 Workflow Time

Measure:

* Input preparation.
* Machine execution.
* Human review.
* Human correction.
* Total completion time.

---

## 10.10 Human Review

Reviewers score:

* Correctness.
* Completeness.
* Explainability.
* Evidence quality.
* Recommendation actionability.
* Migration-task quality.
* Confidence.

A five-point Likert scale is planned.

---

# 11. Experimental Procedure

## 11.1 Dataset Preparation

1. Create synthetic requirement cases.
2. Create synthetic source-project artifacts.
3. Remove confidential information.
4. Create manifests.
5. Create ground truth.
6. Review ground truth.
7. Freeze dataset and ground-truth versions.

---

## 11.2 Pilot Study

C1 will be used as a pilot.

The pilot will validate:

* Output schemas.
* Matching rules.
* Timing collection.
* Failure recording.
* Reviewer package generation.

Comparison logic will be frozen after the pilot.

---

## 11.3 Main Execution

For every selected case:

1. Run the manual baseline.
2. Run the AI-only baseline three times.
3. Run the hybrid approach three times.
4. Preserve raw outputs.
5. Validate structure.
6. Compare with ground truth.
7. Generate blind review packages.
8. Collect human ratings.
9. Aggregate results.

---

## 11.4 Failure Handling

Failed runs must be preserved.

Failure categories include:

* Timeout.
* Empty output.
* Invalid JSON.
* Schema validation failure.
* Rule-engine failure.
* Evidence extraction failure.
* Orchestration failure.
* Review incomplete.

Poor-performing runs must not be excluded solely because they reduce the average.

---

# 12. Results

## 12.1 Dataset Summary

**Status:** Results pending.

Planned table:

| Measure               | Value |
| --------------------- | ----: |
| Cases                 |   TBD |
| Endpoints             |   TBD |
| Expected requirements |   TBD |
| Architecture rules    |   TBD |
| Expected gaps         |   TBD |
| Expected risks        |   TBD |
| Total runs            |   TBD |
| Human reviewers       |   TBD |

---

## 12.2 Requirement Extraction Results

**Status:** Results pending.

Planned table:

| Case | Approach | Precision | Recall |  F1 | Structural Validity |
| ---- | -------- | --------: | -----: | --: | ------------------: |
| C1   | Manual   |       TBD |    TBD | TBD |                 TBD |
| C1   | AI-only  |       TBD |    TBD | TBD |                 TBD |
| C1   | Hybrid   |       TBD |    TBD | TBD |                 TBD |
| C2   | Manual   |       TBD |    TBD | TBD |                 TBD |
| C2   | AI-only  |       TBD |    TBD | TBD |                 TBD |
| C2   | Hybrid   |       TBD |    TBD | TBD |                 TBD |

---

## 12.3 Validation Results

**Status:** Results pending.

| Defect Category         | Injected | Detected | Missed | Detection Rate |
| ----------------------- | -------: | -------: | -----: | -------------: |
| Missing required fields |      TBD |      TBD |    TBD |            TBD |
| Unsupported values      |      TBD |      TBD |    TBD |            TBD |
| Duplicate IDs           |      TBD |      TBD |    TBD |            TBD |
| Invalid relationships   |      TBD |      TBD |    TBD |            TBD |
| Broken evidence links   |      TBD |      TBD |    TBD |            TBD |
| Incomplete tasks        |      TBD |      TBD |    TBD |            TBD |

---

## 12.4 Migration Gap and Risk Results

**Status:** Results pending.

| Case | Approach | Gap F1 | Risk F1 | False Positives |
| ---- | -------- | -----: | ------: | --------------: |
| C5   | AI-only  |    TBD |     TBD |             TBD |
| C5   | Hybrid   |    TBD |     TBD |             TBD |
| C6   | AI-only  |    TBD |     TBD |             TBD |
| C6   | Hybrid   |    TBD |     TBD |             TBD |
| C7   | AI-only  |    TBD |     TBD |             TBD |
| C7   | Hybrid   |    TBD |     TBD |             TBD |

---

## 12.5 Traceability Results

**Status:** Results pending.

| Approach | Findings | Valid Evidence Links | Coverage | Evidence Accuracy |
| -------- | -------: | -------------------: | -------: | ----------------: |
| Manual   |      TBD |                  TBD |      TBD |               TBD |
| AI-only  |      TBD |                  TBD |      TBD |               TBD |
| Hybrid   |      TBD |                  TBD |      TBD |               TBD |

---

## 12.6 Repeated-Run Consistency

**Status:** Results pending.

| Approach | Endpoint Overlap | Gap Overlap | Risk Overlap | Score Variance |
| -------- | ---------------: | ----------: | -----------: | -------------: |
| AI-only  |              TBD |         TBD |          TBD |            TBD |
| Hybrid   |              TBD |         TBD |          TBD |            TBD |

---

## 12.7 Workflow Time

**Status:** Results pending.

| Approach | Preparation | Execution | Review | Correction | Total |
| -------- | ----------: | --------: | -----: | ---------: | ----: |
| Manual   |         TBD |       N/A |    TBD |        TBD |   TBD |
| AI-only  |         TBD |       TBD |    TBD |        TBD |   TBD |
| Hybrid   |         TBD |       TBD |    TBD |        TBD |   TBD |

---

## 12.8 Human Review Results

**Status:** Results pending.

| Approach | Correctness | Completeness | Explainability | Actionability | Confidence |
| -------- | ----------: | -----------: | -------------: | ------------: | ---------: |
| Manual   |         TBD |          TBD |            TBD |           TBD |        TBD |
| AI-only  |         TBD |          TBD |            TBD |           TBD |        TBD |
| Hybrid   |         TBD |          TBD |            TBD |           TBD |        TBD |

---

# 13. Discussion

The final discussion will interpret results rather than merely restate tables.

## 13.1 Requirement Extraction

The analysis will examine whether the hybrid approach:

* Improves structural validity.
* Reduces unsupported fields.
* Detects missing information.
* Preserves requirement traceability.
* Requires less correction than AI-only output.

**Discussion status:** Pending.

---

## 13.2 Migration Analysis

The analysis will examine:

* Gap precision.
* Gap recall.
* Risk accuracy.
* False positives on the well-structured case.
* Handling of partially implemented capabilities.
* Recommendation quality.
* Task completeness.

**Discussion status:** Pending.

---

## 13.3 Explainability

The final discussion will evaluate whether evidence links help reviewers:

* Understand findings.
* Verify source support.
* Reject unsupported conclusions.
* Adjust severity.
* Trust readiness scores.

**Discussion status:** Pending.

---

## 13.4 Efficiency

Execution speed alone is not sufficient.

The study will consider whether:

* AI-only output is fast but correction-heavy.
* Manual analysis is accurate but time-consuming.
* Hybrid analysis balances automation and review effort.

**Discussion status:** Pending.

---

## 13.5 Determinism and Consistency

The final analysis will distinguish:

* Variability in AI-assisted interpretation.
* Stability in deterministic downstream processing.
* Differences in final findings across repeated runs.
* Score variation.

**Discussion status:** Pending.

---

# 14. Threats to Validity

## 14.1 Construct Validity

The selected metrics may not fully represent engineering quality.

For example:

* Precision and recall may not reflect finding severity.
* Traceability coverage may not reflect evidence usefulness.
* Time reduction may not reflect cognitive effort.
* Readiness score agreement may depend on the expert scoring model.

Mitigation:

* Use multiple metrics.
* Include human review.
* Publish metric definitions.
* Report category-level results.

---

## 14.2 Internal Validity

The research team creates the prototype, dataset, rule sets, and ground truth, which may introduce bias.

Mitigation:

* Freeze ground truth before experiments.
* Use secondary review.
* Preserve raw outputs.
* Blind reviewers to approach labels where practical.
* Publish matching rules.
* Preserve failed runs.

---

## 14.3 External Validity

The evaluation may use a small number of synthetic API cases and selected technologies.

The results may not generalize to:

* Every programming language.
* Every framework.
* Every architecture.
* Large enterprise repositories.
* Runtime-generated routes.
* Highly specialized business domains.

Mitigation:

* Include diverse cases.
* State the tested scope.
* Avoid universal claims.
* Plan future real-world evaluation.

---

## 14.4 Conclusion Validity

A limited number of cases and reviewers may reduce statistical power.

Mitigation:

* Report sample size.
* Report raw values.
* Use repeated runs.
* Avoid unsupported significance claims.
* Report variance and ranges.

---

## 14.5 Ground-Truth Bias

Architecture expectations may reflect the selected reference architecture rather than universal best practice.

Mitigation:

* Publish target rules.
* Mark non-applicable rules.
* Permit rule customization.
* Obtain expert review.
* Interpret readiness as architecture-specific.

---

## 14.6 Model Dependence

AI performance may depend on:

* Model version.
* Prompt wording.
* Temperature.
* Output limits.
* Provider changes.

Mitigation:

* Record model configuration.
* Version prompts.
* Preserve raw outputs.
* Repeat runs.
* Avoid claiming model-independent performance.

---

# 15. Limitations

## 15.1 Structured Artifact Input

The Migration Analyzer currently analyzes supplied source artifacts rather than automatically parsing every file in arbitrary repositories.

---

## 15.2 Limited Language Support

The current prototype does not provide comprehensive AST-based support across multiple programming languages.

---

## 15.3 Static Rather Than Runtime Analysis

The system does not analyze:

* Runtime traces.
* Production traffic.
* Dynamic routing.
* Reflection.
* Runtime configuration.
* Performance telemetry.

---

## 15.4 Reference Architecture Dependence

Results depend on configured rules, weights, severity values, and evidence expectations.

---

## 15.5 Human Review Requirement

The platform does not autonomously approve API requirements or migration decisions.

---

## 15.6 Incomplete Browser Automation

Playwright browser validation may remain incomplete or skipped in the final prototype validation.

---

## 15.7 No Production Collaboration Features

The prototype does not yet include:

* User authentication.
* Persistent project storage.
* Role-based access.
* Multi-user collaboration.
* Approval history.
* Audit logs.
* Enterprise deployment.

---

## 15.8 Evaluation Pending

No research hypothesis should be considered supported until the experiments are completed.

---

# 16. Ethical, Security, and Confidentiality Considerations

The evaluation should use synthetic or anonymized cases.

The project should not expose:

* Employer-owned source code.
* Proprietary requirements.
* Customer data.
* Employee data.
* Production credentials.
* Private endpoints.
* Internal architecture documents.

AI-assisted processing should not receive confidential source material without appropriate approval, provider controls, and data-handling policies.

Generated findings should be treated as advisory. Incorrect security, architecture, or migration recommendations could create operational risk if implemented without expert review.

---

# 17. Future Work

## 17.1 Complete Empirical Evaluation

Implement the evaluation cases, freeze ground truth, execute all baselines, and calculate final metrics.

---

## 17.2 Complete Literature Review

Search academic databases, review selected papers, verify the research gap, and create the final bibliography.

---

## 17.3 Full Repository Ingestion

Add:

* Repository upload.
* Git integration.
* File discovery.
* Language detection.
* Artifact classification.
* Ignore rules.

---

## 17.4 AST-Based Analysis

Implement language-specific analyzers for:

* TypeScript and JavaScript.
* Go.
* Java.
* Python.

---

## 17.5 Runtime Evidence

Add optional support for:

* API traffic.
* Distributed traces.
* Logs.
* Coverage data.
* Performance metrics.
* Dependency telemetry.

---

## 17.6 Custom Architecture Rules

Provide an interface for organizations to define:

* Categories.
* Rules.
* Severities.
* Weights.
* Evidence requirements.
* Scoring thresholds.

---

## 17.7 Collaborative Review

Add:

* User accounts.
* Comments.
* Finding acceptance.
* Finding rejection.
* Severity overrides.
* Approval workflows.
* Version history.

---

## 17.8 Migration Backlog Export

Export migration tasks to:

* Jira.
* GitHub Issues.
* GitLab Issues.
* Azure DevOps.
* CSV.
* Markdown.

---

## 17.9 Continuous Architecture Monitoring

Integrate the analyzer into CI/CD to identify architecture drift over time.

---

## 17.10 Recommendation Validation

Evaluate whether generated recommendations:

* Resolve the linked finding.
* Preserve behavior.
* Reduce risk.
* Improve target architecture compliance.

---

## 17.11 Broader Human Study

Recruit architects and senior engineers from multiple organizations to assess:

* Explanation quality.
* Trust.
* Correction effort.
* Architecture-rule usefulness.
* Task actionability.

---

# 18. Conclusion

Enterprise API modernization requires reliable interpretation of incomplete requirements and explainable analysis of existing implementations. Artificial intelligence can assist with unstructured inputs, but generated output alone may not provide the structural validity, traceability, consistency, or governance required for enterprise engineering decisions.

This paper proposes a hybrid architecture combining AI-assisted interpretation with deterministic requirement models, validation, source evidence, architecture rules, scoring, and human review. The architecture is implemented through two connected prototypes: an API Factory for requirement structuring and an API Migration Analyzer for evidence-driven modernization planning.

The current work establishes the technical design, prototype workflow, research questions, evaluation methodology, dataset plan, metrics, and publication structure. The empirical study remains pending. Final conclusions regarding accuracy, traceability, consistency, efficiency, and reviewer confidence will be reported only after the manual, AI-only, and hybrid approaches are evaluated against frozen ground truth.

---

# 19. Planned Figures

## Figure 1

End-to-End Hybrid API Modernization Architecture.

## Figure 2

API Factory Requirement Processing Workflow.

## Figure 3

API Migration Analyzer Workflow.

## Figure 4

Shared Research Domain Model.

## Figure 5

Explainable Architecture Scoring Process.

## Figure 6

Experimental Evaluation Design.

## Figure 7

Human-in-the-Loop Review Workflow.

## Figure 8

Browser-Based Analysis Results Interface.

---

# 20. Planned Tables

## Table 1

Research Questions and Metrics.

## Table 2

Related-Work Capability Comparison.

## Table 3

Proposed Architecture Components.

## Table 4

Requirement Model.

## Table 5

Target Architecture Categories.

## Table 6

Evaluation Dataset.

## Table 7

Requirement Extraction Results.

## Table 8

Validation Defect Detection.

## Table 9

Migration Gap and Risk Results.

## Table 10

Traceability Results.

## Table 11

Repeated-Run Consistency.

## Table 12

Workflow Time.

## Table 13

Human Reviewer Results.

## Table 14

Research Question Summary.

## Table 15

Threats to Validity.

---

# 21. References

The final bibliography will contain approximately 30–45 verified sources covering:

* AI-assisted requirements engineering.
* LLMs for software engineering.
* API and code generation.
* Model-driven API development.
* Static analysis.
* Architecture conformance.
* Legacy modernization.
* Explainable AI.
* Human-in-the-loop engineering.
* Relevant standards.

```text
[1] TBD
[2] TBD
[3] TBD
...
```

No unverified reference should be included in the submission version.

---

# 22. Appendix A – Planned Evaluation Cases

## C1 – Simple Product API

Tests basic requirement extraction and false-positive control.

## C2 – Customer Validation API

Tests conditional and complex validation rules.

## C3 – Wallet Transaction API

Tests transactions, status transitions, and data integrity.

## C4 – Identity Integration API

Tests external-client, timeout, tracing, and resilience analysis.

## C5 – Legacy Order API

Tests architecture-gap and modernization-risk detection.

## C6 – Partially Modernized Discount API

Tests partial compliance and targeted recommendations.

## C7 – Secure Layered Account API

Tests false-positive control and recognition of existing capabilities.

## C8 – Incomplete Shipment Requirement

Tests ambiguity, contradiction, and missing-information handling.

---

# 23. Appendix B – Planned Result Acceptance Targets

These are study targets, not achieved results.

| Metric                         | Initial Target |
| ------------------------------ | -------------: |
| Requirement precision          |            85% |
| Requirement recall             |            85% |
| Hybrid structural validity     |            95% |
| Injected defect detection      |            90% |
| Evidence accuracy              |            90% |
| Gap precision                  |            85% |
| Gap recall                     |            80% |
| Risk precision                 |            80% |
| Published finding traceability |           100% |
| Key module test coverage       |            80% |
| Workflow completion            |            95% |
| Manual time reduction          |            50% |

Failure to meet a target must be reported rather than hidden.

---

# 24. Appendix C – Research Artifact Structure

```text
evaluation/
├── datasets/
├── ground-truth/
├── prompts/
├── runs/
├── reviews/
├── results/
└── scripts/
```

Each run should preserve:

* Metadata.
* Input.
* Raw output.
* Normalized output.
* Validation.
* Analysis result.
* Comparison.
* Review summary.

---

# 25. Phase 5 Completion Checklist

* [x] Created the working title.
* [x] Created the abstract placeholder.
* [x] Drafted the introduction.
* [x] Drafted background and motivation.
* [x] Created the related-work structure.
* [x] Added the provisional research gap.
* [x] Added research questions.
* [x] Added hypotheses.
* [x] Drafted the proposed architecture.
* [x] Drafted the API Factory design.
* [x] Drafted the Migration Analyzer design.
* [x] Drafted the scoring model.
* [x] Added the research methodology.
* [x] Added evaluation cases.
* [x] Added evaluation metrics.
* [x] Added the experimental procedure.
* [x] Added result placeholders.
* [x] Added the discussion structure.
* [x] Added threats to validity.
* [x] Added limitations.
* [x] Added ethical and confidentiality considerations.
* [x] Added future work.
* [x] Added a provisional conclusion.
* [x] Added planned figures.
* [x] Added planned tables.
* [x] Added the bibliography placeholder.
* [x] Added evaluation appendices.
* [x] Preserved the distinction between planned and achieved results.

---

# 26. Phase 5 Completion Status

**Status:** Initial research manuscript completed

**Publication readiness:** Not yet ready for submission

**Remaining requirements:**

* Literature review.
* Verified citations.
* Final repository validation.
* Evaluation dataset implementation.
* Experiment execution.
* Human review.
* Results and statistical analysis.
* Final figures and tables.
* Abstract revision.
* Conclusion revision.
* Venue formatting.
* Proofreading.

**Recommended file:**

```text
docs/day50/research_paper_initial_draft.md
```

---

# 27. Next Phase

**Day 50 Phase 6 – Publication and Evaluation Execution Plan**

Phase 6 will define:

* Post-roadmap research schedule.
* Dataset implementation order.
* Ground-truth review.
* Experiment execution.
* Human reviewer recruitment.
* Literature review.
* Result analysis.
* Chart generation.
* Paper revision.
* Venue selection.
* Preprint and submission preparation.
