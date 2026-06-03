# Evaluation Dataset Definition

## 1. Introduction

This document defines the evaluation dataset for the AI-driven Enterprise API Factory research project.

The goal of the evaluation dataset is to measure how accurately the proposed system can transform semi-structured enterprise requirement documents into implementation-ready artifacts.

The evaluation focuses on four major outputs:

1. Structured requirement extraction
2. Node-RED workflow generation
3. Go Factory Excel intake generation
4. Requirement gap detection

This dataset will serve as the benchmark for evaluating whether the proposed approach can reduce manual API development preparation effort while maintaining high accuracy.

---

## 2. Evaluation Objective

The objective of the evaluation is to answer the following question:

```text
Can an AI-driven system accurately convert enterprise requirement documents into workflow, factory intake, and gap analysis artifacts?
```

The evaluation will compare generated artifacts against manually prepared expected artifacts.

Each API case will include:

* Source requirement document
* Expected structured extraction JSON
* Expected Node-RED flow
* Expected Go Factory Excel intake sheet
* Expected gap report

The generated outputs will be scored against these expected outputs using defined metrics.

---

## 3. Evaluation Scope

The evaluation covers enterprise API modernization and integration scenarios.

### Included Patterns

The dataset includes the following patterns:

* REST API wrappers
* REST-to-SOAP orchestration
* SOAP XML request generation
* SOAP XML response mapping
* REST-to-REST downstream integration
* Header validation
* Request body validation
* Path parameter validation
* Business rule extraction
* Database query mapping
* Multi-step orchestration
* Error handling
* Response transformation
* Excel-based factory metadata generation

### Excluded Patterns

The initial evaluation does not focus on:

* UI generation
* Mobile application generation
* Infrastructure-as-code generation
* Production deployment automation
* Security penetration testing
* Large-scale model training

These areas may be considered future extensions.

---

## 4. Evaluation Cases

The evaluation dataset will include ten enterprise-style API cases.

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

---

## 5. Dataset Folder Structure

The dataset will be stored using the following structure:

```text
datasets/
├── README.md
├── api_001_product_customization_fabrics/
│   ├── requirements.md
│   ├── expected_extraction.json
│   ├── expected_node_red_flow.json
│   ├── expected_factory_intake.xlsx
│   ├── expected_gap_report.md
│   └── evaluation_notes.md
│
├── api_002_product_customization_colors/
│   ├── requirements.md
│   ├── expected_extraction.json
│   ├── expected_node_red_flow.json
│   ├── expected_factory_intake.xlsx
│   ├── expected_gap_report.md
│   └── evaluation_notes.md
│
├── api_003_xcdts005_transfer_details/
│   ├── requirements.md
│   ├── expected_extraction.json
│   ├── expected_node_red_flow.json
│   ├── expected_factory_intake.xlsx
│   ├── expected_gap_report.md
│   └── evaluation_notes.md
│
└── ...
```

Each API folder contains all inputs and expected outputs required for evaluation.

---

## 6. Ground Truth Definition

Ground truth refers to the manually reviewed expected output for each API case.

The ground truth artifacts are:

| Artifact                       | Purpose                                       |
| ------------------------------ | --------------------------------------------- |
| `requirements.md`              | Source input document                         |
| `expected_extraction.json`     | Expected structured requirement extraction    |
| `expected_node_red_flow.json`  | Expected workflow structure                   |
| `expected_factory_intake.xlsx` | Expected Go Factory Excel intake              |
| `expected_gap_report.md`       | Expected missing/ambiguous requirement report |
| `evaluation_notes.md`          | Manual notes about complexity and scoring     |

Each ground truth artifact should be reviewed before running evaluation.

---

## 7. Requirement Extraction Evaluation

### 7.1 Goal

Measure how accurately the system extracts structured technical requirements from the source document.

### 7.2 Fields to Evaluate

| Field Category      | Examples                                              |
| ------------------- | ----------------------------------------------------- |
| API Metadata        | API name, method, endpoint path, version              |
| Headers             | Required headers, optional headers, propagation rules |
| Request Parameters  | Path params, query params, request body fields        |
| Response Fields     | Success response, error response                      |
| Validation Rules    | Required fields, enums, numeric checks, date checks   |
| Business Rules      | Calculations, transformations, conditions             |
| Downstream Services | REST, SOAP, database dependencies                     |
| Database Mapping    | Tables, columns, queries                              |
| Error Handling      | Status codes, message codes, downstream errors        |

### 7.3 Formula

```text
Requirement Extraction Accuracy =
Correctly Extracted Fields / Total Expected Fields
```

### 7.4 Target

```text
Target: 85%+
```

### 7.5 Example

If an API has 40 expected fields and the system correctly extracts 35:

```text
35 / 40 = 87.5%
```

