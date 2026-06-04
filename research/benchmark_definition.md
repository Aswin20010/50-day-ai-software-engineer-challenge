# Benchmark Definition

## 1. Introduction

This document defines the benchmark used to evaluate the AI-driven Enterprise API Factory.

The benchmark provides a structured way to measure whether the system can accurately transform enterprise requirement documents into implementation-ready artifacts.

The benchmark evaluates four core capabilities:

1. Requirement extraction
2. Node-RED workflow generation
3. Go Factory Excel intake generation
4. Requirement gap detection

It also evaluates productivity improvement compared with manual engineering effort.

---

## 2. Benchmark Objective

The benchmark objective is to provide a repeatable and measurable evaluation framework for enterprise API modernization automation.

The benchmark should answer:

```text id="eshf5k"
How accurately and efficiently can the proposed AI system convert enterprise requirement documents into workflow, factory intake, and gap analysis artifacts?
```

The benchmark is designed around realistic enterprise API patterns instead of simple algorithmic coding tasks.

---

## 3. Benchmark Scope

The benchmark focuses on enterprise API preparation tasks that happen before final code generation.

### Included Tasks

* Extract API metadata from requirements.
* Extract headers, parameters, request bodies, and response bodies.
* Extract validation rules.
* Extract business rules.
* Extract downstream REST/SOAP service dependencies.
* Extract database table and query requirements.
* Generate Node-RED workflow structures.
* Generate Go Factory Excel intake metadata.
* Detect missing or ambiguous requirement details.
* Measure time savings.

### Excluded Tasks

* Final production code quality evaluation
* Runtime performance benchmarking
* Load testing
* UI generation
* Cloud deployment automation
* Security penetration testing

These areas may be considered in future work.

---

## 4. Benchmark Dataset

The benchmark uses ten enterprise-style API cases.

| API ID  | API / Flow Name                 | Pattern                                | Difficulty |
| ------- | ------------------------------- | -------------------------------------- | ---------: |
| API-001 | Product Customization Fabrics   | Simple REST wrapper                    |          1 |
| API-002 | Product Customization Colors    | REST wrapper with mapping              |          2 |
| API-003 | XCDTS005 Transfer Details       | REST-to-SOAP wrapper                   |          3 |
| API-004 | XSTX560 Transfer Lookup         | SOAP XML response mapping              |          4 |
| API-005 | XSTX564F Carton Transfer        | SOAP orchestration                     |          4 |
| API-006 | DIF523                          | SOAP nested response mapping           |          4 |
| API-007 | DIF524                          | SOAP repeating record generation       |          5 |
| API-008 | FEED Interrogate Account        | REST downstream integration            |          4 |
| API-009 | FEED Discount Evaluation        | REST + database business orchestration |          5 |
| API-010 | Clienteling Application Message | REST + PostgreSQL                      |          3 |

---

## 5. Difficulty Levels

Each benchmark case is assigned a difficulty level from 1 to 5.

| Difficulty | Description                                  | Example                                    |
| ---------- | -------------------------------------------- | ------------------------------------------ |
| 1          | Simple REST wrapper with basic validation    | Product Customization Fabrics              |
| 2          | REST wrapper with request/response mapping   | Product Customization Colors               |
| 3          | REST-to-SOAP wrapper or DB-backed simple API | XCDTS005 / Clienteling Application Message |
| 4          | SOAP/XML mapping or downstream orchestration | XSTX560 / FEED Interrogate Account         |
| 5          | Complex multi-step business orchestration    | FEED Discount / DIF524                     |

Difficulty is based on:

* Number of input fields
* Number of validation rules
* Number of workflow steps
* Downstream service complexity
* Database involvement
* XML/SOAP complexity
* Business rule complexity
* Error handling complexity

---

## 6. Benchmark Inputs

Each benchmark case contains the following inputs:

```text id="o7vzui"
requirements.md
```

Optional supporting files:

```text id="8iz6zg"
source_node_red_flow.json
source_excel_example.xlsx
notes.md
```

The `requirements.md` file is the primary input.

It should contain:

* API description
* HTTP method
* Endpoint path
* Headers
* Request parameters
* Request body
* Response body
* Business rules
* Validation rules
* Downstream services
* Database dependencies
* Error handling
* Examples

---

## 7. Benchmark Expected Outputs

Each benchmark case contains manually prepared expected outputs:

