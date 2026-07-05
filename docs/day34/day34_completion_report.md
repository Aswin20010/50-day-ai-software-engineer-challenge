# Day 34 — Completion Report

## Title

Rule Mapping Logic Refactor

---

## Purpose

The purpose of Day 34 was to move extracted requirement-to-business-rule mapping logic out of `src/app/page.tsx` and into dedicated mapper modules.

Before Day 34, the extraction success flow in `page.tsx` contained a large inline rule mapping block. That block converted backend `suggestedNodes` into UI rules and populated those rules with extracted requirement fields.

Day 34 refactored that mapping logic into reusable mapper files while preserving the existing behavior.

---

## Day 34 Objective

The main objective was:

```text
Move rule population logic into dedicated mapper files without changing behavior.
```

This included:

* Moving suggested node conversion out of `page.tsx`
* Moving extracted field-to-rule mapping out of `page.tsx`
* Keeping rule population behavior unchanged
* Keeping extraction behavior unchanged
* Keeping generation behavior unchanged
* Making `page.tsx` smaller and easier to maintain

---

## Work Completed

## Phase 1 — Rule Mapping Refactor Plan

Created:

```text
docs/day34/rule_mapping_refactor_plan.md
```

This document defined the Day 34 refactor strategy.

It covered:

* Current inline mapping responsibility
* What logic should move out of `page.tsx`
* What should remain in `page.tsx`
* Suggested mapper folder structure
* Mapping rules to preserve
* Behavior that must not change
* Acceptance criteria
* Final validation flow

---

## Phase 2 — Shared Mapper Types

Created:

```text
src/types/day34.ts
```

This file defines shared mapper input and output types.

Types added:

```text
SuggestedNodesInput
MapSuggestedNodesToRulesInput
MapExtractedFieldsToRulesInput
BuildRulesFromExtractionInput
BuildRulesFromExtractionResult
RuleFieldMapper
```

The Day 34 mapper types reuse existing Day 32 and Day 33 types:

```text
BusinessRule
ExtractedRequirementFields
```

This keeps mapper behavior strongly typed without duplicating existing type definitions.

---

## Phase 3 — Extracted Requirement Rule Mapper

Created:

```text
src/mappers/day34/extractedRequirementRuleMapper.ts
```

This mapper handles the main extracted field-to-rule population logic.

Functions added:

```ts
mapExtractedFieldsToRules
buildRulesFromExtraction
```

### `mapExtractedFieldsToRules`

This function receives:

```text
baseRules
extracted
```

It returns populated `BusinessRule[]`.

It preserves all previous inline mapping behavior for:

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

Unknown rule types are returned unchanged.

### `buildRulesFromExtraction`

This function combines the full rule building flow:

```text
suggestedNodes + fallbackRules + extracted fields
→ final populated BusinessRule[]
```

This is now the main mapper function used by `page.tsx`.

---

## Phase 4 — Suggested Nodes Mapper

Created:

```text
src/mappers/day34/suggestedNodesMapper.ts
```

This mapper handles conversion of backend `suggestedNodes` into UI business rules.

Function added:

```ts
mapSuggestedNodesToRules
```

Behavior preserved:

* Valid non-empty strings become `{ brId }`
* Invalid values are ignored
* Values are trimmed
* Existing fallback rules are used when `suggestedNodes` is not an array

Example:

```ts
["HTTP-IN", "", null, 123, "BR-002"]
```

becomes:

```ts
[
  { brId: "HTTP-IN" },
  { brId: "BR-002" }
]
```

---

## Phase 5 — Updated `page.tsx`

Updated:

```text
src/app/page.tsx
```

Added import:

```ts
import { buildRulesFromExtraction } from "@/mappers/day34/extractedRequirementRuleMapper";
```

Removed the large inline mapping block from `extractRequirementsFromFile`.

The page now uses:

```ts
const populatedRules = buildRulesFromExtraction({
  suggestedNodes: data.suggestedNodes,
  fallbackRules: rules,
  extracted,
});

setRules(populatedRules);
```

