# Experimental Methodology

## 1. Introduction

This document defines the experimental methodology for evaluating the AI-driven Enterprise API Factory.

The purpose of the experiment is to measure whether the proposed system can accurately transform semi-structured enterprise requirement documents into implementation-ready artifacts.

The experiment evaluates four major capabilities:

1. Requirement extraction
2. Node-RED workflow generation
3. Go Factory Excel intake generation
4. Requirement gap detection

In addition, the methodology evaluates productivity improvement by comparing manual artifact creation time against AI-assisted artifact generation time.

---

## 2. Research Objective

The primary objective of this experiment is to determine whether AI can reduce the manual effort required to convert enterprise API requirements into technical artifacts while maintaining acceptable accuracy.

The experiment focuses on the following transformation pipeline:

```text
Requirement Document
        ↓
Structured Requirement Extraction
        ↓
Node-RED Workflow Generation
        ↓
Go Factory Excel Intake Generation
        ↓
Requirement Gap Detection
```

The system will be evaluated using realistic enterprise API examples that include REST APIs, SOAP wrappers, downstream service calls, database interactions, business rules, validation logic, and response mappings.

---

## 3. Research Questions

The experiment is designed to answer the following research questions.

### RQ1: Requirement Extraction Accuracy

How accurately can the AI system extract structured API metadata from semi-structured enterprise requirement documents?

This includes:

* API name
* HTTP method
* Endpoint path
* Required headers
* Optional headers
* Path parameters
* Query parameters
* Request body fields
* Response body fields
* Validation rules
* Business rules
* Downstream services
* Database mappings
* Error handling requirements

---

### RQ2: Workflow Generation Accuracy

How accurately can the AI system generate Node-RED orchestration workflows from extracted requirement metadata?

This includes:

* Correct node selection
* Correct node ordering
* Correct wiring
* Correct validation placement
* Correct downstream invocation placement
* Correct mapper placement
* Correct error handling path

---

### RQ3: Excel Intake Generation Accuracy

How accurately can the AI system generate Go Factory Excel intake sheets from requirement and workflow metadata?

This includes:

* Correct sheet population
* Correct endpoint metadata
* Correct business requirement rows
* Correct external service mappings
* Correct entity and query mapping
* Correct endpoint service mapping
* Correct request/response mapping
* Correct error handling metadata

---

### RQ4: Gap Detection Effectiveness

How effectively can the AI system detect missing, incomplete, or ambiguous requirement details?

This includes gaps related to:

* Missing headers
* Missing request fields
* Missing validation rules
* Missing downstream service information
* Missing database details
* Missing error handling
* Missing security behavior
* Ambiguous optional field behavior

---

### RQ5: Productivity Improvement

How much time can the AI system reduce compared with a manual artifact creation process?

This includes time spent on:

* Reading requirements
* Extracting technical metadata
* Designing workflows
* Creating Excel intake sheets
* Performing gap analysis
* Reviewing generated artifacts

---

## 4. Hypothesis

The experiment tests the following hypothesis:

```text
An AI-driven pipeline that combines requirement extraction, workflow generation, Excel intake generation, and gap detection can reduce enterprise API development preparation time by at least 70% while maintaining high artifact accuracy.
```

Expected target metrics:

| Metric                          | Target |
| ------------------------------- | -----: |
| Requirement Extraction Accuracy |   85%+ |
| Workflow Generation Accuracy    |   80%+ |
| Excel Generation Accuracy       |   90%+ |
| Gap Detection Precision         |   90%+ |
| Gap Detection Recall            |   85%+ |
| Preparation Time Reduction      |   70%+ |

---

## 5. Experimental Design

The experiment follows a case-study-based evaluation design.

Each API case is treated as one evaluation unit.

For each API case, the following artifacts are prepared manually as ground truth:

1. Source requirement document
2. Expected structured extraction JSON
3. Expected Node-RED workflow JSON
4. Expected Go Factory Excel intake sheet
5. Expected gap report

The AI system then generates the same artifact types.

The generated artifacts are compared against the expected artifacts using predefined scoring metrics.

---

## 6. Evaluation Dataset

The experiment uses ten enterprise-style API cases.

