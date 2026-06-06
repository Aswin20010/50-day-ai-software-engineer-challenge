# Day 5 — Phase 5: Requirement Extraction Rules

## Phase Objective

The objective of Phase 5 is to define a clear and reusable set of rules for extracting structured information from semi-structured enterprise API requirement documents.

These rules define how a Confluence-style requirement document should be converted into a structured JSON contract.

The transformation target is:

```text
Confluence Requirement Document
        ↓
Structured Requirement JSON
```

The structured JSON will later be used by downstream automation layers for:

* Node-RED workflow generation
* API Factory Excel workbook generation
* Gap analysis
* Requirement-to-flow traceability
* Requirement-to-Excel traceability
* Future code generation readiness checks

---

## Research Project

**Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis**

---

## Why Extraction Rules Are Needed

Enterprise API requirements are often written in inconsistent formats.

One requirement document may use sections such as:

* API Contract
* REST Details
* Headers
* Business Rules
* Downstream Services
* Database Tables

Another document may use different terms such as:

* Interface Details
* Request Attributes
* Processing Logic
* Integrations
* Data Access
* Functional Rules

Because of this inconsistency, the extraction system needs formal rules for identifying, normalizing, and structuring requirement details.

The goal is not just to copy text from a requirement document. The goal is to convert human-written requirements into a reliable engineering contract that can be used by automated systems.

---

## Extraction Output Target

The extractor should produce a structured JSON document with the following major sections:

```json
{
  "api": {},
  "headers": [],
  "requestBody": {
    "fields": []
  },
  "responseBody": {
    "success": {},
    "error": {}
  },
  "businessRules": [],
  "externalServices": [],
  "database": {
    "tables": []
  },
  "validations": [],
  "traceability": [],
  "gaps": []
}
```

---

## Extraction Categories

The extractor must identify and structure the following categories:

```text
1. API Metadata
2. Endpoint Details
3. Headers
4. Request Body Fields
5. Enum Values
6. Response Body
7. Business Rules
8. External Services
9. Database Tables
10. Validations
11. Traceability
12. Gaps
```

---

# Rule 1 — Extract API Metadata

## Purpose

The extractor must identify basic API-level information from the requirement document.

## Fields to Extract

```json
{
  "api": {
    "name": "",
    "description": "",
    "method": "",
    "path": "",
    "contentType": ""
  }
}
```

## Source Sections to Check

The extractor should search for sections such as:

* API Name
* API Description
* API Overview
* Endpoint Details
* REST Contract
* API Contract
* Interface Details

## Extraction Behavior

The extractor should map source content to the correct target fields.

| Source Information         | Target Field    |
| -------------------------- | --------------- |
| API Name                   | api.name        |
| API Description / Overview | api.description |
| HTTP Method / Method       | api.method      |
| Endpoint Path / URL / URI  | api.path        |
| Content-Type               | api.contentType |

## Example Source

```text
API Name: Evaluate Transaction Discount API
HTTP Method: POST
Endpoint Path: /api/v1/discount/evaluateTransactionDiscount
Content-Type: application/json
```

## Example Output

```json
{
  "api": {
    "name": "Evaluate Transaction Discount API",
    "method": "POST",
    "path": "/api/v1/discount/evaluateTransactionDiscount",
    "contentType": "application/json"
  }
}
```

---

# Rule 2 — Extract Headers

## Purpose

The extractor must identify all request headers.

## Fields to Extract

Each header should be extracted using the following structure:

```json
{
  "name": "",
  "required": true,
  "description": ""
}
```

## Source Sections to Check

The extractor should search for sections such as:

* Required Headers
* Headers
* Request Headers
* HTTP Headers
* Header Parameters

## Required Flag Mapping

| Source Value | Extracted Value |
| ------------ | --------------- |
| Yes          | true            |
| No           | false           |
| Required     | true            |
| Optional     | false           |
| Mandatory    | true            |
| Not Required | false           |