This replaced the previous `baseRules.map(...)` block containing multiple `if (rule.brId === "...")` checks.

Important result:

```text
page.tsx no longer owns detailed rule population logic.
```

---

## Phase 6 — Mapper Test Notes

Created:

```text
docs/day34/mapper_test_notes.md
```

This document defines manual unit-style test cases for the mapper logic.

It covers:

* Valid suggested nodes
* Invalid suggested nodes
* Suggested node trimming
* Fallback rule usage
* Rule-specific field population
* Default field behavior
* Existing rule value preservation
* Unknown rule preservation
* Combined mapper behavior
* Manual UI validation flow

---

## Phase 7 — Testing Checklist

Created:

```text
docs/day34/testing_checklist.md
```

This checklist validates the Day 34 mapper refactor.

It covers:

* File structure validation
* Mapper type validation
* Suggested nodes mapper validation
* Extracted rule mapper validation
* `page.tsx` integration
* Inline mapping removal
* Build validation
* Requirement extraction smoke test
* Suggested node mapping test
* Rule population tests
* Rule editing regression test
* Flow generation smoke test
* Metadata rendering
* Download validation

---

## Final File Summary

The following files were added or updated during Day 34:

```text
docs/day34/rule_mapping_refactor_plan.md
docs/day34/mapper_test_notes.md
docs/day34/testing_checklist.md
docs/day34/day34_completion_report.md

src/types/day34.ts

src/mappers/day34/suggestedNodesMapper.ts
src/mappers/day34/extractedRequirementRuleMapper.ts

src/app/page.tsx
```

---

## Architecture Before Day 34

Before Day 34, `page.tsx` still contained detailed rule mapping logic:

```text
src/app/page.tsx
  ├── React state
  ├── Service calls
  ├── Extraction response handling
  ├── Suggested node conversion
  ├── Large extracted field-to-rule mapping block
  ├── Flow generation
  ├── Download handler
  └── Component composition
```

This made the page responsible for mapping backend extraction output into UI rule state.

---

## Architecture After Day 34

After Day 34, the structure is cleaner:

```text
src/app/page.tsx
  ├── React state
  ├── Service calls
  ├── Extraction response handling
  ├── Calls buildRulesFromExtraction
  ├── Flow generation
  ├── Download handler
  └── Component composition

src/mappers/day34/
  ├── suggestedNodesMapper.ts
  └── extractedRequirementRuleMapper.ts

src/types/day34.ts
  └── Mapper input/output types
```

The mapping responsibility now lives in dedicated mapper files.

---

## Existing Behavior Preserved

The following behavior was intentionally preserved:

| Behavior                                                             | Status    |
| -------------------------------------------------------------------- | --------- |
| Requirement extraction service call                                  | Preserved |
| Missing file validation                                              | Preserved |
| Extraction status messages                                           | Preserved |
| `rawExtracted` update                                                | Preserved |
| `normalizedExtraction` update                                        | Preserved |
| `extractionMetadata` update                                          | Preserved |
| `missingFields` update                                               | Preserved |
| `flowName` update from extracted data                                | Preserved |
| Suggested nodes become rule cards                                    | Preserved |
| Invalid suggested nodes are ignored                                  | Preserved |
| Fallback rules are used when suggested nodes are missing             | Preserved |
| Extracted fields populate matching rule fields                       | Preserved |
| Existing rule values are preserved when extracted fields are missing | Preserved |
| `jsonHeaders` default remains `content-type, accept`                 | Preserved |
| `requestHeaderMappings` default remains unchanged                    | Preserved |
| `operation` fallback from `flowName` remains unchanged               | Preserved |
| Unknown rule types are preserved                                     | Preserved |
| Manual rule editing still works                                      | Preserved |
| Rule removal still works                                             | Preserved |
| Missing field highlighting still works                               | Preserved |
| Flow generation still works                                          | Preserved |
| Generation metadata still displays                                   | Preserved |
| Generated JSON still downloads                                       | Preserved |

---

## Rule Mapping Behavior Preserved

The mapper preserves field population for the following rule IDs.

### `HTTP-IN`

