# Day 49 – Phase 6

# Related-Work Research, Literature Review, and Citation Management Plan

## Project

**AI-Assisted API Factory and Migration Analyzer**

## Research Paper Working Title

**AI-Assisted API Modernization: A Rule-Based Architecture for Requirement Extraction, API Generation, and Migration Analysis**

---

# 1. Phase 6 Objective

The objective of Day 49 Phase 6 is to define a systematic strategy for finding, evaluating, organizing, and citing academic literature relevant to the proposed research.

This phase establishes:

* Related-work research areas.
* Literature-search databases.
* Search queries and keyword combinations.
* Paper inclusion and exclusion criteria.
* Source-quality requirements.
* Paper-screening procedures.
* Citation-note templates.
* Research-gap comparison methods.
* Bibliography organization.
* Citation-verification controls.
* Related-work writing structure.
* Literature-review completion criteria.

This phase defines the literature-review process. It does not claim that specific papers have already been reviewed or cited.

---

# 2. Phase 6 Deliverables

The recommended research-document structure is:

```text
docs/
└── day49/
    ├── research_question_and_contribution.md
    ├── research_paper_structure_and_outline.md
    ├── research_methodology.md
    ├── evaluation_dataset_and_templates.md
    ├── publication_assets_plan.md
    ├── related_work_research_plan.md
    └── literature/
        ├── search_log.md
        ├── candidate_papers.csv
        ├── selected_papers.csv
        ├── excluded_papers.csv
        ├── citation_notes/
        ├── comparison_matrix.csv
        ├── references.bib
        └── citation_verification.md
```

The primary Phase 6 document should be stored at:

```text
docs/day49/related_work_research_plan.md
```

---

# 3. Purpose of the Related-Work Section

The related-work section must establish:

1. What prior research has already addressed.
2. Which technical approaches have been used.
3. What limitations remain.
4. How the proposed system differs.
5. Why the proposed research contribution is necessary.
6. Which ideas or methods the proposed system builds upon.
7. Which claims must be supported through citations.

The section should not be a list of paper summaries.

It should compare research approaches and organize the literature around the capabilities relevant to the proposed system.

---

# 4. Primary Related-Work Research Areas

The literature review should cover eight major research areas.

| Area ID | Research Area                                      | Relevance                                              |
| ------- | -------------------------------------------------- | ------------------------------------------------------ |
| RW1     | AI-assisted requirements engineering               | Supports requirement extraction and ambiguity analysis |
| RW2     | Large language models for software engineering     | Supports AI-assisted generation and analysis           |
| RW3     | AI-assisted code and API generation                | Supports structured artifact generation                |
| RW4     | Model-driven and schema-first API development      | Supports deterministic API specifications              |
| RW5     | Static analysis and architecture conformance       | Supports source evidence and rule evaluation           |
| RW6     | Legacy modernization and cloud migration           | Supports migration analysis and recommendations        |
| RW7     | Explainable AI and evidence-backed developer tools | Supports traceability and explainable findings         |
| RW8     | Human-in-the-loop software engineering             | Supports review, correction, and governance            |

---

# 5. Research Area RW1 – AI-Assisted Requirements Engineering

## 5.1 Research Focus

This area should examine approaches that apply:

* Natural-language processing.
* Machine learning.
* Generative AI.
* Large language models.
* Knowledge graphs.
* Rule-based systems.

To software requirements engineering.

---

## 5.2 Topics to Investigate

Search for research involving:

* Requirement extraction from natural language.
* Requirement classification.
* Functional versus non-functional requirement detection.
* Requirement ambiguity detection.
* Requirement completeness analysis.
* Requirement conflict detection.
* Natural-language-to-UML transformation.
* Natural-language-to-API specification generation.
* Requirement traceability.
* Requirement normalization.
* Human review of generated requirements.

---

## 5.3 Questions to Answer

For each selected paper, determine:

* What input format does the approach accept?
* What requirement categories are extracted?
* Does it produce structured output?
* Does it detect missing information?
* Does it identify ambiguity?
* Is deterministic validation applied?
* Is the output traceable to source text?
* How is the approach evaluated?
* What datasets are used?
* What limitations are reported?

---

## 5.4 Example Search Queries

```text
"AI requirements engineering natural language extraction"

"large language model software requirement extraction"

"generative AI requirement specification generation"

"automatic API requirement extraction from natural language"

"NLP functional non-functional requirement classification"

"software requirement ambiguity detection machine learning"

"requirement traceability generative AI"

"natural language to structured software requirements"

"large language models requirements engineering evaluation"

"schema validation AI generated requirements"
```

---

# 6. Research Area RW2 – Large Language Models for Software Engineering

## 6.1 Research Focus

This area should examine the use of large language models for:

* Code generation.
* Code explanation.
* Program repair.
* Test generation.
* Documentation.
* Refactoring.
* Architecture assistance.
* Software analysis.

---

## 6.2 Topics to Investigate

Include studies on:

* Code-generation correctness.
* Hallucinations in generated code.
* Security weaknesses in generated code.
* Reproducibility of AI-generated software artifacts.
* Prompt sensitivity.
* Output variability.
* Human productivity with coding assistants.
* Developer trust.
* AI-assisted software-engineering evaluation benchmarks.
* Limitations of AI-only workflows.

---

## 6.3 Questions to Answer

For each paper, determine:

* Which model or model family was studied?
* Which software-engineering task was evaluated?
* How was correctness measured?
* Was the result executable or only syntactically valid?
* Were hallucinations measured?
* Was output consistency measured?
* Was human review required?
* Were security issues evaluated?
* What risks were identified?
* What governance or validation mechanisms were proposed?

---

## 6.4 Example Search Queries

