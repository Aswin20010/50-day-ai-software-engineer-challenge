# Day 34 — Rule Mapping Refactor Testing Checklist

## Purpose

This checklist validates the Day 34 rule mapping refactor.

Day 34 moved the extracted requirement-to-business-rule mapping logic out of `src/app/page.tsx` and into dedicated mapper files.

The goal is to confirm that the refactor did not change extraction behavior, suggested node mapping, rule population, rule editing, flow generation, metadata display, or generated JSON download behavior.

---

## Testing Scope

This checklist covers:

* Day 34 file structure
* Mapper type validation
* Suggested nodes mapper behavior
* Extracted field mapper behavior
* `page.tsx` integration
* Build validation
* Requirement extraction smoke test
* Rule population regression test
* Flow generation smoke test
* Download validation

---

## 1. File Structure Validation

Confirm these files exist:

```text
src/types/day34.ts
src/mappers/day34/suggestedNodesMapper.ts
src/mappers/day34/extractedRequirementRuleMapper.ts
docs/day34/rule_mapping_refactor_plan.md
docs/day34/mapper_test_notes.md
docs/day34/testing_checklist.md
```

Expected result:

```text
All Day 34 mapper refactor files are present.
```

---

## 2. Mapper Type Validation

Open:

```text
src/types/day34.ts
```

Confirm it exports:

```ts
SuggestedNodesInput
MapSuggestedNodesToRulesInput
MapExtractedFieldsToRulesInput
BuildRulesFromExtractionInput
BuildRulesFromExtractionResult
RuleFieldMapper
```

Expected result:

```text
Mapper functions have shared input and output types.
```

---

## 3. Suggested Nodes Mapper Validation

Open:

```text
src/mappers/day34/suggestedNodesMapper.ts
```

Confirm it exports:

```ts
mapSuggestedNodesToRules
```

Confirm it keeps only valid suggested node IDs:

```ts
typeof value === "string" && value.trim() !== ""
```

Expected result:

```text
Suggested nodes are converted into BusinessRule objects safely.
```

---

## 4. Suggested Nodes Fallback Validation

Confirm `mapSuggestedNodesToRules` returns fallback rules when `suggestedNodes` is not an array:

```ts
if (!Array.isArray(suggestedNodes)) {
  return fallbackRules.filter((rule): rule is BusinessRule => Boolean(rule));
}
```

Expected result:

```text
Existing rules are preserved when suggested nodes are unavailable.
```

---

## 5. Extracted Rule Mapper Validation

Open:

```text
src/mappers/day34/extractedRequirementRuleMapper.ts
```

Confirm it exports:

```ts
mapExtractedFieldsToRules
buildRulesFromExtraction
```

Expected result:

```text
Extracted requirement field mapping is now handled outside page.tsx.
```

---

## 6. Default Request Header Mapping Validation

In `extractedRequirementRuleMapper.ts`, confirm this default still exists:

```ts
const DEFAULT_REQUEST_HEADER_MAPPINGS = JSON.stringify(
  {
    "x-transaction-id": "transactionId",
    "x-source-system": "sourceSystem",
  },
  null,
  2
);
```

Expected result:

```text
BR-007 default request header mapping behavior is preserved.
```

---

## 7. Rule Mapping Coverage Check

Confirm the mapper handles these rule IDs:

```text
HTTP-IN
BR-002
BR-003
BR-004
BR-005
BR-006
BR-007
BR-008
BR-009
BR-012
BR-013
BR-014
MAP-001
MAP-GENERIC
```

Expected result:

```text
All rule types from the previous inline page mapping are still handled.
```

---

## 8. Unknown Rule Preservation Check

Confirm the mapper returns unknown rule types unchanged:

```ts
return rule;
```

Expected result:

```text
Custom or future rule types are not destroyed by the mapper.
```

---

## 9. `page.tsx` Import Check

Open:

```text
src/app/page.tsx
```

Confirm it imports:

```ts
import { buildRulesFromExtraction } from "@/mappers/day34/extractedRequirementRuleMapper";
```

Expected result:

```text
page.tsx uses the Day 34 mapper.
```

---

## 10. `page.tsx` Inline Mapping Removal Check

Search inside:

```text
src/app/page.tsx
```

Confirm these no longer exist:

```ts
if (rule.brId === "HTTP-IN")
if (rule.brId === "BR-002")
if (rule.brId === "MAP-001")
if (rule.brId === "MAP-GENERIC")
```

Expected result:

```text
The large inline rule mapping block has been removed from page.tsx.
```

---

## 11. `page.tsx` Mapper Usage Check

Confirm the extraction success block now uses:

