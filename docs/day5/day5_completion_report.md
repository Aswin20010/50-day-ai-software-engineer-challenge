# Day 5 — Completion Report

## Day

Day 5

## Topic

Requirement Extraction Foundation

## Research Project

**Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis**

---

## Objective

The objective of Day 5 was to establish the first structured implementation foundation for the research project.

Day 5 focused on defining how a semi-structured enterprise API requirement document can be converted into a structured machine-readable JSON contract.

The main transformation defined in Day 5 is:

```text
Confluence Requirement Document
        ↓
Structured Requirement JSON
```

---

## Why Day 5 Matters

Enterprise API requirements are usually written in semi-structured formats such as Confluence pages, Jira stories, technical design documents, or API specification notes.

These documents are readable by humans, but they are not directly usable by automation systems.

Before an AI system can generate Node-RED flows, Excel workbooks, gap analysis reports, traceability matrices, or code-generation-ready specifications, it must first understand and structure the requirement.

Day 5 establishes that requirement understanding layer.

This layer becomes the foundation for the remaining automation pipeline.

---

## Work Completed

### 1. Created Day 5 Workspace

A dedicated `day5` folder was created to organize all artifacts related to the requirement extraction foundation.

This folder keeps the Day 5 work separate from previous challenge days and makes the project easier to review.

---

### 2. Created Project README

A `README.md` file was created for Day 5.

The README explains:

* The goal of Day 5
* The role of requirement extraction in the overall research project
* The transformation from Confluence requirement to structured JSON
* The expected Day 5 artifacts
* How this work connects to future Node-RED and Excel generation

---

### 3. Created Sample Confluence Requirement Document

A sample enterprise-style requirement document was created in:

```text
sample_confluence_requirement.md
```

The sample requirement is based on an enterprise discount evaluation API.

The document includes:

* API name
* API description
* Endpoint details
* Required headers
* Request body example
* Field-level requirements
* Enum requirements
* Business rules
* External service details
* Downstream request mapping
* Downstream response mapping
* Database table references
* Success response example
* Error response example
* Extraction notes

This sample document acts as the raw input for the requirement extraction process.

---

### 4. Created Requirement Schema

A reusable requirement schema was created in:

```text
requirement_schema.json
```

The schema defines the target structure for extracted requirement data.

The schema includes the following top-level sections:

```text
api
headers
requestBody
responseBody
businessRules
externalServices
database
validations
traceability
gaps
```

This schema provides a consistent contract for downstream automation.

---

### 5. Created Extracted Requirement Example

An extracted requirement example was created in:

```text
extracted_requirement_example.json
```

This file demonstrates how the sample Confluence-style requirement can be converted into structured JSON.

The extracted example includes:

* API metadata
* Header definitions
* Flattened request body fields
* Allowed enum values
* Success and error response structures
* Structured business rules
* External service configuration
* HMAC authentication details
* Request and response mappings
* Database table references
* Validation rules
* Traceability records
* Gap records

This example serves as the expected output pattern for future extractor implementation.

---

### 6. Created Requirement Extraction Rules

A formal extraction rules document was created in:

```text
extraction_rules.md
```

The extraction rules define how the system should identify, classify, and structure information from a requirement document.

The rules cover:

* API metadata extraction
* Header extraction
* Request body field extraction
* Enum value extraction
* Response body extraction
* Business rule extraction
* External service extraction
* Request and response mapping extraction
* Database table extraction
* Validation extraction
* Traceability generation
* Gap identification
* Enterprise naming preservation
* Hallucination prevention
* Unknown value handling
* Future LLM enhancement
* Schema alignment

These rules will guide the future implementation of the extractor logic.

---

## Day 5 Artifacts Created

By the end of Day 5, the following files were created:

```text
day5/
├── README.md
├── sample_confluence_requirement.md
├── requirement_schema.json
├── extracted_requirement_example.json
├── extraction_rules.md
└── day5_completion_report.md
```

---

## Key Technical Decisions

### 1. Structured Requirement JSON as the Core Contract

The structured requirement JSON will act as the central contract between the source requirement document and all generated artifacts.

This contract will later feed:

* Node-RED workflow generation
* API Factory Excel generation
* Gap analysis
* Traceability matrix generation
* Code generation readiness checks

---

### 2. Preserve Enterprise Naming

The extraction layer must preserve exact enterprise names.

Examples include:

```text
requestorInfo-clientId
requestorInfo-subclientId
requestorinfo-version
x-macys-apikey
x-hmac-secret
account.encryptInfo.keyType
transaction.items[].classNbr
Disc_By_Dept_Vnd_Class
Addtnl_Disc_Dept_Vnd_Class_Exclusions
```

Renaming these fields could break downstream mapping and code generation.

---

### 3. Use Gap Detection Instead of Assumptions

The extractor should not invent missing information.

If SQL queries, downstream URLs, error mappings, authentication details, response structures, or business rule behavior are missing, they should be captured in the `gaps` section.

This makes the system more trustworthy and easier to evaluate.

---

### 4. Start with Rule-Based Extraction

The initial extractor should be rule-based and deterministic.

This gives the project a reliable baseline before adding LLM enhancement.

A rule-based baseline also makes evaluation easier because the output is more predictable.

---

### 5. Use LLMs as an Enhancement Layer

LLMs can later improve business rule classification, gap detection, input/output identification, and traceability notes.

However, LLM output must still follow the schema and extraction rules defined in Day 5.

The LLM should improve extraction quality, not replace the structured contract.

---

## How Day 5 Connects to the Overall 50-Day Challenge

Day 5 supports the first major technical milestone of the challenge:

```text
Requirement Understanding Layer
```

This layer is the first step in the overall automation pipeline:

```text
Confluence Requirement Document
        ↓
Structured Requirement JSON
        ↓
Node-RED Flow Generation
        ↓
API Factory Excel Workbook Generation
        ↓
Gap Analysis
        ↓
Traceability Matrix
        ↓
Code Generation Readiness
```

Without this structured requirement layer, the later automation stages would not have a reliable source of truth.

---

## Evaluation Direction

The Day 5 artifacts also prepare the project for future evaluation.

Possible evaluation metrics include:

| Evaluation Area             | Possible Metric                        |
| --------------------------- | -------------------------------------- |
| API metadata extraction     | Field-level accuracy                   |
| Header extraction           | Precision and recall                   |
| Request body extraction     | Field coverage                         |
| Enum extraction             | Allowed-value accuracy                 |
| Business rule extraction    | Rule identification accuracy           |
| External service extraction | Service mapping accuracy               |
| Database extraction         | Table reference accuracy               |
| Validation extraction       | Validation coverage                    |
| Traceability generation     | Requirement-to-artifact coverage       |
| Gap detection               | Missing-requirement detection accuracy |

These metrics can later be used to compare:

* Manual requirement extraction
* Rule-based extraction
* LLM-enhanced extraction

---

## Final Day 5 Summary

Day 5 successfully established the foundation for converting enterprise API requirement documents into structured requirement JSON.

The project now has:

* A clear Day 5 README
* A realistic sample Confluence-style requirement
* A reusable requirement schema
* A completed extracted JSON example
* A formal extraction rules document
* A completion report

This prepares the project for Day 6, where the actual extractor implementation can begin.

---

## Status

Day 5 completed successfully.
