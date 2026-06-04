# System Architecture

## 1. Introduction

This document defines the system architecture for the AI-driven Enterprise API Factory.

The proposed system transforms semi-structured enterprise requirement documents into implementation-ready artifacts, including structured requirement metadata, Node-RED workflow definitions, Go Factory Excel intake sheets, and requirement gap reports.

The architecture is designed to support enterprise API modernization by reducing the manual effort required to interpret business requirements and convert them into technical artifacts.

---

## 2. Architecture Objective

The objective of the system architecture is to create a repeatable and measurable pipeline that converts requirement documents into engineering artifacts.

The system should support the following capabilities:

1. Ingest enterprise requirement documents.
2. Extract structured API metadata.
3. Identify validations, business rules, downstream services, and database dependencies.
4. Generate Node-RED workflow JSON.
5. Generate Go Factory Excel intake sheets.
6. Detect missing or ambiguous requirement details.
7. Produce reviewable artifacts for engineers.

The architecture is not intended to fully replace engineers. Instead, it accelerates the preparation phase of enterprise API development while preserving human review and validation.

---

## 3. High-Level System Flow

The complete system flow is:

```text
Confluence Requirement Document
        ↓
Requirement Preprocessor
        ↓
AI Requirement Extractor
        ↓
Structured Requirement JSON
        ↓
Workflow Generator
        ↓
Node-RED Flow JSON
        ↓
Excel Generator
        ↓
Go Factory Excel Intake Sheet
        ↓
Gap Analyzer
        ↓
Gap Report
        ↓
Human Review
        ↓
Factory Code Generation
```

---

## 4. Core Components

The proposed architecture consists of the following components:

| Component                    | Purpose                                               |
| ---------------------------- | ----------------------------------------------------- |
| Requirement Preprocessor     | Cleans and normalizes raw requirement documents       |
| AI Requirement Extractor     | Extracts structured metadata from requirements        |
| Requirement Schema Validator | Validates extracted output against a fixed schema     |
| Workflow Generator           | Generates Node-RED orchestration flow JSON            |
| Workflow Validator           | Ensures generated Node-RED flow is structurally valid |
| Excel Generator              | Generates Go Factory intake sheets                    |
| Excel Schema Validator       | Ensures sheets match expected factory format          |
| Gap Analyzer                 | Detects missing or ambiguous requirement details      |
| Evaluation Engine            | Scores generated artifacts against ground truth       |
| Human Review Interface       | Allows engineers to review and correct outputs        |

---

## 5. Component 1: Requirement Preprocessor

### Purpose

The Requirement Preprocessor converts raw enterprise documentation into cleaner input for AI extraction.

Enterprise requirements may contain:

* Long paragraphs
* Tables
* Inconsistent formatting
* Screenshots
* Partial examples
* Repeated sections
* Business notes
* Technical notes
* Legacy terminology

The preprocessor standardizes this content before extraction.

### Inputs

```text
Raw Confluence export
Markdown file
Text file
HTML export
PDF text extraction
```

### Outputs

```text
Normalized requirement text
Section-aware requirement blocks
Detected tables
Detected endpoint sections
Detected business rule sections
```

### Responsibilities

* Remove unnecessary formatting.
* Preserve important tables.
* Identify endpoint sections.
* Identify request and response examples.
* Preserve business rule wording.
* Split long documents into manageable chunks.
* Remove sensitive values when required.

---

## 6. Component 2: AI Requirement Extractor

### Purpose

The AI Requirement Extractor converts normalized requirement text into structured metadata.

### Input

```text
Normalized requirement document
```

### Output

```json
{
  "apiName": "",
  "method": "",
  "endpointPath": "",
  "headers": [],
  "pathParameters": [],
  "queryParameters": [],
  "requestBody": [],
  "responseBody": [],
  "validations": [],
  "businessRules": [],
  "downstreamServices": [],
  "databaseMappings": [],
  "errorMappings": []
}
```

### Extracted Fields

The extractor identifies:

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
* Downstream REST services
* SOAP services
* Database tables
* SQL/query requirements
* Error responses
* Security rules

### LLM Strategy

During local development, the system can use:

```text
Ollama + Mistral / Llama
```

For enterprise deployment, the system can use:

```text
Vertex AI / Gemini / enterprise-approved LLM
```

The extractor should be prompt-driven initially and later improved using schema-guided generation.

---

## 7. Component 3: Requirement Schema Validator

### Purpose

The Requirement Schema Validator ensures that the AI-generated requirement extraction follows the expected structure.

### Validation Checks

