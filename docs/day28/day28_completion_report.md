# Day 28 — Completion Report

## Purpose

This document summarizes the completion of Day 28 of the 50-Day AI Software Engineer Challenge.

Day 28 focused on connecting the normalized extraction schema to the Node-RED flow generation pipeline.

Day 26 introduced the normalized extraction schema.
Day 27 made the normalized extraction visible in the UI.
Day 28 added the bridge that allows `normalizedExtraction` to become the preferred input source for generating Node-RED flows.

---

## Day 28 Theme

```text
Generate Node-RED Flow from Normalized Schema
```

The main objective was:

```text
Use normalizedExtraction as the source of truth for preparing Node-RED generator input.
```

---

## Why Day 28 Was Needed

Before Day 28, the flow generation process still depended mainly on the older legacy business-rule format.

The existing pipeline was:

```text
Raw extraction
        ↓
Legacy extracted fields
        ↓
Legacy businessRules config
        ↓
Node-RED flow generation
```

This worked, but it did not fully use the normalized schema introduced on Day 26.

Day 28 changed the pipeline to support this target flow:

```text
Raw extraction
        ↓
Normalized extraction
        ↓
Normalized-to-generator adapter
        ↓
Generator-ready business rules
        ↓
Node-RED flow generation
```

This makes the project closer to the final goal of a schema-driven API factory.

---

# 1. Day 28 Work Completed

Day 28 completed the following phases:

| Phase   | Deliverable                                     | Status    |
| ------- | ----------------------------------------------- | --------- |
| Phase 1 | `docs/day28/normalized_to_generator_mapping.md` | Completed |
| Phase 2 | `src/lib/normalized-flow-adapter.ts`            | Completed |
| Phase 3 | Updated `/api/generate` route                   | Completed |
| Phase 4 | Updated frontend generate request               | Completed |
| Phase 5 | Added generation metadata/debug display         | Completed |
| Phase 6 | `docs/day28/day28_completion_report.md`         | Completed |

---

# 2. Phase 1 Summary — Normalized-to-Generator Mapping Plan

## File

```text
api-factory-ui/docs/day28/normalized_to_generator_mapping.md
```

## Summary

Phase 1 defined how the Day 26 normalized extraction schema should map to the existing Node-RED generator input.

The document explained how the adapter should convert:

```text
normalizedExtraction
```

into:

```text
generator-ready businessRules
```

The plan preserved the existing generator instead of rewriting it.

---

## Key Mapping Decisions

| Normalized Schema Field      | Generator Field          |
| ---------------------------- | ------------------------ |
| `api.apiName`                | `flowName`               |
| `api.endpoint.method`        | `HTTP-IN.httpMethod`     |
| `api.endpoint.path`          | `HTTP-IN.httpPath`       |
| `request.headers`            | `BR-002.requiredHeaders` |
| `request.bodyFields`         | `BR-003.requiredFields`  |
| `validationRules`            | `BR-004.numericFields`   |
| `downstreamDependencies`     | `includeSoapSubflow`     |
| `api.endpoint.operationName` | `BR-007.operation`       |
| `api.endpoint.path`          | `BR-007.endpoint`        |

---

# 3. Phase 2 Summary — Normalized Flow Adapter

## File

```text
api-factory-ui/src/lib/normalized-flow-adapter.ts
```

## Summary

Phase 2 introduced a new adapter file responsible for converting normalized extraction into the format expected by the existing Node-RED generator.

The adapter exposes:

```ts
adaptNormalizedExtractionToFlowInput(normalizedExtraction)
```

The adapter returns:

```ts
{
  flowName,
  businessRules,
  includeSoapSubflow,
  adapterMetadata
}
```

---

## Adapter Responsibilities

The adapter currently handles:

* Flow name creation
* HTTP input rule creation
* Payload validation rule creation
* Required header mapping
* Required body field mapping
* Numeric validation field mapping
* Controlled SOAP default mapping
* Known normalization mapping
* Request metadata mapping
* SOAP subflow decision
* Adapter metadata creation