```text id="h5jl5q"
expected_extraction.json
expected_node_red_flow.json
expected_factory_intake.xlsx
expected_gap_report.md
evaluation_notes.md
```

These files represent the ground truth for evaluation.

---

## 8. Benchmark Generated Outputs

The system generates the following outputs:

```text id="pe0x37"
generated_extraction.json
generated_node_red_flow.json
generated_factory_intake.xlsx
generated_gap_report.md
generated_evaluation_report.md
```

The generated outputs are compared against expected outputs.

---

## 9. Benchmark Directory Structure

Recommended structure:

```text id="2ktj2p"
benchmarks/
├── README.md
├── api_001_product_customization_fabrics/
│   ├── requirements.md
│   ├── expected_extraction.json
│   ├── expected_node_red_flow.json
│   ├── expected_factory_intake.xlsx
│   ├── expected_gap_report.md
│   ├── generated_extraction.json
│   ├── generated_node_red_flow.json
│   ├── generated_factory_intake.xlsx
│   ├── generated_gap_report.md
│   ├── generated_evaluation_report.md
│   └── evaluation_notes.md
│
├── api_002_product_customization_colors/
│   └── ...
│
└── api_010_clienteling_application_message/
    └── ...
```

---

## 10. Benchmark Task 1: Requirement Extraction

### Goal

Evaluate whether the system correctly extracts structured requirements from the input document.

### Expected Extraction Categories

| Category            | Examples                                              |
| ------------------- | ----------------------------------------------------- |
| API Metadata        | Name, method, endpoint path, version                  |
| Headers             | Required headers, optional headers, propagation rules |
| Path Parameters     | Path variable names and validation                    |
| Query Parameters    | Query names and validation                            |
| Request Body        | Field names, types, required flags                    |
| Response Body       | Success and error response fields                     |
| Validations         | Required fields, enums, numeric checks                |
| Business Rules      | Conditions, calculations, transformations             |
| Downstream Services | REST, SOAP, database services                         |
| Database Mappings   | Tables, columns, queries                              |
| Error Mappings      | HTTP status, internal code, downstream error behavior |

### Scoring

Each expected field receives:

| Score | Meaning              |
| ----- | -------------------- |
| 1.0   | Correctly extracted  |
| 0.5   | Partially extracted  |
| 0.0   | Missing or incorrect |

### Formula

```text id="3on77n"
Requirement Extraction Accuracy =
Sum of extraction scores / Total expected fields
```

---

## 11. Benchmark Task 2: Workflow Generation

### Goal

Evaluate whether the system generates the expected Node-RED workflow structure.

### Expected Workflow Components

| Component             | Examples                                         |
| --------------------- | ------------------------------------------------ |
| Entry Node            | HTTP In                                          |
| Validation Nodes      | Header validation, body validation               |
| Normalization Nodes   | Payload normalization, path param handling       |
| Business Rule Nodes   | Rule execution, calculations                     |
| Request Builder Nodes | REST request builder, SOAP XML builder           |
| Integration Nodes     | REST call, SOAP wrapper, DB query                |
| Response Mapper Nodes | Downstream-to-REST mapping                       |
| Error Handling Nodes  | Validation error, downstream error, mapper error |
| Exit Node             | HTTP Response                                    |
| Wiring                | Correct execution sequence                       |

### Scoring

Each expected workflow component receives:

| Score | Meaning                                              |
| ----- | ---------------------------------------------------- |
| 1.0   | Correct node and correct placement                   |
| 0.5   | Correct node but incomplete logic or wrong placement |
| 0.0   | Missing or incorrect                                 |

### Formula

```text id="b1kn3f"
Workflow Generation Accuracy =
Sum of workflow scores / Total expected workflow components
```

---

## 12. Benchmark Task 3: Excel Factory Intake Generation

### Goal

Evaluate whether the system correctly generates Go Factory Excel intake sheets.

### Expected Excel Components

| Sheet                      | Expected Content                                    |
| -------------------------- | --------------------------------------------------- |
| `technical_requirements`   | Project language, framework, database, architecture |
| `api_endpoints`            | Endpoint method, path, request/response details     |
| `entities`                 | Database table/entity references                    |
| `external_services`        | Downstream service metadata                         |
| `business_requirements`    | Business rules and acceptance criteria              |
| `endpoint_service_mapping` | Orchestration and service linkage                   |
| `queries`                  | SQL/query definitions                               |
| `request_response`         | DTO and field mapping                               |
| `security`                 | Auth, HMAC, token, header rules                     |
| `error_handling`           | Error code and status mapping                       |