* Required top-level fields exist.
* HTTP method is valid.
* Endpoint path starts with `/`.
* Headers are represented consistently.
* Request fields contain names and types where available.
* Business rules contain IDs or generated IDs.
* Downstream services include service type.
* Database mappings include table and operation where available.
* Error mappings include status code and message where available.

### Output

```text
Validated structured requirement JSON
Validation errors
Warnings
Missing fields
```

This component prevents malformed AI output from entering downstream generation steps.

---

## 8. Component 4: Workflow Generator

### Purpose

The Workflow Generator converts structured requirement JSON into a Node-RED flow.

### Input

```json
Structured Requirement JSON
```

### Output

```json
Node-RED Flow JSON
```

### Generated Node Types

The workflow generator can create:

* HTTP In node
* Header validation node
* Path parameter validation node
* Query parameter validation node
* Body validation node
* Business rule function node
* SOAP config builder node
* REST request node
* Database query node
* Response mapper node
* Error handler node
* HTTP response node

### Workflow Pattern Examples

#### REST Wrapper Pattern

```text
HTTP In
  ↓
Validate Headers
  ↓
Validate Body
  ↓
Build Downstream Request
  ↓
Invoke REST Service
  ↓
Map Response
  ↓
HTTP Response
```

#### REST-to-SOAP Wrapper Pattern

```text
HTTP In
  ↓
Validate Headers / Params
  ↓
Normalize Request
  ↓
Build SOAP XML Config
  ↓
Invoke SOAP Wrapper
  ↓
Parse SOAP Response
  ↓
Map SOAP Result to REST
  ↓
HTTP Response
```

#### REST + Database Orchestration Pattern

```text
HTTP In
  ↓
Validate Headers
  ↓
Validate Body
  ↓
Invoke Downstream Service
  ↓
Prepare DB Context
  ↓
Execute DB Queries
  ↓
Apply Business Rules
  ↓
Map Final Response
  ↓
HTTP Response
```

---

## 9. Component 5: Workflow Validator

### Purpose

The Workflow Validator checks whether the generated Node-RED flow is structurally valid and aligned with the extracted requirement.

### Validation Checks

* Flow JSON is valid.
* Required nodes exist.
* HTTP method and path match requirement.
* Nodes are wired correctly.
* SOAP APIs include SOAP config builder.
* REST downstream APIs include request node.
* DB-based APIs include query node.
* Error path exists.
* Final HTTP response node exists.

### Output

```text
Workflow validation report
Valid / invalid status
Missing node warnings
Wiring warnings
```

---

## 10. Component 6: Excel Generator

### Purpose

The Excel Generator converts extracted requirements and workflow metadata into Go Factory intake sheets.

### Input

```text
Structured Requirement JSON
Node-RED Workflow JSON
```

### Output

```text
Go Factory Excel Workbook
```

### Generated Sheets

The Excel Generator may produce:

* `technical_requirements`
* `api_endpoints`
* `entities`
* `external_services`
* `business_requirements`
* `endpoint_service_mapping`
* `queries`
* `request_response`
* `security`
* `error_handling`

### Generation Strategy

The Excel Generator maps requirement metadata into the fixed factory schema.

| Requirement Field  | Excel Sheet                              |
| ------------------ | ---------------------------------------- |
| Endpoint path      | `api_endpoints`                          |
| HTTP method        | `api_endpoints`                          |
| Header validations | `api_endpoints`, `business_requirements` |
| Downstream service | `external_services`                      |
| Business rules     | `business_requirements`                  |
| Orchestration flow | `endpoint_service_mapping`               |
| Database tables    | `entities`                               |
| SQL logic          | `queries`                                |
| Error behavior     | `error_handling`                         |

---

## 11. Component 7: Excel Schema Validator

### Purpose

The Excel Schema Validator checks whether generated Excel sheets follow the factory-required format.

### Validation Checks

* Required sheets exist.
* Required columns exist.
* Mandatory rows are populated.
* Endpoint IDs are consistent across sheets.
* Business rule IDs are referenced correctly.
* Service IDs are referenced correctly.
* Query IDs are referenced correctly.
* No unsupported technology values are used.
* PostgreSQL is used when requirement specifies PostgreSQL.
* Prototype-only details such as SQLite are not incorrectly carried into final factory sheets.

### Output

```text
Excel validation report
Missing sheet errors
Missing column errors
Reference mismatch warnings
Factory readiness score
```

---

## 12. Component 8: Gap Analyzer

### Purpose

The Gap Analyzer detects missing, incomplete, or ambiguous requirement details before implementation.

### Gap Categories