```text
"large language models software engineering systematic review"

"LLM code generation correctness empirical study"

"AI generated code hallucination software engineering"

"security vulnerabilities AI generated code study"

"large language models code generation reproducibility"

"developer productivity generative AI coding assistants"

"human trust AI software engineering tools"

"LLM software architecture design evaluation"

"prompt sensitivity code generation study"

"deterministic validation of LLM generated code"
```

---

# 7. Research Area RW3 – AI-Assisted Code and API Generation

## 7.1 Research Focus

This area should examine systems that generate:

* API contracts.
* OpenAPI specifications.
* Server stubs.
* Data models.
* Validation code.
* Tests.
* Documentation.
* API workflows.

---

## 7.2 Topics to Investigate

Search for:

* Natural-language-to-OpenAPI generation.
* API code generation from descriptions.
* Automated REST API generation.
* AI-assisted microservice generation.
* API test generation.
* API documentation generation.
* Contract-first code generation.
* Low-code API creation.
* Generated API validation.
* API specification consistency.

---

## 7.3 Questions to Answer

Determine:

* Does the approach generate an API contract or implementation?
* Is a structured intermediate representation used?
* Is schema validation applied?
* Are generated endpoints traceable to source requirements?
* Are business rules represented?
* Are tests generated?
* Does the system support human correction?
* How is correctness evaluated?
* Is the system limited to simple CRUD APIs?
* Are enterprise concerns such as security and transactions represented?

---

## 7.4 Example Search Queries

```text
"natural language to OpenAPI specification generation"

"AI assisted REST API generation"

"large language model API specification generation"

"automated API code generation from requirements"

"generative AI microservice generation"

"OpenAPI code generation research"

"API contract generation natural language processing"

"AI generated API validation"

"LLM API test generation"

"model driven REST API development"
```

---

# 8. Research Area RW4 – Model-Driven and Schema-First API Development

## 8.1 Research Focus

This area provides the deterministic engineering foundation for the proposed hybrid architecture.

It should examine:

* Model-driven engineering.
* Schema-first development.
* Contract-first API development.
* Domain-specific languages.
* Interface definition languages.
* OpenAPI-based generation.
* Constraint validation.
* Code generation from formal models.

---

## 8.2 Topics to Investigate

Search for research on:

* API model transformation.
* Model-driven API engineering.
* OpenAPI contract validation.
* Schema evolution.
* API consistency checking.
* Domain-model-to-service generation.
* Formal API specification.
* Constraint-based validation.
* Round-trip engineering.
* Model-to-code traceability.

---

## 8.3 Questions to Answer

Determine:

* What structured model is used?
* How are constraints represented?
* How is model correctness checked?
* Can models generate code or documentation?
* How are changes propagated?
* Is source traceability preserved?
* What enterprise concerns are represented?
* How does the approach handle incomplete requirements?
* Does it include legacy source analysis?
* Does it integrate generative AI?

---

## 8.4 Example Search Queries

```text
"model driven API development research"

"schema first REST API development"

"OpenAPI specification validation research"

"model driven microservice generation"

"contract first API engineering"

"API model transformation software engineering"

"formal REST API specification"

"OpenAPI consistency checking"

"domain specific language API generation"

"model to code traceability API"
```

---

# 9. Research Area RW5 – Static Analysis and Architecture Conformance

## 9.1 Research Focus

This area should examine techniques that analyze source code to detect architecture structure, implementation characteristics, and violations.

---

## 9.2 Topics to Investigate

Include:

* Static code analysis.
* Architecture recovery.
* Architecture reconstruction.
* Architecture conformance checking.
* Design-rule enforcement.
* Dependency analysis.
* Code-smell detection.
* Technical-debt analysis.
* Layer violation detection.
* Software quality metrics.

---

## 9.3 Questions to Answer

For each approach, determine:

* What source artifacts are analyzed?
* Is an abstract syntax tree used?
* Which languages are supported?
* Can endpoints and routes be identified?
* Can architecture layers be recovered?
* How are expected architecture rules represented?
* Does the tool provide evidence?
* Are findings explainable?
* Can false positives be reviewed?
* Does the approach generate remediation tasks?

---

## 9.4 Example Search Queries

```text
"software architecture conformance checking static analysis"

"architecture recovery source code analysis"

"layered architecture violation detection"

"static analysis microservice architecture"

"software architecture reconstruction research"

"architecture technical debt detection"

"design rule checking source code"

"evidence based static code analysis findings"

"API endpoint detection static analysis"

"automated architecture assessment software systems"
```

---

# 10. Research Area RW6 – Legacy Modernization and Cloud Migration

## 10.1 Research Focus

This area should examine methods and tools for modernizing:

* Monolithic applications.
* Legacy APIs.
* Service-oriented systems.
* On-premises applications.
* Existing data-access layers.
* Enterprise integration systems.

---

## 10.2 Topics to Investigate

Search for:

* Legacy-system modernization.
* Cloud-readiness assessment.
* Monolith-to-microservice migration.
* API modernization.
* Automated modernization planning.
* Migration-pattern recommendation.
* Strangler-pattern implementation.
* Technical-debt prioritization.
* Modernization risk analysis.
* Migration task planning.

---

## 10.3 Questions to Answer

Determine:

* What type of legacy system is evaluated?
* How is modernization readiness measured?
* Are target architectures defined?
* Are gaps and risks identified?
* Is source evidence provided?
* Are recommendations prioritized?
* Are migration tasks generated?
* Is human review included?
* Are runtime dependencies analyzed?
* How is the approach evaluated?

---

## 10.4 Example Search Queries

```text
"legacy API modernization research"

"automated software modernization assessment"

"cloud readiness assessment source code"

"monolith to microservices migration analysis"

"API migration planning automation"

"legacy system modernization recommendation engine"

"software modernization risk assessment"

"architecture migration gap analysis"

"technical debt prioritization legacy modernization"

"evidence based cloud migration assessment"
```

