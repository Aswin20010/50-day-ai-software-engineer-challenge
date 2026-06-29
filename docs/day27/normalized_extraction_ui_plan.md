# Day 27 — Phase 1: Normalized Extraction UI Plan

## Purpose

This document defines the UI plan for displaying and reviewing normalized extraction output in the API Factory Generator project.

Day 26 introduced the standard extraction schema and added a normalizer that converts raw extraction output into a stable `normalizedExtraction` structure.

Day 27 focuses on making that normalized output visible in the frontend so users can review the extracted requirement details before generating Node-RED flow JSON.

---

## Day 27 Objective

The objective of Day 27 is:

```text
Display normalizedExtraction in the UI so the user can review the extracted API contract, business rules, validation rules, open questions, gaps, and traceability metadata.
```

This improves transparency, traceability, and trust in the generated artifacts.

---

## Why the UI Needs Normalized Extraction Review

Before Day 26, the frontend mainly worked with raw extracted fields.

Raw extracted fields are useful for quick generation, but they are not enough for a reliable API factory because the structure may vary between requirements.

For example, raw extraction may produce:

```text
headers
requestHeaders
requiredHeaders
```

The normalized schema converts these into one stable location:

```text
normalizedExtraction.request.headers
```

The UI should now show the normalized output so the user can confirm whether the system understood the requirement correctly.

---

## Updated Pipeline After Day 27

The project pipeline becomes:

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

Day 27 adds the review layer between normalized extraction and generation.

---

## Current Backend Output

After Day 26, the `/api/extract-requirements` endpoint returns both old and new structures.

Existing backward-compatible output:

```json
{
  "success": true,
  "source": "rule-based",
  "extracted": {},
  "suggestedNodes": [],
  "missingFields": []
}
```

New Day 26 standardized output:

```json
{
  "rawExtracted": {},
  "normalizedExtraction": {},
  "extractionMetadata": {}
}
```

Day 27 will store and display the new fields in the UI.

---

# 1. Frontend State Plan

The frontend should continue supporting the existing UI state, but add new state for Day 27.

## Existing State To Keep

The current UI should continue using the existing extracted values so nothing breaks.

Examples of existing state:

```text
flowName
operation
endpoint
businessRules
generatedFlow
```

These should remain unchanged.

---

## New State To Add

The frontend should add state for:

```text
normalizedExtraction
extractionMetadata
rawExtracted
```

Recommended state variables:

```ts
const [normalizedExtraction, setNormalizedExtraction] = useState<any | null>(null);
const [extractionMetadata, setExtractionMetadata] = useState<any | null>(null);
const [rawExtracted, setRawExtracted] = useState<any | null>(null);
```

For Day 27, using `any` is acceptable to avoid blocking UI progress. Later days can import the TypeScript schema type from `src/lib/extraction-schema.ts`.

---

# 2. API Response Handling Plan

When the UI calls:

```text
/api/extract-requirements
```

The response should be handled like this:

```ts
setRawExtracted(data.rawExtracted || null);
setNormalizedExtraction(data.normalizedExtraction || null);
setExtractionMetadata(data.extractionMetadata || null);
```

The existing UI should still use:

```ts
data.extracted
```

for backward compatibility.

This means Day 27 should not break the current generate-flow process.

---

# 3. Normalized Extraction Review Panel

A new UI section should be added after extraction succeeds.

Recommended section title:

```text
Normalized Extraction Review
```

This panel should summarize the most important normalized extraction fields.

---

## Summary Fields To Display

The panel should display:

| Field                         | Source                                               |
| ----------------------------- | ---------------------------------------------------- |
| Requirement ID                | `normalizedExtraction.requirementId`                 |
| API Name                      | `normalizedExtraction.api.apiName`                   |
| Description                   | `normalizedExtraction.api.description`               |
| HTTP Method                   | `normalizedExtraction.api.endpoint.method`           |
| Endpoint Path                 | `normalizedExtraction.api.endpoint.path`             |
| API Version                   | `normalizedExtraction.api.endpoint.version`          |
| Operation Name                | `normalizedExtraction.api.endpoint.operationName`    |
| Request Headers Count         | `normalizedExtraction.request.headers.length`        |
| Request Body Fields Count     | `normalizedExtraction.request.bodyFields.length`     |
| Business Rules Count          | `normalizedExtraction.businessRules.length`          |
| Validation Rules Count        | `normalizedExtraction.validationRules.length`        |
| Downstream Dependencies Count | `normalizedExtraction.downstreamDependencies.length` |
| Database Dependencies Count   | `normalizedExtraction.databaseDependencies.length`   |
| Open Questions Count          | `normalizedExtraction.openQuestions.length`          |
| Gaps Count                    | `normalizedExtraction.gaps.length`                   |

---

## Example UI Layout

Recommended layout:

```text
Normalized Extraction Review

Requirement ID: REQ-XCDTS005
API Name: XCDTS005 Transfer Details
Endpoint: POST /api/v1/cdt/transfer-details
Operation: XCDTS005

Request Headers: 4
Request Body Fields: 3
Business Rules: 7
Validation Rules: 10
Downstream Dependencies: 1
Open Questions: 1
Gaps: 1
```

