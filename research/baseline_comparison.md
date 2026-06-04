# Baseline Comparison

## 1. Introduction

This document defines the baseline comparison strategy for evaluating the AI-driven Enterprise API Factory.

The goal is to compare the proposed system against existing approaches used in enterprise API development and modernization.

The baseline comparison helps answer the following question:

```text id="7s96a4"
Does the proposed AI-driven Enterprise API Factory improve accuracy, consistency, and productivity compared with existing manual and tool-assisted approaches?
```

The comparison focuses on requirement extraction, workflow generation, Excel factory intake generation, gap detection, and preparation time reduction.

---

## 2. Purpose of Baseline Comparison

A baseline is required to measure improvement.

Without a baseline, it is difficult to prove whether the proposed system provides meaningful value.

The baseline comparison will evaluate the proposed system against:

1. Manual engineering process
2. AI coding assistant process
3. Low-code/workflow tool process
4. Model-driven engineering process
5. Proposed AI Enterprise API Factory

The purpose is not only to show that the proposed system generates artifacts, but also to show that it performs better than existing alternatives for the specific enterprise API modernization pipeline.

---

## 3. Compared Approaches

The following approaches will be compared:

| Approach                           | Description                                                                                    |
| ---------------------------------- | ---------------------------------------------------------------------------------------------- |
| Manual Engineering                 | Engineer manually reads requirements and creates artifacts                                     |
| AI Coding Assistant                | Developer uses tools such as ChatGPT, GitHub Copilot, or Cursor manually                       |
| Low-Code / Workflow Tool           | Engineer manually creates workflow in Node-RED, Camunda, Power Platform, Mendix, or OutSystems |
| Model-Driven Engineering           | Engineer creates formal model, then generates code/artifacts                                   |
| Proposed AI Enterprise API Factory | Automated pipeline for extraction, workflow generation, Excel generation, and gap analysis     |

---

## 4. Baseline 1: Manual Engineering Process

The manual process represents the current state of practice in many enterprise teams.

### Process

```text id="xdxid0"
Engineer reads requirement document
        ↓
Engineer identifies API metadata
        ↓
Engineer identifies headers and validations
        ↓
Engineer identifies downstream integrations
        ↓
Engineer designs workflow
        ↓
Engineer fills factory Excel sheet
        ↓
Engineer checks for missing requirements
        ↓
Engineer prepares implementation handoff
```

### Strengths

* High domain understanding when performed by experienced engineers.
* Flexible for ambiguous requirements.
* Can handle complex business logic.
* Allows human judgment.

### Limitations

* Slow and time-consuming.
* Inconsistent across engineers.
* Difficult to scale.
* Prone to missed details.
* Depends heavily on senior engineering knowledge.
* Poor repeatability.
* Manual gap analysis is often incomplete.

### Expected Performance

| Metric                          |   Expected Manual Performance |
| ------------------------------- | ----------------------------: |
| Requirement Extraction Accuracy | High, but depends on engineer |
| Workflow Generation Accuracy    |                High, but slow |
| Excel Generation Accuracy       |         High, but error-prone |
| Gap Detection                   |                Medium to High |
| Time Required                   |                       Highest |

---

## 5. Baseline 2: AI Coding Assistant Process

This baseline represents using general AI tools manually.

Examples include:

* ChatGPT
* GitHub Copilot
* Cursor
* Claude
* Gemini

### Process

```text id="06zxj5"
Developer pastes requirement into AI assistant
        ↓
Developer asks for extraction or code
        ↓
AI produces partial output
        ↓
Developer manually corrects output
        ↓
Developer asks follow-up prompts
        ↓
Developer manually creates workflow or Excel
        ↓
Developer reviews and fixes artifacts
```

### Strengths

* Faster than fully manual work.
* Helpful for summarization.
* Helpful for code generation.
* Can assist with debugging.
* Can generate drafts of mappings and documentation.

### Limitations

* Prompt-dependent.
* Output may be inconsistent.
* May hallucinate missing details.
* Does not enforce factory Excel schema.
* Does not automatically generate valid Node-RED flows.
* Does not provide deterministic gap analysis.
* Still requires significant developer effort.

### Expected Performance

| Metric                          | Expected AI Assistant Performance |
| ------------------------------- | --------------------------------: |
| Requirement Extraction Accuracy |                    Medium to High |
| Workflow Generation Accuracy    |                     Low to Medium |
| Excel Generation Accuracy       |                            Medium |
| Gap Detection                   |                            Medium |
| Time Required                   |                            Medium |

---

## 6. Baseline 3: Low-Code / Workflow Tool Process

This baseline represents using low-code or workflow platforms manually.

Examples include:

* Node-RED
* Camunda
* Microsoft Power Platform
* Mendix
* OutSystems

### Process