---

# 11. Research Area RW7 – Explainable AI and Evidence-Backed Developer Tools

## 11.1 Research Focus

This area should examine how software-engineering tools explain:

* Generated outputs.
* Static-analysis warnings.
* Architecture findings.
* Model decisions.
* Recommended changes.

---

## 11.2 Topics to Investigate

Include:

* Explainable AI for developers.
* Explainable code recommendations.
* Evidence-backed static analysis.
* Traceability in AI systems.
* Confidence scoring.
* Source attribution.
* Explainable program repair.
* Developer trust in AI assistants.
* Explanation usefulness.
* Verification effort.

---

## 11.3 Questions to Answer

Determine:

* What form of explanation is provided?
* Does the explanation link to source code?
* Can the user inspect supporting evidence?
* Is confidence communicated?
* Can explanations be challenged or corrected?
* Is explainability evaluated with users?
* Does evidence improve reviewer trust?
* Is the explanation generated or rule-based?
* Can the reasoning be reproduced?
* Does the system distinguish evidence from inference?

---

## 11.4 Example Search Queries

```text
"explainable AI software engineering tools"

"explainable code recommendation systems"

"evidence based static analysis explanations"

"source attribution large language models code"

"developer trust explainable AI coding assistant"

"traceability AI generated software artifacts"

"explainable program repair"

"human evaluation explanations software engineering"

"confidence scoring AI code generation"

"evidence linked architecture analysis"
```

---

# 12. Research Area RW8 – Human-in-the-Loop Software Engineering

## 12.1 Research Focus

This area should examine systems where AI or automated analysis supports engineers but does not independently make final engineering decisions.

---

## 12.2 Topics to Investigate

Include:

* Human-AI collaboration.
* Interactive program synthesis.
* Human review of generated code.
* Developer feedback loops.
* AI-assisted decision support.
* Mixed-initiative software engineering.
* Human validation of static-analysis findings.
* Governance of generative AI.
* Review effort.
* Correction workflows.

---

## 12.3 Questions to Answer

Determine:

* At what stages do humans participate?
* What can users accept, reject, or correct?
* Does feedback improve future outputs?
* How is reviewer effort measured?
* Does the system reduce or increase cognitive load?
* Are final decisions auditable?
* Does the workflow preserve accountability?
* Are reviewers shown evidence?
* Is blind evaluation used?
* How is user trust measured?

---

## 12.4 Example Search Queries

```text
"human in the loop software engineering AI"

"human AI collaboration code generation"

"developer review AI generated code empirical study"

"mixed initiative software engineering"

"human validation static analysis findings"

"interactive program synthesis human feedback"

"AI assisted software engineering governance"

"human oversight generative AI development"

"review effort AI generated software artifacts"

"developer confidence AI recommendations"
```

---

# 13. Literature Databases and Search Sources

The literature search should prioritize academic databases and primary research sources.

## 13.1 Primary Databases

Use:

* IEEE Xplore.
* ACM Digital Library.
* SpringerLink.
* ScienceDirect.
* Scopus.
* Web of Science.
* Google Scholar.
* arXiv for recent preprints.
* DBLP for publication metadata.
* Semantic Scholar for citation exploration.

---

## 13.2 Preferred Source Types

Priority order:

1. Peer-reviewed journal articles.
2. Peer-reviewed conference papers.
3. Peer-reviewed workshop papers.
4. Systematic literature reviews.
5. Technical standards and specifications.
6. Doctoral dissertations where directly relevant.
7. High-quality preprints for emerging topics.
8. Official technical documentation for implementation context.

Vendor blogs and marketing materials should not serve as primary evidence for research claims.

---

## 13.3 Primary Versus Secondary Sources

Prefer primary sources when discussing:

* A proposed algorithm.
* A system architecture.
* An empirical experiment.
* A benchmark.
* A dataset.
* A measured result.

Use systematic reviews and surveys to:

* Identify research themes.
* Find foundational papers.
* Understand field-wide limitations.
* Discover terminology.

---

# 14. Search Period

Because generative AI research evolves rapidly, the literature review should include:

* Foundational work from earlier periods.
* Recent work from the last five years.
* Very recent LLM and software-engineering studies.
* Current preprints only when peer-reviewed alternatives are unavailable.

The final paper should document the literature-search completion date.

---

# 15. Search Query Construction

## 15.1 Query Components

Build queries by combining terms from four groups.

### Technology Terms

```text
"artificial intelligence"
"generative AI"
"large language model"
"machine learning"
"natural language processing"
"rule based"
"static analysis"
```

### Software-Engineering Terms

```text
"requirements engineering"
"code generation"
"API development"
"software architecture"
"software modernization"
"legacy migration"
"developer tools"
```

### Capability Terms

```text
"extraction"
"validation"
"traceability"
"explainability"
"evidence"
"recommendation"
"conformance"
"assessment"
```

### Evaluation Terms

```text
"empirical evaluation"
"precision recall"
"human study"
"case study"
"benchmark"
"experiment"
"systematic review"
```

---

## 15.2 Example Combined Query

```text
("large language model" OR "generative AI")
AND
("requirements engineering" OR "API specification")
AND
("validation" OR "traceability")
AND
("empirical evaluation" OR "case study")
```

---

## 15.3 Static Analysis Query

```text
("static analysis" OR "architecture recovery")
AND
("API" OR "microservice" OR "legacy system")
AND
("architecture conformance" OR "modernization")
AND
("evidence" OR "recommendation")
```

---

## 15.4 Modernization Query