| Gap Type                  | Example                                     |
| ------------------------- | ------------------------------------------- |
| Missing Header            | Required header list is incomplete          |
| Missing Validation        | Enum values are not provided                |
| Missing Response          | Success or error response example is absent |
| Missing Downstream Detail | Downstream URL or method is not specified   |
| Missing DB Detail         | Table or column mapping is missing          |
| Missing Error Handling    | No mapping for downstream failure           |
| Ambiguous Requirement     | Optional field behavior is unclear          |
| Security Gap              | HMAC or token behavior is incomplete        |

### Output

```md
# Gap Report

## Missing Information

## Ambiguous Information

## Implementation Risks

## Questions for Requirement Owner

## Readiness Score
```

---

## 13. Component 9: Evaluation Engine

### Purpose

The Evaluation Engine measures generated artifacts against ground truth artifacts.

### Evaluation Metrics

| Metric                          | Formula                                                          |
| ------------------------------- | ---------------------------------------------------------------- |
| Requirement Extraction Accuracy | Correct extracted fields / Total expected fields                 |
| Workflow Generation Accuracy    | Correct workflow components / Total expected workflow components |
| Excel Generation Accuracy       | Correct Excel mappings / Total expected mappings                 |
| Gap Detection Precision         | Correct detected gaps / Total detected gaps                      |
| Gap Detection Recall            | Correct detected gaps / Actual known gaps                        |
| Time Reduction                  | Manual time saved / Manual baseline time                         |

### Output

```text
Evaluation report per API
Aggregate evaluation report
Error analysis
```

---

## 14. Human Review Layer

The system includes human review as a required step.

### Purpose

Human review ensures that generated artifacts are correct before code generation.

### Reviewer Responsibilities

* Confirm extracted requirements.
* Validate workflow structure.
* Review Excel factory sheets.
* Confirm detected gaps.
* Resolve ambiguous requirements.
* Approve factory code generation readiness.

This human-in-the-loop design improves reliability and makes the system practical for enterprise use.

---

## 15. Data Flow Between Components

```text
[Requirement Document]
        ↓
[Requirement Preprocessor]
        ↓
[AI Requirement Extractor]
        ↓
[Requirement Schema Validator]
        ↓
[Workflow Generator]
        ↓
[Workflow Validator]
        ↓
[Excel Generator]
        ↓
[Excel Schema Validator]
        ↓
[Gap Analyzer]
        ↓
[Human Review]
        ↓
[Factory Code Generation]
```

---

## 16. Example End-to-End Case

Example API:

```text
FEED Interrogate Account
```

### Input

```text
Confluence requirement describing account interrogation API
```

### Extracted Metadata

```text
POST /api/v1/discount/interrogateAccount
Required requestorInfo headers
Account encryptInfo request body
Entry mode enum validation
PIIDLookup downstream service
AccountInfo response mapping
```

### Generated Workflow

```text
HTTP In
  ↓
Validate Headers
  ↓
Validate Body
  ↓
Validate Enum Fields
  ↓
Build PIIDLookup Request
  ↓
Invoke PIIDLookup
  ↓
Validate Downstream Response
  ↓
Map AccountInfo Response
  ↓
HTTP Response
```

### Generated Excel Sheets

```text
api_endpoints
external_services
business_requirements
endpoint_service_mapping
request_response
error_handling
```

### Gap Report Examples

```text
Missing retry policy
Missing downstream timeout
Ambiguous HMAC validation source
Incomplete downstream 5xx error mapping
```

---

## 17. Architecture Benefits

The architecture provides several benefits:

* Reduces manual requirement interpretation effort.
* Produces consistent technical artifacts.
* Improves traceability from requirements to implementation.
* Detects missing requirement details early.
* Supports factory-based Go code generation.
* Enables measurable evaluation.
* Preserves human review for enterprise reliability.

---

## 18. Architecture Limitations

The architecture has limitations:

* AI extraction may miss deeply implicit requirements.
* Generated workflows require validation.
* Excel generation depends on factory schema stability.
* Complex business rules may require manual correction.
* Sensitive enterprise data must be anonymized.
* Evaluation requires manually prepared ground truth.

These limitations will be addressed through validation, human review, prompt improvement, and schema-based generation.

---

## 19. Summary

This system architecture defines the AI-driven Enterprise API Factory as a multi-stage pipeline that transforms enterprise requirement documents into implementation-ready artifacts.

The architecture combines requirement extraction, workflow synthesis, Excel intake generation, gap analysis, validation, and human review.

This design supports the broader research objective of reducing enterprise API development preparation effort while maintaining high artifact accuracy.
