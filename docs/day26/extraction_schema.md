# Day 26 — Phase 1: Standard Extraction Schema

## Purpose

This document defines the standard extraction schema for the API Factory Generator project.

Day 25 confirmed that the existing TypeScript and Next.js base is already aligned with the original 50-day project roadmap. The project already supports requirement input, extraction, business-rule configuration, SOAP wrapper support, and Node-RED flow generation.

Day 26 focuses on standardizing the extraction output so that every requirement document is converted into one consistent structure before being used by downstream modules.

The goal is to prevent inconsistent field names, incomplete mappings, and hallucinated implementation details.

---

## Why a Standard Extraction Schema Is Needed

The API Factory Generator will eventually support this full pipeline:

```text
Confluence Requirement
        ↓
Requirement Extraction
        ↓
Standard Extraction Schema
        ↓
Business Rule Generation
        ↓
Node-RED Flow Generation
        ↓
Excel Intake Generation
        ↓
Gap Analysis
        ↓
Factory LLM Code Generation
```

Without a standard schema, each module may expect different field names or structures.

For example, one extraction output may use:

```text
headers
```

Another may use:

```text
requestHeaders
```

Another may use:

```text
requiredHeaders
```

This creates problems for Node-RED generation, Excel generation, gap analysis, evaluation metrics, and future code generation.

The standard extraction schema solves this by defining one stable contract.

---

## Day 26 Objective

The Day 26 objective is:

```text
Create a stable, traceable, and reusable extraction schema that can support all future artifact generation.
```

This schema will become the bridge between raw requirement extraction and all downstream generation modules.

---

## Schema Design Principles

The standard extraction schema must follow these principles:

1. **Consistency**
   Every extraction output should use the same field names and structure.

2. **Traceability**
   Each extracted item should be traceable back to the source requirement when possible.

3. **No Hallucination**
   Missing information must not be invented. It should be captured under `openQuestions` or `gaps`.

4. **Extensibility**
   The schema should support future outputs such as Node-RED, Excel, gap analysis, and code generation.

5. **Machine Readability**
   The schema should be easy for TypeScript code, LLM prompts, and factory generators to consume.

6. **Human Reviewability**
   The schema should be readable enough for engineers to review and correct.

---

## Standard Extraction Output

The normalized extraction output should follow this top-level structure:

```json
{
  "requirementId": "REQ-001",
  "source": {
    "sourceType": "confluence",
    "sourceName": "",
    "sourcePath": "",
    "rawTextAvailable": true
  },
  "api": {
    "apiName": "",
    "description": "",
    "endpoint": {
      "method": "",
      "path": "",
      "version": "",
      "operationName": ""
    }
  },
  "request": {
    "headers": [],
    "pathParameters": [],
    "queryParameters": [],
    "bodyFields": []
  },
  "response": {
    "successFields": [],
    "errorFields": [],
    "examples": []
  },
  "businessRules": [],
  "validationRules": [],
  "downstreamDependencies": [],
  "databaseDependencies": [],
  "workflowHints": [],
  "nodeRedHints": [],
  "assumptions": [],
  "openQuestions": [],
  "gaps": [],
  "traceability": {
    "schemaVersion": "day26-v1",
    "extractionMethod": "rule_based",
    "llmUsed": false,
    "generatedAt": "",
    "generatedBy": "api-factory-generator"
  }
}
```

---

# 1. Top-Level Fields

## `requirementId`

Unique identifier for the requirement.

Example:

```json
"requirementId": "REQ-001"
```

Rules:

* Must be present.
* Can be extracted from requirement text, file name, or generated as a placeholder.
* If unknown, use `"UNKNOWN_REQUIREMENT"` and add an open question.

---

## `source`

Information about where the requirement came from.

Structure:

```json
"source": {
  "sourceType": "confluence",
  "sourceName": "",
  "sourcePath": "",
  "rawTextAvailable": true
}
```

Field rules:

| Field              | Type    | Description                                                                |
| ------------------ | ------- | -------------------------------------------------------------------------- |
| `sourceType`       | string  | Source type such as `confluence`, `markdown`, `manual`, or `uploaded_file` |
| `sourceName`       | string  | Name or title of the source document                                       |
| `sourcePath`       | string  | File path or source reference if available                                 |
| `rawTextAvailable` | boolean | Indicates whether raw requirement text was available during extraction     |

---

## `api`

API-level information.

Structure:

```json
"api": {
  "apiName": "",
  "description": "",
  "endpoint": {
    "method": "",
    "path": "",
    "version": "",
    "operationName": ""
  }
}
```

Field rules:

| Field                    | Type   | Description                                                    |
| ------------------------ | ------ | -------------------------------------------------------------- |
| `apiName`                | string | Human-readable API name                                        |
| `description`            | string | Short API purpose                                              |
| `endpoint.method`        | string | HTTP method such as `GET`, `POST`, `PUT`, `PATCH`, or `DELETE` |
| `endpoint.path`          | string | API endpoint path                                              |
| `endpoint.version`       | string | API version if available                                       |
| `endpoint.operationName` | string | Operation or business name                                     |

If endpoint path or method is missing, do not invent it. Add a gap and open question.

---

# 2. Request Schema

The request section captures all client input details.

Structure:

```json
"request": {
  "headers": [],
  "pathParameters": [],
  "queryParameters": [],
  "bodyFields": []
}
```

---

## Request Header Object

Each request header should follow this structure:

```json
{
  "name": "x-transaction-id",
  "required": true,
  "type": "string",
  "description": "Transaction correlation identifier",
  "example": "",
  "sourceText": "",
  "confidence": "high"
}
```

Field rules:

| Field         | Type            | Description                     |
| ------------- | --------------- | ------------------------------- |
| `name`        | string          | Exact header name               |
| `required`    | boolean or null | Whether the header is required  |
| `type`        | string          | Data type if known              |
| `description` | string          | Header purpose                  |
| `example`     | string          | Example value if available      |
| `sourceText`  | string          | Source text used for extraction |
| `confidence`  | string          | `high`, `medium`, or `low`      |

---

## Path Parameter Object

Each path parameter should follow this structure:

```json
{
  "name": "upcNumber",
  "required": true,
  "type": "string",
  "description": "UPC number from URL path",
  "example": "123456789012",
  "sourceText": "",
  "confidence": "high"
}
```

Rules:

* Use path parameters only when the endpoint path includes placeholders or when the requirement explicitly says the value comes from the URL.
* Do not move body fields into path parameters unless clearly documented.

---

## Query Parameter Object

Each query parameter should follow this structure:

```json
{
  "name": "storeNumber",
  "required": false,
  "type": "string",
  "description": "",
  "example": "",
  "sourceText": "",
  "confidence": "medium"
}
```

Rules:

* Query parameters should only be used when the requirement explicitly defines query string input.
* If query parameter location is unclear, add an open question.

---

## Request Body Field Object

Each request body field should follow this structure:

```json
{
  "path": "account.encryptInfo.encryptedData",
  "name": "encryptedData",
  "required": true,
  "type": "string",
  "description": "Encrypted account data",
  "example": "",
  "sourceText": "",
  "confidence": "high"
}
```

Field rules:

| Field         | Type            | Description                         |
| ------------- | --------------- | ----------------------------------- |
| `path`        | string          | Dot-notation path for nested fields |
| `name`        | string          | Field name                          |
| `required`    | boolean or null | Whether field is required           |
| `type`        | string          | Data type if known                  |
| `description` | string          | Field purpose                       |
| `example`     | string          | Example value if available          |
| `sourceText`  | string          | Source text used for extraction     |
| `confidence`  | string          | `high`, `medium`, or `low`          |

---

# 3. Response Schema

The response section captures success and error response details.

Structure:

```json
"response": {
  "successFields": [],
  "errorFields": [],
  "examples": []
}
```

---

## Success Response Field Object

```json
{
  "path": "result.cdtInfo.destinationDivision",
  "name": "destinationDivision",
  "type": "number",
  "description": "Destination division number",
  "nullable": false,
  "sourceText": "",
  "confidence": "high"
}
```

Rules:

* Use dot notation for nested objects.
* Use `[]` for arrays.
* Do not invent fields that are not present in the requirement.

---

## Error Response Field Object

```json
{
  "path": "errors[].message",
  "name": "message",
  "type": "string",
  "description": "Error message",
  "nullable": false,
  "sourceText": "",
  "confidence": "medium"
}
```

Rules:

* Capture error response fields if explicitly documented.
* If error format is missing, add a gap.

---

## Response Example Object

```json
{
  "exampleType": "success",
  "description": "Successful response example",
  "payload": {}
}
```

Allowed `exampleType` values:

```text
success
error
partial
unknown
```

---

# 4. Business Rules

Business rules define core API behavior.

Structure:

```json
{
  "ruleId": "BR-001",
  "name": "Validate Required Headers",
  "description": "Validate that all required REST headers are present.",
  "category": "validation",
  "condition": "Required headers are missing",
  "action": "Return validation error",
  "priority": 1,
  "sourceText": "",
  "confidence": "high"
}
```

Field rules:

| Field         | Type   | Description                     |
| ------------- | ------ | ------------------------------- |
| `ruleId`      | string | Stable rule identifier          |
| `name`        | string | Rule name                       |
| `description` | string | Human-readable rule description |
| `category`    | string | Rule category                   |
| `condition`   | string | When the rule applies           |
| `action`      | string | What the system should do       |
| `priority`    | number | Execution order if known        |
| `sourceText`  | string | Source text used for extraction |
| `confidence`  | string | `high`, `medium`, or `low`      |

Allowed categories:

```text
validation
normalization
transformation
business_logic
downstream_call
database
response_mapping
error_handling
metadata
```

---

# 5. Validation Rules

Validation rules are a specialized subset of business rules.

Structure:

```json
{
  "validationId": "VAL-001",
  "fieldPath": "request.headers.x-transaction-id",
  "ruleType": "required",
  "message": "x-transaction-id header is required",
  "severity": "error",
  "sourceText": "",
  "confidence": "high"
}
```

Allowed `ruleType` values:

```text
required
type
enum
format
range
length
conditional
custom
```

Allowed `severity` values:

```text
error
warning
info
```

---

# 6. Downstream Dependencies

Downstream dependencies include REST, SOAP, GraphQL, or other service calls.

Structure:

```json
{
  "dependencyId": "DEP-001",
  "name": "XCDTS005",
  "type": "SOAP",
  "method": "",
  "endpoint": "",
  "purpose": "Retrieve CDT transfer details",
  "requestMapping": [],
  "responseMapping": [],
  "errorHandling": [],
  "sourceText": "",
  "confidence": "medium"
}
```

Allowed dependency types:

```text
REST
SOAP
GraphQL
Database
Queue
File
Unknown
```

---

## Dependency Mapping Object

Used inside `requestMapping` and `responseMapping`.

```json
{
  "source": "request.body.upcNumber",
  "target": "soap.upc_number",
  "transformation": "pad or normalize if required",
  "required": true
}
```

Rules:

* Source and target should be explicit.
* If mapping is unclear, do not guess. Add a gap.

---

# 7. Database Dependencies

Database dependencies capture table, column, and operation details.

Structure:

```json
{
  "dependencyId": "DB-001",
  "databaseName": "",
  "tableName": "New_Acct_Hdr",
  "operation": "SELECT",
  "lookupKeys": [],
  "fieldMappings": [],
  "transactionRequired": false,
  "lockingRequired": false,
  "sourceText": "",
  "confidence": "medium"
}
```

Allowed operations:

```text
SELECT
INSERT
UPDATE
DELETE
UPSERT
UNKNOWN
```

---

# 8. Workflow Hints

Workflow hints help the system build ordered processing steps.

Structure:

```json
{
  "stepId": "STEP-001",
  "stepName": "Validate Request",
  "stepType": "validation",
  "description": "Validate required headers and request fields.",
  "dependsOn": [],
  "sourceRuleIds": ["BR-001", "BR-002"]
}
```

Allowed step types:

```text
input
validation
normalization
transformation
business_rule
downstream_call
database_read
database_write
response_mapping
error_handling
output
```