```text
("legacy modernization" OR "cloud migration")
AND
("source code analysis" OR "architecture assessment")
AND
("gap analysis" OR "risk analysis")
AND
("automation" OR "decision support")
```

---

# 16. Search Log

Every literature search should be recorded.

Recommended file:

```text
docs/day49/literature/search_log.md
```

Template:

```md
# Literature Search Log

## Search Entry

- Search ID:
- Date:
- Database:
- Query:
- Filters:
- Results returned:
- Papers reviewed by title:
- Papers reviewed by abstract:
- Papers selected:
- Notes:
```

---

## 16.1 Search Log Example

```md
## Search Entry

- Search ID: SEARCH-RW1-001
- Date: TBD
- Database: IEEE Xplore
- Query: "large language model" AND "requirements engineering"
- Filters: English, journal or conference, 2020–present
- Results returned: TBD
- Papers reviewed by title: TBD
- Papers reviewed by abstract: TBD
- Papers selected: TBD
- Notes: Focused on structured requirement extraction and evaluation.
```

---

# 17. Paper Collection Workflow

Use the following screening stages.

## Stage 1 – Discovery

Collect papers based on:

* Search queries.
* Survey references.
* Citation networks.
* Related-paper recommendations.
* Author publication lists.
* Research-group repositories.

## Stage 2 – Title Screening

Remove papers clearly unrelated to:

* Software engineering.
* API development.
* Requirements.
* Static analysis.
* Modernization.
* Explainability.
* Human-AI collaboration.

## Stage 3 – Abstract Screening

Review:

* Research problem.
* Proposed method.
* Evaluation.
* Relevance to research questions.

## Stage 4 – Full-Text Review

Read the complete paper and record:

* Contribution.
* Methodology.
* Dataset.
* Evaluation.
* Results.
* Limitations.
* Relevance.
* Comparison with the proposed system.

## Stage 5 – Citation Network Review

Inspect:

* Important references cited by the paper.
* Later papers that cite the paper.
* Related work from the same authors.

---

# 18. Paper Inclusion Criteria

A paper may be included when it meets one or more of the following:

* Directly addresses one of the eight related-work areas.
* Proposes a relevant technical method.
* Provides an empirical evaluation.
* Introduces a relevant dataset or benchmark.
* Studies human interaction with AI software tools.
* Evaluates AI-generated software reliability.
* Discusses evidence, traceability, or explainability.
* Presents architecture-conformance or modernization analysis.
* Is foundational to a method used in the proposed system.
* Helps define a research gap.

---

# 19. Paper Exclusion Criteria

Exclude papers when:

* The full text is unavailable.
* The paper is unrelated to software engineering.
* It makes unsupported claims without methodology.
* It is primarily promotional.
* It duplicates another version of the same work.
* It contains insufficient detail to evaluate.
* It discusses generic AI without connection to the research.
* It focuses only on user-interface design without relevant engineering analysis.
* It is superseded by a complete peer-reviewed version.
* Its findings cannot be reliably verified.

Do not exclude a paper only because its findings conflict with the proposed research.

---

# 20. Duplicate Paper Handling

The same research may appear as:

* Preprint.
* Workshop paper.
* Conference paper.
* Journal extension.
* Technical report.

Prefer the most complete peer-reviewed version.

Record relationships between versions.

Example:

```text
Paper A – arXiv preprint
Paper B – conference version
Paper C – extended journal version
```

Cite the version that best supports the specific claim.

---

# 21. Candidate Paper Tracking

Recommended file:

```text
docs/day49/literature/candidate_papers.csv
```

Columns:

```csv
paper_id,title,authors,year,venue,doi_or_identifier,research_area,source_database,screening_status,full_text_available,notes
```

Suggested screening statuses:

* `discovered`
* `title-approved`
* `abstract-approved`
* `full-text-review`
* `selected`
* `excluded`
* `duplicate`

---

# 22. Selected Paper Tracking

Recommended file:

```text
docs/day49/literature/selected_papers.csv
```

Columns:

```csv
paper_id,title,year,venue,research_area,method,dataset,evaluation,relevance,quality_score,citation_key,review_complete
```

---

# 23. Excluded Paper Tracking

Recommended file:

```text
docs/day49/literature/excluded_papers.csv
```

Columns:

```csv
paper_id,title,year,screening_stage,exclusion_reason,notes
```

Suggested exclusion reasons:

* `not-relevant`
* `no-full-text`
* `duplicate`
* `insufficient-methodology`
* `non-academic`
* `superseded-version`
* `outside-scope`

---

# 24. Paper Quality Assessment

Each selected paper should receive a quality review.

## 24.1 Quality Questions

Score each question from 0 to 2.

| Question                                 | Score |
| ---------------------------------------- | ----: |
| Is the research problem clearly defined? |   0–2 |
| Is the methodology clearly described?    |   0–2 |
| Is the evaluation appropriate?           |   0–2 |
| Is the dataset or evidence described?    |   0–2 |
| Are limitations discussed?               |   0–2 |
| Are conclusions supported by results?    |   0–2 |
| Is the venue or source credible?         |   0–2 |

Maximum score:

```text
14
```

---

## 24.2 Quality Interpretation

| Score | Interpretation      |
| ----: | ------------------- |
| 12–14 | Strong source       |
|  9–11 | Good source         |
|   6–8 | Usable with caution |
|   0–5 | Weak source         |

A low-quality paper may still be discussed when historically important or when it represents a relevant viewpoint, but its limitations must be acknowledged.

---

# 25. Paper Reading Template

Create one Markdown note per selected paper.

Recommended location:

```text
docs/day49/literature/citation_notes/<citation-key>.md
```

Template:

```md
# Paper Review

## Citation Key

TBD

## Full Citation

TBD

## Research Area

RW1–RW8

## Research Problem

What problem does the paper address?

## Proposed Approach

What method, architecture, model, or system is proposed?

## Inputs

What inputs does the approach process?

## Outputs

What outputs does the approach produce?

## Evaluation

- Dataset:
- Number of cases:
- Baseline:
- Metrics:
- Human reviewers:
- Repetitions:

## Main Results

Summarize only results supported by the paper.

## Strengths

List important strengths.

## Limitations

List limitations stated by the authors and limitations identified during review.

## Relevance to Proposed Research

Explain how the paper relates to the API Factory or Migration Analyzer.

## Similarities

List capabilities shared with the proposed system.

## Differences

List important differences.

## Research Gap Contribution

Explain how this paper helps establish the research gap.

## Claims Supported

List exact paper claims that may be supported by this citation.

## Important Pages or Sections

Record page numbers, section numbers, figures, or tables.

## Quotable Definitions

Store only short excerpts needed for accurate paraphrasing.

## Citation Verification Status

- [ ] Metadata verified.
- [ ] Full text reviewed.
- [ ] Claims verified.
- [ ] Citation added to bibliography.
```

---

# 26. Literature Comparison Matrix

Recommended file:

```text
docs/day49/literature/comparison_matrix.csv
```

Columns:

```csv
paper_id,requirement_extraction,structured_model,api_generation,legacy_analysis,deterministic_validation,evidence_traceability,target_architecture,explainable_scoring,recommendations,migration_tasks,human_review,evaluation_type
```

Use values such as:

* `yes`
* `partial`
* `no`
* `not-reported`

---

# 27. Related-Work Capability Matrix

The final paper should include a condensed comparison table.

| Approach        | Requirements | API Generation | Legacy Analysis | Deterministic Validation | Evidence | Scoring | Migration Tasks |
| --------------- | -----------: | -------------: | --------------: | -----------------------: | -------: | ------: | --------------: |
| Selected work A |          Yes |             No |              No |                  Partial |  Partial |      No |              No |
| Selected work B |           No |            Yes |         Partial |                       No |       No |      No |              No |
| Selected work C |           No |             No |             Yes |                      Yes |      Yes | Partial |         Partial |
| Proposed system |          Yes |            Yes |             Yes |                      Yes |      Yes |     Yes |             Yes |

The actual paper names and values must be assigned only after the papers are fully reviewed.

---

# 28. Literature Synthesis Method

The related-work section should synthesize findings across papers.

Avoid this structure:

```text
Paper A did this.
Paper B did this.
Paper C did this.
```

Prefer:

```text
Prior research has applied natural-language processing and large language
models to requirement extraction. These approaches commonly evaluate
classification or entity extraction accuracy, but many produce isolated
requirement artifacts without integrating source-code evidence or migration
analysis. Other work focuses on architecture recovery and modernization;
however, these systems generally begin with existing source code rather than
connecting modernization findings to structured business requirements.
```

The paragraph must then cite the relevant papers.

---

# 29. Related-Work Section Structure

The final related-work section should use the following structure.

## 29.1 AI for Requirements Engineering

Discuss:

* Requirement extraction.
* Classification.
* Ambiguity.
* Structured representation.
* Limitations.

## 29.2 Generative AI for Code and API Development

Discuss:

* Code generation.
* API generation.
* Test generation.
* Reliability concerns.
* Validation limitations.

## 29.3 Model-Driven and Schema-First Engineering

Discuss:

* Structured specifications.
* Contract-first development.
* Deterministic code generation.
* Traceability.

## 29.4 Static Analysis and Architecture Conformance

Discuss:

* Architecture recovery.
* Rule checking.
* Source evidence.
* Technical-debt detection.

## 29.5 Legacy Modernization and Migration Planning

Discuss:

* Readiness assessment.
* Gap analysis.
* Modernization recommendations.
* Migration strategies.

## 29.6 Explainability and Human Review

Discuss:

* Evidence-backed findings.
* Developer trust.
* Human correction.
* Governance.

## 29.7 Research Gap

Conclude by explaining the missing integration across these research areas.

---

# 30. Research Gap Framework

The research gap should be evaluated across the following dimensions.

## 30.1 Workflow Coverage

Does prior work cover:

* Requirement input.
* Structured extraction.
* Validation.
* API artifact generation.
* Legacy source analysis.
* Migration findings.
* Migration tasks.

## 30.2 Governance

Does prior work include:

* Human review.
* Evidence inspection.
* Correction workflow.
* Decision auditability.

## 30.3 Determinism

Does prior work provide:

* Schema validation.
* Rule validation.
* Reproducible downstream analysis.
* Explicit scoring.

## 30.4 Traceability

Can outputs be linked to:

* Source requirements.
* Source code.
* Architecture rules.
* Supporting evidence.

## 30.5 Evaluation

Does prior work measure:

* Accuracy.
* Recall.
* Consistency.
* Traceability.
* Human effort.
* Engineering time.

---

# 31. Planned Research Gap Statement

The final research gap may follow this structure:

> Existing research has explored AI-assisted requirement extraction, code generation, model-driven API development, static architecture analysis, and legacy modernization. However, these capabilities are commonly evaluated as separate workflows. Limited work integrates unstructured requirement interpretation, deterministic requirement validation, API artifact generation, source-code evidence extraction, target-architecture comparison, explainable scoring, and migration-task generation in a unified human-reviewed process. The proposed research addresses this integration gap through a hybrid AI-assisted and rule-based architecture.

This statement must be adjusted based on the actual literature findings.

Do not preserve it unchanged when selected papers contradict or narrow the claim.

---

# 32. Citation Categories

Each citation used in the paper should serve a clear purpose.

Suggested categories:

* `BACKGROUND`
* `DEFINITION`
* `PROBLEM_EVIDENCE`
* `METHOD_FOUNDATION`
* `RELATED_WORK`
* `EVALUATION_METHOD`
* `LIMITATION`
* `DATASET`
* `STANDARD`
* `TOOL_DOCUMENTATION`

A source may support multiple categories.

---

# 33. Citation Claim Tracking

Recommended file:

```text
docs/day49/literature/citation_claims.csv
```

Columns:

```csv
claim_id,paper_section,claim_text,citation_keys,verification_status,notes
```

Example:

```csv
CLAIM-001,Introduction,"LLM-generated code can contain functional and security defects.","paperA;paperB",verified,"Supported by empirical evaluations."
```

---

# 34. Citation Verification Rules

Before citing a source:

* Read the relevant section of the source.
* Verify the exact claim.
* Confirm the paper evaluated what the manuscript states.
* Distinguish author claims from measured results.
* Check whether the paper reports limitations.
* Confirm publication metadata.
* Verify the DOI or permanent identifier.
* Confirm author names and publication year.
* Record page or section numbers where useful.
* Avoid citing a source based only on another paper’s summary.

---

# 35. Unsupported Citation Practices to Avoid

Do not:

* Cite a paper without reading it.
* Cite an abstract as support for detailed results.
* Cite a secondary source when the primary source is available.
* Claim a paper achieved a result it only proposed.
* Treat a preprint as peer reviewed.
* Cite vendor marketing as independent evaluation.
* Use a citation to support multiple unrelated claims.
* Copy language too closely from the source.
* Include references that are never cited.
* Cite inaccessible or unverifiable metadata.

---

# 36. Citation Style

The final citation style depends on the selected venue.

Common options:

* IEEE numbered style.
* ACM numbered or author-year style.
* APA for non-computing venues.

For a software-engineering conference or journal, IEEE or ACM style will likely be appropriate.

Do not manually format each reference until the target venue is selected.

Use BibTeX or another structured bibliography system.

---

# 37. BibTeX File

Recommended file:

```text
docs/day49/literature/references.bib
```

Example structure:

```bibtex
@inproceedings{example2026paper,
  author    = {Author One and Author Two},
  title     = {Example Paper Title},
  booktitle = {Proceedings of the Example Conference},
  year      = {2026},
  pages     = {1--10},
  doi       = {TBD}
}
```

The example above is a template and must not be included in the final bibliography as an actual source.

---

# 38. Citation Key Convention

Use:

```text
<first-author-last-name><year><short-topic>
```

Examples:

```text
smith2024requirements
chen2025llmcode
patel2023modernization
```

For duplicate keys:

```text
smith2024requirementsA
smith2024requirementsB
```

---

# 39. Reference Metadata Requirements

Each bibliography entry should include, when available:

* Authors.
* Title.
* Year.
* Journal or conference.
* Volume.
* Issue.
* Page range.
* Publisher.
* DOI.
* URL or archive identifier where required.
* Access date for web-only sources.

Do not add fields containing guessed information.

---

# 40. Source Storage and Copyright

Do not store or publish copyrighted papers in the project repository unless their license permits redistribution.

The repository may store:

* Citation metadata.
* DOI.
* Legal public URL.
* Personal research notes.
* Short compliant excerpts.
* Open-access papers where redistribution is allowed.

Do not commit unauthorized publisher PDFs.

---

# 41. Minimum Literature Targets

The recommended initial bibliography target is:

| Research Area                                | Minimum Selected Sources |
| -------------------------------------------- | -----------------------: |
| AI requirements engineering                  |                        5 |
| LLMs for software engineering                |                        6 |
| AI-assisted API or code generation           |                        4 |
| Model-driven API development                 |                        4 |
| Static analysis and architecture conformance |                        5 |
| Legacy modernization                         |                        5 |
| Explainability and evidence                  |                        4 |
| Human-in-the-loop engineering                |                        4 |

These categories may overlap.

Recommended final paper target:

```text
Approximately 30–45 strong references
```

A shorter workshop paper may use fewer references when page limits require it.

Quality and relevance are more important than the total count.

---

# 42. Foundational and Recent Source Balance

The bibliography should include:

* Foundational requirements-engineering research.
* Foundational model-driven engineering research.
* Established static-analysis and architecture-recovery work.
* Modern software-modernization research.
* Recent generative AI studies.
* Recent human-AI software-engineering studies.

Avoid a bibliography composed only of recent LLM papers.

The proposed hybrid architecture also depends on established deterministic software-engineering methods.

---

# 43. Citation Distribution

Citations should appear throughout the paper.

Expected distribution:

| Paper Section | Citation Need   |
| ------------- | --------------- |
| Introduction  | Moderate        |
| Background    | High            |
| Related Work  | Very high       |
| Methodology   | Moderate        |
| Architecture  | Low to moderate |
| Evaluation    | Moderate        |
| Discussion    | Moderate        |
| Limitations   | Moderate        |
| Conclusion    | Low             |

The architecture and implementation sections should primarily describe the proposed system, but citations should be used where the design builds on established methods.

---

# 44. Standards and Technical Documentation

The paper may cite official standards and documentation for:

* OpenAPI.
* HTTP.
* REST principles.
* JSON Schema.
* Software architecture terminology.
* Evaluation tools.
* Framework behavior.

Official documentation should support technical definitions or implementation details, not substitute for academic evidence regarding research effectiveness.

---

# 45. Literature Search Execution Order

Recommended sequence:

1. Find systematic reviews on LLMs in software engineering.
2. Find systematic reviews on AI in requirements engineering.
3. Identify foundational model-driven API research.
4. Identify architecture-conformance and recovery research.
5. Identify modernization and cloud-readiness studies.
6. Identify explainability and human-review research.
7. Review citations from selected surveys.
8. Conduct forward-citation searches.
9. Complete full-text reviews.
10. Build the comparison matrix.
11. Draft the related-work synthesis.
12. Verify every citation.