---

## Generated Rule IDs

The adapter generates these initial rules:

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

This gives the generator enough information to begin producing a Node-RED flow from the normalized schema.

---

# 4. Phase 3 Summary — Generate API Update

## File Modified

```text
api-factory-ui/src/app/api/generate/route.ts
```

## Summary

Phase 3 updated the `/api/generate` endpoint so it can accept both old and new request styles.

Legacy request style:

```json
{
  "flowName": "API Factory Generated Flow",
  "businessRules": [],
  "includeSoapSubflow": true
}
```

New Day 28 request style:

```json
{
  "normalizedExtraction": {}
}
```

---

## Generation Source Selection

The generate route now follows this logic:

```text
If normalizedExtraction exists:
    Use normalized-flow-adapter
    Generate from adapted business rules
    generationSource = normalized_schema

Else:
    Use legacy businessRules
    generationSource = legacy_rules
```

This keeps old functionality working while introducing normalized-schema-based generation.

---

## Response Metadata Added

The generate API now returns:

```json
{
  "generationMetadata": {
    "generationVersion": "day28-v1",
    "generationSource": "normalized_schema",
    "adapterMetadata": {},
    "flowName": "",
    "ruleCount": 8,
    "includeSoapSubflow": true,
    "generatedAt": "",
    "generatedBy": "api-factory-generator"
  }
}
```

The most important field is:

```text
generationSource
```

It proves whether the flow was generated from:

```text
normalized_schema
```

or:

```text
legacy_rules
```

---

# 5. Phase 4 Summary — Frontend Generate Request Update

## File Modified

```text
api-factory-ui/src/app/page.tsx
```

## Summary

Phase 4 updated the frontend generate flow action so it sends `normalizedExtraction` to `/api/generate`.

The request payload now includes:

```ts
{
  flowName,
  businessRules,
  includeSoapSubflow,
  normalizedExtraction
}
```

This means that after a successful extraction, the generated flow can use the normalized schema path.

---

## Backward Compatibility

The frontend still sends legacy `businessRules`.

This is intentional.

It allows the backend to fall back to legacy generation when `normalizedExtraction` is not available.

---

# 6. Phase 5 Summary — Generation Metadata UI

## File Modified

```text
api-factory-ui/src/app/page.tsx
```

## Summary

Phase 5 added a visible generation metadata/debug section to the UI.

After generating a flow, the UI now displays:

* Generation source
* Raw source value
* Generation version
* Rule count
* SOAP subflow decision
* Generated by
* Adapter version
* Schema version
* Requirement ID
* Open question count
* Gap count
* Full generation metadata JSON

---

## Expected UI Values

When generation uses normalized extraction, the UI should show:

```text
Generation Source: Normalized Schema
Raw Source Value: normalized_schema
Adapter Version: day28-v1
```

When generation uses legacy rules, the UI should show:

```text
Generation Source: Legacy Rules
Raw Source Value: legacy_rules
```

This makes Day 28 behavior easy to verify.

---

# 7. Current Pipeline After Day 28

The project pipeline is now:

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
Node-RED Flow Generator
        ↓
