# Day 27 — Completion Report

## Purpose

This document summarizes the completion of Day 27 of the 50-Day AI Software Engineer Challenge.

Day 27 focused on making the normalized extraction output visible and reviewable in the API Factory Generator UI.

Day 26 introduced the standard extraction schema and extraction normalizer. Day 27 built on that by updating the frontend so users can inspect the normalized extraction result, traceability metadata, open questions, and requirement gaps before generating a Node-RED flow.

---

## Day 27 Theme

```text
Display and review normalized extraction output
```

The main objective was:

```text
Show normalizedExtraction in the UI so the user can review extracted API details, business rules, validation rules, gaps, open questions, and traceability before generating Node-RED flow.
```

---

## Why Day 27 Was Needed

After Day 26, the backend returned the following new fields from `/api/extract-requirements`:

```json
{
  "rawExtracted": {},
  "normalizedExtraction": {},
  "extractionMetadata": {}
}
```

However, the frontend was still mainly using the older `extracted` object.

This meant the normalized schema existed in the backend response, but it was not visible or reviewable in the UI.

Day 27 fixed this by adding frontend state and UI panels for reviewing the standardized extraction output.

---

## Day 27 Work Completed

Day 27 completed the following phases:

| Phase   | Deliverable                                    | Status    |
| ------- | ---------------------------------------------- | --------- |
| Phase 1 | `docs/day27/normalized_extraction_ui_plan.md`  | Completed |
| Phase 2 | Added frontend state for normalized extraction | Completed |
| Phase 3 | Stored normalized extraction response from API | Completed |
| Phase 4 | Added normalized extraction review panel       | Completed |
| Phase 5 | Added open questions and gaps review panel     | Completed |
| Phase 6 | `docs/day27/day27_completion_report.md`        | Completed |

---

# 1. Phase 1 Summary — Normalized Extraction UI Plan

## File

```text
api-factory-ui/docs/day27/normalized_extraction_ui_plan.md
```

## Summary

Phase 1 defined the plan for displaying normalized extraction data in the frontend.

The plan explained:

* Why normalized extraction review is needed
* What backend fields should be displayed
* Which frontend state variables should be added
* What summary fields should appear in the UI
* How open questions and gaps should be shown
* Why traceability metadata matters
* What should remain out of scope for Day 27

---

## UI Review Goal

The goal of the review panel is:

```text
A user should be able to see what the system understood from the requirement before generating a Node-RED flow.
```

This improves trust and makes the factory pipeline more explainable.

---

# 2. Phase 2 Summary — Frontend State Updates

## File Modified

```text
api-factory-ui/src/app/page.tsx
```

## Summary

Phase 2 added frontend state to store the new Day 26 backend response fields.

The following state values were added:

```tsx
const [rawExtracted, setRawExtracted] = useState<Record<string, unknown> | null>(null);

const [normalizedExtraction, setNormalizedExtraction] =
  useState<NormalizedExtraction | null>(null);

const [extractionMetadata, setExtractionMetadata] =
  useState<ExtractionMetadata | null>(null);

const [showNormalizedJson, setShowNormalizedJson] = useState(false);
```

---

## Types Added

Frontend-safe TypeScript types were added for:

```text
NormalizedExtraction
ExtractionMetadata
```

These types allow the UI to safely display normalized extraction data without directly depending on backend schema imports.

---

# 3. Phase 3 Summary — Store Normalized Extraction Response

## File Modified

```text
api-factory-ui/src/app/page.tsx
```

## Summary

Phase 3 updated the extraction API call so the frontend stores the new response fields from `/api/extract-requirements`.

The UI now stores:

```tsx
setRawExtracted(data.rawExtracted || null);
setNormalizedExtraction(data.normalizedExtraction || null);
setExtractionMetadata(data.extractionMetadata || null);
```

---

## Backward Compatibility Preserved

The old UI behavior was preserved by continuing to use:

```tsx
const extracted = data.extracted || {};
```