```ts
const populatedRules = buildRulesFromExtraction({
  suggestedNodes: data.suggestedNodes,
  fallbackRules: rules,
  extracted,
});

setRules(populatedRules);
```

Expected result:

```text
page.tsx delegates rule population to the mapper.
```

---

## 12. Build Validation

Run:

```bash
npm run build
```

Expected result:

```text
Build completes successfully.
```

If build fails, check:

* Missing mapper import
* Incorrect mapper file path
* Missing `src/types/day34.ts`
* Type mismatch for `data.suggestedNodes`
* Type mismatch for `extracted`
* Circular import issues
* Unused imports blocked by lint

---

## 13. Dev Server Validation

Run:

```bash
npm run dev
```

Open the app in the browser.

Expected result:

```text
API Factory UI loads successfully.
```

Check browser console for:

* Module not found errors
* Undefined mapper function errors
* Runtime mapping errors
* Hydration errors

---

## 14. Missing File Validation Test

Without uploading a file, click:

```text
Extract Requirements from Confluence
```

Expected result:

```text
Please upload a Confluence export file first.
```

This confirms the page-level validation still works before mapper logic is used.

---

## 15. Requirement Extraction Smoke Test

Upload a `.txt`, `.md`, or `.html` Confluence export file.

Click:

```text
Extract Requirements from Confluence
```

Expected status during processing:

```text
Extracting requirements...
```

Expected status after success:

```text
Requirement extraction completed. Review normalized extraction, gaps, and highlighted missing fields.
```

Expected result:

```text
Requirement extraction still works after the mapper refactor.
```

---

## 16. Suggested Nodes Mapping UI Test

After extraction, confirm suggested nodes still produce rule cards.

Example:

```text
suggestedNodes: ["HTTP-IN", "BR-002", "MAP-001", "MAP-GENERIC"]
```

Expected UI result:

```text
HTTP In card appears
Validate Required Headers card appears
Build SOAP Configuration card appears
Output Mapper card appears
```

Expected result:

```text
Suggested nodes are mapped into UI rule cards correctly.
```

---

## 17. Invalid Suggested Nodes Test

If possible, test with backend response containing invalid suggested nodes:

```ts
["HTTP-IN", "", null, 123, "BR-002"]
```

Expected result:

```text
Only HTTP-IN and BR-002 are converted into rules.
```

Invalid entries should not create blank or broken rule cards.

---

## 18. Fallback Rules Test

If extraction returns no `suggestedNodes`, confirm existing manual rules remain available.

Expected result:

```text
Fallback rules are preserved and then populated with extracted values where available.
```

---

## 19. HTTP-IN Population Test

After extraction, confirm the `HTTP In` rule card is populated with:

```text
HTTP Method
HTTP Path
```

Expected result:

```text
HTTP-IN mapping behavior is unchanged.
```

---

## 20. BR-002 Population Test

After extraction, confirm the `Validate Required Headers` card is populated with:

```text
Required Headers
JSON Headers
```

If `jsonHeaders` is missing, confirm it defaults to:

```text
content-type, accept
```

Expected result:

```text
BR-002 mapping behavior is unchanged.
```

---

## 21. BR-003 and BR-004 Population Test

Confirm these cards populate correctly:

| Rule   | Field          |
| ------ | -------------- |
| BR-003 | requiredFields |
| BR-004 | numericFields  |

Expected result:

```text
Required field and numeric field mapping behavior is unchanged.
```

---

## 22. BR-005 and BR-006 Population Test

Confirm these cards populate correctly:

| Rule   | Fields                               |
| ------ | ------------------------------------ |
| BR-005 | optionalHeaderDefaults, soapDefaults |
| BR-006 | outputPayloadMappings                |

Expected result:

```text
Default and payload mapping behavior is unchanged.
```

---

## 23. BR-007 Population Test

Confirm `Set Request Metadata` card is populated with:

```text
Endpoint
Operation
Request Header Mappings
```

Confirm endpoint uses:

```text
extracted.httpPath
```

Confirm operation uses:

```text
extracted.operation
```

If operation is missing, confirm it falls back to:

```text
extracted.flowName.replace(" Generated Flow", "")
```

Expected result:

```text
BR-007 mapping behavior is unchanged.
```

---

## 24. BR-008 and BR-009 Population Test

Confirm these cards populate correctly:

| Rule   | Field                 |
| ------ | --------------------- |
| BR-008 | runtimeConfig         |
| BR-009 | requiredPayloadFields |

Expected result:

```text
Runtime config and required payload field mapping behavior is unchanged.
```

---

## 25. BR-012, BR-013, and BR-014 Population Test

Confirm these cards populate correctly:

| Rule   | Field               |
| ------ | ------------------- |
| BR-012 | pathParams          |
| BR-013 | queryParams         |
| BR-014 | recordBuilderConfig |

Expected result:

```text
Path params, query params, and repeating record config mapping behavior is unchanged.
```

---

## 26. MAP-001 Population Test

Confirm `Build SOAP Configuration` card is populated with:

```text
Mapping Name
Method
Endpoint Source
Headers JSON
SOAP XML Template
Request Replacements
Success Fields
Failure Rules
```

Expected result:

```text
MAP-001 mapping behavior is unchanged.
```

---

## 27. MAP-GENERIC Population Test

Confirm `Output Mapper` card is populated with:

```text
Return Code Paths
Message Paths
Success Code
Success Response Mapping
Failure Response Mapping
Required Success Fields
Business Code Map
```

Expected result:

```text
MAP-GENERIC mapping behavior is unchanged.
```

---

## 28. Rule Editing Regression Test

After extraction:

1. Edit one populated rule field manually.
2. Click outside the field.
3. Confirm the value remains.
4. Generate flow.

Expected result:

```text
Manual rule editing still works after mapper refactor.
```

---

## 29. Rule Remove Regression Test

After extraction, click `Remove` on one rule card.

Expected result:

```text
Only the selected rule is removed.
```

---

## 30. Missing Field Highlight Test

If extraction returns `missingFields`, confirm matching fields are highlighted with:

```text
red border
red background
```

Expected result:

```text
Missing field highlighting still works.
```

---

## 31. Generate Flow Smoke Test

After extraction and rule review, click:

```text
Generate Flow
```

Expected result:

```text
Generated flow JSON appears in the output textarea.
```

Expected result:

```text
Flow generation behavior is unchanged.
```

---

## 32. Generation Metadata Test

After successful generation, confirm metadata still displays:

* Generation Metadata
* Generation Source
* Raw Source Value
* Generation Version
* Rule Count
* SOAP Subflow
* Generated By
* Generated Flow Inspection
* Adapter details
* Adapter warnings
* Full metadata JSON

Expected result:

```text
Generation metadata display is unchanged.
```

---

## 33. Download JSON Test

After successful generation, click:

```text
Download JSON
```

Expected result:

```text
generated-flow.json downloads successfully.
```

---

## 34. Regression Checklist

Use this table during manual validation:

| Feature                             | Pass / Fail |
| ----------------------------------- | ----------- |
| App loads                           |             |
| Build passes                        |             |
| Dev server starts                   |             |
| Missing file validation works       |             |
| Requirement upload works            |             |
| Requirement extraction works        |             |
| Suggested nodes become rule cards   |             |
| Invalid suggested nodes are ignored |             |
| Fallback rules are preserved        |             |
| HTTP-IN fields populate             |             |
| BR-002 fields populate              |             |
| BR-003 fields populate              |             |
| BR-004 fields populate              |             |
| BR-005 fields populate              |             |
| BR-006 fields populate              |             |
| BR-007 fields populate              |             |
| BR-008 fields populate              |             |
| BR-009 fields populate              |             |
| BR-012 fields populate              |             |
| BR-013 fields populate              |             |
| BR-014 fields populate              |             |
| MAP-001 fields populate             |             |
| MAP-GENERIC fields populate         |             |
| Rule editing works                  |             |
| Rule removal works                  |             |
| Missing field highlighting works    |             |
| Generate flow works                 |             |
| Metadata renders                    |             |
| Download JSON works                 |             |

---

## 35. Final Smoke Test

Run this full workflow:

```text
Upload Confluence export file
→ Click Extract Requirements from Confluence
→ Confirm suggested nodes populate rule cards
→ Confirm extracted values populate correct fields
→ Show normalized JSON
→ Hide normalized JSON
→ Edit flow name
→ Edit one rule field manually
→ Remove one unnecessary rule
→ Generate flow
→ Review generation metadata
→ Expand full metadata JSON
→ Download generated-flow.json
```

Expected result:

```text
The full workflow behaves the same as before Day 34.
```

---

## 36. Day 34 Phase 7 Completion Criteria

Phase 7 is complete when:

* `npm run build` passes.
* `npm run dev` starts.
* `page.tsx` no longer contains inline rule mapping logic.
* `page.tsx` uses `buildRulesFromExtraction`.
* Suggested nodes still populate rules.
* Extracted fields still populate rule cards.
* Manual rule editing still works.
* Flow generation still works.
* Metadata rendering still works.
* Download JSON still works.

---

## Final Validation Commands

Run:

```bash
npm run build
```

Then run:

```bash
npm run dev
```

Perform the full smoke test from Section 35.

If all checks pass, Day 34 Phase 7 is complete.