---

# 9. Node-RED Hints

Node-RED hints are generation-specific hints for producing Node-RED flow JSON.

Structure:

```json
{
  "nodeId": "NODE-001",
  "nodeType": "function",
  "nodeName": "Validate Required Headers",
  "sourceRuleId": "BR-001",
  "templateName": "requiredHeaders",
  "outputs": 2
}
```

Rules:

* Node-RED hints should not replace business rules.
* They should only guide Node-RED generation.
* Each generated node should remain traceable to a business rule.

---

# 10. Assumptions

Assumptions must be explicitly marked.

Structure:

```json
{
  "assumptionId": "ASM-001",
  "description": "Ollama enhancement is optional and not required for Day 26 validation.",
  "riskLevel": "low",
  "approvalStatus": "approved"
}
```

Allowed risk levels:

```text
low
medium
high
```

Allowed approval statuses:

```text
approved
pending
rejected
```

---

# 11. Open Questions

Open questions capture missing or unclear details.

Structure:

```json
{
  "questionId": "OQ-001",
  "question": "Is upcNumber passed as a path parameter or request body field?",
  "area": "request",
  "severity": "medium"
}
```

Allowed areas:

```text
api
request
response
business_rule
validation
downstream
database
error_handling
workflow
node_red
unknown
```

Allowed severity values:

```text
critical
medium
low
```

---

# 12. Gaps

Gaps capture missing information that may block implementation.

Structure:

```json
{
  "gapId": "GAP-001",
  "category": "missing_required_detail",
  "description": "Error response schema is not documented.",
  "impact": "Factory generator may create inconsistent error responses.",
  "severity": "critical",
  "recommendedAction": "Confirm standard error response format."
}
```

Allowed categories:

```text
missing_required_detail
ambiguous_requirement
conflicting_requirement
unsupported_assumption
downstream_gap
database_gap
validation_gap
error_handling_gap
response_mapping_gap
workflow_gap
node_red_generation_gap
```

Allowed severity values:

```text
critical
medium
low
```

---

# 13. Traceability Metadata

Traceability metadata is required for every normalized extraction output.

Structure:

```json
"traceability": {
  "schemaVersion": "day26-v1",
  "extractionMethod": "rule_based",
  "llmUsed": false,
  "generatedAt": "2026-06-29T00:00:00.000Z",
  "generatedBy": "api-factory-generator"
}
```

Field rules:

| Field              | Type    | Description                                   |
| ------------------ | ------- | --------------------------------------------- |
| `schemaVersion`    | string  | Schema version                                |
| `extractionMethod` | string  | `rule_based`, `ollama`, `hybrid`, or `manual` |
| `llmUsed`          | boolean | Whether LLM was used                          |
| `generatedAt`      | string  | ISO timestamp                                 |
| `generatedBy`      | string  | Generator name                                |

Allowed extraction methods:

```text
rule_based
ollama
hybrid
manual
unknown
```

---

## Required Traceability Rules

Every extracted item should include at least:

```json
{
  "sourceText": "",
  "confidence": "medium"
}
```

Where possible, extracted items should also include IDs such as:

```text
ruleId
validationId
dependencyId
gapId
questionId
```

This makes the output easier to debug, evaluate, and use in research documentation.

---

# 14. Confidence Levels

Confidence indicates how strongly the system believes the extracted item is correct.

Allowed values:

```text
high
medium
low
unknown
```

Usage:

| Confidence | Meaning                               |
| ---------- | ------------------------------------- |
| `high`     | Explicitly present in the requirement |
| `medium`   | Reasonably inferred from nearby text  |
| `low`      | Weak inference, needs review          |
| `unknown`  | Confidence could not be determined    |

---

# 15. Normalization Rules

The normalizer should follow these rules:

1. Convert all header lists into `request.headers`.
2. Convert all request body field lists into `request.bodyFields`.
3. Convert all response field lists into `response.successFields` or `response.errorFields`.
4. Convert business-rule-like items into `businessRules`.
5. Convert validation-specific items into `validationRules`.
6. Convert downstream references into `downstreamDependencies`.
7. Convert database references into `databaseDependencies`.
8. Preserve original raw extraction separately if needed.
9. Add gaps for missing required information.
10. Add open questions for ambiguous information.
11. Do not invent unknown endpoint, method, field, or dependency details.

