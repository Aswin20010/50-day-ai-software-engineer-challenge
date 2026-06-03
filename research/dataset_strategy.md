# Dataset Strategy

## 1. Introduction

This research focuses on building an AI-driven Enterprise API Factory that converts semi-structured enterprise requirements into implementation-ready artifacts.

The dataset strategy defines the real-world API examples, input/output artifacts, evaluation structure, and benchmarking approach required to validate the system.

The purpose of the dataset is to evaluate whether the proposed system can accurately transform enterprise requirement documents into:

1. Structured requirement metadata
2. Node-RED workflow definitions
3. Go Factory Excel intake sheets
4. Gap analysis reports
5. Code-generation-ready artifacts

---

## 2. Dataset Objective

The objective of the dataset is to provide realistic enterprise API examples that represent common modernization and integration patterns.

The dataset should test whether the proposed AI system can understand and generate artifacts for:

* REST API endpoints
* SOAP service wrappers
* Downstream REST integrations
* Header validation
* Request validation
* Business rule extraction
* Database query mappings
* Response transformation
* Error handling
* Factory intake sheet generation

The dataset should not be limited to simple examples. It should include real enterprise-style APIs with multiple layers of orchestration.

---

## 3. Dataset Sources

The strongest dataset for this research comes from enterprise integration flows already analyzed and implemented during the project.

### Candidate API Examples

| API ID  | API / Flow Name                 | Category                         | Pattern                                   |
| ------- | ------------------------------- | -------------------------------- | ----------------------------------------- |
| API-001 | XCDTS005 Transfer Details       | SOAP Wrapper                     | REST to SOAP transformation               |
| API-002 | XSTX560 Transfer Lookup         | SOAP Wrapper                     | XML request/response mapping              |
| API-003 | XSTX564F Carton Transfer        | SOAP Wrapper                     | SOAP orchestration                        |
| API-004 | DIF523                          | SOAP Wrapper + Mapping           | Nested XML response mapping               |
| API-005 | DIF524                          | SOAP Wrapper + Repeating Records | Complex record generation                 |
| API-006 | FEED Interrogate Account        | REST Integration                 | Downstream PIID/account lookup            |
| API-007 | FEED Discount Evaluation        | REST + DB Orchestration          | Multi-step discount calculation           |
| API-008 | Product Customization Fabrics   | REST Wrapper                     | Header/body validation + downstream call  |
| API-009 | Product Customization Colors    | REST Wrapper                     | Request mapping + response transformation |
| API-010 | Clienteling Application Message | REST + PostgreSQL                | DB insert + overlap validation            |

---

## 4. Dataset Artifact Structure

Each API example should contain a complete set of artifacts.

Recommended folder structure:

```text
datasets/
├── api_001_xcdts005_transfer_details/
│   ├── requirements.md
│   ├── node_red_flow.json
│   ├── factory_intake.xlsx
│   ├── expected_extraction.json
│   ├── expected_gap_report.md
│   └── notes.md
│
├── api_002_xstx560_transfer_lookup/
│   ├── requirements.md
│   ├── node_red_flow.json
│   ├── factory_intake.xlsx
│   ├── expected_extraction.json
│   ├── expected_gap_report.md
│   └── notes.md
│
├── api_003_feed_interrogate_account/
│   ├── requirements.md
│   ├── node_red_flow.json
│   ├── factory_intake.xlsx
│   ├── expected_extraction.json
│   ├── expected_gap_report.md
│   └── notes.md
```

Each folder represents one full evaluation case.

---

## 5. Required Artifact Definitions

### 5.1 Requirement Document

File:

```text
requirements.md
```

This file contains the source requirement document for the API.

It should include:

* Business purpose
* Endpoint method
* Endpoint path
* Required headers
* Optional headers
* Path parameters
* Query parameters
* Request body
* Response body
* Validation rules
* Business rules
* Downstream services
* Database tables
* Error handling rules
* Example requests and responses

This is the primary input to the AI system.

---

### 5.2 Node-RED Flow

File:

```text
node_red_flow.json
```

This file contains the expected Node-RED workflow representation.

It should include nodes such as:

* HTTP In
* Header validation
* Body validation
* Business rule mapping
* SOAP config builder
* REST wrapper invocation
* Database lookup
* Response mapper
* Error handler
* HTTP Response

This is used to evaluate workflow generation accuracy.