## Extraction Behavior

The extractor must preserve exact header names from the source document.

For example:

```text
requestorInfo-clientId
```

must not be renamed to:

```text
clientId
```

## Example Output

```json
{
  "name": "requestorInfo-clientId",
  "required": true,
  "description": "Client application identifier"
}
```

---

# Rule 3 — Extract Request Body Fields

## Purpose

The extractor must identify request body fields and flatten nested objects into dot-path notation.

## Fields to Extract

Each request body field should be extracted using the following structure:

```json
{
  "path": "",
  "type": "",
  "required": true,
  "description": "",
  "allowedValues": []
}
```

## Source Sections to Check

The extractor should search for sections such as:

* Request Body
* Request Body Example
* Field-Level Requirements
* Request Attributes
* Input Fields
* Payload Definition

## Flattening Rule

Nested JSON fields must be flattened using dot notation.

Array elements must use `[]`.

## Examples

| Source JSON Field                 | Extracted Field Path              |
| --------------------------------- | --------------------------------- |
| account.encryptInfo.encryptedData | account.encryptInfo.encryptedData |
| account.entryMode                 | account.entryMode                 |
| transaction.items.upc             | transaction.items[].upc           |
| transaction.items.department      | transaction.items[].department    |
| transaction.items.classNbr        | transaction.items[].classNbr      |

## Required Behavior

If a field is listed in a field-level requirements table, use the table as the source of truth for:

* Type
* Required flag
* Description

If the field appears only in a JSON example but not in the table, extract it but mark unclear values as `UNKNOWN`.

## Example Output

```json
{
  "path": "transaction.items[].upc",
  "type": "string",
  "required": true,
  "description": "Item UPC",
  "allowedValues": []
}
```

---

# Rule 4 — Extract Enum Values

## Purpose

The extractor must identify allowed values for enum fields.

## Source Sections to Check

The extractor should search for sections such as:

* Enum Requirements
* Allowed Values
* Valid Values
* Code Values
* Field Constraints

## Extraction Behavior

If the source document contains an enum list, the extractor must attach those values to the correct request field.

## Example Source

```text
Allowed keyType Values:
- AES
- RSA
- TOKEN
```

## Example Output

```json
{
  "path": "account.encryptInfo.keyType",
  "type": "string",
  "required": true,
  "description": "Encryption key type",
  "allowedValues": ["AES", "RSA", "TOKEN"]
}
```

## Validation Rule

Enum values must also be added to the `validations` section as validation type `enum`.

---

# Rule 5 — Extract Response Body

## Purpose

The extractor must identify success and error response structures.

## Fields to Extract

```json
{
  "responseBody": {
    "success": {},
    "error": {}
  }
}
```

## Source Sections to Check

The extractor should search for sections such as:

* Success Response
* Success Response Example
* Error Response
* Error Response Example
* Response Contract
* Output Payload

## Extraction Behavior

If JSON response examples are available, preserve the structure exactly.

If only text is available, create a structured representation only from clearly stated fields.

The extractor must not invent response fields.

## Example Output

```json
{
  "responseBody": {
    "success": {
      "code": "SUCCESS",
      "discountType": "Employee"
    },
    "error": {
      "code": "BAD_REQUEST",
      "message": "Missing required header"
    }
  }
}
```

---

# Rule 6 — Extract Business Rules

## Purpose

The extractor must identify business rules and convert each rule into a structured object.

## Fields to Extract

```json
{
  "id": "",
  "name": "",
  "description": "",
  "ruleType": "",
  "inputs": [],
  "outputs": [],
  "failureBehavior": ""
}
```

## Source Sections to Check

The extractor should search for sections such as:

* Business Rules
* Processing Rules
* Functional Rules
* Validation Rules
* Discount Logic
* Eligibility Logic
* Mapping Rules

## Rule ID Behavior