```text id="4k7d32"
Engineer reads requirement
        ↓
Engineer manually designs workflow
        ↓
Engineer configures nodes/components
        ↓
Engineer maps request and response data
        ↓
Engineer configures downstream services
        ↓
Engineer manually documents factory inputs
```

### Strengths

* Reduces amount of hand-written code.
* Provides visual workflow representation.
* Useful for orchestration and integration.
* Accelerates implementation after design is clear.

### Limitations

* Does not automatically parse requirement documents.
* Requires manual workflow modeling.
* Does not generate Go Factory Excel sheets.
* Does not perform requirement gap analysis.
* Workflow correctness depends on engineer experience.
* Still requires separate factory handoff preparation.

### Expected Performance

| Metric                          | Expected Low-Code / Workflow Tool Performance |
| ------------------------------- | --------------------------------------------: |
| Requirement Extraction Accuracy |                                 Not supported |
| Workflow Generation Accuracy    |                        High if manually built |
| Excel Generation Accuracy       |                                 Not supported |
| Gap Detection                   |                                           Low |
| Time Required                   |                                Medium to High |

---

## 7. Baseline 4: Model-Driven Engineering Process

This baseline represents a formal model-to-code approach.

### Process

```text id="uhwboh"
Engineer converts requirements into formal model
        ↓
Engineer validates model
        ↓
Generator transforms model into code/artifacts
        ↓
Engineer reviews generated output
        ↓
Engineer manually handles missing integration details
```

### Strengths

* Strong formal structure.
* Good for repeatable architectures.
* Can generate consistent code from models.
* Useful when domain models are mature.

### Limitations

* Requires upfront formal modeling.
* Difficult to use with messy enterprise requirement documents.
* Requires specialized tooling.
* Does not naturally support Confluence-to-Node-RED generation.
* Does not naturally produce Excel factory intake sheets.
* Adoption cost can be high.

### Expected Performance

| Metric                          | Expected Model-Driven Performance |
| ------------------------------- | --------------------------------: |
| Requirement Extraction Accuracy |                            Medium |
| Workflow Generation Accuracy    |                            Medium |
| Excel Generation Accuracy       |                     Low to Medium |
| Gap Detection                   |                            Medium |
| Time Required                   |                    Medium to High |

---

## 8. Proposed Approach: AI Enterprise API Factory

The proposed approach automates the full preparation pipeline.

### Process

```text id="mx2dpo"
Requirement document uploaded
        ↓
Requirement preprocessing
        ↓
AI requirement extraction
        ↓
Schema validation
        ↓
Node-RED workflow generation
        ↓
Workflow validation
        ↓
Go Factory Excel generation
        ↓
Excel validation
        ↓
Requirement gap analysis
        ↓
Human review
```

### Strengths

* Automates requirement extraction.
* Generates structured metadata.
* Generates Node-RED workflow JSON.
* Generates Go Factory Excel intake sheets.
* Detects requirement gaps.
* Supports repeatable enterprise API patterns.
* Produces measurable artifacts.
* Reduces preparation time.
* Preserves human review.

### Limitations

* Requires schema definitions.
* Requires validated prompt/templates.
* Complex business rules may need correction.
* Generated artifacts still require review.
* Depends on quality of requirement documents.
* Factory schema changes may require template updates.

### Expected Performance

| Metric                          | Expected Proposed System Performance |
| ------------------------------- | -----------------------------------: |
| Requirement Extraction Accuracy |                                 85%+ |
| Workflow Generation Accuracy    |                                 80%+ |
| Excel Generation Accuracy       |                                 90%+ |
| Gap Detection Precision         |                                 90%+ |
| Gap Detection Recall            |                                 85%+ |
| Time Reduction                  |                                 70%+ |

---

## 9. Baseline Evaluation Matrix

| Capability               | Manual Engineering | AI Coding Assistant | Low-Code / Workflow Tool | Model-Driven Engineering | Proposed AI Enterprise API Factory |
| ------------------------ | ------------------ | ------------------- | ------------------------ | ------------------------ | ---------------------------------- |
| Requirement Extraction   | Manual             | Partial             | No                       | Partial                  | Automated                          |
| Workflow Generation      | Manual             | Partial             | Manual                   | Partial                  | Automated                          |
| Excel Factory Generation | Manual             | Partial             | No                       | Limited                  | Automated                          |
| Gap Analysis             | Manual             | Partial             | No                       | Partial                  | Automated                          |
| Schema Validation        | Manual             | No                  | Limited                  | Yes                      | Automated                          |
| Enterprise API Awareness | High               | Prompt-dependent    | Medium                   | Medium                   | High                               |
| Repeatability            | Low                | Medium              | Medium                   | High                     | High                               |
| Human Effort             | High               | Medium              | Medium/High              | Medium/High              | Low/Medium                         |
| Artifact Traceability    | Medium             | Low/Medium          | Medium                   | High                     | High                               |
| Time Efficiency          | Low                | Medium              | Medium                   | Medium                   | High                               |

