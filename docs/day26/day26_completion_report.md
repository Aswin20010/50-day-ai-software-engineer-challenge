# Day 26 — Completion Report

## Purpose

This document summarizes the completion of Day 26 of the 50-Day AI Software Engineer Challenge.

Day 26 focused on standardizing the extraction output for the API Factory Generator project.

Day 25 confirmed that the existing TypeScript and Next.js base was already aligned with the roadmap and ahead of the original Day 25 implementation plan. Day 26 builds on that base by creating a stable extraction schema and adding normalization support.

---

## Day 26 Theme

```text
Standardize extraction schema and traceability
```

The goal of Day 26 was to make sure raw extraction output does not directly drive generation.

Instead, raw extraction should first be converted into a stable, traceable, normalized schema.

---

## Why Day 26 Was Needed

The current project already supports requirement input, extraction, business-rule generation, SOAP wrapper support, and Node-RED flow generation.

However, without a standard schema, extracted data can become inconsistent.

For example, the same concept may appear as:

```text
headers
requestHeaders
requiredHeaders
```

Or body fields may appear as:

```text
fields
requiredFields
requestBodyFields
bodyFields
```

This inconsistency can create problems for:

* Node-RED flow generation
* Excel intake generation
* Gap analysis
* Factory LLM code generation
* Evaluation metrics
* Research documentation

Day 26 solves this by defining one normalized extraction contract.

---

## Day 26 Work Completed

Day 26 completed the following phases:

| Phase   | Deliverable                                         | Status    |
| ------- | --------------------------------------------------- | --------- |
| Phase 1 | `docs/day26/extraction_schema.md`                   | Completed |
| Phase 2 | `src/lib/extraction-schema.ts`                      | Completed |
| Phase 3 | `src/lib/extraction-normalizer.ts`                  | Completed |
| Phase 4 | Updated `src/app/api/extract-requirements/route.ts` | Completed |
| Phase 5 | `docs/day26/day26_completion_report.md`             | Completed |

---

# 1. Phase 1 Summary — Standard Extraction Schema Document

## File

```text
api-factory-ui/docs/day26/extraction_schema.md
```

## Summary

Phase 1 defined the standard normalized extraction schema.

The schema defines the expected structure for all future extraction outputs.

The top-level normalized extraction structure is:

```json
{
  "requirementId": "",
  "source": {},
  "api": {},
  "request": {},
  "response": {},
  "businessRules": [],
  "validationRules": [],
  "downstreamDependencies": [],
  "databaseDependencies": [],
  "workflowHints": [],
  "nodeRedHints": [],
  "assumptions": [],
  "openQuestions": [],
  "gaps": [],
  "traceability": {}
}
```

## Key Design Principles

The schema was designed around the following principles:

1. Consistency
2. Traceability
3. No hallucination
4. Extensibility
5. Machine readability
6. Human reviewability

---

# 2. Phase 2 Summary — TypeScript Schema Types

## File

```text
api-factory-ui/src/lib/extraction-schema.ts
```

## Summary

Phase 2 added TypeScript interfaces and helper functions for the standard extraction schema.

This file defines the normalized contract used by the API Factory Generator.

## Main Types Added

The following major types were added:

```text
ExtractedRequirement
RequirementSource
ApiDetails
ApiEndpoint
RequestSchema
RequestHeader
PathParameter
QueryParameter
RequestBodyField
ResponseSchema
SuccessResponseField
ErrorResponseField
ResponseExample
BusinessRule
ValidationRule
DownstreamDependency
DatabaseDependency
WorkflowHint
NodeRedHint
Assumption
OpenQuestion
RequirementGap
TraceabilityMetadata
```

## Helper Functions Added

The following helper functions were added:

```text
createEmptyExtractedRequirement()
createOpenQuestion()
createRequirementGap()
createTrace()
```

These helpers ensure that normalized extraction output always has the required top-level structure.

---

# 3. Phase 3 Summary — Extraction Normalizer

## File

```text
api-factory-ui/src/lib/extraction-normalizer.ts
```

## Summary

Phase 3 added the extraction normalizer.

The normalizer converts raw extraction output into the Day 26 standard schema.

The purpose is:

```text
Raw extracted output
        ↓
Normalized extraction schema
```

## What the Normalizer Handles

The normalizer supports:

* Requirement ID inference
* Source metadata
* API metadata
* Endpoint method normalization
* Endpoint path normalization
* Request header normalization
* Request body field normalization
* Business-rule normalization
* Validation-rule generation
* Downstream dependency normalization
* Database dependency normalization
* Workflow hint creation
* Node-RED hint creation
* Open question creation
* Gap creation
* Traceability metadata creation

---

## Important Normalization Behavior

The normalizer converts multiple possible raw field names into one stable structure.

Examples:

```text
headers
requestHeaders
requiredHeaders
```

are normalized into:

```text
request.headers
```

And:

```text
fields
requiredFields
requestBodyFields
bodyFields
```

are normalized into:

```text
request.bodyFields
```

This prevents downstream modules from needing to understand many different raw extraction shapes.

---

# 4. Phase 4 Summary — Extract Requirements API Update

## File

```text
api-factory-ui/src/app/api/extract-requirements/route.ts
```

## Summary

Phase 4 updated the extract requirements API route to return both backward-compatible output and the new normalized Day 26 output.

## Existing Backward-Compatible Output