Generated Flow JSON
```

Day 28 added the adapter layer and generation metadata visibility.

---

# 8. Files Added or Modified

## New Files

```text
api-factory-ui/docs/day28/normalized_to_generator_mapping.md
api-factory-ui/docs/day28/day28_completion_report.md
api-factory-ui/src/lib/normalized-flow-adapter.ts
```

## Modified Files

```text
api-factory-ui/src/app/api/generate/route.ts
api-factory-ui/src/app/page.tsx
```

---

# 9. Validation Steps

Run these commands from the Next.js app folder:

```powershell
cd C:\Users\BH31021\api-factory-generator\api-factory-ui
npm run build
npm run dev
```

Then open:

```text
http://localhost:3000
```

---

## Manual Validation Checklist

Day 28 should be considered complete when these checks pass:

* [ ] App builds successfully
* [ ] App starts successfully
* [ ] Requirement file can be uploaded
* [ ] Requirement extraction succeeds
* [ ] Normalized extraction review appears
* [ ] Open questions and gaps appear
* [ ] Generate button works
* [ ] Generated flow appears
* [ ] Generation metadata appears
* [ ] Generation source shows `normalized_schema` after extraction
* [ ] Adapter metadata appears when normalized schema is used
* [ ] Rule count is displayed
* [ ] SOAP subflow decision is displayed
* [ ] Full generation metadata JSON can be viewed
* [ ] Legacy generation still works when normalized extraction is not available

---

# 10. Day 28 Completion Criteria

Day 28 is complete when:

* [ ] `normalized-flow-adapter.ts` exists
* [ ] Adapter converts normalized extraction into generator-ready rules
* [ ] `/api/generate` accepts `normalizedExtraction`
* [ ] `/api/generate` still accepts legacy `businessRules`
* [ ] Frontend sends `normalizedExtraction` during generation
* [ ] UI displays generation metadata
* [ ] UI shows whether generation used normalized schema or legacy rules
* [ ] Node-RED flow still generates successfully
* [ ] Build succeeds
* [ ] Documentation is added
* [ ] Changes are committed

---

# 11. Remaining Gaps After Day 28

The following gaps remain for future days:

| Gap                                                           | Recommended Future Day  |
| ------------------------------------------------------------- | ----------------------- |
| Adapter only maps initial core rules                          | Day 29                  |
| Response mapping is not fully normalized-schema-driven        | Day 29                  |
| SOAP XML template mapping still depends on legacy fields      | Day 29                  |
| Critical gaps do not block generation                         | Future enhancement      |
| Normalized extraction cannot be edited in UI                  | Future enhancement      |
| Generated flow is not compared against expected golden output | Future evaluation phase |
| Automated tests are not added yet                             | Future hardening phase  |
| Excel generation is not implemented                           | Future Project 2 phase  |
| Gap analysis module is not implemented                        | Future Project 2 phase  |

---

# 12. Recommended Day 29 Direction

The recommended next step is:

```text
Day 29 — Improve Normalized Adapter Coverage
```

Day 29 should focus on making the normalized adapter more complete.

Possible Day 29 tasks:

* Map normalized downstream dependency request mappings into `MAP-001`
* Map response fields into `MAP-GENERIC`
* Add SOAP XML template support from normalized downstream dependency data
* Improve response mapping generation
* Preserve traceability from normalized fields into generated Node-RED nodes
* Add adapter warnings when normalized data is insufficient

---

## Day 29 Target

The Day 29 target should be:

```text
Generate more complete Node-RED flows from normalized schema, including downstream mapping and response mapping.
```

---

# 13. Technical Notes

## Backward Compatibility Strategy

Day 28 intentionally kept the old legacy business-rule path.

This means the generate API supports both:

```text
legacy_rules
```

and:

```text
normalized_schema
```

This approach reduces risk and allows gradual migration.

---

## TypeScript Fix Applied

During Day 28, a type-check issue occurred because `businessRules` was initially inferred as `unknown[]`.

The fix was to derive the expected generator input type directly from `generateNodeRedFlow`:

```ts
type GenerateNodeRedInput = Parameters<typeof generateNodeRedFlow>[0];
type GeneratorBusinessRules = GenerateNodeRedInput["businessRules"];
```

This ensured the adapted business rules match the expected generator input type.

---

# 14. Final Summary

Day 28 successfully connected normalized extraction to Node-RED generation.

The most important improvement is that the system can now generate from the standardized normalized schema instead of relying only on legacy extracted fields.

The key principle from Day 28 is:

```text
Normalized schema should become the source of truth, but legacy generation must continue to work during the transition.
```

This improves the architecture, traceability, and long-term reliability of the API Factory Generator.

---

## Final Status

```text
Day 28 completed successfully.

Normalized-to-generator adapter added.
Generate API supports normalizedExtraction.
Frontend sends normalizedExtraction.
Generation metadata is visible in UI.
Legacy generation fallback is preserved.
Project ready for Day 29.
```