### Scoring

Each expected Excel mapping receives:

| Score | Meaning                                                |
| ----- | ------------------------------------------------------ |
| 1.0   | Correct sheet, row, field, and reference               |
| 0.5   | Correct concept but incomplete formatting or reference |
| 0.0   | Missing or incorrect                                   |

### Formula

```text id="aeh5xq"
Excel Generation Accuracy =
Sum of Excel mapping scores / Total expected Excel mappings
```

---

## 13. Benchmark Task 4: Gap Detection

### Goal

Evaluate whether the system correctly detects missing, ambiguous, or incomplete requirement details.

### Gap Categories

| Gap Type                  | Example                                    |
| ------------------------- | ------------------------------------------ |
| Missing Header            | Required header not specified              |
| Missing Validation        | Enum values missing                        |
| Missing Downstream Detail | Method, URL, or request mapping missing    |
| Missing DB Detail         | Table, column, or query not specified      |
| Missing Error Mapping     | No behavior defined for downstream 4xx/5xx |
| Missing Security Rule     | HMAC/token behavior not defined            |
| Ambiguous Business Rule   | Rule has unclear condition or calculation  |
| Incomplete Response       | Missing response example or field mapping  |

### Scoring Terms

| Term           | Meaning                                |
| -------------- | -------------------------------------- |
| True Positive  | System detected a real gap             |
| False Positive | System reported a gap that is not real |
| False Negative | System missed a real gap               |

### Precision Formula

```text id="747kyo"
Gap Detection Precision =
True Positives / (True Positives + False Positives)
```

### Recall Formula

```text id="q2lrtk"
Gap Detection Recall =
True Positives / (True Positives + False Negatives)
```

---

## 14. Benchmark Task 5: Productivity Measurement

### Goal

Evaluate how much time the system saves compared with manual engineering effort.

### Manual Process Time

Record time for:

| Task                | Description                          |
| ------------------- | ------------------------------------ |
| Requirement Reading | Time to understand requirement       |
| Extraction          | Time to identify metadata and rules  |
| Workflow Design     | Time to design Node-RED flow         |
| Excel Creation      | Time to create factory intake sheet  |
| Gap Analysis        | Time to identify missing information |
| Review              | Time to verify final artifacts       |

### AI-Assisted Process Time

Record time for:

| Task                  | Description                      |
| --------------------- | -------------------------------- |
| Requirement Upload    | Time to load input               |
| Extraction Generation | Time to generate structured JSON |
| Workflow Generation   | Time to generate Node-RED flow   |
| Excel Generation      | Time to generate workbook        |
| Gap Report Generation | Time to generate gaps            |
| Human Review          | Time to review AI output         |

### Formula

```text id="1j3dwq"
Time Reduction =
(Manual Time - AI-Assisted Time) / Manual Time
```

---

## 15. Benchmark Result Template

Each API benchmark case should produce an evaluation report.

```md id="451jxn"
# Benchmark Evaluation Report

## API ID

API-XXX

## API Name

[Name]

## Difficulty

[1-5]

## Requirement Extraction

| Category | Correct | Partial | Missing | Total |
|---|---:|---:|---:|---:|
| API Metadata |  |  |  |  |
| Headers |  |  |  |  |
| Request Fields |  |  |  |  |
| Response Fields |  |  |  |  |
| Validations |  |  |  |  |
| Business Rules |  |  |  |  |
| Downstream Services |  |  |  |  |
| Database Mappings |  |  |  |  |
| Error Mappings |  |  |  |  |

## Workflow Generation

| Component | Score | Notes |
|---|---:|---|
| HTTP In |  |  |
| Validation Nodes |  |  |
| Business Rule Nodes |  |  |
| Request Builder Nodes |  |  |
| Integration Nodes |  |  |
| Response Mapper Nodes |  |  |
| Error Handling Nodes |  |  |
| HTTP Response |  |  |
| Wiring |  |  |

## Excel Generation

| Sheet | Score | Notes |
|---|---:|---|
| technical_requirements |  |  |
| api_endpoints |  |  |
| entities |  |  |
| external_services |  |  |
| business_requirements |  |  |
| endpoint_service_mapping |  |  |
| queries |  |  |
| request_response |  |  |
| security |  |  |
| error_handling |  |  |

## Gap Detection

| Gap | Expected? | Detected? | Result |
|---|---|---|---|

## Productivity

| Process | Time |
|---|---:|
| Manual |  |
| AI-Assisted |  |
| Reduction |  |

## Final Score

| Metric | Result | Target | Status |
|---|---:|---:|---|
| Requirement Extraction Accuracy |  | 85% |  |
| Workflow Generation Accuracy |  | 80% |  |
| Excel Generation Accuracy |  | 90% |  |
| Gap Detection Precision |  | 90% |  |
| Gap Detection Recall |  | 85% |  |
| Time Reduction |  | 70% |  |

## Notes

[Error analysis and observations]
```

