# Day 28 — Phase 1: Normalized-to-Generator Mapping Plan

## Purpose

This document defines the mapping plan for converting the Day 26 normalized extraction schema into the existing Node-RED generator input format.

Day 26 introduced the standard extraction schema and extraction normalizer. Day 27 made the normalized extraction output visible in the UI. Day 28 now connects the normalized schema to actual Node-RED flow generation.

The goal of Day 28 is to make `normalizedExtraction` the preferred source of truth for preparing generator-ready business rules while preserving backward compatibility with the existing legacy rule format.

---

## Day 28 Objective

The objective of Day 28 is:

```text
Use normalizedExtraction as the source of truth for preparing Node-RED generator input.
```

Current generation path:

```text
Raw extraction
        ↓
Legacy extracted fields
        ↓
Legacy businessRules config
        ↓
Node-RED flow generation
```

Target generation path:

```text
Raw extraction
        ↓
Normalized extraction
        ↓
Normalized-to-generator adapter
        ↓
Generator-ready businessRules
        ↓
Node-RED flow generation
```

---

## Why This Mapping Layer Is Needed

The existing Node-RED generator already expects a specific business-rule configuration format.

However, the new Day 26 normalized schema stores information differently.

For example, the normalized schema stores request headers in:

```text
normalizedExtraction.request.headers
```

But the existing generator may expect a rule like:

```json
{
  "brId": "BR-002",
  "requiredHeaders": ["content-type", "accept", "x-transaction-id", "x-source-system"]
}
```

The adapter layer bridges this difference.

The adapter will convert normalized extraction fields into the existing generator-ready rule format.

---

## Design Principle

The key principle for Day 28 is:

```text
Do not rewrite the Node-RED generator yet.
Add an adapter that converts normalized schema into the existing generator input format.
```

This reduces risk because the generator already works.

Instead of changing everything at once, Day 28 introduces a controlled transition layer.

---

## Inputs and Outputs

## Input

The adapter input is:

```text
normalizedExtraction
```

Expected type:

```text
ExtractedRequirement
```

Source file:

```text
src/lib/extraction-schema.ts
```

---

## Output

The adapter output should be compatible with the current `generateNodeRedFlow()` function.

Expected output shape:

```json
{
  "flowName": "Generated Flow Name",
  "businessRules": [],
  "includeSoapSubflow": true
}
```

The exact shape should match the existing input expected by:

```text
src/lib/node-red-flow-generator.ts
```

---

## Mapping Overview

The normalized extraction schema should map to generator input as follows:

| Normalized Schema Field                     | Generator Field                                         |
| ------------------------------------------- | ------------------------------------------------------- |
| `api.apiName`                               | `flowName`                                              |
| `api.endpoint.operationName`                | `operation`                                             |
| `api.endpoint.path`                         | HTTP-IN / metadata endpoint                             |
| `api.endpoint.method`                       | HTTP-IN method                                          |
| `request.headers`                           | `BR-002.requiredHeaders`                                |
| `request.bodyFields`                        | `BR-003.requiredFields`                                 |
| `validationRules` with `ruleType = type`    | `BR-004.numericFields` where applicable                 |
| `businessRules`                             | Business-rule nodes                                     |
| `downstreamDependencies` with `type = SOAP` | `includeSoapSubflow = true`                             |
| `nodeRedHints`                              | Node names/templates where useful                       |
| `traceability.schemaVersion`                | Generation metadata                                     |
| `gaps`                                      | Not used for generation yet, but preserved for metadata |
| `openQuestions`                             | Not used for generation yet, but preserved for metadata |

---

# 1. Flow Name Mapping

## Source

```text
normalizedExtraction.api.apiName
```

## Target

```text
flowName
```

## Rule

If `api.apiName` exists, use:

```text
<apiName> Generated Flow
```

If not available, use:

```text
<operationName> Generated Flow
```

If neither exists, use:

```text
Generated Flow
```

## Example

Input:

```json
{
  "api": {
    "apiName": "XCDTS005 Transfer Details"
  }
}
```

Output:

```json
{
  "flowName": "XCDTS005 Transfer Details Generated Flow"
}
```

---

# 2. HTTP-IN Rule Mapping

The first generator-ready rule should represent the API input route.

## Source

```text
normalizedExtraction.api.endpoint.method
normalizedExtraction.api.endpoint.path
```

## Target

```json
{
  "brId": "HTTP-IN",
  "httpMethod": "",
  "httpPath": ""
}
```

## Mapping