This means existing fields and Node-RED flow generation still work while the new normalized review layer is added.

---

## Failure Handling

The frontend clears normalized extraction state when extraction fails:

```tsx
setRawExtracted(null);
setNormalizedExtraction(null);
setExtractionMetadata(null);
setShowNormalizedJson(false);
```

This prevents old extraction data from being displayed after a failed extraction.

---

# 4. Phase 4 Summary — Normalized Extraction Review Panel

## File Modified

```text
api-factory-ui/src/app/page.tsx
```

## Summary

Phase 4 added a new UI section:

```text
Normalized Extraction Review
```

This panel appears when `normalizedExtraction` is available.

---

## Summary Fields Displayed

The review panel displays:

| Field             | Source                                                                                |
| ----------------- | ------------------------------------------------------------------------------------- |
| Requirement ID    | `normalizedExtraction.requirementId`                                                  |
| API Name          | `normalizedExtraction.api.apiName`                                                    |
| Endpoint          | `normalizedExtraction.api.endpoint.method` + `normalizedExtraction.api.endpoint.path` |
| Operation         | `normalizedExtraction.api.endpoint.operationName`                                     |
| Schema Version    | `normalizedExtraction.traceability.schemaVersion`                                     |
| Extraction Method | `normalizedExtraction.traceability.extractionMethod`                                  |
| LLM Used          | `normalizedExtraction.traceability.llmUsed`                                           |
| Generated At      | `normalizedExtraction.traceability.generatedAt`                                       |

---

## Count Metrics Displayed

The panel also displays count-based extraction quality signals:

| Metric           | Source                                                |
| ---------------- | ----------------------------------------------------- |
| Headers          | `normalizedExtraction.request.headers.length`         |
| Path Params      | `normalizedExtraction.request.pathParameters.length`  |
| Query Params     | `normalizedExtraction.request.queryParameters.length` |
| Body Fields      | `normalizedExtraction.request.bodyFields.length`      |
| Business Rules   | `normalizedExtraction.businessRules.length`           |
| Validation Rules | `normalizedExtraction.validationRules.length`         |
| Downstreams      | `normalizedExtraction.downstreamDependencies.length`  |
| Databases        | `normalizedExtraction.databaseDependencies.length`    |

---

## Why This Matters

These summary fields help the user quickly understand whether the requirement was extracted correctly.

For example, if the requirement clearly has headers but the UI shows:

```text
Headers: 0
```

then the user immediately knows the extraction needs review.

---

# 5. Phase 5 Summary — Open Questions and Gaps Review Panel

## File Modified

```text
api-factory-ui/src/app/page.tsx
```

## Summary

Phase 5 added focused review sections for:

```text
Open Questions
Gaps
```

These are displayed from:

```tsx
normalizedExtraction.openQuestions
normalizedExtraction.gaps
```

---

## Open Questions Display

Each open question displays:

* Question ID
* Question text
* Area
* Severity

Example:

```text
OQ-001 — API endpoint path is missing or unclear.
Area: api
Severity: critical
```

---

## Gaps Display

Each gap displays:

* Gap ID
* Description
* Category
* Severity
* Impact
* Recommended action

Example:

```text
GAP-001 — API endpoint path is missing.
Category: missing_required_detail
Severity: critical
Impact: Flow generation and future code generation may not know which route to create.
Recommended Action: Confirm the API endpoint path.
```

---

## Why This Matters

The open questions and gaps panels support the no-hallucination principle:

```text
If information is missing, expose it for review instead of inventing it.
```

This is important because the future factory should not blindly generate implementation artifacts from incomplete requirements.

---

# 6. Raw Normalized JSON Preview

Day 27 also added a debug/review option:

```text
Show Normalized JSON
```

When clicked, the UI displays:

```tsx
JSON.stringify(
  {
    rawExtracted,
    extractionMetadata,
    normalizedExtraction,
  },
  null,
  2
)
```

This helps with:

* Debugging
* Copying normalized samples
* Research documentation
* Comparing raw extraction with normalized extraction
* Validating schema output