---

# 16. Minimum Required Fields for Day 26

For Day 26, a normalized extraction is valid if it includes:

* `requirementId`
* `source`
* `api`
* `request`
* `response`
* `businessRules`
* `validationRules`
* `downstreamDependencies`
* `databaseDependencies`
* `openQuestions`
* `gaps`
* `traceability`

Even if some lists are empty, the fields must exist.

---

# 17. Example Normalized Extraction

```json
{
  "requirementId": "REQ-XCDTS005",
  "source": {
    "sourceType": "confluence",
    "sourceName": "XCDTS005 CDT Transfer Details",
    "sourcePath": "",
    "rawTextAvailable": true
  },
  "api": {
    "apiName": "XCDTS005 Transfer Details",
    "description": "Retrieve CDT transfer details using UPC and store information.",
    "endpoint": {
      "method": "POST",
      "path": "/api/v1/cdt/transfer-details",
      "version": "v1",
      "operationName": "XCDTS005"
    }
  },
  "request": {
    "headers": [
      {
        "name": "content-type",
        "required": true,
        "type": "string",
        "description": "Request content type",
        "example": "application/json",
        "sourceText": "content-type header is required",
        "confidence": "high"
      }
    ],
    "pathParameters": [],
    "queryParameters": [],
    "bodyFields": [
      {
        "path": "divisionNumber",
        "name": "divisionNumber",
        "required": true,
        "type": "string",
        "description": "Division number",
        "example": "71",
        "sourceText": "divisionNumber is required",
        "confidence": "high"
      }
    ]
  },
  "response": {
    "successFields": [],
    "errorFields": [],
    "examples": []
  },
  "businessRules": [
    {
      "ruleId": "BR-001",
      "name": "Validate Request Payload",
      "description": "Validate request payload exists and is an object.",
      "category": "validation",
      "condition": "Request payload is missing or not an object",
      "action": "Return validation error",
      "priority": 1,
      "sourceText": "",
      "confidence": "high"
    }
  ],
  "validationRules": [
    {
      "validationId": "VAL-001",
      "fieldPath": "request.headers.content-type",
      "ruleType": "required",
      "message": "content-type header is required",
      "severity": "error",
      "sourceText": "",
      "confidence": "high"
    }
  ],
  "downstreamDependencies": [
    {
      "dependencyId": "DEP-001",
      "name": "XCDTS005",
      "type": "SOAP",
      "method": "",
      "endpoint": "",
      "purpose": "Retrieve CDT transfer details",
      "requestMapping": [],
      "responseMapping": [],
      "errorHandling": [],
      "sourceText": "",
      "confidence": "medium"
    }
  ],
  "databaseDependencies": [],
  "workflowHints": [],
  "nodeRedHints": [],
  "assumptions": [],
  "openQuestions": [],
  "gaps": [],
  "traceability": {
    "schemaVersion": "day26-v1",
    "extractionMethod": "rule_based",
    "llmUsed": false,
    "generatedAt": "",
    "generatedBy": "api-factory-generator"
  }
}
```

---

# 18. Day 26 Success Criteria

Day 26 Phase 1 is complete when:

* [ ] The standard extraction schema is documented.
* [ ] All top-level fields are defined.
* [ ] Request, response, business-rule, validation, downstream, database, gap, and traceability structures are defined.
* [ ] No-hallucination rules are included.
* [ ] Normalization rules are documented.
* [ ] The schema can support Node-RED generation, Excel generation, gap analysis, and future code generation.

---

## Final Summary

The Day 26 standard extraction schema creates the contract between requirement extraction and every future generation module.

The key principle is:

```text
Raw extraction should never directly drive generation.
Raw extraction should first be normalized into a stable schema.
```

This prevents inconsistent outputs, reduces hallucination risk, improves traceability, and prepares the project for Excel intake generation, gap analysis, and factory LLM code generation in future days.

---
