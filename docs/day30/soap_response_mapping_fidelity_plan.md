# Day 30 — Phase 1: SOAP and Response Mapping Fidelity Plan

## Purpose

This document defines the Day 30 plan for improving SOAP request mapping and REST response mapping fidelity in the API Factory Generator.

Day 28 connected `normalizedExtraction` to Node-RED generation.

Day 29 improved adapter coverage by adding:

```text
MAP-001
MAP-GENERIC
adapter warnings
generatedRuleIds
UI warning display
```

Day 30 now focuses on making the normalized-schema-generated flow closer to a real enterprise wrapper flow, especially the XCDTS005-style SOAP wrapper pattern.

---

## Day 30 Theme

```text
Improve SOAP and Response Mapping Fidelity
```

---

## Day 30 Objective

The objective of Day 30 is:

```text
Improve normalized schema generation so the generated Node-RED flow is closer to the existing XCDTS005-style wrapper pattern.
```

---

## Current State After Day 29

After Day 29, the normalized adapter generates the following rule IDs:

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
MAP-GENERIC
```

This means the adapter now supports:

* HTTP route metadata
* Request payload validation
* Header validation
* Required body field validation
* Numeric validation
* Default values
* Request normalization
* Request metadata
* Downstream mapping
* Generic response mapping
* Adapter warnings

However, some generated fields are still basic.

Day 30 improves the quality and fidelity of these mappings.

---

# 1. Why Day 30 Is Needed

The current adapter can generate `MAP-001` and `MAP-GENERIC`, but the mapping quality is still limited.

For real enterprise API wrapper generation, the generated flow should understand:

* SOAP XML template structure
* SOAP request placeholder replacements
* Downstream operation metadata
* REST response target shape
* Success response mapping
* Failure response mapping
* Generated flow completeness indicators

Without these improvements, the generated flow may have the right node structure but incomplete mapping details.

Day 30 improves the generated flow from:

```text
structurally correct
```

to:

```text
closer to executable enterprise wrapper behavior
```

---

# 2. Target Pipeline After Day 30

The pipeline after Day 30 should look like this:

```text
Confluence Requirement
        ↓
Raw Extraction
        ↓
Normalized Extraction Schema
        ↓
UI Review
        ↓
Normalized-to-Generator Adapter
        ↓
SOAP request mapping + response mapping fidelity improvements
        ↓
Node-RED Flow Generator
        ↓
Generated Flow JSON
        ↓