| API ID  | API / Flow Name                 | Category                    | Difficulty |
| ------- | ------------------------------- | --------------------------- | ---------: |
| API-001 | Product Customization Fabrics   | REST Wrapper                |          1 |
| API-002 | Product Customization Colors    | REST Wrapper + Mapping      |          2 |
| API-003 | XCDTS005 Transfer Details       | REST-to-SOAP Wrapper        |          3 |
| API-004 | XSTX560 Transfer Lookup         | SOAP XML Mapping            |          4 |
| API-005 | XSTX564F Carton Transfer        | SOAP Orchestration          |          4 |
| API-006 | DIF523                          | SOAP + Nested Mapping       |          4 |
| API-007 | DIF524                          | SOAP + Repeating Records    |          5 |
| API-008 | FEED Interrogate Account        | REST Downstream Integration |          4 |
| API-009 | FEED Discount Evaluation        | REST + DB Orchestration     |          5 |
| API-010 | Clienteling Application Message | REST + PostgreSQL           |          3 |

The dataset includes multiple enterprise integration patterns, including simple REST wrappers, SOAP transformations, downstream service integration, database mapping, and multi-step business rule orchestration.

---

## 7. Dataset Split

Because this research uses a small but realistic enterprise-style dataset, the experiment uses a case-study split instead of a large-scale machine learning train/test split.

| Split           | Purpose                                        | Number of APIs |
| --------------- | ---------------------------------------------- | -------------: |
| Development Set | Used to refine prompts, schemas, and templates |              3 |
| Evaluation Set  | Used for final scoring                         |              5 |
| Stress Test Set | Used to test complex edge cases                |              2 |

### Development Set

| API                           | Reason                              |
| ----------------------------- | ----------------------------------- |
| Product Customization Fabrics | Simple REST wrapper baseline        |
| XCDTS005 Transfer Details     | REST-to-SOAP transformation pattern |
| FEED Interrogate Account      | REST downstream integration pattern |

### Evaluation Set

| API                             | Reason                       |
| ------------------------------- | ---------------------------- |
| Product Customization Colors    | REST wrapper with mapping    |
| XSTX560 Transfer Lookup         | SOAP XML response mapping    |
| XSTX564F Carton Transfer        | SOAP orchestration           |
| DIF523                          | Nested SOAP response mapping |
| Clienteling Application Message | REST + PostgreSQL pattern    |

### Stress Test Set

| API                      | Reason                                      |
| ------------------------ | ------------------------------------------- |
| DIF524                   | Repeating record generation                 |
| FEED Discount Evaluation | Multi-step REST + DB business orchestration |

---

## 8. Experimental Procedure

The experiment will be conducted in the following steps.

### Step 1: Prepare Source Requirements

For each API case, prepare a clean `requirements.md` file.

The requirement document should include:

* API purpose
* HTTP method
* Endpoint path
* Headers
* Request body
* Response body
* Validation rules
* Business rules
* Downstream services
* Database dependencies
* Error handling
* Example requests and responses

---

### Step 2: Prepare Ground Truth Artifacts

For each API case, manually prepare expected outputs:

```text
expected_extraction.json
expected_node_red_flow.json
expected_factory_intake.xlsx
expected_gap_report.md
evaluation_notes.md
```

These ground truth artifacts must be created before evaluating AI-generated outputs.

---

### Step 3: Run Requirement Extraction

The AI system processes the requirement document and generates structured requirement JSON.

Output:

```text
generated_extraction.json
```

The generated extraction is compared against `expected_extraction.json`.

---

### Step 4: Run Workflow Generation

The system uses the generated requirement JSON to produce a Node-RED workflow.

Output:

```text
generated_node_red_flow.json
```

The generated workflow is compared against `expected_node_red_flow.json`.

---

### Step 5: Run Excel Generation

The system uses requirement metadata and workflow metadata to generate Go Factory Excel intake sheets.

Output:

```text
generated_factory_intake.xlsx
```

The generated workbook is compared against `expected_factory_intake.xlsx`.

---

### Step 6: Run Gap Analysis

The system analyzes the requirement document and generated artifacts to identify missing or ambiguous requirements.

Output:

```text
generated_gap_report.md
```

The generated gap report is compared against `expected_gap_report.md`.

---

### Step 7: Record Time Measurements

For each API case, record two time values:

1. Manual artifact creation time
2. AI-assisted artifact creation time

The productivity improvement is calculated using:

```text
Time Reduction =
(Manual Time - AI-Assisted Time) / Manual Time
```

---