If the source document contains rule IDs like `BR-001`, preserve them exactly.

If no rule ID exists, generate a temporary ID using the following format:

```text
BR-UNKNOWN-001
```

## Rule Type Classification

| Rule Type        | Meaning                                           |
| ---------------- | ------------------------------------------------- |
| validation       | Validates headers, body fields, enums, or formats |
| transformation   | Builds, converts, maps, or normalizes data        |
| external_service | Calls another API or downstream service           |
| database_lookup  | Reads data from a database                        |
| database_write   | Writes data to a database                         |
| calculation      | Performs business calculation                     |
| response_mapping | Maps internal result to API response              |
| error_mapping    | Maps failure result to API error response         |

## Input and Output Extraction

Inputs should be extracted from fields, headers, service responses, or database records mentioned in the rule.

Outputs should be extracted from the expected result of the rule.

If inputs or outputs are not clearly stated, mark them as:

```text
UNKNOWN
```

## Example Output

```json
{
  "id": "BR-001",
  "name": "Validate Mandatory Headers",
  "description": "Validate all required headers before processing the request.",
  "ruleType": "validation",
  "inputs": [
    "requestorInfo-clientId",
    "requestorInfo-subclientId",
    "x-macys-apikey"
  ],
  "outputs": [
    "validated headers"
  ],
  "failureBehavior": "Return 400 Bad Request with the missing header name."
}
```

---

# Rule 7 — Extract External Services

## Purpose

The extractor must identify downstream APIs, services, or systems.

## Fields to Extract

```json
{
  "name": "",
  "method": "",
  "url": "",
  "purpose": "",
  "authentication": {
    "type": "",
    "algorithm": "",
    "encoding": "",
    "headerFormat": ""
  },
  "requestMapping": [],
  "responseMapping": []
}
```

## Source Sections to Check

The extractor should search for sections such as:

* External Services
* Downstream Services
* Service Calls
* Integration Details
* Dependency Services
* REST Services
* SOAP Services

## Extraction Behavior

The extractor must capture:

* Service name
* HTTP method
* URL
* Purpose
* Authentication type
* Request mapping
* Response mapping

If authentication is mentioned, extract the authentication type, algorithm, encoding, and header format when provided.

## Example Source

```text
The downstream Interrogate Account service must be called using HMAC authentication.
The HMAC signature must be generated using SHA-256 and encoded as Base64.
The generated signature must be passed using the header format HMAC: MAC <signature>.
```

## Example Output

```json
{
  "authentication": {
    "type": "HMAC",
    "algorithm": "SHA-256",
    "encoding": "Base64",
    "headerFormat": "HMAC: MAC <signature>"
  }
}
```

---

# Rule 8 — Extract Request and Response Mappings

## Purpose

The extractor must identify mappings between API request fields, downstream service request fields, downstream response fields, and internal API context fields.

## Source Sections to Check

The extractor should search for sections such as:

* Request Mapping
* Response Mapping
* Downstream Request Mapping
* Downstream Response Mapping
* Field Mapping
* Mapper Rules

## Request Mapping Format

```json
{
  "source": "",
  "target": ""
}
```

## Example Request Mapping

```json
{
  "source": "account.encryptInfo.encryptedData",
  "target": "elements[0].encryptInfo.encryptedData"
}
```

## Response Mapping Format

```json
{
  "source": "",
  "target": ""
}
```

## Example Response Mapping

```json
{
  "source": "internalAccountToken",
  "target": "account.internalAccountToken"
}
```

---

# Rule 9 — Extract Database Tables

## Purpose

The extractor must identify all database tables referenced in the requirement.

## Fields to Extract

```json
{
  "name": "",
  "purpose": "",
  "operations": []
}
```

## Source Sections to Check

The extractor should search for sections such as:

* Database Tables
* Data Access
* Persistence
* DB Tables
* Entity Mapping
* SQL Logic

## Supported Operations