Generated Flow Inspection
```

---

# 3. Day 30 Focus Areas

Day 30 focuses on five improvement areas:

```text
1. Stable sample normalized extraction JSON
2. Improved SOAP XML template detection
3. Improved request replacement mapping
4. Improved MAP-GENERIC response mapping
5. Generated flow inspection visibility
```

---

# 4. Area 1 — Stable Sample Normalized Extraction JSON

## Purpose

A stable sample normalized extraction file is needed for manual regression testing.

The file will represent an XCDTS005-style normalized extraction object.

## Deliverable

```text
api-factory-ui/samples/normalized-xcdts005-day30.json
```

## Why This Matters

The project needs a repeatable sample that can be used to validate whether adapter changes are improving the generated output.

This sample helps test:

* Endpoint mapping
* Header mapping
* Body field mapping
* SOAP downstream mapping
* Request replacement mapping
* Response mapping
* Adapter warnings
* Generated rule IDs

---

## Expected Sample Contents

The sample should include:

* Requirement ID
* API name
* Endpoint method and path
* Request headers
* Request body fields
* Validation rules
* SOAP downstream dependency
* SOAP XML template
* Request mapping
* Response success fields
* Response error fields
* Traceability metadata

---

# 5. Area 2 — Improved SOAP XML Template Detection

## Purpose

The adapter should detect SOAP XML templates from multiple possible normalized locations.

Currently, SOAP template extraction only checks simple downstream fields.

Day 30 should improve this so the adapter can find SOAP templates from several likely structures.

---

## Candidate Source Locations

The adapter should check for SOAP XML template values in locations such as:

```text
downstreamDependencies[0].xmlTemplate
downstreamDependencies[0].soapTemplate
downstreamDependencies[0].requestTemplate
downstreamDependencies[0].template
downstreamDependencies[0].soap.xmlTemplate
downstreamDependencies[0].soap.template
downstreamDependencies[0].metadata.xmlTemplate
downstreamDependencies[0].metadata.soapTemplate
```

---

## Target Field

The detected SOAP XML template should map into:

```text
MAP-001.xmlTemplate
```

---

## No-Hallucination Rule

If no SOAP template exists in the normalized extraction, the adapter should not invent one.

Instead, it should:

```text
Leave xmlTemplate empty
Return MISSING_SOAP_TEMPLATE warning
```

---

# 6. Area 3 — Improved Request Replacement Mapping

## Purpose

The adapter should improve how normalized request fields map into SOAP placeholders.

The goal is to make `MAP-001.requestReplacements` closer to the existing enterprise SOAP wrapper pattern.

---

## Common Input Fields

For XCDTS005-style APIs, normalized request body fields commonly include:

```text
divisionNumber
storeNumber
upcNumber
```

---

## Target SOAP Fields

These fields should map to SOAP-oriented target names:

```text
divisionNumber -> div_number
storeNumber -> location_number
upcNumber -> upc_number
```

---

## Target Placeholder Format

The adapter should support SOAP placeholder-style replacements such as:

```text
{{div_number}}
{{location_number}}
{{upc_number}}
```

---

## Expected Mapping Example

Input normalized request fields:

```json
[
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
```

Expected request replacements:

```json
{
  "{{div_number}}": "divisionNumber",
  "{{location_number}}": "storeNumber",
  "{{upc_number}}": "upcNumber"
}
```

---

## Existing Mapping Compatibility

If normalized downstream dependency already provides request mapping, the adapter should prefer that explicit mapping.

Expected priority:

```text
1. Explicit downstream requestMapping
2. Explicit downstream requestReplacements
3. Explicit downstream fieldMapping
4. Derived known enterprise wrapper mapping
5. Empty mapping + warning
```

---

# 7. Area 4 — Improved MAP-GENERIC Response Mapping

## Purpose

The adapter should improve `MAP-GENERIC` so response mapping is closer to the target REST response shape.

---

## Expected Source

```text
normalizedExtraction.response.successFields
normalizedExtraction.response.errorFields
normalizedExtraction.response.examples
```

---

## Target Rule

```text
MAP-GENERIC
```

---

## Mapping Goals

The adapter should improve:

* `successResponseMapping`
* `failureResponseMapping`
* `requiredSuccessFields`
* `returnCodePaths`
* `messagePaths`
* `successCode`

---

## Response Mapping Strategy

The adapter should build mappings using response field paths.

Example normalized success fields:

```json
[
  {
    "path": "code",
    "required": true,
    "sourcePath": "soap.return_code_o"
  },
  {
    "path": "message",
    "required": true,
    "sourcePath": "soap.return_message"
  },
  {
    "path": "result.cdtInfo.documentNumber",
    "required": true,
    "sourcePath": "soap.transfer_number"
  }
]
```

Expected `successResponseMapping`:

```json
{
  "code": "soap.return_code_o",
  "message": "soap.return_message",
  "result.cdtInfo.documentNumber": "soap.transfer_number"
}
```

---

## Return Code Path Detection

The adapter should infer return code paths only if the normalized response explicitly contains likely code fields.

Candidate field names:

```text
code
returnCode
return_code
return_code_o
statusCode
```

The adapter should not invent return code paths.

---

## Message Path Detection

The adapter should infer message paths only if the normalized response explicitly contains likely message fields.

Candidate field names:

```text
message
msg
errorMessage
returnMessage
return_message
```

The adapter should not invent message paths.

---

# 8. Area 5 — Generated Flow Inspection Notes in UI

## Purpose

The UI should provide a compact inspection panel after generation.

This gives the user quick visibility into whether the generated flow contains the expected adapter outputs.

---

## Inspection Panel Fields

The UI should display:

```text
Generation source
SOAP subflow enabled?
Warning count
Generated rule IDs
MAP-001 present?
MAP-GENERIC present?
Adapter version
Schema version
```

---

## Why This Matters

The user should not need to inspect the full generated JSON manually every time.

The inspection panel gives a quick quality signal:

```text
Did the normalized adapter generate the expected rule coverage?
```

---

# 9. Day 30 Implementation Phases

## Phase 1 — SOAP and Response Mapping Fidelity Plan

Deliverable:

```text
api-factory-ui/docs/day30/soap_response_mapping_fidelity_plan.md
```

Status:

```text
Completed by this document.
```

---

## Phase 2 — Add Sample Normalized Extraction JSON

Deliverable:

```text
api-factory-ui/samples/normalized-xcdts005-day30.json
```

This file will provide a stable normalized extraction sample for manual regression testing.

---

## Phase 3 — Improve SOAP Template Extraction

File:

```text
api-factory-ui/src/lib/normalized-flow-adapter.ts
```

Improve SOAP template extraction from multiple possible normalized source locations.

---

## Phase 4 — Improve Request Replacement Mapping

File:

```text
api-factory-ui/src/lib/normalized-flow-adapter.ts
```

Improve request replacement mapping for common XCDTS005-style SOAP fields.

---

## Phase 5 — Improve MAP-GENERIC Response Mapping

File:

```text
api-factory-ui/src/lib/normalized-flow-adapter.ts
```

Improve success and failure response mapping quality.

---

## Phase 6 — Add Flow Inspection Notes in UI

File:

```text
api-factory-ui/src/app/page.tsx
```

Add a generated-flow inspection panel using generation metadata and adapter metadata.

---

## Phase 7 — Completion Report

Deliverable:

```text
api-factory-ui/docs/day30/day30_completion_report.md
```

---

## Phase 8 — Push Commands

Run build and commit all Day 30 changes.

---

# 10. Day 30 Success Criteria

Day 30 is complete when:

* [ ] Sample normalized extraction JSON exists
* [ ] SOAP template detection checks multiple normalized locations
* [ ] Explicit downstream mapping is preferred over derived mapping
* [ ] Known request field mapping is derived when explicit mapping is missing
* [ ] `MAP-001.requestReplacements` is improved
* [ ] `MAP-001.xmlTemplate` is improved when source template exists
* [ ] `MAP-GENERIC.successResponseMapping` is improved
* [ ] `MAP-GENERIC.failureResponseMapping` is improved
* [ ] Generated flow inspection panel appears in UI
* [ ] Inspection panel shows whether `MAP-001` exists
* [ ] Inspection panel shows whether `MAP-GENERIC` exists
* [ ] Adapter warnings still appear
* [ ] Legacy generation still works
* [ ] Normalized schema generation still works
* [ ] Build succeeds
* [ ] Documentation is added
* [ ] Changes are committed

---

# 11. Validation Plan

Run:

```powershell
cd C:\Users\BH31021\api-factory-generator\api-factory-ui
npm run build
npm run dev
```

Then manually validate:

* Upload or use the normalized XCDTS005 sample
* Extract requirements
* Generate flow
* Confirm generation source is `normalized_schema`
* Confirm adapter version is `day29-v1` or later
* Confirm generated rule IDs include `MAP-001`
* Confirm generated rule IDs include `MAP-GENERIC`
* Confirm SOAP template is populated when available
* Confirm request replacements are populated
* Confirm response mapping is populated
* Confirm adapter warnings appear only for genuinely missing details
* Confirm legacy generation still works

---

# 12. No-Hallucination Rule

Day 30 must continue following the no-hallucination principle:

```text
Generate only what can be safely derived.
Prefer explicit normalized input.
Use controlled derived mappings only for known enterprise wrapper patterns.
Warn when required information is missing.
Do not invent downstream behavior.
```

---

## Allowed Controlled Derivations

Day 30 may derive known mappings such as:

```text
divisionNumber -> div_number
storeNumber -> location_number
upcNumber -> upc_number
```

because these are known wrapper normalization patterns already used in the project.

---

## Not Allowed

Day 30 should not invent:

* Unknown downstream endpoints
* Unknown SOAP operations
* Unknown SOAP namespaces
* Unknown response success codes
* Unknown business error codes
* Unknown database behavior
* Unknown XML templates

---

# 13. Recommended Day 31 Direction

After Day 30, the recommended next direction is:

```text
Day 31 — Add Golden Sample Comparison for Generated Flow
```

Day 31 can compare generated flow output against a known expected XCDTS005-style flow and produce a gap report.

Possible Day 31 tasks:

* Save golden expected flow JSON
* Generate flow from normalized sample
* Compare generated nodes against expected nodes
* Report missing node types
* Report missing mapping fields
* Report unsupported logic
* Add generation quality score

---

# 14. Final Summary

Day 30 improves the fidelity of normalized-schema-driven flow generation.

The main goal is not just to generate a flow, but to generate a flow that is closer to a real enterprise SOAP wrapper pattern.

The key principle is:

```text
The adapter should prefer explicit normalized schema data, safely derive known wrapper mappings, and clearly warn when mapping details are missing.
```

This improves generation quality, traceability, and confidence in the API Factory Generator.

---