The system meets the target.

---

## 8. Workflow Generation Evaluation

### 8.1 Goal

Measure how accurately the system generates the expected Node-RED workflow.

### 8.2 Workflow Components to Evaluate

| Component             | Description                  |
| --------------------- | ---------------------------- |
| HTTP Input Node       | Correct method and path      |
| Validation Nodes      | Header/body/path validation  |
| Business Rule Nodes   | Correct business logic steps |
| Transformation Nodes  | Request and response mapping |
| Downstream Call Nodes | REST/SOAP/DB invocation      |
| Error Handling Nodes  | Standardized error paths     |
| HTTP Response Node    | Final response mapping       |
| Wiring                | Correct execution order      |

### 8.3 Formula

```text
Workflow Generation Accuracy =
Correct Workflow Components / Total Expected Workflow Components
```

### 8.4 Target

```text
Target: 80%+
```

### 8.5 Example

If the expected flow has 15 components and the system correctly generates 13:

```text
13 / 15 = 86.7%
```

The system meets the target.

---

## 9. Excel Generation Evaluation

### 9.1 Goal

Measure how accurately the system generates Go Factory Excel intake sheets.

### 9.2 Excel Components to Evaluate

| Sheet                      | Evaluation Focus                                     |
| -------------------------- | ---------------------------------------------------- |
| `api_endpoints`            | Endpoint path, method, request/response mapping      |
| `business_requirements`    | Business rule IDs, descriptions, acceptance criteria |
| `external_services`        | Downstream services, endpoints, methods, headers     |
| `endpoint_service_mapping` | Orchestration order and service linkage              |
| `entities`                 | Database entities and table mapping                  |
| `queries`                  | SQL/query definitions                                |
| `request_response`         | DTO and payload mapping                              |
| `security`                 | Auth and security metadata                           |
| `error_handling`           | Error response mapping                               |

### 9.3 Formula

```text
Excel Generation Accuracy =
Correct Excel Rows or Mappings / Total Expected Excel Rows or Mappings
```

### 9.4 Target

```text
Target: 90%+
```

### 9.5 Example

If the expected Excel contains 100 important mappings and the system correctly generates 92:

```text
92 / 100 = 92%
```

The system meets the target.

---

## 10. Gap Detection Evaluation

### 10.1 Goal

Measure how effectively the system identifies missing, ambiguous, or incomplete requirement details.

### 10.2 Gap Categories

| Gap Category            | Examples                              |
| ----------------------- | ------------------------------------- |
| Missing Field           | Required header not specified         |
| Missing Validation      | Enum values not defined               |
| Missing Error Handling  | No downstream 500 mapping             |
| Missing Service Detail  | Downstream URL or method absent       |
| Missing Database Detail | Table or column not specified         |
| Ambiguous Requirement   | Optional header propagation unclear   |
| Incomplete Response     | Missing sample success/error response |
| Security Gap            | HMAC, token, or auth behavior unclear |

### 10.3 Formula

```text
Gap Detection Precision =
Correctly Detected Gaps / Total Detected Gaps
```

### 10.4 Recall Formula

```text
Gap Detection Recall =
Correctly Detected Gaps / Actual Known Gaps
```

### 10.5 Target

```text
Precision Target: 90%+
Recall Target: 85%+
```

### 10.6 Example

If the expected gap report contains 10 actual gaps and the system detects 9, but 1 detected gap is incorrect:

```text
Precision = 8 / 9 = 88.9%
Recall = 8 / 10 = 80%
```

The system would not fully meet the target and would require improvement.

---

## 11. Productivity Evaluation

### 11.1 Goal

Measure time saved compared with manual artifact creation.

### 11.2 Manual Baseline

For each API, record the estimated manual time required for:

| Task                   | Manual Time |
| ---------------------- | ----------: |
| Requirement reading    |  30 minutes |
| Requirement extraction |  45 minutes |
| Workflow design        |  60 minutes |
| Excel intake creation  |  90 minutes |
| Gap analysis           |  45 minutes |
| Review and correction  |  30 minutes |

Example total:

```text
300 minutes = 5 hours
```

### 11.3 AI-Assisted Time

Record the time required using the proposed system:

| Task                | AI-Assisted Time |
| ------------------- | ---------------: |
| Upload requirement  |        5 minutes |
| Generate extraction |        5 minutes |
| Generate workflow   |        5 minutes |
| Generate Excel      |       10 minutes |
| Generate gap report |        5 minutes |
| Human review        |       30 minutes |

Example total:

```text
60 minutes = 1 hour
```

### 11.4 Formula

```text
Time Reduction =
(Manual Time - AI-Assisted Time) / Manual Time
```

### 11.5 Target

```text
Target: 70%+ time reduction
```

### 11.6 Example

