# Day 6 — Extraction Output Schema

## Purpose

This document defines the expected output schema for the future Requirement Extraction Agent.

The agent will eventually read Confluence-style API requirements and produce structured JSON that can be used by downstream agents for Node-RED flow generation, Excel generation, and gap analysis.

This is a non-coding schema document. Actual implementation will begin from Day 25.

---

## Schema Objective

The extraction output should be:

1. Structured
2. Consistent
3. Traceable
4. Evaluation-friendly
5. Useful for downstream workflow generation

The output should avoid hallucinated assumptions. If a value is not present in the source requirement, it should be marked as `unknown`, `null`, or added to `open_questions`.

---

## Top-Level JSON Structure

The future extraction agent should produce JSON in this format:

```json
{
  "requirement_id": "REQ-001",
  "source_document": "REQ-001-feed-discount.md",
  "api_metadata": {},
  "endpoint": {},
  "headers": [],
  "request_body": [],
  "response_body": [],
  "business_rules": [],
  "external_services": [],
  "database_references": [],
  "error_handling": [],
  "mapping_logic": [],
  "security_requirements": [],
  "non_functional_notes": [],
  "open_questions": [],
  "extraction_metadata": {}
}
```

---

## 1. Requirement Identity

```json
{
  "requirement_id": "REQ-001",
  "source_document": "REQ-001-feed-discount.md"
}
```

| Field           | Type   | Required | Description                        |
| --------------- | ------ | -------- | ---------------------------------- |
| requirement_id  | string | yes      | Stable requirement identifier      |
| source_document | string | yes      | Source file name or document title |

---

## 2. API Metadata

```json
{
  "api_metadata": {
    "api_name": "FEED Discount Evaluation API",
    "api_domain": "Discount",
    "api_description": "Evaluates employee and new-account discount eligibility",
    "owner": "unknown",
    "system_context": "Enterprise API modernization workflow"
  }
}
```

| Field           | Type   | Required | Description                  |
| --------------- | ------ | -------- | ---------------------------- |
| api_name        | string | yes      | Name of the API              |
| api_domain      | string | no       | Business or technical domain |
| api_description | string | no       | Short API purpose            |
| owner           | string | no       | Owning team if present       |
| system_context  | string | no       | Broader workflow context     |

---

## 3. Endpoint

```json
{
  "endpoint": {
    "method": "POST",
    "path": "/api/v1/discount/evaluateTransactionDiscount",
    "version": "v1",
    "operation_name": "evaluateTransactionDiscount",
    "request_content_type": "application/json",
    "response_content_type": "application/json"
  }
}
```

| Field                 | Type   | Required | Description              |
| --------------------- | ------ | -------- | ------------------------ |
| method                | string | yes      | HTTP method              |
| path                  | string | yes      | REST endpoint path       |
| version               | string | no       | API version              |
| operation_name        | string | no       | Human-readable operation |
| request_content_type  | string | no       | Request media type       |
| response_content_type | string | no       | Response media type      |

---

## 4. Headers

```json
{
  "headers": [
    {
      "name": "requestorInfo-clientId",
      "required": true,
      "type": "string",
      "description": "Client identifier",
      "source": "header requirements section"
    }
  ]
}
```

| Field       | Type    | Required | Description                     |
| ----------- | ------- | -------- | ------------------------------- |
| name        | string  | yes      | Header name                     |
| required    | boolean | yes      | Whether the header is mandatory |
| type        | string  | no       | Expected type                   |
| description | string  | no       | Purpose of the header           |
| source      | string  | no       | Source section or evidence      |

---

## 5. Request Body

```json
{
  "request_body": [
    {
      "name": "entryMode",
      "path": "$.account.entryMode",
      "type": "string",
      "required": true,
      "allowed_values": ["KEYED", "SWIPED", "TOKENIZED"],
      "description": "Account entry mode",
      "source": "request body section"
    }
  ]
}
```

| Field          | Type    | Required | Description                |
| -------------- | ------- | -------- | -------------------------- |
| name           | string  | yes      | Field name                 |
| path           | string  | yes      | JSON path                  |
| type           | string  | yes      | Data type                  |
| required       | boolean | yes      | Whether field is mandatory |
| allowed_values | array   | no       | Enum values if available   |
| description    | string  | no       | Field purpose              |
| source         | string  | no       | Source section or evidence |

---

## 6. Response Body

```json
{
  "response_body": [
    {
      "name": "discountDetails",
      "path": "$.discountDetails",
      "type": "array",
      "required": true,
      "description": "Item-level discount results",
      "source": "expected response section"
    }
  ]
}
```

Response fields should follow the same structure as request body fields.

---

## 7. Business Rules

```json
{
  "business_rules": [
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
  ]
}
```

| Field          | Type   | Required | Description               |
| -------------- | ------ | -------- | ------------------------- |
| rule_id        | string | yes      | Stable business rule ID   |
| title          | string | yes      | Rule title                |
| description    | string | yes      | Rule explanation          |
| inputs         | array  | no       | Data required by the rule |
| decision_logic | string | no       | How rule is evaluated     |
| outputs        | array  | no       | Rule outputs              |
| priority       | string | no       | High, Medium, Low         |
| source_section | string | no       | Source evidence           |

