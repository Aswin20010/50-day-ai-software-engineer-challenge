# Day 6 — Requirement Extraction Specification

## Purpose

This document defines the expected behavior of the future Requirement Extraction Agent for the research project:

**Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis**

The agent will eventually read enterprise API requirements, usually exported from Confluence, and extract structured API implementation details.

This is a non-coding specification document. It will guide later implementation beginning from Day 25.

---

## Agent Objective

The Requirement Extraction Agent should convert unstructured or semi-structured requirement documents into a structured representation that can support:

1. API endpoint documentation
2. Node-RED flow generation
3. Excel requirement template generation
4. Gap analysis
5. Experimental evaluation

---

## Input Document Type

The expected input is a Confluence-style requirement document.

The document may contain:

* API overview
* Endpoint details
* Request headers
* Request body
* Response body
* Business rules
* Validation rules
* Downstream service calls
* Database references
* Error handling rules
* Mapping logic
* Assumptions or open questions

---

## Core Extraction Fields

The agent should extract the following high-level sections.

| Field Group           | Description                                              |
| --------------------- | -------------------------------------------------------- |
| API Metadata          | API name, description, domain, owner if available        |
| Endpoint Details      | HTTP method, path, version, operation name               |
| Headers               | Required and optional request headers                    |
| Request Body          | Payload fields, types, required flags, nested objects    |
| Response Body         | Expected response fields and structure                   |
| Business Rules        | Validation, eligibility, decision, and calculation rules |
| External Services     | REST, SOAP, database, queue, or other integrations       |
| Database Tables       | Tables, columns, lookup rules, query purpose             |
| Error Handling        | Validation errors, downstream errors, business errors    |
| Mapping Logic         | Source-to-target field mappings                          |
| Security Requirements | API keys, HMAC, auth headers, tokens                     |
| Non-Functional Notes  | Performance, logging, tracing, observability             |
| Open Questions        | Ambiguous or missing requirement details                 |

---

## API Metadata Extraction

The agent should identify:

| Extracted Field | Example                                                 |
| --------------- | ------------------------------------------------------- |
| api_name        | FEED Discount Evaluation API                            |
| api_domain      | Discount                                                |
| api_description | Evaluates employee and new-account discount eligibility |
| source_document | REQ-001-feed-discount.md                                |
| requirement_id  | REQ-001                                                 |

If a field is not explicitly available, the agent should mark it as:

```text
unknown
```

The agent should not invent values.

---

## Endpoint Extraction

The agent should extract:

| Extracted Field       | Description                     |
| --------------------- | ------------------------------- |
| method                | HTTP method such as GET or POST |
| path                  | REST endpoint path              |
| version               | API version if present          |
| operation_name        | Human-readable operation name   |
| request_content_type  | Usually application/json        |
| response_content_type | Usually application/json        |

Example:

```json
{
  "method": "POST",
  "path": "/api/v1/discount/evaluateTransactionDiscount",
  "version": "v1",
  "operation_name": "evaluateTransactionDiscount",
  "request_content_type": "application/json",
  "response_content_type": "application/json"
}
```

---

## Header Extraction

The agent should extract all headers and classify them as required or optional.

Example:

```json
[
  {
    "name": "requestorInfo-clientId",
    "required": true,
    "type": "string",
    "description": "Client identifier"
  },
  {
    "name": "requestorInfo-traceId",
    "required": false,
    "type": "string",
    "description": "Optional trace identifier"
  }
]
```

The agent should preserve exact header casing when possible.

---

## Request Body Extraction

The agent should extract:

| Field          | Description                                     |
| -------------- | ----------------------------------------------- |
| name           | Field name                                      |
| path           | JSON path                                       |
| type           | string, number, integer, boolean, object, array |
| required       | true or false                                   |
| description    | Business meaning                                |
| allowed_values | Enum values if available                        |
| example        | Sample value if available                       |

Example:

```json
{
  "name": "entryMode",
  "path": "$.account.entryMode",
  "type": "string",
  "required": true,
  "allowed_values": ["KEYED", "SWIPED", "TOKENIZED"],
  "description": "Account entry mode"
}
```

---

## Response Body Extraction

The agent should extract expected response fields using the same structure as request fields.

Example:

```json
{
  "name": "discountDetails",
  "path": "$.discountDetails",
  "type": "array",
  "required": true,
  "description": "List of item-level discount calculations"
}
```

---

## Business Rule Extraction