```text
(300 - 60) / 300 = 80%
```

The system exceeds the target.

---

## 12. Evaluation Scoring Template

Each API should be evaluated using the following template.

```md
# Evaluation Report: API-XXX

## API Name

[API Name]

## Difficulty Level

[1-5]

## Requirement Extraction

- Correct fields:
- Total expected fields:
- Accuracy:

## Workflow Generation

- Correct workflow components:
- Total expected workflow components:
- Accuracy:

## Excel Generation

- Correct Excel mappings:
- Total expected Excel mappings:
- Accuracy:

## Gap Detection

- Correct detected gaps:
- Total detected gaps:
- Actual known gaps:
- Precision:
- Recall:

## Productivity

- Manual time:
- AI-assisted time:
- Time reduction:

## Overall Result

| Metric | Result | Target | Status |
|---|---:|---:|---|
| Requirement Extraction |  | 85% |  |
| Workflow Generation |  | 80% |  |
| Excel Generation |  | 90% |  |
| Gap Precision |  | 90% |  |
| Gap Recall |  | 85% |  |
| Time Reduction |  | 70% |  |

## Notes

[Observations, failure points, corrections, and improvement opportunities]
```

---

## 13. Aggregate Evaluation

After all API cases are evaluated, compute aggregate performance.

### Aggregate Metrics

| Metric                                  | Formula              |
| --------------------------------------- | -------------------- |
| Average Requirement Extraction Accuracy | Mean across all APIs |
| Average Workflow Generation Accuracy    | Mean across all APIs |
| Average Excel Generation Accuracy       | Mean across all APIs |
| Average Gap Precision                   | Mean across all APIs |
| Average Gap Recall                      | Mean across all APIs |
| Average Time Reduction                  | Mean across all APIs |

### Aggregate Results Table

```md
| Metric | Average Result | Target | Status |
|---|---:|---:|---|
| Requirement Extraction Accuracy | TBD | 85% | TBD |
| Workflow Generation Accuracy | TBD | 80% | TBD |
| Excel Generation Accuracy | TBD | 90% | TBD |
| Gap Detection Precision | TBD | 90% | TBD |
| Gap Detection Recall | TBD | 85% | TBD |
| Time Reduction | TBD | 70% | TBD |
```

---

## 14. Error Analysis

Evaluation should include error analysis to understand where the system fails.

### Error Categories

| Error Type              | Description                                         |
| ----------------------- | --------------------------------------------------- |
| Missed Field            | Expected field was not extracted                    |
| Incorrect Field         | Wrong value was extracted                           |
| Hallucinated Field      | Field was generated but not present in requirement  |
| Wrong Workflow Node     | Incorrect Node-RED node generated                   |
| Missing Workflow Node   | Expected workflow step not generated                |
| Incorrect Excel Mapping | Wrong sheet, row, or mapping                        |
| Missing Excel Mapping   | Expected factory row not generated                  |
| False Gap               | System reported a gap that was not actually missing |
| Missed Gap              | System failed to detect a known gap                 |

Error analysis will help improve prompts, templates, schema validation, and generation rules.

---

## 15. Evaluation Validity

### Internal Validity

The evaluation depends on manually prepared ground truth artifacts. To reduce bias:

* Ground truth should be prepared before system evaluation.
* Expected gap reports should be written before reviewing generated output.
* Evaluation criteria should be fixed before scoring.
* Ambiguous cases should be documented in `evaluation_notes.md`.

### External Validity

The dataset is based on enterprise-style API patterns. While the number of cases may be small, the cases are realistic and represent common enterprise modernization scenarios.

### Construct Validity

The selected metrics evaluate the main goals of the system:

* Extraction accuracy
* Workflow correctness
* Factory readiness
* Gap detection quality
* Productivity improvement

These metrics align directly with the research questions.

---

## 16. Expected Evaluation Outcome

The expected outcome is that the proposed system will perform strongly on structured and repeated enterprise API patterns.

Expected strengths:

* Header extraction
* Endpoint extraction
* Request/response extraction
* Repeated workflow node generation
* Factory sheet row generation
* Common gap detection

Expected challenges:

* Deeply nested XML response mapping
* Complex business calculations
* Ambiguous requirements
* Incomplete downstream service documentation
* Enterprise-specific naming inconsistencies

The evaluation will show where AI automation is reliable and where human review remains necessary.

---

## 17. Summary

This document defines the evaluation dataset and scoring methodology for the AI-driven Enterprise API Factory.

The evaluation will measure requirement extraction, workflow generation, Excel intake generation, gap detection, and productivity improvement across ten enterprise-style API cases.

The results will provide evidence for whether AI can reduce manual effort in enterprise API modernization while maintaining artifact quality.

This evaluation framework will become the foundation for the experimental methodology section of the research paper.
