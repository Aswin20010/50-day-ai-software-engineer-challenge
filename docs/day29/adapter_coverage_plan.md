# Day 29 — Phase 1: Adapter Coverage Plan

## Purpose

This document defines the Day 29 plan for improving the normalized schema adapter coverage.

Day 28 introduced the first version of the normalized-to-generator adapter:

```text id="bem7t5"
src/lib/normalized-flow-adapter.ts
```

That adapter successfully converted the Day 26 normalized extraction schema into the initial Node-RED generator input format.

Day 29 improves this adapter so that normalized extraction can generate more complete Node-RED flows, including downstream request mapping, response mapping, adapter warnings, and UI visibility for adapter quality issues.

---

## Day 29 Theme

```text id="y73hcb"
Improve Normalized Adapter Coverage
```

---

## Day 29 Objective

The objective of Day 29 is:

```text id="vu8j01"
Improve normalized-flow-adapter.ts so normalizedExtraction can generate a more complete Node-RED flow, including downstream request mapping and response mapping.
```

---

## Current State After Day 28

After Day 28, the adapter can generate the following rule IDs from normalized extraction:

```text id="3540kd"
HTTP-IN
BR-001
BR-002
BR-003
BR-004
BR-005
BR-006
BR-007
```

These rules cover the basic API entry point, request validation, field validation, defaults, normalization, and request metadata.

---

## Current Day 28 Adapter Pipeline

```text id="1s7u0w"
normalizedExtraction
        ↓
adaptNormalizedExtractionToFlowInput()
        ↓
generator-ready businessRules
        ↓
generateNodeRedFlow()
        ↓
Node-RED flow JSON
```

---

## Day 29 Target Pipeline

Day 29 should extend the adapter so the pipeline becomes:

```text id="rrd04f"
normalizedExtraction
        ↓
adaptNormalizedExtractionToFlowInput()
        ↓
core rules + downstream mapping + response mapping + warnings
        ↓
generateNodeRedFlow()
        ↓
more complete Node-RED flow JSON
```

---

# 1. Why Adapter Coverage Must Improve

The Day 28 adapter was intentionally limited.

It created enough generator-ready rules to prove that normalized schema generation works, but it did not fully cover downstream and response transformation logic.

For a real API factory, the adapter needs to prepare more complete generation input.

Specifically, it should begin mapping:

* Downstream dependency details
* SOAP request configuration
* Request replacement mappings
* Success response fields
* Failure response rules
* Generic response mapper fields
* Adapter warnings when important details are missing

---

# 2. Day 29 Scope

## In Scope

Day 29 includes:

* Extending adapter output types
* Adding `MAP-001` generation from normalized downstream dependencies
* Adding `MAP-GENERIC` generation from normalized response schema
* Adding adapter warning collection
* Returning warnings in adapter metadata
* Displaying adapter warnings in the UI
* Preserving Day 28 normalized generation behavior
* Preserving legacy generation fallback

---

## Out of Scope

Day 29 does not include:

* Rewriting the full Node-RED generator
* Building a complete visual flow editor
* Blocking generation based on warnings
* Editing normalized extraction in the UI
* Excel generation
* Gap analysis engine
* Go code generation
* Automated evaluation scoring

These will be handled in future phases.

---

# 3. Adapter Coverage Areas

Day 29 will improve the adapter in three main areas:

```text id="nq1pf3"
1. Downstream request mapping
2. Response mapping
3. Adapter warnings
```

---

# 4. Area 1 — Downstream Request Mapping

## Purpose

The adapter should generate a `MAP-001` rule from normalized downstream dependency data.

This allows the flow generator to understand how to call downstream services.

---

## Target Rule

```text id="pm8ckw"
MAP-001
```

---

## Expected Source

```text id="dpw6h8"
normalizedExtraction.downstreamDependencies
```

---

## Expected Output Shape

The adapter should generate a rule similar to:

```json id="s1e04i"
{
  "brId": "MAP-001",
  "mappingName": "Downstream Request Mapping",
  "method": "POST",
  "endpointSource": "configured",
  "headers": {},
  "requestReplacements": {},
  "successFields": {},
  "failureRules": {},
  "xmlTemplate": ""
}
```

---

## Mapping Plan

| Normalized Source                           | Generator Target                          |
| ------------------------------------------- | ----------------------------------------- |
| `downstreamDependencies[0].name`            | `mappingName`                             |
| `downstreamDependencies[0].method`          | `method`                                  |
| `downstreamDependencies[0].endpoint`        | `endpointSource` or runtime config source |
| `downstreamDependencies[0].headers`         | `headers`                                 |
| `downstreamDependencies[0].requestMapping`  | `requestReplacements`                     |
| `downstreamDependencies[0].responseMapping` | `successFields`                           |
| `downstreamDependencies[0].errorMapping`    | `failureRules`                            |
| SOAP template, if available                 | `xmlTemplate`                             |

---

## Day 29 Approach