---

### 5.3 Factory Intake Excel

File:

```text
factory_intake.xlsx
```

This file contains the expected Go Factory intake sheet.

It should include sheets such as:

* `api_endpoints`
* `business_requirements`
* `external_services`
* `endpoint_service_mapping`
* `entities`
* `queries`
* `request_response`
* `security`
* `error_handling`

This is used to evaluate whether the AI system generates factory-ready artifacts.

---

### 5.4 Expected Extraction JSON

File:

```text
expected_extraction.json
```

This file contains the ground-truth structured extraction output.

Example:

```json
{
  "apiName": "FEED Interrogate Account",
  "method": "POST",
  "path": "/api/v1/discount/interrogateAccount",
  "headers": [
    "requestorInfo-clientId",
    "requestorInfo-subclientId",
    "requestorInfo-traceId",
    "requestorinfo-version",
    "x-macys-apikey"
  ],
  "requestFields": [
    "account.encryptInfo.encryptedData",
    "account.encryptInfo.keyType",
    "account.entryMode"
  ],
  "downstreamServices": [
    "PIIDLookup"
  ],
  "businessRules": [
    "Validate keyType enum",
    "Validate entryMode enum",
    "Build PIIDLookup request",
    "Map downstream response to accountInfo"
  ]
}
```

This file is used to calculate requirement extraction accuracy.

---

### 5.5 Expected Gap Report

File:

```text
expected_gap_report.md
```

This file defines known missing or incomplete requirement areas.

Example:

```md
# Expected Gap Report

## Missing Fields

- Downstream timeout value is not specified.
- Retry policy is not specified.
- Error code mapping for downstream 5xx response is incomplete.

## Ambiguous Fields

- Trace ID is optional but downstream propagation behavior must be clarified.
- HMAC validation is mentioned but signature source is not fully defined.

## Completeness Status

The requirement is approximately 85% complete for implementation.
```

This file is used to evaluate gap detection accuracy.

---

## 6. Dataset Labeling Strategy

Each dataset case should be manually labeled with expected values.

### Label Categories

| Label Category          | Description                                |
| ----------------------- | ------------------------------------------ |
| API Metadata            | Method, path, version, name                |
| Headers                 | Required and optional headers              |
| Request Fields          | Body, path, and query parameters           |
| Response Fields         | Success and error response structures      |
| Business Rules          | Validations, transformations, calculations |
| Downstream Integrations | REST/SOAP/DB dependencies                  |
| Workflow Nodes          | Expected Node-RED nodes                    |
| Excel Rows              | Expected Go Factory intake rows            |
| Gaps                    | Missing or ambiguous requirements          |

---

## 7. Dataset Difficulty Levels

The dataset should be divided into difficulty levels to evaluate system performance across complexity.

| Difficulty | Description                              | Example                       |
| ---------- | ---------------------------------------- | ----------------------------- |
| Level 1    | Simple REST wrapper                      | Product Customization Fabrics |
| Level 2    | REST wrapper with validation and mapping | Product Customization Colors  |
| Level 3    | REST to SOAP wrapper                     | XCDTS005                      |
| Level 4    | SOAP wrapper with nested XML mapping     | XSTX560 / XSTX564F            |
| Level 5    | Multi-step REST + DB orchestration       | FEED Discount Evaluation      |

This allows the research to show how accuracy changes as enterprise complexity increases.

---

## 8. Evaluation Split

Because the dataset size is small but domain-specific, the research should use case-study evaluation rather than large-scale ML training evaluation.

Recommended split:

| Split           | Purpose                           | Examples |
| --------------- | --------------------------------- | -------- |
| Development Set | Used to improve prompts/templates | 3 APIs   |
| Evaluation Set  | Used for final measurement        | 5 APIs   |
| Stress Test Set | Used for hardest edge cases       | 2 APIs   |

Example assignment:

### Development Set

* Product Customization Fabrics
* XCDTS005 Transfer Details
* FEED Interrogate Account

### Evaluation Set

* Product Customization Colors
* XSTX560 Transfer Lookup
* XSTX564F Carton Transfer
* DIF523
* Clienteling Application Message

### Stress Test Set

* DIF524
* FEED Discount Evaluation

---

## 9. Input and Output Flow

The dataset will support the following experimental workflow:

```text
Requirement Document
        ↓
AI Requirement Extractor
        ↓
Structured Requirement JSON
        ↓
Node-RED Flow Generator
        ↓
Node-RED Flow JSON
        ↓
Excel Generator
        ↓
Go Factory Intake Excel
        ↓
Gap Analyzer
        ↓
Gap Report
```

Each generated artifact will be compared against the expected artifact stored in the dataset.

---

## 10. Metrics Supported by Dataset

The dataset supports four major metrics.

### 10.1 Requirement Extraction Accuracy

Measures whether the system correctly extracts fields from the requirement document.

```text
Correctly Extracted Fields / Total Expected Fields
```

Target:

```text
85%+
```

---

### 10.2 Workflow Generation Accuracy

Measures whether the system generates the correct Node-RED nodes and wiring.

```text
Correct Workflow Nodes / Total Expected Workflow Nodes
```

Target:

```text
80%+
```

---

### 10.3 Excel Generation Accuracy

Measures whether the system generates correct Go Factory intake rows.

```text
Correct Excel Mappings / Total Expected Excel Mappings
```

Target:

```text
90%+
```

---

### 10.4 Gap Detection Precision

Measures whether the system correctly identifies missing or ambiguous requirement areas.

```text
Correctly Detected Gaps / Actual Known Gaps
```

Target:

```text
90%+
```

---

## 11. Data Quality Rules

Each dataset entry should follow these rules:

1. Requirement documents must be realistic and enterprise-style.
2. Sensitive company data should be removed or anonymized.
3. API names can be generalized if needed.
4. Internal hostnames, tokens, secrets, and credentials must not be included.
5. Expected outputs should be manually reviewed.
6. Node-RED flows should be valid JSON.
7. Excel files should follow the factory schema exactly.
8. Gap reports should be written before evaluating generated outputs to avoid bias.

---

## 12. Anonymization Strategy

Since the dataset is based on enterprise-style examples, sensitive details must be anonymized.

### Replace Sensitive Values

| Original Type            | Replacement                                |
| ------------------------ | ------------------------------------------ |
| Internal hostnames       | `https://example-enterprise-service.local` |
| API keys                 | `<API_KEY>`                                |
| Tokens                   | `<TOKEN>`                                  |
| User IDs                 | `<USER_ID>`                                |
| Customer/account numbers | `<ACCOUNT_TOKEN>`                          |
| Database names           | Generic names                              |
| Internal project names   | Public-safe aliases                        |

### Example

Original:

```text
https://internal-dev.company.net/provider/stx/wstx560
```

Anonymized:

```text
https://example-enterprise-service.local/provider/stx/wstx560
```

---

## 13. Dataset Governance

The dataset should be version-controlled in the GitHub repository.

Recommended structure:

```text
datasets/
├── README.md
├── api_001_xcdts005_transfer_details/
├── api_002_xstx560_transfer_lookup/
├── api_003_xstx564f_carton_transfer/
├── api_004_dif523/
├── api_005_dif524/
├── api_006_feed_interrogate_account/
├── api_007_feed_discount_evaluation/
├── api_008_product_customization_fabrics/
├── api_009_product_customization_colors/
└── api_010_clienteling_application_message/
```

Each dataset folder should contain a `notes.md` file explaining:

* What pattern the API represents
* Why it is useful for evaluation
* Known complexity
* Expected difficulty level
* Any anonymization performed

---

## 14. Expected Dataset Contribution

The dataset is a major contribution of this research because it represents realistic enterprise API modernization scenarios.

Unlike benchmark datasets focused on algorithmic coding problems, this dataset captures enterprise integration complexity, including:

* Semi-structured requirements
* API metadata
* Validation rules
* Downstream integrations
* SOAP/XML transformations
* Database interactions
* Excel-based factory intake formats
* Requirement ambiguity

This makes the dataset suitable for evaluating AI systems designed for enterprise software engineering automation.

---

## 15. Summary

The dataset strategy defines a realistic evaluation foundation for the proposed AI Enterprise API Factory.

The dataset will consist of multiple enterprise API examples, each containing requirement documents, Node-RED flows, Go Factory Excel sheets, expected extraction JSON, and gap reports.

The dataset will support evaluation of requirement extraction, workflow generation, Excel generation, and gap detection.

This strategy ensures that the research is evaluated against real enterprise software engineering patterns rather than simple coding benchmarks.