---

## 8. External Services

```json
{
  "external_services": [
    {
      "service_id": "SRV-001",
      "service_name": "interrogateAccount",
      "service_type": "REST",
      "endpoint_or_resource": "/api/v1/discount/interrogateAccount",
      "method_or_operation": "POST",
      "request_mapping": ["account.encryptInfo", "account.entryMode"],
      "response_mapping": ["accountInfo", "employeeFlag"],
      "error_handling": "Return downstream error if service fails."
    }
  ]
}
```

| Field                | Type   | Required | Description                   |
| -------------------- | ------ | -------- | ----------------------------- |
| service_id           | string | yes      | Stable service ID             |
| service_name         | string | yes      | Service name                  |
| service_type         | string | yes      | REST, SOAP, DB, Queue         |
| endpoint_or_resource | string | no       | URL, operation, table, topic  |
| method_or_operation  | string | no       | HTTP method or SOAP operation |
| request_mapping      | array  | no       | Inputs sent to service        |
| response_mapping     | array  | no       | Outputs received              |
| error_handling       | string | no       | Failure behavior              |

---

## 9. Database References

```json
{
  "database_references": [
    {
      "table_name": "Emp_Acct_Profile",
      "operation": "SELECT",
      "purpose": "Identify employee account profile",
      "key_columns": ["internalAccountToken"],
      "output_columns": ["employeeFlag", "homeDivision"],
      "related_business_rules": ["BR-003"]
    }
  ]
}
```

| Field                  | Type   | Required | Description                    |
| ---------------------- | ------ | -------- | ------------------------------ |
| table_name             | string | yes      | Table name                     |
| operation              | string | no       | SELECT, INSERT, UPDATE, DELETE |
| purpose                | string | no       | Why table is used              |
| key_columns            | array  | no       | Lookup columns                 |
| output_columns         | array  | no       | Returned columns               |
| related_business_rules | array  | no       | Related BR IDs                 |

---

## 10. Error Handling

```json
{
  "error_handling": [
    {
      "error_type": "VALIDATION_ERROR",
      "condition": "Required header is missing",
      "expected_status": 400,
      "message": "Missing required header",
      "source": "error handling section"
    }
  ]
}
```

| Field           | Type    | Required | Description       |
| --------------- | ------- | -------- | ----------------- |
| error_type      | string  | yes      | Error category    |
| condition       | string  | yes      | Trigger condition |
| expected_status | integer | no       | HTTP status code  |
| message         | string  | no       | Expected message  |
| source          | string  | no       | Source evidence   |

---

## 11. Mapping Logic

```json
{
  "mapping_logic": [
    {
      "mapping_id": "MAP-001",
      "source": "interrogateAccount.accountInfo",
      "target": "response.account",
      "description": "Echo account details from interrogateAccount into final response.",
      "source_section": "response mapping section"
    }
  ]
}
```

| Field          | Type   | Required | Description            |
| -------------- | ------ | -------- | ---------------------- |
| mapping_id     | string | yes      | Stable mapping ID      |
| source         | string | yes      | Source field or object |
| target         | string | yes      | Target field or object |
| description    | string | no       | Mapping explanation    |
| source_section | string | no       | Source evidence        |

---

## 12. Security Requirements

```json
{
  "security_requirements": [
    {
      "type": "API_KEY",
      "name": "x-macys-apikey",
      "required": true,
      "description": "API key required for downstream service access"
    }
  ]
}
```

---

## 13. Non-Functional Notes

```json
{
  "non_functional_notes": [
    {
      "category": "Tracing",
      "description": "Trace ID should be propagated to downstream services if available."
    }
  ]
}
```

---

## 14. Open Questions

```json
{
  "open_questions": [
    {
      "question_id": "Q-001",
      "question": "Is requestorInfo-traceId required or optional?",
      "reason": "Requirement mentions trace propagation but does not clearly mark required flag.",
      "impact": "Affects validation logic and downstream mapping."
    }
  ]
}
```

---

## 15. Extraction Metadata

```json
{
  "extraction_metadata": {
    "extraction_version": "v1",
    "created_for": "Day 6 non-coding schema planning",
    "implementation_start_day": "Day 25",
    "confidence_notes": "Confidence should be tracked per section in future implementation."
  }
}
```

---

## Handling Missing Values

The extractor should use the following rules:

| Situation                   | Output                                        |
| --------------------------- | --------------------------------------------- |
| Field is explicitly present | Extract exact value                           |
| Field is partially present  | Extract available value and add open question |
| Field is not present        | Use `unknown` or `null`                       |
| Field is ambiguous          | Add to `open_questions`                       |
| Field requires assumption   | Do not assume silently                        |

---

## Downstream Usage

This schema will support:

1. Requirement extraction evaluation
2. Node-RED flow planning
3. Excel sheet generation planning
4. Gap analysis
5. Research paper experiments