---

# 7. Current Pipeline After Day 27

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
Business Rule / Node-RED Generation
        ↓
Generated Flow JSON
```

Day 27 added the UI review layer.

---

# 8. Files Added or Modified

## New Files

```text
api-factory-ui/docs/day27/normalized_extraction_ui_plan.md
api-factory-ui/docs/day27/day27_completion_report.md
```

## Modified Files

```text
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

Day 27 should be considered complete when the following checks pass:

* [ ] App starts successfully
* [ ] Requirement file can be uploaded
* [ ] Extraction runs successfully
* [ ] Existing extracted fields still populate the UI
* [ ] `normalizedExtraction` is stored in frontend state
* [ ] `extractionMetadata` is stored in frontend state
* [ ] `rawExtracted` is stored in frontend state
* [ ] Normalized Extraction Review panel appears
* [ ] Requirement ID is displayed
* [ ] API name is displayed
* [ ] Endpoint method and path are displayed
* [ ] Schema version is displayed
* [ ] Extraction method is displayed
* [ ] LLM used value is displayed
* [ ] Counts for headers, body fields, business rules, validations, downstreams, and databases are displayed
* [ ] Open questions are displayed
* [ ] Gaps are displayed
* [ ] Show/Hide Normalized JSON works
* [ ] Node-RED generation still works
* [ ] Generated flow can still be downloaded

---

# 10. Day 27 Completion Criteria

Day 27 is complete when:

* [ ] UI stores normalized extraction data
* [ ] UI displays normalized extraction summary
* [ ] UI displays traceability metadata
* [ ] UI displays open questions
* [ ] UI displays gaps
* [ ] UI can show raw normalized JSON
* [ ] Old extracted-field UI still works
* [ ] Node-RED generation still works
* [ ] Build succeeds
* [ ] Documentation is added
* [ ] Changes are committed

---

# 11. Remaining Gaps After Day 27

The following gaps remain for future days:

| Gap                                                          | Recommended Future Day  |
| ------------------------------------------------------------ | ----------------------- |
| UI does not allow editing normalized extraction              | Future enhancement      |
| Node-RED generator still consumes older business-rule config | Day 28                  |
| Critical gaps do not block generation yet                    | Future enhancement      |
| Normalized extraction is not saved as a sample JSON file     | Future enhancement      |
| Excel intake generation is not implemented                   | Future Project 2 phase  |
| Gap analysis module is not implemented                       | Future Project 2 phase  |
| Evaluation scoring is not implemented                        | Future evaluation phase |
| Automated UI/component tests are not implemented             | Future hardening phase  |

---

# 12. Recommended Day 28 Direction

The recommended next step is:

```text
Day 28 — Generate Node-RED Flow from Normalized Schema
```

Day 28 should focus on connecting the new normalized extraction schema to the existing Node-RED generator.

Currently, the UI still preserves older generated business-rule configuration for compatibility.

The next improvement is to make the normalized schema the source of truth for generation.

Possible Day 28 tasks:

* Convert `normalizedExtraction.businessRules` into generator-ready business rules
* Convert `normalizedExtraction.nodeRedHints` into Node-RED nodes
* Ensure rule IDs remain traceable
* Compare old generation output vs normalized-schema generation output
* Keep backward compatibility during the transition

---

## Final Summary

Day 27 successfully made the normalized extraction output visible and reviewable in the UI.

The key improvement is that users can now inspect what the system understood from the requirement before generating a Node-RED flow.

The most important principle from Day 27 is:

```text
A factory should not generate blindly from hidden extraction output.
The user should be able to inspect the normalized requirement understanding before generation.
```

This improves transparency, traceability, debugging, and research value for the API Factory Generator project.

---

## Final Status

```text
Day 27 completed successfully.

Normalized extraction review UI added.
Open questions and gaps are visible.
Traceability metadata is visible.
Old UI behavior is preserved.
Project ready for Day 28.
```