---

## 10. Quantitative Baseline Comparison

The experiment will compare approaches using the following quantitative metrics.

| Metric                          | Manual | AI Assistant | Workflow Tool | Model-Driven |       Proposed System |
| ------------------------------- | -----: | -----------: | ------------: | -----------: | --------------------: |
| Requirement Extraction Accuracy |    TBD |          TBD |           N/A |          TBD |           Target 85%+ |
| Workflow Generation Accuracy    |    TBD |          TBD |           TBD |          TBD |           Target 80%+ |
| Excel Generation Accuracy       |    TBD |          TBD |           N/A |          TBD |           Target 90%+ |
| Gap Detection Precision         |    TBD |          TBD |           N/A |          TBD |           Target 90%+ |
| Gap Detection Recall            |    TBD |          TBD |           N/A |          TBD |           Target 85%+ |
| Preparation Time                |    TBD |          TBD |           TBD |          TBD | Target 70%+ reduction |

For unavailable capabilities, the value will be marked as `N/A`.

---

## 11. Time Baseline

Time will be measured for each approach.

### Manual Baseline Time

Estimated medium-complexity API:

| Task                   |        Time |
| ---------------------- | ----------: |
| Requirement reading    |  30 minutes |
| Requirement extraction |  45 minutes |
| Workflow design        |  60 minutes |
| Excel intake creation  |  90 minutes |
| Gap analysis           |  45 minutes |
| Review and correction  |  30 minutes |
| Total                  | 300 minutes |

### Proposed System Time

Estimated medium-complexity API:

| Task                |       Time |
| ------------------- | ---------: |
| Upload requirement  |  5 minutes |
| Generate extraction |  5 minutes |
| Generate workflow   |  5 minutes |
| Generate Excel      | 10 minutes |
| Generate gap report |  5 minutes |
| Human review        | 30 minutes |
| Total               | 60 minutes |

### Expected Improvement

```text id="mw3ywc"
Time Reduction =
(300 - 60) / 300 = 80%
```

This exceeds the research target of 70% time reduction.

---

## 12. Accuracy Baseline

Accuracy will be measured by comparing generated artifacts against ground truth.

### Manual Process

Manual artifacts are expected to be accurate when created by expert engineers but may still contain omissions due to fatigue, ambiguity, or inconsistent interpretation.

### AI Coding Assistant

AI assistant outputs may be useful but can vary based on prompt quality and context length.

### Workflow Tool

Workflow tools can produce accurate workflows when manually configured but do not automatically extract requirements or generate factory intake sheets.

### Model-Driven Engineering

Model-driven approaches can be accurate when formal models are well-defined but require significant upfront modeling effort.

### Proposed System

The proposed system aims to combine automation with validation and human review to achieve high accuracy and repeatability.

---

## 13. Fairness of Comparison

To ensure fairness:

1. All approaches use the same requirement documents.
2. Ground truth artifacts are prepared before evaluation.
3. Metrics are fixed before scoring begins.
4. The same API cases are used across comparisons.
5. Human review time is included for all approaches.
6. Unavailable capabilities are marked as `N/A`.
7. Development cases are excluded from final evaluation scoring.
8. Stress test cases are reported separately.

---

## 14. Expected Results

The proposed system is expected to outperform baselines in end-to-end preparation efficiency.

Expected outcome:

| Area                   | Expected Result                                                     |
| ---------------------- | ------------------------------------------------------------------- |
| Requirement Extraction | Better than manual AI prompting due to structured schema            |
| Workflow Generation    | Better than AI assistant because of reusable workflow templates     |
| Excel Generation       | Better than general tools because of factory-specific mapping rules |
| Gap Detection          | Better because gap detection is explicit and systematic             |
| Time Reduction         | Better because artifacts are generated automatically                |
| Repeatability          | Better because schema, templates, and validators standardize output |

---

## 15. Interpretation of Results

The proposed system does not need to outperform expert engineers on every individual judgment.

Instead, the research goal is to show that the system can:

* Reduce preparation time significantly.
* Produce high-quality first drafts.
* Improve consistency.
* Detect missing information early.
* Make code factory generation more reliable.
* Reduce dependency on manual artifact creation.

A successful result means the system reaches target accuracy thresholds while reducing preparation time by at least 70%.

---

## 16. Summary

This baseline comparison defines how the proposed AI Enterprise API Factory will be evaluated against existing approaches.

The comparison includes manual engineering, AI coding assistants, low-code/workflow tools, model-driven engineering, and the proposed system.

The proposed system is expected to provide the strongest end-to-end support because it automates requirement extraction, workflow generation, Excel factory intake generation, and gap analysis in one pipeline.

This document supports the experimental methodology by establishing a clear comparison framework.