This gives the user a quick view of extraction quality.

---

# 4. Traceability Metadata Panel

The UI should display traceability metadata from:

```text
normalizedExtraction.traceability
```

and/or:

```text
extractionMetadata
```

Recommended fields:

| Field             | Source                                               |
| ----------------- | ---------------------------------------------------- |
| Schema Version    | `normalizedExtraction.traceability.schemaVersion`    |
| Extraction Method | `normalizedExtraction.traceability.extractionMethod` |
| LLM Used          | `normalizedExtraction.traceability.llmUsed`          |
| Generated At      | `normalizedExtraction.traceability.generatedAt`      |
| Generated By      | `normalizedExtraction.traceability.generatedBy`      |

---

## Why Traceability Matters

Traceability helps answer:

* Which schema version produced this output?
* Was an LLM used?
* When was the extraction generated?
* Which generator produced it?
* Can this result be compared in evaluation metrics later?

This is important for both engineering trust and research documentation.

---

# 5. Open Questions Panel

The UI should show open questions clearly.

Open questions come from:

```text
normalizedExtraction.openQuestions
```

Each open question should display:

| Field        | Meaning                  |
| ------------ | ------------------------ |
| `questionId` | Stable question ID       |
| `question`   | The actual question      |
| `area`       | Requirement area         |
| `severity`   | Critical, medium, or low |

---

## Example Display

```text
Open Questions

OQ-001 — API endpoint path is missing or unclear.
Area: api
Severity: critical
```

---

## Why Open Questions Matter

Open questions prevent the system from silently guessing missing requirement details.

This supports the project’s no-hallucination principle:

```text
If information is missing, expose it for review instead of inventing it.
```

---

# 6. Gaps Panel

The UI should show requirement gaps clearly.

Gaps come from:

```text
normalizedExtraction.gaps
```

Each gap should display:

| Field               | Meaning                      |
| ------------------- | ---------------------------- |
| `gapId`             | Stable gap ID                |
| `category`          | Gap category                 |
| `description`       | What is missing or unclear   |
| `impact`            | Why it matters               |
| `severity`          | Critical, medium, or low     |
| `recommendedAction` | What the user should do next |

---

## Example Display

```text
Gaps

GAP-001 — API endpoint path is missing.
Category: missing_required_detail
Severity: critical
Impact: Flow generation and future code generation may not know which route to create.
Recommended Action: Confirm the API endpoint path.
```

---

## Why Gaps Matter

Gaps help decide whether generation should proceed.

A future version can block generation when critical gaps exist.

For Day 27, gaps should be displayed but generation does not need to be blocked yet.

---

# 7. Raw JSON Debug View

The UI should optionally include a raw JSON view for debugging.

Recommended section:

```text
Normalized Extraction JSON
```

This can display:

```tsx
<pre>{JSON.stringify(normalizedExtraction, null, 2)}</pre>
```

This is useful for:

* Debugging
* Research documentation
* Verifying schema output
* Copying normalized samples
* Comparing raw vs normalized extraction

---

# 8. UI Placement Plan

Recommended placement in `page.tsx`:

```text
Requirement Input Section
        ↓
Extracted Configuration Section
        ↓
Normalized Extraction Review Section
        ↓
Open Questions and Gaps Section
        ↓
Generated Flow Section
```

The normalized review should appear after extraction and before generated flow output.

---

# 9. UI Behavior Rules

The UI should follow these rules:

1. If `normalizedExtraction` is null, do not show the review panel.
2. If `openQuestions` is empty, show a message like `No open questions found.`
3. If `gaps` is empty, show a message like `No gaps found.`
4. If traceability metadata is missing, show `Not available`.
5. Existing extraction and generation UI should continue working.
6. Do not block flow generation yet.
7. Do not remove the old `extracted` state yet.

---

# 10. Day 27 Implementation Scope

## In Scope

Day 27 includes:

* Add frontend state for normalized extraction
* Store normalized extraction from API response
* Display normalized extraction summary
* Display traceability metadata
* Display open questions
* Display gaps
* Add optional raw JSON preview
* Keep old UI behavior working

---

## Out of Scope

Day 27 does not include:

* Blocking generation based on critical gaps
* Editing normalized extraction in the UI
* Saving normalized extraction to a file
* Making Node-RED generator consume normalized extraction directly
* Excel generation
* Gap analysis engine
* Evaluation scoring
* Automated tests

These will be handled in future days.

---

# 11. Success Criteria

Day 27 Phase 1 is successful when this plan clearly defines:

* What normalized extraction data should be displayed
* Where the UI should display it
* How gaps and open questions should appear
* How traceability metadata should be shown
* How existing UI behavior should be preserved
* What is in scope and out of scope for Day 27

---

## Final Summary

Day 27 makes the normalized extraction schema visible and reviewable.

The main principle is:

```text
A factory should not generate blindly from hidden extraction output.
The user should be able to inspect the normalized requirement understanding before generation.
```

This improves trust, debugging, research value, and future readiness for Excel generation, gap analysis, and factory LLM code generation.

---