Day 29 should use the first downstream dependency as the initial mapping source.

Future days can support multiple downstream calls.

For now:

```text id="vcum1v"
primaryDownstream = normalizedExtraction.downstreamDependencies[0]
```

---

# 5. Area 2 — Generic Response Mapping

## Purpose

The adapter should generate a `MAP-GENERIC` rule from normalized response schema data.

This allows the flow generator to map downstream or workflow results into the final REST response.

---

## Target Rule

```text id="y842aa"
MAP-GENERIC
```

---

## Expected Source

```text id="9uae0r"
normalizedExtraction.response.successFields
normalizedExtraction.response.errorFields
normalizedExtraction.response.examples
```

---

## Expected Output Shape

The adapter should generate a rule similar to:

```json id="sg7vok"
{
  "brId": "MAP-GENERIC",
  "returnCodePaths": "",
  "messagePaths": "",
  "successCode": "0",
  "successResponseMapping": {},
  "failureResponseMapping": {},
  "requiredSuccessFields": [],
  "businessCodeMap": {},
  "technicalErrorMap": {}
}
```

---

## Mapping Plan

| Normalized Source        | Generator Target                         |
| ------------------------ | ---------------------------------------- |
| `response.successFields` | `successResponseMapping`                 |
| Required success fields  | `requiredSuccessFields`                  |
| `response.errorFields`   | `failureResponseMapping`                 |
| Known return code fields | `returnCodePaths`                        |
| Known message fields     | `messagePaths`                           |
| Success indicator        | `successCode`                            |
| Error mappings           | `businessCodeMap` or `technicalErrorMap` |

---

## Day 29 Approach

For Day 29, the adapter should create a basic response mapping using normalized response fields.

If exact downstream return code paths are not available, the adapter should use safe empty strings and add warnings.

The adapter should not invent unknown return-code paths.

---

# 6. Area 3 — Adapter Warnings

## Purpose

The adapter should collect warnings when required or useful generation information is missing.

Warnings are not the same as backend errors.

Warnings mean:

```text id="ogos07"
The adapter can still generate a flow, but the generated flow may be incomplete.
```

---

## Warning Examples

The adapter should warn when:

* Endpoint path is missing
* HTTP method is missing
* Operation name is missing
* Request headers are missing
* Required body fields are missing
* Downstream dependencies are missing
* SOAP dependency exists but SOAP template is missing
* Downstream request mappings are missing
* Response success fields are missing
* Response error fields are missing
* Generic response mapping may be incomplete

---

## Expected Warning Shape

Warnings should be returned in adapter metadata.

Expected structure:

```json id="97n40u"
{
  "warnings": [
    {
      "code": "MISSING_ENDPOINT_PATH",
      "message": "API endpoint path is missing. HTTP-IN node may be incomplete.",
      "severity": "high",
      "source": "normalizedExtraction.api.endpoint.path"
    }
  ]
}
```

---

## Warning Severity Levels

Day 29 should use three severity levels:

```text id="n43z3d"
low
medium
high
```

---

## Example Warning Codes

| Warning Code                          | Severity | Meaning                                            |
| ------------------------------------- | -------: | -------------------------------------------------- |
| `MISSING_ENDPOINT_PATH`               |     high | API route path is missing                          |
| `MISSING_HTTP_METHOD`                 |     high | HTTP method is missing                             |
| `MISSING_OPERATION_NAME`              |   medium | Operation name is missing                          |
| `MISSING_DOWNSTREAM_DEPENDENCY`       |   medium | No downstream dependency was found                 |
| `MISSING_SOAP_TEMPLATE`               |   medium | SOAP dependency exists but XML template is missing |
| `MISSING_REQUEST_MAPPING`             |   medium | Downstream request mapping is missing              |
| `MISSING_RESPONSE_SUCCESS_FIELDS`     |   medium | Response success fields are missing                |
| `MISSING_RESPONSE_ERROR_FIELDS`       |      low | Response error fields are missing                  |
| `INCOMPLETE_GENERIC_RESPONSE_MAPPING` |   medium | MAP-GENERIC may be incomplete                      |

---

# 7. Adapter Metadata Enhancement

Day 28 adapter metadata included:

```json id="0o90ey"
{
  "adapterVersion": "day28-v1",
  "generationSource": "normalized_schema",
  "schemaVersion": "",
  "requirementId": "",
  "openQuestionCount": 0,
  "gapCount": 0,
  "generatedAt": ""
}
```

Day 29 should extend it to:

```json id="l4gfqv"
{
  "adapterVersion": "day29-v1",
  "generationSource": "normalized_schema",
  "schemaVersion": "",
  "requirementId": "",
  "openQuestionCount": 0,
  "gapCount": 0,
  "warningCount": 0,
  "warnings": [],
  "generatedRuleIds": [],
  "generatedAt": ""
}
```

---

# 8. Updated Rule List After Day 29

After Day 29, the adapter should generate:

