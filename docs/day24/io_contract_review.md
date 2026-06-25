# Day 24 — Phase 2: Input/Output Contract Review

## Purpose

This document reviews the input and output contracts required for the first implementation phase of the 50-Day AI Software Engineer Challenge.

The goal of this phase is to make sure every major artifact has a clear contract before coding begins on Day 25.

A contract defines:

* What input is accepted
* What output is produced
* What fields are required
* What fields are optional
* What format must be followed
* How missing or unclear information should be handled

This review is important because the future system will depend on consistent data movement between requirement extraction, workflow generation, Excel generation, gap analysis, and factory LLM code generation.

---

## Overall System Flow

The long-term system flow is:

```text
Confluence Requirement
        ↓
Requirement Extraction Output
        ↓
Workflow JSON
        ↓
Excel Intake Sheet
        ↓
Gap Analysis Report
        ↓
Factory LLM Code Generation
        ↓
Generated API Prototype
```

Each step must produce an output that can be safely used as the input for the next step.

---

# 1. Confluence Requirement Input Contract

## Purpose

The Confluence requirement input represents the original business and technical requirement provided by clients, product owners, or engineering teams.

For Day 25, this input will be stored as a markdown file.

---

## Input File Format

Expected format:

```text
.md
```

Example file:

```text
datasets/confluence_requirements/REQ-001-feed-discount.md
```

---

## Required Input Characteristics

The input should be plain text or markdown.

It may contain:

* API overview
* Endpoint details
* Request format
* Response format
* Business rules
* Validation rules
* Downstream service details
* Database details
* Error scenarios
* Assumptions
* Open questions

---

## Example Input Sections

A requirement document may include sections like:

```text
# API Name

## Endpoint

## Request Headers

## Request Body

## Response Body

## Business Rules

## Validation Rules

## Downstream Systems

## Database Tables

## Error Handling

## Notes
```

However, the extraction system should not depend completely on these exact headings.

---

## Required Fields to Extract

The extractor should attempt to identify the following information:

| Field                   | Description                         | Required for Extraction Output   |
| ----------------------- | ----------------------------------- | -------------------------------- |
| requirement_id          | Unique ID for the requirement       | Yes                              |
| api_name                | Name of the API                     | Yes, but can be empty if unknown |
| endpoint                | API endpoint path                   | Yes, but can be empty if unknown |
| method                  | HTTP method                         | Yes, but can be empty if unknown |
| description             | Short API purpose                   | Yes, but can be empty if unknown |
| request_headers         | Headers expected from client        | Yes                              |
| request_body_fields     | Request body fields                 | Yes                              |
| response_fields         | Response body fields                | Yes                              |
| business_rules          | Business logic rules                | Yes                              |
| validation_rules        | Request validation rules            | Yes                              |
| downstream_dependencies | External services or APIs           | Yes                              |
| database_dependencies   | Tables, queries, or DB dependencies | Yes                              |
| error_scenarios         | Known error cases                   | Yes                              |
| assumptions             | Accepted assumptions                | Yes                              |
| open_questions          | Missing or unclear details          | Yes                              |

---

## Handling Missing Information

The extractor should not hallucinate missing details.

If a section is missing:

* String fields should be returned as empty strings
* List fields should be returned as empty arrays
* Unclear information should be captured under `open_questions`

Example:

```json
{
  "endpoint": "",
  "open_questions": [
    "Endpoint path is not clearly mentioned in the requirement document."
  ]
}
```

---

# 2. Requirement Extraction Output Contract

## Purpose

The requirement extraction output is the first structured version of the raw requirement document.

It converts unstructured or semi-structured requirement text into machine-readable JSON.

This output becomes the input for workflow generation and Excel generation.

---

## Output File Format

Expected format:

```text
.json
```

Example output file:

```text
outputs/day25/extracted_requirements/REQ-001-extraction.json
```

---

## Required JSON Structure

```json
{
  "requirement_id": "REQ-001",
  "source_file": "datasets/confluence_requirements/REQ-001-feed-discount.md",
  "api_name": "",
  "endpoint": "",
  "method": "",
  "description": "",
  "request_headers": [],
  "request_body_fields": [],
  "response_fields": [],
  "business_rules": [],
  "validation_rules": [],
  "downstream_dependencies": [],
  "database_dependencies": [],
  "error_scenarios": [],
  "assumptions": [],
  "open_questions": [],
  "extraction_metadata": {
    "extraction_version": "v1",
    "extraction_method": "rule_based",
    "confidence_level": "initial"
  }
}
```