The existing UI still receives:

```json
{
  "success": true,
  "source": "",
  "extracted": {},
  "suggestedNodes": [],
  "missingFields": []
}
```

This keeps the current UI working.

---

## New Day 26 Output

The API now also returns:

```json
{
  "rawExtracted": {},
  "normalizedExtraction": {},
  "extractionMetadata": {}
}
```

This allows future modules to consume the stable normalized schema.

---

## Final API Response Shape

The updated API response follows this structure:

```json
{
  "success": true,
  "source": "rule-based",
  "extracted": {},
  "suggestedNodes": [],
  "missingFields": [],
  "rawExtracted": {},
  "normalizedExtraction": {
    "requirementId": "",
    "source": {},
    "api": {},
    "request": {},
    "response": {},
    "businessRules": [],
    "validationRules": [],
    "downstreamDependencies": [],
    "databaseDependencies": [],
    "workflowHints": [],
    "nodeRedHints": [],
    "assumptions": [],
    "openQuestions": [],
    "gaps": [],
    "traceability": {}
  },
  "extractionMetadata": {
    "extractionVersion": "day26-v1",
    "extractionMethod": "",
    "source": "",
    "project": "api-factory-generator",
    "outputTarget": "node-red-flow",
    "schemaVersion": "day26-v1",
    "llmUsed": false,
    "generatedAt": "",
    "generatedBy": "api-factory-generator"
  }
}
```

---

# 5. Files Added or Modified

## New Files

```text
api-factory-ui/docs/day26/extraction_schema.md
api-factory-ui/docs/day26/day26_completion_report.md
api-factory-ui/src/lib/extraction-schema.ts
api-factory-ui/src/lib/extraction-normalizer.ts
```

## Modified Files

```text
api-factory-ui/src/app/api/extract-requirements/route.ts
```

---

# 6. Validation Steps

Run the following commands from the Next.js app folder:

```powershell
cd C:\Users\BH31021\api-factory-generator\api-factory-ui
npm run build
npm run dev
```

If `lint` is available in the app package, also run:

```powershell
npm run lint
```

Then open:

```text
http://localhost:3000
```

Manual validation:

* [ ] App starts successfully
* [ ] Requirement text can be entered or uploaded
* [ ] Extraction runs successfully
* [ ] Existing UI still receives `extracted`
* [ ] API response includes `rawExtracted`
* [ ] API response includes `normalizedExtraction`
* [ ] API response includes `extractionMetadata`
* [ ] `normalizedExtraction.traceability.schemaVersion` is `day26-v1`
* [ ] Missing required details are added to `openQuestions` and `gaps`
* [ ] Node-RED generation still works

---

# 7. Day 26 Completion Criteria

Day 26 is complete when:

* [ ] Standard extraction schema is documented
* [ ] TypeScript schema types are added
* [ ] Extraction normalizer is added
* [ ] Extract requirements API returns normalized output
* [ ] Backward compatibility with existing UI is preserved
* [ ] Traceability metadata exists
* [ ] Open questions and gaps are generated for missing details
* [ ] Build succeeds
* [ ] Documentation is committed

---

# 8. Importance for Future Days

Day 26 creates the foundation for future modules.

The normalized schema can now be used by:

* Node-RED flow generation
* Excel intake generation
* Gap analysis
* Factory LLM code generation
* Evaluation scoring
* Research paper evidence

This is important because future modules should not depend directly on raw extraction output.

---

# 9. Updated Project Pipeline After Day 26

The pipeline is now:

```text
Confluence Requirement
        ↓
Raw Extraction
        ↓
Normalized Extraction Schema
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

Day 26 added the missing normalization layer between raw extraction and generation.

---

# 10. Remaining Gaps After Day 26

The following gaps remain for future days:

| Gap                                                                | Recommended Future Day  |
| ------------------------------------------------------------------ | ----------------------- |
| UI does not yet display normalized extraction                      | Day 27                  |
| Normalizer does not yet fully extract response schemas             | Day 27 or Day 28        |
| Node-RED generator does not yet consume normalized schema directly | Day 28                  |
| Excel intake generation is not implemented                         | Future Project 2 phase  |
| Gap analysis module is not implemented                             | Future Project 2 phase  |
| Evaluation scoring is not implemented                              | Future evaluation phase |
| Automated tests are not implemented                                | Future hardening phase  |

---

# 11. Recommended Day 27 Direction

The recommended next step is:

```text
Day 27 — Display and Review Normalized Extraction Output
```

Day 27 should focus on updating the UI so the user can inspect the normalized extraction output.

Possible Day 27 tasks:

* Add a normalized extraction preview panel
* Show open questions and gaps
* Show traceability metadata
* Allow generated flow to be reviewed against normalized extraction
* Save normalized extraction sample output

---

## Final Summary

Day 26 successfully standardized the extraction output for the API Factory Generator.

The key improvement is that raw extraction is no longer the only structured output. The project now has a normalized extraction schema that can support future artifact generation.

The most important principle from Day 26 is:

```text
Raw extraction should never directly drive generation.
Raw extraction should first be normalized into a stable schema.
```

This improves consistency, traceability, and safety for the rest of the 50-day project.

---

## Final Status

```text
Day 26 completed successfully.

Standard extraction schema created.
TypeScript schema types added.
Extraction normalizer added.
Extract requirements API updated.
Project ready for Day 27.
```