```text id="21fnn1"
HTTP-IN
BR-001
BR-002
BR-003
BR-004
BR-005
BR-006
BR-007
MAP-001
MAP-GENERIC
```

This improves the generated flow coverage from basic validation and metadata rules into downstream and response mapping rules.

---

# 9. Backward Compatibility Requirement

Day 29 must not break existing Day 28 behavior.

The generate API must continue supporting:

```json id="yj5jg7"
{
  "businessRules": []
}
```

and:

```json id="oub4jb"
{
  "normalizedExtraction": {}
}
```

If `normalizedExtraction` exists, adapter generation should be used.

If it does not exist, legacy rule generation should be used.

---

# 10. UI Requirement

The UI should display adapter warnings inside the Generation Metadata panel.

Expected UI behavior:

```text id="bpwy1u"
If adapterMetadata.warnings exists:
    Show warning count
    Show warning code
    Show severity
    Show warning message
    Show source path
```

This helps the user understand whether the generated flow is complete or requires review.

---

# 11. No-Hallucination Rule

The adapter should not invent unknown downstream behavior.

Allowed controlled defaults:

* Known payload validation rule
* Known header validation rule
* Known request body validation rule
* Known numeric validation extraction
* Existing SOAP wrapper defaults
* Empty mappings when source data is missing
* Adapter warnings for missing mapping details

Not allowed:

* Inventing downstream endpoint URLs
* Inventing response return code paths
* Inventing SOAP XML templates without input
* Inventing business success/error codes
* Inventing database operations

---

# 12. Day 29 Implementation Phases

## Phase 1 — Adapter Coverage Plan

Deliverable:

```text id="zzllsd"
api-factory-ui/docs/day29/adapter_coverage_plan.md
```

Status:

```text id="xbepjp"
Completed by this document.
```

---

## Phase 2 — Extend Adapter Types

File:

```text id="rjju7s"
api-factory-ui/src/lib/normalized-flow-adapter.ts
```

Add fields for:

* `MAP-001`
* `MAP-GENERIC`
* adapter warnings
* response mapping
* SOAP request mapping

---

## Phase 3 — Add MAP-001 Adapter Rule

File:

```text id="fp97no"
api-factory-ui/src/lib/normalized-flow-adapter.ts
```

Add:

```ts id="6yooqh"
buildDownstreamMappingRule()
```

This function should create `MAP-001`.

---

## Phase 4 — Add MAP-GENERIC Adapter Rule

File:

```text id="bolm3c"
api-factory-ui/src/lib/normalized-flow-adapter.ts
```

Add:

```ts id="hud0a5"
buildGenericResponseMappingRule()
```

This function should create `MAP-GENERIC`.

---

## Phase 5 — Add Adapter Warnings

File:

```text id="axp1h3"
api-factory-ui/src/lib/normalized-flow-adapter.ts
```

Add warning collection and return warnings in:

```text id="999kap"
adapterMetadata.warnings
```

---

## Phase 6 — Show Adapter Warnings in UI

File:

```text id="gpq4hi"
api-factory-ui/src/app/page.tsx
```

Display adapter warnings in the Generation Metadata section.

---

## Phase 7 — Completion Report

Deliverable:

```text id="oo0v79"
api-factory-ui/docs/day29/day29_completion_report.md
```

---

## Phase 8 — Push Commands

Run build and commit all Day 29 changes.

---

# 13. Success Criteria

Day 29 is complete when:

* [ ] Adapter types support `MAP-001`
* [ ] Adapter types support `MAP-GENERIC`
* [ ] Adapter returns warnings
* [ ] Adapter metadata version is updated to `day29-v1`
* [ ] `MAP-001` is generated from normalized downstream dependencies
* [ ] `MAP-GENERIC` is generated from normalized response schema
* [ ] UI displays adapter warnings
* [ ] Legacy generation still works
* [ ] Normalized schema generation still works
* [ ] Build succeeds
* [ ] Documentation is added
* [ ] Changes are committed

---

# 14. Recommended Validation

After implementation, run:

```powershell id="yjjx0s"
cd C:\Users\BH31021\api-factory-generator\api-factory-ui
npm run build
npm run dev
```

Then validate:

* Upload requirement file
* Extract requirements
* Confirm normalized extraction appears
* Generate flow
* Confirm generation source is `normalized_schema`
* Confirm generated rule count increased
* Confirm `MAP-001` appears in generated rules or flow
* Confirm `MAP-GENERIC` appears in generated rules or flow
* Confirm adapter warnings appear if data is incomplete
* Confirm legacy generation still works when normalized extraction is not available

---

# 15. Final Summary

Day 29 improves the normalized adapter from a basic proof-of-connection layer into a more useful generation preparation layer.

The most important principle is:

```text id="52zb3v"
The adapter should generate as much as it safely can from normalized schema, and clearly warn when important information is missing.
```

This keeps the factory honest, traceable, and safer for enterprise API generation.

---
