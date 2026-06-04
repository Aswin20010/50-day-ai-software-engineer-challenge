# Threats to Validity

## 1. Introduction

This document identifies the major threats to validity for the AI-driven Enterprise API Factory research project.

Threats to validity describe factors that may affect the reliability, generalizability, and interpretation of the research results.

The proposed research evaluates whether an AI-driven system can transform semi-structured enterprise requirement documents into implementation-ready artifacts, including structured requirement metadata, Node-RED workflows, Go Factory Excel intake sheets, and gap reports.

Because this research involves AI-generated artifacts, human evaluation, enterprise-style datasets, and small case-study samples, validity risks must be clearly documented.

---

## 2. Internal Validity

Internal validity refers to whether the experiment correctly measures the effect of the proposed system.

### 2.1 Ground Truth Bias

The evaluation depends on manually prepared ground truth artifacts such as expected extraction JSON, expected Node-RED flows, expected Excel sheets, and expected gap reports.

If these ground truth artifacts are incomplete or incorrect, the evaluation results may be misleading.

#### Mitigation

* Prepare ground truth before running AI generation.
* Review ground truth artifacts manually.
* Document assumptions in `evaluation_notes.md`.
* Use consistent scoring rubrics across all APIs.
* Separate development-set examples from final evaluation examples.

---

### 2.2 Scoring Subjectivity

Some scoring decisions may require judgment. For example, a generated workflow node may be partially correct, or a gap may be valid but worded differently from the expected gap report.

This can introduce evaluator bias.

#### Mitigation

* Use explicit scoring rules.
* Assign scores of `1`, `0.5`, or `0`.
* Define clear criteria for correct, partial, and incorrect outputs.
* Document all partial-credit decisions.
* Use the same scoring template across all benchmark cases.

---

### 2.3 Prompt Overfitting

If prompts are repeatedly adjusted using the same evaluation cases, the system may become overfitted to those examples rather than generalizing to new requirements.

#### Mitigation

* Use a development set for prompt improvement.
* Keep evaluation-set APIs separate from prompt tuning.
* Report stress-test results separately.
* Avoid changing prompts after final evaluation begins.

---

### 2.4 Human Correction Influence

If human corrections are included in generated outputs before scoring, the system’s performance may appear better than it actually is.

#### Mitigation

* Store raw generated outputs separately.
* Score raw AI outputs before human correction.
* Track human-corrected outputs separately.
* Report human review time independently.

---

### 2.5 Inconsistent Requirement Quality

Some requirement documents may be more detailed than others. A well-written requirement will naturally produce better AI outputs than an incomplete or ambiguous requirement.

#### Mitigation

* Categorize requirement quality in `evaluation_notes.md`.
* Include gap detection as a measured task.
* Report performance by difficulty level.
* Report performance by requirement completeness.

---

## 3. External Validity

External validity refers to whether the results generalize beyond the evaluated examples.

### 3.1 Small Dataset Size

The benchmark uses a small number of enterprise-style API cases. Although these cases are realistic, they may not represent all enterprise software systems.

#### Mitigation

* Use diverse API patterns.
* Include REST, SOAP, DB, and multi-step orchestration examples.
* Report results as case-study evidence.
* Avoid claiming universal generalization.
* Expand dataset size in future work.

---

### 3.2 Enterprise-Specific Patterns

The dataset is based on enterprise API patterns such as Node-RED workflows, Go Factory Excel intake sheets, SOAP wrappers, and PostgreSQL mappings.

Other organizations may use different tools, schemas, or development processes.

#### Mitigation

* Clearly describe the enterprise context.
* Present the system as configurable rather than universal.
* Separate general pipeline ideas from organization-specific templates.
* Discuss adaptability to other workflow engines or factory schemas.

---

### 3.3 Tool-Specific Dependence

The proposed system uses specific artifact formats such as Node-RED JSON and Excel-based Go Factory intake sheets.

This may limit applicability to teams that do not use these tools.

#### Mitigation

* Frame Node-RED as one workflow representation layer.
* Frame Excel intake as one example of factory metadata.
* Design architecture so generators can be replaced.
* Discuss future support for OpenAPI, BPMN, AsyncAPI, or YAML-based schemas.

---

### 3.4 Limited Domain Coverage

The current examples focus mostly on API modernization and enterprise integration flows.

The results may not apply to unrelated software domains such as frontend UI generation, machine learning pipelines, mobile applications, or embedded systems.

#### Mitigation

* Limit claims to enterprise API modernization.
* Define included and excluded task scopes.
* Propose future evaluation on additional domains.

---

## 4. Construct Validity

Construct validity refers to whether the selected metrics accurately measure the intended research goals.

### 4.1 Artifact Accuracy vs Production Readiness

High artifact accuracy does not always mean the generated API is production-ready.

A generated Excel sheet may match expected mappings but still require business approval, security review, or integration testing.

#### Mitigation

* Define the research scope as preparation-phase automation.
* Do not claim full production automation.
* Include human review as part of the architecture.
* Include factory-readiness scoring instead of production-readiness claims.

---

### 4.2 Field-Level Scoring Limitations

Requirement extraction accuracy measures extracted fields, but some fields are more important than others.

For example, missing an endpoint path is more severe than missing an optional description.

#### Mitigation

* Use weighted scoring in future versions.
* Document critical fields separately.
* Report critical-field accuracy as an optional metric.
* Include qualitative error analysis.

---

### 4.3 Workflow Component Scoring Limitations

Workflow generation accuracy based on node counts may not fully capture semantic correctness.