---

## Field-Level Contract

### requirement_id

Unique ID for the requirement.

Example:

```json
"requirement_id": "REQ-001"
```

Rules:

* Must not be null
* Should match the source requirement file where possible
* Can be derived from filename if not present inside the document

---

### source_file

Path of the source requirement file.

Example:

```json
"source_file": "datasets/confluence_requirements/REQ-001-feed-discount.md"
```

Rules:

* Must contain the relative file path
* Helps trace generated output back to original input

---

### api_name

Name of the API.

Example:

```json
"api_name": "Evaluate Transaction Discount"
```

Rules:

* Should be extracted from title or API overview
* Empty string is allowed if unavailable

---

### endpoint

API endpoint path.

Example:

```json
"endpoint": "/api/v1/discount/evaluateTransactionDiscount"
```

Rules:

* Should begin with `/` when available
* Empty string is allowed if unavailable

---

### method

HTTP method.

Example:

```json
"method": "POST"
```

Rules:

* Should be uppercase
* Expected values include `GET`, `POST`, `PUT`, `PATCH`, `DELETE`
* Empty string is allowed if unavailable

---

### description

Short explanation of the API purpose.

Example:

```json
"description": "Evaluates whether a transaction is eligible for new account or employee discount."
```

Rules:

* Should be concise
* Should not include unrelated notes
* Empty string is allowed if unavailable

---

### request_headers

List of request headers expected by the API.

Example:

```json
"request_headers": [
  {
    "name": "requestorinfo-clientid",
    "required": true,
    "description": "Client identifier"
  },
  {
    "name": "requestorinfo-subclientid",
    "required": true,
    "description": "Sub-client identifier"
  }
]
```

Rules:

* Should be an array
* Each item should include `name`, `required`, and `description`
* If required status is unclear, use `required: null`

---

### request_body_fields

List of request body fields.

Example:

```json
"request_body_fields": [
  {
    "path": "account.encryptInfo.encryptedData",
    "type": "string",
    "required": true,
    "description": "Encrypted account information"
  }
]
```

Rules:

* Use dot notation for nested fields
* If type is unknown, use empty string
* If required status is unclear, use `required: null`

---

### response_fields

List of response fields.

Example:

```json
"response_fields": [
  {
    "path": "discountDetails[].discountEligible",
    "type": "boolean",
    "description": "Indicates whether the item is eligible for discount"
  }
]
```

Rules:

* Use dot notation for nested fields
* Use `[]` to represent array items
* If type is unknown, use empty string

---

### business_rules

List of business rules extracted from the requirement.

Example:

```json
"business_rules": [
  "If the account is a new account, apply the configured new account discount percentage.",
  "Discount amount must not exceed the remaining account cap."
]
```

Rules:

* Each rule should be a separate string
* Rules should be written clearly
* Avoid combining multiple unrelated rules into one item

---

### validation_rules

List of request validation rules.

Example:

```json
"validation_rules": [
  "transactionDetails.transactionId is required.",
  "account.accountToken is required.",
  "transactionDetails.transactionAmount must be greater than zero."
]
```

Rules:

* Each validation should be atomic
* Required field checks should be listed separately
* Enum constraints should be captured where available

---

### downstream_dependencies

List of downstream APIs, services, or external systems.

Example:

```json
"downstream_dependencies": [
  {
    "name": "PIIDLookup",
    "type": "REST API",
    "purpose": "Retrieve account details from encrypted PIID."
  }
]
```

Rules:

* Should include service name when available
* Type may be `REST API`, `SOAP API`, `GraphQL`, `Database`, or empty string
* Purpose should explain why the dependency is used

---

### database_dependencies

List of database tables or database operations.

Example:

```json
"database_dependencies": [
  {
    "table": "New_Acct_Hdr",
    "operation": "SELECT/UPDATE",
    "purpose": "Read and update new account discount cap balance."
  }
]
```

Rules:

* Should include table name where available
* Operation can be empty if unknown
* Purpose should explain how the database is used

---

### error_scenarios

List of known error cases.

Example:

```json
"error_scenarios": [
  {
    "condition": "Missing required request field",
    "status_code": 400,
    "message": "Bad Request"
  },
  {
    "condition": "Downstream PIIDLookup failure",
    "status_code": 502,
    "message": "Downstream service error"
  }
]
```

Rules:

* Status code can be null if unknown
* Message can be empty if unknown
* Condition should be clearly described

---

### assumptions

List of assumptions accepted during extraction.

Example:

```json
"assumptions": [
  "Input requirement is provided as a markdown file.",
  "Missing optional fields are represented as empty values."
]
```

Rules:

* Assumptions must be explicit
* Assumptions should not replace missing business requirements
* Risky assumptions should be moved to `open_questions`

---

### open_questions

List of missing or unclear items.

Example:

```json
"open_questions": [
  "The requirement does not clearly specify all possible error responses.",
  "The exact downstream timeout behavior is not mentioned."
]
```

Rules:

* Must capture gaps instead of inventing answers
* Should be written as clear questions or missing-detail statements
* Should be useful for client or engineer review

---

### extraction_metadata

Metadata about the extraction process.

Example:

```json
"extraction_metadata": {
  "extraction_version": "v1",
  "extraction_method": "rule_based",
  "confidence_level": "initial"
}
```

Rules:

* Used for traceability
* Helps compare extraction quality across versions
* Should be included in every output

---

# 3. Workflow JSON Output Contract

## Purpose

The workflow JSON represents the API as a sequence of logical processing steps.

It is created after requirement extraction.

The workflow JSON should help future modules generate:

* Node-RED flows
* Excel workflow sheets
* Go service orchestration
* Test scenarios
* Gap analysis reports

---

## Expected Workflow Output Format

```json
{
  "workflow_id": "REQ-001-WF",
  "requirement_id": "REQ-001",
  "api_name": "",
  "endpoint": "",
  "method": "",
  "workflow_steps": []
}
```

---

## Workflow Step Structure

Each workflow step should follow this structure:

```json
{
  "step_id": "STEP-001",
  "step_name": "Validate Request",
  "step_type": "validation",
  "description": "Validate required request fields.",
  "inputs": [],
  "outputs": [],
  "rules": [],
  "dependencies": [],
  "error_handling": []
}
```

---

## Workflow Step Types

Allowed initial step types:

| Step Type           | Meaning                                      |
| ------------------- | -------------------------------------------- |
| input_normalization | Normalize request headers, params, or body   |
| validation          | Validate required fields, enums, and formats |
| transformation      | Transform request values                     |
| downstream_call     | Call external REST, SOAP, or GraphQL service |
| database_read       | Read from database                           |
| database_write      | Insert or update database                    |
| business_rule       | Apply business logic                         |
| response_mapping    | Map internal result to API response          |
| error_handling      | Handle failure or exception path             |

---

## Workflow Contract Rules

The workflow JSON should:

* Preserve the original `requirement_id`
* Preserve API endpoint and method
* Break logic into ordered steps
* Make dependencies explicit
* Capture errors at the step level where possible
* Avoid implementation-specific code in early versions

---

# 4. Excel Intake Output Contract

## Purpose

The Excel intake sheet is the structured artifact that a factory LLM can use to generate code.

It should represent the API contract, business logic, data mapping, validations, dependencies, and error handling in a highly structured format.

---

## Expected Excel Workbook

Example output:

```text
outputs/excel/REQ-001-feed-discount.xlsx
```

---

## Expected Sheets

Initial workbook should include:

| Sheet Name              | Purpose                                 |
| ----------------------- | --------------------------------------- |
| api_endpoint            | API-level endpoint information          |
| request_headers         | Required and optional request headers   |
| request_body            | Request body fields                     |
| response_body           | Response structure                      |
| business_rules          | Business logic rules                    |
| validations             | Required fields, enums, and constraints |
| downstream_dependencies | External API calls                      |
| database_mapping        | Database tables and field mappings      |
| error_handling          | Error scenarios and responses           |
| open_questions          | Missing or unclear items                |

---

## Excel Contract Rules

The Excel output should:

* Use consistent sheet names
* Use consistent column names
* Avoid merged cells
* Avoid vague values
* Use one row per field, rule, or dependency
* Preserve traceability to the original requirement
* Be machine-readable by the factory LLM
* Avoid assumptions unless explicitly marked

---

# 5. Gap Analysis Report Output Contract

## Purpose

The gap analysis report identifies missing, unclear, risky, or conflicting requirement details before implementation.

This helps reduce incorrect code generation from incomplete requirements.

---

## Expected Output Format

The gap report may be generated as markdown or JSON.

Recommended markdown output:

```text
outputs/gap_analysis/REQ-001-gap-analysis.md
```

---

## Gap Categories

The gap analysis should classify issues into the following categories:

| Category                | Description                                      |
| ----------------------- | ------------------------------------------------ |
| missing_required_detail | Required implementation detail is missing        |
| ambiguous_requirement   | Requirement can be interpreted in multiple ways  |
| conflicting_requirement | Two requirement statements contradict each other |
| unsupported_assumption  | System would need to assume something not stated |
| downstream_gap          | Missing downstream system details                |
| database_gap            | Missing database table, query, or mapping detail |
| validation_gap          | Missing validation rule                          |
| error_handling_gap      | Missing error behavior                           |
| response_mapping_gap    | Missing or unclear response field mapping        |

---

## Gap Report Structure

Recommended markdown structure:

```text
# Gap Analysis Report

## Requirement Summary

## Critical Gaps

## Medium Gaps

## Low Gaps

## Open Questions

## Implementation Risk

## Recommendation
```

---

## Gap Severity Levels

| Severity | Meaning                             |
| -------- | ----------------------------------- |
| Critical | Blocks correct implementation       |
| Medium   | Can be implemented, but risk exists |
| Low      | Minor clarification needed          |

---

## Gap Report Rules

The report should:

* Be specific
* Reference the missing or unclear requirement area
* Avoid generic statements
* Clearly explain why each gap matters
* Separate true gaps from acceptable assumptions
* Recommend whether implementation can proceed

---

# 6. Factory LLM Input Contract

## Purpose

The factory LLM input is the final structured package given to an LLM or code-generation factory.

The quality of this input directly affects the quality of generated code.

---

## Factory LLM Input Package

The factory should receive:

```text
1. Extracted requirement JSON
2. Workflow JSON
3. Excel intake workbook
4. Gap analysis report
5. Approved assumptions
6. Target framework instructions
```

---

## Factory LLM Contract Rules

The factory input should provide:

* Endpoint path
* HTTP method
* Request headers
* Request body schema
* Response schema
* Validation rules
* Business rules
* Downstream calls
* Database mappings
* Error scenarios
* Logging requirements
* Security requirements
* Config values
* Open questions
* Approved assumptions

---

## Information the Factory Must Not Invent

The factory LLM must not invent:

* New endpoints
* New request fields
* New response fields
* New database tables
* New downstream services
* Business rules not present in the requirement
* Validation rules not specified
* Error responses not documented or approved
* Security behavior not mentioned
* Config values not provided

Any missing information must be flagged as a gap.

---

# 7. Contract Validation Checklist

Before coding starts, each contract should be reviewed against the following checklist.

## Confluence Requirement Input

* [ ] Requirement file exists
* [ ] Requirement ID is identifiable
* [ ] API purpose is described
* [ ] Endpoint is present or gap is captured
* [ ] Method is present or gap is captured
* [ ] Request details are available or gap is captured
* [ ] Response details are available or gap is captured
* [ ] Business rules are available or gap is captured
* [ ] Dependencies are available or gap is captured

---

## Requirement Extraction Output

* [ ] Output is valid JSON
* [ ] Required top-level fields exist
* [ ] Missing values are not hallucinated
* [ ] Open questions are captured
* [ ] Source file is preserved
* [ ] Extraction metadata is included

---

## Workflow JSON

* [ ] Workflow ID exists
* [ ] Requirement ID is preserved
* [ ] Steps are ordered
* [ ] Step types are valid
* [ ] Inputs and outputs are clear
* [ ] Dependencies are explicit
* [ ] Error handling is captured where possible

---

## Excel Intake

* [ ] Workbook has expected sheets
* [ ] Sheet names are consistent
* [ ] Column names are consistent
* [ ] Each row represents one field, rule, or dependency
* [ ] Open questions are included
* [ ] No unsupported assumptions are hidden

---

## Gap Analysis Report

* [ ] Missing details are identified
* [ ] Ambiguities are identified
* [ ] Risks are categorized by severity
* [ ] Open questions are clearly written
* [ ] Recommendation is provided
* [ ] Implementation readiness is stated

---

# 8. Day 25 Contract Priority

For Day 25, the highest-priority contract is the requirement extraction output contract.

The Day 25 implementation must focus on producing this output:

```text
outputs/day25/extracted_requirements/REQ-001-extraction.json
```

The workflow JSON, Excel workbook, and gap analysis report are future outputs, but their contracts are reviewed now so that Day 25 does not build the extraction layer in a way that blocks later phases.

---

# 9. Final Review Summary

The input/output contract review confirms that the project is ready to begin implementation from Day 25.

The first implementation should focus on the requirement extraction contract because it is the foundation for every later artifact.

A clean extraction output will allow the project to later generate workflow JSON, Excel intake sheets, gap analysis reports, and factory LLM inputs in a consistent and traceable way.

---