### Step 8: Perform Human Review

An engineer reviews the generated artifacts and identifies:

* Correct outputs
* Incorrect outputs
* Missing outputs
* Hallucinated outputs
* Ambiguous outputs
* Required corrections

Human review is used to support final scoring and error analysis.

---

## 9. Measurement Metrics

The experiment uses the following primary metrics.

---

### 9.1 Requirement Extraction Accuracy

Measures how many expected requirement fields were correctly extracted.

```text
Requirement Extraction Accuracy =
Correctly Extracted Fields / Total Expected Fields
```

Example:

```text
35 correctly extracted fields / 40 expected fields = 87.5%
```

Target:

```text
85%+
```

---

### 9.2 Workflow Generation Accuracy

Measures how many expected workflow components were correctly generated.

```text
Workflow Generation Accuracy =
Correct Workflow Components / Total Expected Workflow Components
```

Example:

```text
13 correct workflow components / 15 expected components = 86.7%
```

Target:

```text
80%+
```

---

### 9.3 Excel Generation Accuracy

Measures how many expected Excel rows, mappings, or references were correctly generated.

```text
Excel Generation Accuracy =
Correct Excel Mappings / Total Expected Excel Mappings
```

Example:

```text
92 correct mappings / 100 expected mappings = 92%
```

Target:

```text
90%+
```

---

### 9.4 Gap Detection Precision

Measures how many detected gaps were actually valid.

```text
Gap Detection Precision =
Correctly Detected Gaps / Total Detected Gaps
```

Example:

```text
9 correct detected gaps / 10 detected gaps = 90%
```

Target:

```text
90%+
```

---

### 9.5 Gap Detection Recall

Measures how many known gaps were detected.

```text
Gap Detection Recall =
Correctly Detected Gaps / Actual Known Gaps
```

Example:

```text
8 correctly detected gaps / 10 actual known gaps = 80%
```

Target:

```text
85%+
```

---

### 9.6 Productivity Improvement

Measures reduction in manual preparation effort.

```text
Time Reduction =
(Manual Time - AI-Assisted Time) / Manual Time
```

Example:

```text
(300 minutes - 60 minutes) / 300 minutes = 80%
```

Target:

```text
70%+
```

---

## 10. Scoring Method

Each generated artifact will be scored using manually defined expected outputs.

### Requirement Extraction Scoring

Each expected field is marked as:

| Score | Meaning              |
| ----- | -------------------- |
| 1     | Correctly extracted  |
| 0.5   | Partially extracted  |
| 0     | Missing or incorrect |

---

### Workflow Generation Scoring

Each expected workflow component is marked as:

| Score | Meaning                                              |
| ----- | ---------------------------------------------------- |
| 1     | Correct node and correct placement                   |
| 0.5   | Correct node but wrong placement or incomplete logic |
| 0     | Missing or incorrect node                            |

---

### Excel Generation Scoring

Each expected Excel mapping is marked as:

| Score | Meaning                                            |
| ----- | -------------------------------------------------- |
| 1     | Correct sheet, row, field, and reference           |
| 0.5   | Correct concept but incomplete or wrong formatting |
| 0     | Missing or incorrect mapping                       |

---

### Gap Detection Scoring

Each detected gap is marked as:

| Score          | Meaning                                 |
| -------------- | --------------------------------------- |
| True Positive  | Correctly detected actual gap           |
| False Positive | Reported gap that is not actually a gap |
| False Negative | Actual gap that system missed           |

---

## 11. Manual Baseline

The manual baseline represents the current engineering process.

Manual process:

```text
Engineer reads requirement document
        ↓
Engineer extracts API metadata
        ↓
Engineer designs workflow
        ↓
Engineer prepares Excel intake sheet
        ↓
Engineer checks for missing information
        ↓
Engineer reviews output
```

Manual time is recorded for each API case.

Estimated manual time per medium-complexity API:

| Task                   | Estimated Time |
| ---------------------- | -------------: |
| Requirement reading    |     30 minutes |
| Requirement extraction |     45 minutes |
| Workflow design        |     60 minutes |
| Excel intake creation  |     90 minutes |
| Gap analysis           |     45 minutes |
| Review and correction  |     30 minutes |
| Total                  |    300 minutes |

---

## 12. AI-Assisted Process

The AI-assisted process represents the proposed system.

AI-assisted process:

```text
Upload requirement document
        ↓
Generate structured extraction
        ↓
Generate workflow
        ↓
Generate Excel intake
        ↓
Generate gap report
        ↓
Human review
```

Estimated AI-assisted time per medium-complexity API:

| Task                | Estimated Time |
| ------------------- | -------------: |
| Upload requirement  |      5 minutes |
| Generate extraction |      5 minutes |
| Generate workflow   |      5 minutes |
| Generate Excel      |     10 minutes |
| Generate gap report |      5 minutes |
| Human review        |     30 minutes |
| Total               |     60 minutes |

---

## 13. Error Analysis

After scoring, errors will be categorized to understand failure patterns.

| Error Category            | Description                                       |
| ------------------------- | ------------------------------------------------- |
| Missed Requirement Field  | Expected field was not extracted                  |
| Incorrect Field Value     | Wrong method, path, header, type, or value        |
| Hallucinated Field        | Generated field not present in source requirement |
| Missing Workflow Node     | Expected workflow component not generated         |
| Incorrect Workflow Order  | Nodes generated but incorrectly ordered           |
| Missing Excel Mapping     | Expected factory row or mapping missing           |
| Incorrect Excel Reference | Wrong BR ID, service ID, query ID, or endpoint ID |
| False Gap                 | Gap incorrectly reported                          |
| Missed Gap                | Known gap not detected                            |
| Formatting Error          | Output exists but does not match required schema  |

Error analysis will help refine prompts, schemas, templates, validators, and generation rules.

---

## 14. Tools and Implementation Environment

The experimental implementation may use the following tools.

| Area               | Tool                                  |
| ------------------ | ------------------------------------- |
| Local LLM          | Ollama                                |
| Local Model        | Mistral / Llama                       |
| Enterprise LLM     | Vertex AI / Gemini                    |
| Workflow Format    | Node-RED JSON                         |
| Factory Format     | Excel Workbook                        |
| Backend Prototype  | Next.js API routes / Python / Node.js |
| Evaluation Scripts | Python                                |
| Version Control    | GitHub                                |
| Documentation      | Markdown                              |

The implementation can begin with rule-based extraction and prompt-driven generation, then evolve toward schema-guided generation and validation.

---

## 15. Reproducibility Plan

To make the experiment reproducible, each API case will include:

```text
requirements.md
expected_extraction.json
expected_node_red_flow.json
expected_factory_intake.xlsx
expected_gap_report.md
generated_extraction.json
generated_node_red_flow.json
generated_factory_intake.xlsx
generated_gap_report.md
evaluation_report.md
```

Each generated output should be stored in the dataset folder.

The repository should also include:

```text
scripts/
├── run_extraction_eval.py
├── run_workflow_eval.py
├── run_excel_eval.py
├── run_gap_eval.py
└── aggregate_results.py
```

This structure allows future researchers or reviewers to reproduce the evaluation process.

---

## 16. Validity Controls

To improve experiment reliability:

1. Ground truth artifacts should be prepared before evaluating generated outputs.
2. Scoring criteria should be defined before scoring begins.
3. The same prompts/templates should be used across comparable API cases.
4. Development APIs should not be included in final evaluation results.
5. Stress test results should be reported separately.
6. Ambiguous cases should be documented in `evaluation_notes.md`.
7. Sensitive data should be anonymized consistently.
8. Human corrections should be tracked separately from generated output.

---

## 17. Expected Outcomes

The experiment is expected to show that AI performs well on repeated enterprise API patterns.

Expected strengths:

* Endpoint extraction
* Header extraction
* Request and response field extraction
* Standard validation extraction
* Standard Node-RED workflow generation
* Factory sheet mapping for repeated patterns
* Common gap detection

Expected weaknesses:

* Deep nested XML mapping
* Complex business calculations
* Ambiguous business wording
* Missing downstream documentation
* Inconsistent enterprise naming conventions
* Highly customized Excel schema rules

---

## 18. Summary

This experimental methodology defines how the AI-driven Enterprise API Factory will be evaluated.

The methodology uses ten enterprise-style API cases and compares AI-generated artifacts against manually prepared ground truth artifacts.

The experiment measures requirement extraction accuracy, workflow generation accuracy, Excel generation accuracy, gap detection precision and recall, and productivity improvement.

This methodology provides a structured foundation for the experimental section of the research paper.