```text
SELECT
INSERT
UPDATE
DELETE
UPSERT
```

## Extraction Behavior

If the document only lists table names and purpose, default the operation to:

```text
SELECT
```

unless the requirement clearly states insert, update, delete, or upsert.

The extractor must preserve exact table names.

For example:

```text
Disc_By_Dept_Vnd_Class
```

must not be renamed to:

```text
DiscountRule
```

## Example Output

```json
{
  "name": "Disc_By_Dept_Vnd_Class",
  "purpose": "Stores base discount rules by department, vendor, and class.",
  "operations": ["SELECT"]
}
```

---

# Rule 10 — Extract Validations

## Purpose

The extractor must identify validation requirements from headers, request fields, enums, arrays, formats, and business rules.

## Fields to Extract

```json
{
  "field": "",
  "validationType": "",
  "required": true,
  "allowedValues": [],
  "errorMessage": ""
}
```

## Validation Types

| Validation Type  | Meaning                                     |
| ---------------- | ------------------------------------------- |
| required_header  | Required request header                     |
| required_body    | Required request body field                 |
| enum             | Field must match allowed values             |
| numeric          | Field must be numeric                       |
| date_format      | Field must match date/timestamp format      |
| array_not_empty  | Array must contain at least one item        |
| string_not_blank | String must not be blank                    |
| min_value        | Numeric field must meet minimum value       |
| max_value        | Numeric field must not exceed maximum value |

## Extraction Behavior

Required headers must become `required_header` validations.

Required request fields must become `required_body` validations.

Enum fields must become `enum` validations.

Array requirements such as `items must contain at least one item` must become `array_not_empty`.

## Example Output

```json
{
  "field": "account.entryMode",
  "validationType": "enum",
  "required": true,
  "allowedValues": [
    "KEYED",
    "SWIPED",
    "TOKENIZED"
  ],
  "errorMessage": "Invalid account.entryMode"
}
```

---

# Rule 11 — Generate Traceability

## Purpose

The extractor must create traceability between source requirement sections and target generated artifacts.

## Fields to Extract

```json
{
  "requirementId": "",
  "sourceSection": "",
  "targetArtifact": "",
  "notes": ""
}
```

## Traceability Behavior

Every business rule should have at least one traceability entry.

The extractor should map rules to expected downstream artifacts.

## Target Artifact Examples

| Requirement Type        | Target Artifact                                                |
| ----------------------- | -------------------------------------------------------------- |
| Header validation       | Node-RED validation node and Excel business_requirements sheet |
| Request body validation | Node-RED validation node and Excel business_requirements sheet |
| External service call   | Node-RED REST/SOAP node and Excel external_services sheet      |
| Database lookup         | Excel entities, queries, and endpoint_service_mapping sheets   |
| Calculation             | Node-RED function node and Excel business_requirements sheet   |
| Response mapping        | Node-RED mapper node and Excel endpoint_service_mapping sheet  |

## Example Output

```json
{
  "requirementId": "BR-005",
  "sourceSection": "External Services / Interrogate Account Service",
  "targetArtifact": "Node-RED REST call node and Excel external_services sheet",
  "notes": "Used to generate downstream REST service call configuration."
}
```

---

# Rule 12 — Identify Gaps

## Purpose

The extractor must identify missing or unclear requirement details.

## Fields to Extract

```json
{
  "section": "",
  "missingDetail": "",
  "impact": "",
  "suggestedResolution": ""
}
```

## Gap Examples

The extractor should create gaps for missing information such as:

* Missing SQL queries
* Missing downstream URL by environment
* Missing error mapping
* Missing authentication details
* Missing response examples
* Missing field type
* Missing required flag
* Missing business rule failure behavior
* Missing timeout and retry configuration
* Missing downstream response examples

## Rule

The extractor must not silently invent missing details.

If a requirement detail is unclear, mark it as a gap.

## Example Output