---

## 16. Aggregate Benchmark Report

After all benchmark cases are evaluated, create an aggregate report.

```md id="mjv9we"
# Aggregate Benchmark Report

## Summary

| Metric | Average Result | Target | Status |
|---|---:|---:|---|
| Requirement Extraction Accuracy |  | 85% |  |
| Workflow Generation Accuracy |  | 80% |  |
| Excel Generation Accuracy |  | 90% |  |
| Gap Detection Precision |  | 90% |  |
| Gap Detection Recall |  | 85% |  |
| Time Reduction |  | 70% |  |

## Results by Difficulty

| Difficulty | Avg Extraction | Avg Workflow | Avg Excel | Avg Gap Precision | Avg Time Reduction |
|---|---:|---:|---:|---:|---:|
| Level 1 |  |  |  |  |  |
| Level 2 |  |  |  |  |  |
| Level 3 |  |  |  |  |  |
| Level 4 |  |  |  |  |  |
| Level 5 |  |  |  |  |  |

## Results by Pattern

| Pattern | Avg Extraction | Avg Workflow | Avg Excel | Avg Gap Precision | Avg Time Reduction |
|---|---:|---:|---:|---:|---:|
| REST Wrapper |  |  |  |  |  |
| REST-to-SOAP |  |  |  |  |  |
| SOAP Mapping |  |  |  |  |  |
| REST + DB |  |  |  |  |  |
| Multi-step Orchestration |  |  |  |  |  |

## Major Error Categories

| Error Category | Count | Example |
|---|---:|---|
| Missed field |  |  |
| Incorrect mapping |  |  |
| Missing workflow node |  |  |
| Incorrect Excel reference |  |  |
| False gap |  |  |
| Missed gap |  |  |

## Conclusion

[Summary of benchmark findings]
```

---

## 17. Benchmark Success Criteria

The proposed system will be considered successful if it meets the following targets on the evaluation set:

| Metric                          | Success Target |
| ------------------------------- | -------------: |
| Requirement Extraction Accuracy |           85%+ |
| Workflow Generation Accuracy    |           80%+ |
| Excel Generation Accuracy       |           90%+ |
| Gap Detection Precision         |           90%+ |
| Gap Detection Recall            |           85%+ |
| Time Reduction                  |           70%+ |

The stress test set may have lower scores, but it should be reported separately to show performance under high complexity.

---

## 18. Benchmark Failure Criteria

The system will be considered insufficient if:

* Requirement extraction falls below 70%.
* Workflow generation falls below 65%.
* Excel generation falls below 75%.
* Gap detection precision falls below 70%.
* The system frequently hallucinates unsupported services or fields.
* The generated Excel cannot be understood by the Go Factory.
* Human correction time exceeds manual creation time.

These failure criteria define when the system requires further improvement before practical use.

---

## 19. Benchmark Governance

To keep the benchmark reliable:

1. Ground truth must be finalized before generated outputs are scored.
2. Evaluation rules must not change during scoring.
3. Development-set APIs must not be included in final evaluation averages.
4. Stress-test APIs must be reported separately.
5. Sensitive enterprise details must be anonymized.
6. All scoring decisions should be documented.
7. Human corrections should be tracked separately.
8. Generated artifacts should be version-controlled.

---

## 20. Summary

This benchmark definition provides a measurable evaluation framework for the AI-driven Enterprise API Factory.

The benchmark evaluates requirement extraction, workflow generation, Excel factory intake generation, gap detection, and productivity improvement.

The benchmark is designed around realistic enterprise API modernization cases and supports both per-case and aggregate analysis.

This benchmark will allow the research to demonstrate whether AI can meaningfully reduce manual effort while maintaining artifact quality in enterprise software engineering.