Maps:

```text
httpMethod
httpPath
```

---

### `BR-002`

Maps:

```text
requiredHeaders
jsonHeaders
```

Default:

```text
content-type, accept
```

---

### `BR-003`

Maps:

```text
requiredFields
```

---

### `BR-004`

Maps:

```text
numericFields
```

---

### `BR-005`

Maps:

```text
optionalHeaderDefaults
soapDefaults
```

---

### `BR-006`

Maps:

```text
outputPayloadMappings
```

---

### `BR-007`

Maps:

```text
endpoint
operation
requestHeaderMappings
```

Special behavior preserved:

```text
endpoint uses extracted.httpPath
operation uses extracted.operation
operation falls back to extracted.flowName with " Generated Flow" removed
requestHeaderMappings falls back to default JSON mapping
```

---

### `BR-008`

Maps:

```text
runtimeConfig
```

---

### `BR-009`

Maps:

```text
requiredPayloadFields
```

---

### `BR-012`

Maps:

```text
pathParams
```

---

### `BR-013`

Maps:

```text
queryParams
```

---

### `BR-014`

Maps:

```text
recordBuilderConfig
```

---

### `MAP-001`

Maps:

```text
mappingName
method
endpointSource
headers
requestReplacements
successFields
failureRules
xmlTemplate
```

---

### `MAP-GENERIC`

Maps:

```text
returnCodePaths
messagePaths
successCode
successResponseMapping
failureResponseMapping
requiredSuccessFields
businessCodeMap
```

---

## Why This Refactor Matters

The rule mapping logic is important business logic in the API Factory UI.

Keeping it inline inside `page.tsx` made it harder to:

* Review
* Test
* Reuse
* Extend
* Debug
* Maintain

By moving it into mappers, the project now has a clearer separation:

```text
Services fetch backend data.
Mappers transform backend data into UI state.
Components render UI.
page.tsx orchestrates the workflow.
```

This is a cleaner architecture for future API Factory capabilities.

---

## Reusable Pattern Established

Day 34 established this pattern:

```text
Extraction response
→ suggestedNodesMapper
→ extractedRequirementRuleMapper
→ populated BusinessRule[]
→ page state
→ RuleBuilderPanel
```

This pattern can be reused later for:

* Excel sheet mapping
* Workflow schema mapping
* Gap analysis mapping
* Node-RED validation mapping
* API endpoint sheet generation
* Factory pipeline transformation

---

## Validation Commands

Run:

```bash
npm run build
```

Expected result:

```text
Build completes successfully.
```

Run:

```bash
npm run dev
```

Expected result:

```text
The API Factory UI starts successfully.
```

---

## Final Smoke Test

Perform this full UI test:

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
The workflow behaves the same as it did before the Day 34 refactor.
```

---

## Day 34 Completion Criteria

Day 34 is complete when:

* `src/types/day34.ts` exists.
* `src/mappers/day34/suggestedNodesMapper.ts` exists.
* `src/mappers/day34/extractedRequirementRuleMapper.ts` exists.
* `page.tsx` imports `buildRulesFromExtraction`.
* `page.tsx` no longer contains inline `if (rule.brId === "...")` mapping logic.
* Suggested node conversion works.
* Extracted field population works.
* Existing rule fallback behavior works.
* Unknown rule preservation works.
* Rule builder UI still works.
* Flow generation still works.
* Metadata rendering still works.
* Download JSON still works.
* `npm run build` passes.
* Full smoke test passes.

---

## Status

```text
Day 34 completed successfully.
```

---

## Next Step

Day 35 should focus on extracting flow download and generated output utilities.

Recommended Day 35 focus:

```text
Move generated flow download and generated output formatting into utility modules.
```

Suggested files for Day 35:

```text
src/lib/day35/downloadHelpers.ts
src/lib/day35/generatedOutputHelpers.ts
docs/day35/output_utility_refactor_plan.md
docs/day35/testing_checklist.md
docs/day35/day35_completion_report.md
```

This will continue reducing `page.tsx` and make generated output handling reusable for future exports.