```json
{
  "section": "Database Queries",
  "missingDetail": "Exact SQL queries are not provided in the source requirement.",
  "impact": "Excel queries sheet cannot be fully generated without assumptions.",
  "suggestedResolution": "Add SQL query definitions or database access rules in future phases."
}
```

---

# Rule 13 — Preserve Enterprise Naming

## Purpose

Enterprise naming must be preserved exactly because downstream systems depend on exact field, header, table, and service names.

## Names to Preserve

The extractor must preserve names such as:

```text
requestorInfo-clientId
requestorInfo-subclientId
requestorInfo-traceId
requestorinfo-version
x-macys-apikey
x-hmac-secret
account.encryptInfo.keyType
account.encryptInfo.storeNbr
transaction.items[].classNbr
Disc_By_Dept_Vnd_Class
Addtnl_Disc_Dept_Vnd_Class_Exclusions
```

## Do Not Rename

Do not rename:

```text
storeNbr
```

to:

```text
storeNumber
```

Do not rename:

```text
classNbr
```

to:

```text
classNumber
```

Do not rename:

```text
requestorInfo-clientId
```

to:

```text
clientId
```

Do not rename:

```text
Disc_By_Dept_Vnd_Class
```

to:

```text
DiscountRule
```

---

# Rule 14 — Avoid Hallucination

## Purpose

The extractor must only extract information supported by the source document.

## Behavior

If a field is not present, do not invent it.

If the source says only:

```text
Call downstream account service.
```

Do not invent:

* Exact URL
* Headers
* Authentication
* Request body
* Response body
* Retry logic
* Timeout values

unless those details are provided somewhere else in the document.

## Missing Details

Missing or unclear details should be placed in the `gaps` section.

---

# Rule 15 — Mark Unknowns Clearly

## Purpose

If a required extraction field cannot be confidently determined, it must be marked clearly.

## Allowed Unknown Marker

Use:

```text
UNKNOWN
```

## Example

```json
{
  "name": "Account Service",
  "method": "UNKNOWN",
  "url": "UNKNOWN",
  "purpose": "Identifies account information."
}
```

## Rule

Do not leave important fields blank if the detail is missing.

Use `UNKNOWN` or create a gap entry.

---

# Rule 16 — Support Future LLM Enhancement

## Purpose

The rule-based extractor should be designed so that an LLM can later enhance the extracted output.

## Current Day 5 Scope

For Day 5, extraction is being defined conceptually.

No actual parser or LLM implementation is required in this phase.

## Future Enhancement

In later days, an LLM can improve extraction of:

* Business rule descriptions
* Input and output identification
* Rule type classification
* Gap analysis
* Traceability notes
* Mapping interpretation
* Ambiguous requirement sections

However, the LLM must still follow the schema and extraction rules defined in Day 5.

The LLM should enhance extraction quality, not replace the structured contract.

---

# Rule 17 — Output Must Match Schema

## Purpose

The final extracted JSON must match the requirement schema created in Day 5.

## Required Behavior

The extractor output must include the following top-level keys:

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

If a section is not available in the source document, the extractor should still include the key with an empty object or empty array.

## Example

```json
{
  "externalServices": [],
  "gaps": [
    {
      "section": "External Services",
      "missingDetail": "No downstream service details provided.",
      "impact": "External service generation cannot be completed.",
      "suggestedResolution": "Add downstream service details to requirement."
    }
  ]
}
```

---

# Phase 5 Outcome

Phase 5 defines the formal requirement extraction rules for converting semi-structured enterprise API requirements into structured JSON.

These rules will guide the future implementation of the extractor logic.

They also ensure that downstream generated artifacts remain aligned with the original source requirement.

The rules support the long-term goal of generating:

* Node-RED workflow prototypes
* API Factory Excel workbooks
* Gap analysis reports
* Traceability matrices
* Code generation readiness checks

---

# Status

Phase 5 completed successfully.