The agent should extract business rules as independent rule objects.

Each rule should include:

| Field          | Description                      |
| -------------- | -------------------------------- |
| rule_id        | Stable identifier such as BR-001 |
| title          | Short name of the rule           |
| description    | Full rule explanation            |
| inputs         | Fields or data needed            |
| decision_logic | How the rule is evaluated        |
| outputs        | Result of the rule               |
| priority       | High, Medium, or Low             |
| source_section | Where the rule came from         |

Example:

```json
{
  "rule_id": "BR-001",
  "title": "Validate required headers",
  "description": "Reject the request if mandatory headers are missing.",
  "inputs": ["request_headers"],
  "decision_logic": "If any mandatory header is missing or blank, return validation error.",
  "outputs": ["validation_error"],
  "priority": "High",
  "source_section": "Validation Rules"
}
```

---

## External Service Extraction

The agent should extract downstream systems.

Each external service should include:

| Field                | Description                     |
| -------------------- | ------------------------------- |
| service_id           | Stable identifier               |
| service_name         | Human-readable service name     |
| service_type         | REST, SOAP, DB, Queue           |
| endpoint_or_resource | URL, operation, table, or topic |
| method_or_operation  | HTTP method or SOAP operation   |
| request_mapping      | Inputs sent to service          |
| response_mapping     | Outputs received from service   |
| error_handling       | How failures are handled        |

Example:

```json
{
  "service_id": "SRV-001",
  "service_name": "interrogateAccount",
  "service_type": "REST",
  "endpoint_or_resource": "/api/v1/discount/interrogateAccount",
  "method_or_operation": "POST",
  "request_mapping": ["account.encryptInfo", "account.entryMode"],
  "response_mapping": ["accountInfo", "employeeFlag"],
  "error_handling": "Return downstream service error if call fails."
}
```

---

## Database Extraction

The agent should extract database references.

Each database reference should include:

| Field                  | Description                    |
| ---------------------- | ------------------------------ |
| table_name             | Table used                     |
| operation              | SELECT, INSERT, UPDATE, DELETE |
| purpose                | Why the table is used          |
| key_columns            | Lookup columns if available    |
| output_columns         | Columns returned if available  |
| related_business_rules | Rules depending on this table  |

Example:

```json
{
  "table_name": "Emp_Acct_Profile",
  "operation": "SELECT",
  "purpose": "Identify employee account profile",
  "key_columns": ["internalAccountToken"],
  "output_columns": ["employeeFlag", "homeDivision"],
  "related_business_rules": ["BR-003"]
}
```

---

## Error Handling Extraction

The agent should extract:

* Validation errors
* Business rule failures
* Downstream service failures
* Database errors
* Mapping errors
* Unknown system errors

Each error should include:

```json
{
  "error_type": "VALIDATION_ERROR",
  "condition": "Required header is missing",
  "expected_status": 400,
  "message": "Missing required header"
}
```

---

## Mapping Logic Extraction

The agent should identify mappings such as:

| Source                         | Target                                 |
| ------------------------------ | -------------------------------------- |
| Request header traceId         | Downstream traceId                     |
| interrogateAccount.accountInfo | Final response.account                 |
| DB discount percentage         | discountDetails.discountPercentApplied |
| DB exclusion match             | discountDetails.isItemExcluded         |

Mappings should be traceable and should not be guessed if missing.

---

## Ambiguity Handling Rules

The agent should not invent missing details.

If the requirement is unclear, the agent should:

1. Extract what is explicitly present.
2. Mark missing fields as `unknown`.
3. Add ambiguous items to `open_questions`.
4. Avoid silently assuming default behavior.
5. Preserve original wording where possible.

---

## Expected Agent Behavior

The Requirement Extraction Agent should be:

| Behavior             | Requirement                                              |
| -------------------- | -------------------------------------------------------- |
| Conservative         | Do not hallucinate missing technical details             |
| Traceable            | Link extracted details to source sections where possible |
| Structured           | Return consistent JSON                                   |
| Implementation-aware | Extract details useful for Node-RED and Excel generation |
| Evaluation-friendly  | Produce output that can be compared to ground truth      |

---

## Out of Scope for This Agent

The Requirement Extraction Agent should not:

1. Generate executable code.
2. Generate complete Node-RED flows directly.
3. Generate Excel files directly.
4. Call downstream services.
5. Modify source requirements.
6. Resolve business ambiguity without evidence.

Those tasks belong to later agents or later implementation phases.