---

# 46. Snowballing Strategy

## 46.1 Backward Snowballing

For each high-quality selected paper:

* Review its reference list.
* Identify foundational methods.
* Identify datasets and evaluation methods.
* Identify prior limitations.

## 46.2 Forward Snowballing

Find later studies that:

* Cite the selected paper.
* Reproduce its results.
* Challenge its findings.
* Extend its method.
* Apply it to another domain.

## 46.3 Stop Criteria

Stop snowballing when:

* New searches mostly return already reviewed papers.
* No new research capability is identified.
* Each research area has sufficient high-quality coverage.
* The research gap remains stable after additional papers.

---

# 47. Review Consistency

When more than one reviewer participates in literature screening:

* Use the same inclusion criteria.
* Independently review a sample of papers.
* Compare inclusion decisions.
* Resolve disagreements.
* Record the final decision.
* Update the criteria when ambiguity is identified.

---

# 48. Research Note Quality Checklist

For each selected paper:

* [ ] Full citation recorded.
* [ ] Full text reviewed.
* [ ] Research problem summarized.
* [ ] Proposed method summarized.
* [ ] Evaluation recorded.
* [ ] Main results recorded.
* [ ] Limitations recorded.
* [ ] Relevance to the proposed system recorded.
* [ ] Similarities identified.
* [ ] Differences identified.
* [ ] Supported claims listed.
* [ ] Page or section references recorded.
* [ ] Citation metadata verified.
* [ ] BibTeX entry created.

---

# 49. Related-Work Drafting Rules

The final section should:

* Group papers by capability or research problem.
* Compare methods rather than only listing papers.
* State important limitations.
* Represent conflicting findings fairly.
* Distinguish AI-based and deterministic techniques.
* Explain how the proposed work combines prior ideas.
* End with a precise research gap.
* Avoid claiming novelty solely because no identical product exists.
* Avoid dismissing previous work without evidence.
* Avoid overstating differences.

---

# 50. Novelty Evaluation Questions

Before claiming novelty, answer:

* Has another system combined requirement extraction and source modernization?
* Has another system used both AI and deterministic validation?
* Has another system provided source-evidence-linked migration findings?
* Has another system created explainable readiness scores?
* Has another system generated migration tasks?
* Has another system evaluated all these capabilities together?
* Is the difference architectural, methodological, empirical, or only implementation-specific?

Novelty should be framed around the combination, governance, and evaluation of capabilities when individual capabilities already exist.

---

# 51. Planned Related-Work Comparison Dimensions

Use the following dimensions when comparing selected research.

| Dimension                | Description                                    |
| ------------------------ | ---------------------------------------------- |
| Input type               | Requirements, code, models, runtime data       |
| Output type              | Specification, code, findings, recommendations |
| AI usage                 | None, classification, generation, hybrid       |
| Deterministic validation | Schema, rules, static checks                   |
| Source evidence          | Whether outputs link to supporting artifacts   |
| Target architecture      | Whether expected architecture is explicit      |
| Explainability           | Whether findings can be inspected              |
| Human review             | Whether correction or approval is supported    |
| Evaluation               | Dataset, baseline, metrics and reviewers       |
| Reproducibility          | Whether implementation or data is available    |

---

# 52. Related-Work Evidence Table

A detailed internal table should be maintained.

| Paper | Problem | Method | Dataset | Metrics | Main Result | Limitation | Relevance |
| ----- | ------- | ------ | ------- | ------- | ----------- | ---------- | --------- |
| TBD   | TBD     | TBD    | TBD     | TBD     | TBD         | TBD        | TBD       |

This table supports writing but may not appear in the final paper.

---

# 53. Citation Risk Categories

Mark citations with potential risks.

Suggested values:

* `verified`
* `metadata-unverified`
* `abstract-only`
* `preprint`
* `secondary-source`
* `conflicting-results`
* `weak-methodology`
* `requires-recheck`

No citation marked `abstract-only` or `metadata-unverified` should remain in the final paper.

---

# 54. Citation Verification Document

Recommended file:

```text
docs/day49/literature/citation_verification.md
```

Template:

```md
# Citation Verification

## Citation Key

### Metadata

- [ ] Authors verified.
- [ ] Title verified.
- [ ] Venue verified.
- [ ] Year verified.
- [ ] DOI or identifier verified.

### Claim Verification

- [ ] Paper supports the cited claim.
- [ ] Claim is not stronger than the paper result.
- [ ] Relevant section or page recorded.
- [ ] Limitations considered.
- [ ] Primary source used when available.

### Manuscript Verification

- [ ] Citation appears in the bibliography.
- [ ] Citation is referenced in the manuscript.
- [ ] Citation style is correct.
- [ ] No duplicate bibliography entry exists.
```

---

# 55. Reference Consistency Checks

Before submission:

* Every in-text citation must appear in the bibliography.
* Every bibliography entry should be cited.
* Citation keys must be unique.
* Author names must be consistent.
* Publication years must match the source.
* Titles must preserve correct capitalization where required.
* DOI values must resolve.
* Preprints must be labeled correctly.
* Updated peer-reviewed versions should replace older preprints where appropriate.
* Duplicate versions must be removed.

---

# 56. Literature Review Failure Modes

Avoid:

## 56.1 Citation Dumping

Adding many citations without explaining their relevance.

## 56.2 Confirmation Bias

Selecting only papers that support the proposed approach.

## 56.3 Recency Bias

Ignoring foundational work because it predates generative AI.

## 56.4 Tool Comparison Without Research Comparison

Comparing commercial product features instead of research methods.

## 56.5 Unsupported Novelty