| Source                | Target       |
| --------------------- | ------------ |
| `api.endpoint.method` | `httpMethod` |
| `api.endpoint.path`   | `httpPath`   |

## Example

Input:

```json
{
  "api": {
    "endpoint": {
      "method": "POST",
      "path": "/api/v1/cdt/transfer-details"
    }
  }
}
```

Output:

```json
{
  "brId": "HTTP-IN",
  "httpMethod": "POST",
  "httpPath": "/api/v1/cdt/transfer-details"
}
```

---

# 3. Required Headers Rule Mapping

## Source

```text
normalizedExtraction.request.headers
```

## Target

```json
{
  "brId": "BR-002",
  "requiredHeaders": [],
  "jsonHeaders": []
}
```

## Mapping Rule

Use all headers where:

```text
required = true
```

Map header names into:

```text
requiredHeaders
```

If a header name is `content-type` or `accept`, also include it in:

```text
jsonHeaders
```

## Example

Input:

```json
{
  "request": {
    "headers": [
      {
        "name": "content-type",
        "required": true
      },
      {
        "name": "accept",
        "required": true
      },
      {
        "name": "x-transaction-id",
        "required": true
      }
    ]
  }
}
```

Output:

```json
{
  "brId": "BR-002",
  "requiredHeaders": ["content-type", "accept", "x-transaction-id"],
  "jsonHeaders": ["content-type", "accept"]
}
```

---

# 4. Required Body Fields Rule Mapping

## Source

```text
normalizedExtraction.request.bodyFields
```

## Target

```json
{
  "brId": "BR-003",
  "requiredFields": []
}
```

## Mapping Rule

Use all body fields where:

```text
required = true
```

Map field paths into:

```text
requiredFields
```

## Example

Input:

```json
{
  "request": {
    "bodyFields": [
      {
        "path": "divisionNumber",
        "required": true
      },
      {
        "path": "storeNumber",
        "required": true
      },
      {
        "path": "upcNumber",
        "required": true
      }
    ]
  }
}
```

Output:

```json
{
  "brId": "BR-003",
  "requiredFields": ["divisionNumber", "storeNumber", "upcNumber"]
}
```

---

# 5. Numeric Validation Rule Mapping

## Source

```text
normalizedExtraction.validationRules
```

## Target

```json
{
  "brId": "BR-004",
  "numericFields": []
}
```

## Mapping Rule

Use validation rules where:

```text
ruleType = "type"
```

and the message or field path indicates numeric validation.

Extract field names from:

```text
fieldPath
```

## Example

Input:

```json
{
  "validationRules": [
    {
      "fieldPath": "request.body.divisionNumber",
      "ruleType": "type",
      "message": "divisionNumber must be numeric"
    }
  ]
}
```

Output:

```json
{
  "brId": "BR-004",
  "numericFields": ["divisionNumber"]
}
```

---

# 6. Default Header and SOAP Default Mapping

## Source

```text
normalizedExtraction.businessRules
normalizedExtraction.downstreamDependencies
```

## Target

```json
{
  "brId": "BR-005",
  "optionalHeaderDefaults": {},
  "soapDefaults": {}
}
```

## Mapping Rule

For Day 28, if SOAP downstream exists, use safe existing SOAP defaults from the previous generator behavior.

This keeps the existing XCDTS005 flow working.

Default mapping:

```json
{
  "optionalHeaderDefaults": {
    "x-device-type": {
      "target": "device_type",
      "default": "PDA"
    },
    "x-device-name": {
      "target": "device_name",
      "default": "PDA123"
    },
    "x-associate-id": {
      "target": "associate_id",
      "default": "01003739"
    }
  },
  "soapDefaults": {
    "hdr_version": "1",
    "seq_number": "1",
    "retry_flag": "0",
    "application_ver": "1.0",
    "application_id": "CDT",
    "message_id": "XDTR",
    "flags": "",
    "reserved_data": "",
    "req_flag": "",
    "req_reserved": ""
  }
}
```

## Important

This is a controlled fallback.

Future days should extract these defaults from the normalized schema rather than relying on fixed values.

---

# 7. Normalization Mapping Rule

## Source

```text
normalizedExtraction.downstreamDependencies.requestMapping
normalizedExtraction.request.bodyFields
```

## Target

```json
{
  "brId": "BR-006",
  "normalizationMappings": []
}
```

## Mapping Rule

For Day 28, create normalization mappings from known body fields.

Common mapping:

| Source Field     | Target SOAP Field |
| ---------------- | ----------------- |
| `divisionNumber` | `div_number`      |
| `storeNumber`    | `location_number` |
| `upcNumber`      | `upc_number`      |