A workflow may contain the correct nodes but still have incorrect logic inside a function node.

#### Mitigation

* Score both node presence and node logic.
* Include partial scoring.
* Validate wiring order.
* Include runtime validation in future work.

---

### 4.4 Excel Mapping Scoring Limitations

Excel generation accuracy depends on the factory schema. If the schema changes, the scoring method may need adjustment.

#### Mitigation

* Version the Excel schema.
* Store the specification used for each experiment.
* Validate required sheets and columns.
* Report schema version in evaluation reports.

---

### 4.5 Gap Detection Measurement Challenges

Gap detection can be difficult to score because requirements may be ambiguous in multiple valid ways.

A detected gap may be useful even if it does not exactly match the expected gap wording.

#### Mitigation

* Score gap detection by meaning rather than exact wording.
* Use categories such as missing field, missing validation, missing service detail, and ambiguous rule.
* Record false positives and false negatives separately.
* Include qualitative examples of useful detected gaps.

---

## 5. Conclusion Validity

Conclusion validity refers to whether the conclusions drawn from the experiment are justified by the data.

### 5.1 Overstating Automation Capability

The system may generate strong first-draft artifacts, but that does not mean it can replace engineers.

#### Mitigation

* Clearly position the system as human-in-the-loop.
* Emphasize preparation acceleration rather than full automation.
* Report manual review requirements.
* Include limitations in the final paper.

---

### 5.2 Time Reduction Estimation Risk

Time savings may be estimated rather than measured in early experiments.

Estimated times can be inaccurate.

#### Mitigation

* Track actual time during manual and AI-assisted artifact creation.
* Record time per task.
* Report median and average times.
* Distinguish estimated results from measured results.

---

### 5.3 Small Sample Statistical Risk

With only a small number of API cases, statistical significance may be limited.

#### Mitigation

* Present results as case-study evaluation.
* Use descriptive statistics.
* Avoid strong statistical claims.
* Expand the dataset in future work.

---

### 5.4 Positive Result Bias

Because the system is designed and evaluated by the researcher, there may be a tendency to highlight successful cases more than failures.

#### Mitigation

* Report failure cases.
* Include error analysis.
* Include stress-test results.
* Preserve generated artifacts for review.
* Document weaknesses honestly.

---

## 6. Reliability Threats

Reliability refers to whether the experiment can be repeated with similar results.

### 6.1 LLM Output Variability

LLMs may produce different outputs for the same prompt across runs.

#### Mitigation

* Use fixed prompts.
* Store model name and version.
* Use deterministic settings where possible.
* Store generated outputs.
* Run multiple trials for selected cases.

---

### 6.2 Model Version Changes

Cloud-based LLMs may change over time, affecting reproducibility.

#### Mitigation

* Record model version.
* Record execution date.
* Store raw prompts and outputs.
* Prefer versioned enterprise models where available.
* Use local models for reproducibility when possible.

---

### 6.3 Environment Differences

Generated outputs may depend on local tooling, Excel libraries, Node-RED versions, or parser behavior.

#### Mitigation

* Document tool versions.
* Version-control scripts.
* Store generated artifacts.
* Use containerized evaluation where possible.
* Include setup instructions.

---

## 7. Ethical and Privacy Considerations

The dataset may be based on enterprise-style requirement documents and integration examples.

Sensitive information must not be exposed.

### Risks

* Internal hostnames may be leaked.
* API keys or tokens may appear in examples.
* Customer/account identifiers may be exposed.
* Proprietary business logic may be revealed.
* Company-specific implementation details may be disclosed.

### Mitigation

* Anonymize all sensitive data.
* Replace hostnames with public-safe placeholders.
* Replace credentials with placeholders such as `<API_KEY>` and `<TOKEN>`.
* Avoid including real customer data.
* Store only sanitized examples in public repositories.
* Review all files before publishing.

---

## 8. Summary of Threats and Mitigations

| Threat                         | Category            | Mitigation                                        |
| ------------------------------ | ------------------- | ------------------------------------------------- |
| Ground truth bias              | Internal validity   | Prepare and review ground truth before evaluation |
| Subjective scoring             | Internal validity   | Use fixed scoring rubrics                         |
| Prompt overfitting             | Internal validity   | Separate development and evaluation sets          |
| Small dataset                  | External validity   | Report as case-study evaluation                   |
| Enterprise-specific tools      | External validity   | Describe configurable architecture                |
| Artifact accuracy vs readiness | Construct validity  | Limit claims to preparation-stage automation      |
| LLM output variability         | Reliability         | Store prompts, outputs, model version             |
| Time-saving estimation         | Conclusion validity | Measure actual task time                          |
| Sensitive data exposure        | Ethics/privacy      | Anonymize all public artifacts                    |

---

## 9. Final Validity Statement

The proposed research should be interpreted as a case-study evaluation of AI-assisted enterprise API preparation.

The results will demonstrate whether AI can reduce manual effort and produce high-quality first-draft artifacts for enterprise API modernization.

The research does not claim that AI can fully replace engineers or generate production-ready systems without review.

Instead, the contribution is a structured, measurable, and human-reviewed pipeline for transforming semi-structured requirements into workflow, factory intake, and gap analysis artifacts.

---

## 10. Summary

This document identifies key threats to validity and proposes mitigation strategies.

The major risks include ground truth bias, scoring subjectivity, prompt overfitting, small dataset size, enterprise-specific tooling, LLM variability, and privacy concerns.

By documenting these risks early, the research becomes more credible, transparent, and suitable for future publication.