Claiming no prior work exists without a systematic search.

## 56.6 Result Misrepresentation

Presenting an author’s proposed future work as an achieved result.

## 56.7 Benchmark Mismatch

Comparing results measured using incompatible datasets or definitions.

## 56.8 Overgeneralization

Treating performance on one language, dataset, or task as universal.

---

# 57. Related-Work Completion Criteria

The literature-review process is complete when:

* All eight research areas have been searched.
* Search queries and databases are documented.
* Candidate papers are tracked.
* Inclusion and exclusion decisions are recorded.
* Selected papers have full notes.
* Quality assessments are complete.
* Comparison matrices are populated.
* The research gap is supported by reviewed literature.
* Every related-work claim has citations.
* Citation metadata has been verified.
* The bibliography is complete and consistent.

---

# 58. Minimum Viable Literature Review

When time is limited, the minimum viable literature review should include:

* Four papers on AI requirements engineering.
* Five papers on LLMs for software engineering.
* Three papers on API or model-driven generation.
* Four papers on architecture conformance or static analysis.
* Four papers on legacy modernization.
* Three papers on explainability or human review.
* Two systematic reviews.
* Relevant technical standards.

This produces approximately:

```text
25–30 strong sources
```

Overlapping papers may contribute to more than one research area.

---

# 59. Full Literature Review Target

The preferred full review should contain:

```text
30–45 selected references
```

Recommended composition:

```text
5–8 systematic reviews or surveys
15–25 empirical or technical research papers
5–10 foundational papers
3–6 recent high-quality preprints when necessary
Relevant official standards
```

---

# 60. Phase 6 File Structure

```text
docs/day49/
├── related_work_research_plan.md
└── literature/
    ├── search_log.md
    ├── candidate_papers.csv
    ├── selected_papers.csv
    ├── excluded_papers.csv
    ├── comparison_matrix.csv
    ├── citation_claims.csv
    ├── citation_verification.md
    ├── references.bib
    └── citation_notes/
        ├── paper-01.md
        ├── paper-02.md
        └── paper-03.md
```

---

# 61. Related-Work Execution Checklist

## Search Preparation

* [ ] Research areas finalized.
* [ ] Databases selected.
* [ ] Search queries prepared.
* [ ] Search completion date format defined.
* [ ] Inclusion criteria defined.
* [ ] Exclusion criteria defined.
* [ ] Candidate-paper tracker created.

## Search Execution

* [ ] RW1 searched.
* [ ] RW2 searched.
* [ ] RW3 searched.
* [ ] RW4 searched.
* [ ] RW5 searched.
* [ ] RW6 searched.
* [ ] RW7 searched.
* [ ] RW8 searched.
* [ ] Backward snowballing completed.
* [ ] Forward snowballing completed.

## Paper Review

* [ ] Titles screened.
* [ ] Abstracts screened.
* [ ] Full texts reviewed.
* [ ] Duplicate versions resolved.
* [ ] Quality scores assigned.
* [ ] Citation notes completed.
* [ ] Comparison matrix populated.

## Citation Preparation

* [ ] BibTeX entries created.
* [ ] Metadata verified.
* [ ] Claims linked to sources.
* [ ] Preprints labeled.
* [ ] Unsupported citations removed.
* [ ] Reference list checked for duplicates.

## Related-Work Writing

* [ ] Papers grouped by research area.
* [ ] Methods compared.
* [ ] Limitations synthesized.
* [ ] Conflicting findings represented.
* [ ] Research gap drafted.
* [ ] Novelty statement reviewed.
* [ ] Every factual claim cited.

---

# 62. Claim-Control Rules

The related-work section must not claim:

* That no prior research exists without extensive verification.
* That the proposed system is the first of its kind without strong evidence.
* That AI-only methods always fail.
* That deterministic systems are always correct.
* That cited approaches are directly comparable when datasets differ.
* That preprint results have been peer reviewed.
* That a paper supports a claim not measured in its evaluation.
* That one survey represents the complete field.
* That commercial tools have capabilities not publicly documented.
* That the proposed system outperforms previous work before experiments are complete.

---

# 63. Phase 6 Completion Checklist

* [x] Defined the purpose of the related-work section.
* [x] Defined eight research areas.
* [x] Defined research questions for each area.
* [x] Created literature-search queries.
* [x] Selected academic databases.
* [x] Defined preferred source types.
* [x] Defined the literature-search period.
* [x] Defined search-query construction.
* [x] Defined the search log.
* [x] Defined the paper-screening workflow.
* [x] Defined inclusion criteria.
* [x] Defined exclusion criteria.
* [x] Defined duplicate-handling rules.
* [x] Defined paper-quality scoring.
* [x] Created the paper-reading template.
* [x] Defined the comparison matrix.
* [x] Defined the synthesis method.
* [x] Defined the related-work section structure.
* [x] Defined the research-gap framework.
* [x] Drafted a provisional research-gap statement.
* [x] Defined citation categories.
* [x] Defined claim tracking.
* [x] Defined citation-verification rules.
* [x] Defined bibliography organization.
* [x] Defined citation-key conventions.
* [x] Defined minimum literature targets.
* [x] Defined foundational and recent source balance.
* [x] Defined snowballing procedures.
* [x] Defined novelty-evaluation questions.
* [x] Defined citation-risk categories.
* [x] Defined reference-consistency checks.
* [x] Defined the literature-review completion criteria.

---

# 64. Phase 6 Completion Status

**Status:** Completed

**Recommended file:**

```text
docs/day49/related_work_research_plan.md
```

**Next Phase:**

Day 49 Phase 7 will create the Day 49 completion report, summarize all research-paper preparation deliverables, record remaining work, and define the Day 50 publication and project-finalization plan.