If `storeNumber` is mapped to `location_number`, use:

```json
{
  "padStart": 3
}
```

## Example Output

```json
{
  "brId": "BR-006",
  "normalizationMappings": [
    {
      "target": "div_number",
      "source": "divisionNumber"
    },
    {
      "target": "location_number",
      "source": "storeNumber",
      "padStart": 3
    },
    {
      "target": "upc_number",
      "source": "upcNumber"
    }
  ]
}
```

---

# 8. Request Metadata Rule Mapping

## Source

```text
normalizedExtraction.api.endpoint.path
normalizedExtraction.api.endpoint.operationName
```

## Target

```json
{
  "brId": "BR-007",
  "endpoint": "",
  "operation": "",
  "requestHeaderMappings": {}
}
```

## Mapping

| Source                       | Target      |
| ---------------------------- | ----------- |
| `api.endpoint.path`          | `endpoint`  |
| `api.endpoint.operationName` | `operation` |

Default request header mappings:

```json
{
  "x-transaction-id": "transactionId",
  "x-source-system": "sourceSystem"
}
```

---

# 9. SOAP Subflow Decision

## Source

```text
normalizedExtraction.downstreamDependencies
```

## Target

```text
includeSoapSubflow
```

## Mapping Rule

Set:

```text
includeSoapSubflow = true
```

if any downstream dependency has:

```text
type = "SOAP"
```

Otherwise:

```text
includeSoapSubflow = false
```

---

# 10. Business Rule Ordering

The generator-ready rules should be ordered as:

```text
HTTP-IN
BR-001
BR-002
BR-003
BR-004
BR-005
BR-006
BR-007
MAP-001
SOAP-SUBFLOW
MAP-GENERIC
HTTP-RESPONSE
```

For Day 28, the adapter should generate at least:

```text
HTTP-IN
BR-001
BR-002
BR-003
BR-004
BR-005
BR-006
BR-007
```

Future days can improve mapping for:

```text
MAP-001
MAP-GENERIC
HTTP-RESPONSE
```

---

# 11. Backward Compatibility

Day 28 must preserve the existing generation behavior.

The `/api/generate` endpoint should support both:

```json
{
  "businessRules": []
}
```

and:

```json
{
  "normalizedExtraction": {}
}
```

Expected logic:

```text
If normalizedExtraction exists:
    Convert normalizedExtraction using normalized-flow-adapter
    Generate flow from adapted business rules
Else:
    Use legacy businessRules input
```

This allows the project to move toward normalized-schema generation without breaking the current UI.

---

# 12. Generation Metadata

The generate API response should identify which generation source was used.

Expected metadata field:

```json
{
  "generationSource": "normalized_schema"
}
```

or:

```json
{
  "generationSource": "legacy_rules"
}
```

This helps prove whether Day 28 behavior is active.

---

# 13. No-Hallucination Rule

The adapter should not invent unknown business logic.

It can apply controlled defaults only for existing known generator behavior, such as:

* standard payload validation node
* required headers rule from normalized headers
* required body fields rule from normalized body fields
* numeric validation rule from normalized validation rules
* SOAP defaults only when SOAP dependency exists

If a required mapping is missing, the adapter should leave the field empty or rely on gaps/open questions already produced by the normalizer.

---

# 14. Day 28 Scope

## In Scope

Day 28 includes:

* Defining normalized-to-generator mapping
* Creating adapter layer
* Updating generate API to accept normalized extraction
* Updating frontend to send normalized extraction
* Adding generation source metadata
* Preserving legacy generation fallback

---

## Out of Scope

Day 28 does not include:

* Rewriting the entire Node-RED generator
* Blocking generation based on critical gaps
* Fully mapping response transformation
* Fully mapping SOAP XML templates
* Excel generation
* Gap analysis engine
* Code generation
* Automated tests

These will be handled in later days.

---

# 15. Success Criteria

Day 28 Phase 1 is complete when this document clearly defines:

* How normalized extraction maps into generator input
* How required headers map to `BR-002`
* How request body fields map to `BR-003`
* How validation rules map to `BR-004`
* How SOAP dependencies control `includeSoapSubflow`
* How request metadata maps to `BR-007`
* How backward compatibility is preserved
* How generation source metadata is reported

---

## Final Summary

Day 28 creates the bridge between normalized extraction and Node-RED generation.

The key principle is:

```text
Normalized schema should become the source of truth, but legacy generation must continue to work during the transition.
```

This allows the project to move from a raw-extraction-driven generator to a normalized-schema-driven API factory without breaking the existing implementation.

---
