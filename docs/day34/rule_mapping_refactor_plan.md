# Day 34 — Rule Mapping Refactor Plan

## Purpose

The purpose of Day 34 is to move the extracted requirement-to-business-rule mapping logic out of `src/app/page.tsx` and into a dedicated mapper module.

Day 32 refactored the UI into smaller components.

Day 33 moved API calls into service files.

Day 34 continues the cleanup by extracting the large rule population block from the page.

---

## Day 34 Goal

The main goal is:

```text
Move rule mapping logic into a reusable mapper without changing behavior.
```

Currently, after requirement extraction succeeds, `page.tsx` performs several responsibilities:

* Reads `data.suggestedNodes`
* Converts suggested node IDs into `BusinessRule` objects
* Falls back to existing rules when suggested nodes are unavailable
* Reads `data.extracted`
* Populates rule fields based on each `brId`
* Updates the `rules` state with populated rules

This behavior works, but it makes `page.tsx` longer and harder to maintain.

Day 34 will move that mapping behavior into mapper/helper files.

---

## Current Mapping Responsibility in `page.tsx`

The current page handles logic like:

```text
suggestedNodes
→ baseRules
→ map each rule by brId
→ populate extracted fields
→ setRules(populatedRules)
```

Example current behavior:

```text
HTTP-IN receives httpMethod and httpPath
BR-002 receives requiredHeaders and jsonHeaders
BR-003 receives requiredFields
BR-004 receives numericFields
BR-005 receives optionalHeaderDefaults and soapDefaults
BR-006 receives outputPayloadMappings
BR-007 receives endpoint, operation, and requestHeaderMappings
BR-008 receives runtimeConfig
BR-009 receives requiredPayloadFields
BR-012 receives pathParams
BR-013 receives queryParams
BR-014 receives recordBuilderConfig
MAP-001 receives SOAP request mapping fields
MAP-GENERIC receives response mapping fields
```

This full mapping block should move out of the page.

---

## What Should Move

The following logic should move into dedicated Day 34 modules:

### 1. Suggested Node Conversion

Move this behavior:

```text
data.suggestedNodes
→ valid string node IDs
→ BusinessRule[]
```

Into a helper function.

Target file:

```text
src/mappers/day34/suggestedNodesMapper.ts
```

---

### 2. Extracted Field Rule Population

Move this behavior:

```text
baseRules + extracted fields
→ populated BusinessRule[]
```

Into a mapper function.

Target file:

```text
src/mappers/day34/extractedRequirementRuleMapper.ts
```

---

### 3. Mapper Types

Create Day 34 types for mapper inputs and outputs.

Target file:

```text
src/types/day34.ts
```

---

## What Should Stay in `page.tsx`

The page should still own:

* React state
* Requirement file validation
* Calling `extractRequirements`
* Updating extraction state
* Updating missing fields
* Updating flow name
* Calling mapper functions
* Calling `setRules`
* Flow generation
* Download behavior
* Component composition

The page should not contain the detailed `if (rule.brId === "...")` mapping block anymore.

---

## Proposed Folder Structure

Create:

```text
src/
  mappers/
    day34/
      suggestedNodesMapper.ts
      extractedRequirementRuleMapper.ts

  types/
    day34.ts

docs/
  day34/
    rule_mapping_refactor_plan.md
    mapper_test_notes.md
    testing_checklist.md
    day34_completion_report.md
```

---

## Proposed Mapper Functions

### `mapSuggestedNodesToRules`

Target file:

```text
src/mappers/day34/suggestedNodesMapper.ts
```

Function:

```ts
mapSuggestedNodesToRules(suggestedNodes, fallbackRules)
```

Responsibility:

* Check if `suggestedNodes` is an array.
* Keep only valid non-empty strings.
* Convert each valid string into `{ brId }`.
* If `suggestedNodes` is not a valid array, return existing fallback rules.

Expected behavior:

```text
["HTTP-IN", "BR-002"]
→ [{ brId: "HTTP-IN" }, { brId: "BR-002" }]
```

Invalid entries should be ignored:

```text
["HTTP-IN", "", null, 123, "BR-002"]
→ [{ brId: "HTTP-IN" }, { brId: "BR-002" }]
```

---

### `mapExtractedFieldsToRules`

Target file:

```text
src/mappers/day34/extractedRequirementRuleMapper.ts
```

Function:

```ts
mapExtractedFieldsToRules(baseRules, extracted)
```

Responsibility:

* Loop through each base rule.
* Check the rule `brId`.
* Apply extracted values to the correct rule fields.
* Preserve existing rule values when extracted values are missing.
* Return populated rules.

Expected behavior:

```text
baseRules + extracted fields
→ populated BusinessRule[]
```

---

### `buildRulesFromExtraction`

Target file:

```text
src/mappers/day34/extractedRequirementRuleMapper.ts
```

Function:

```ts
buildRulesFromExtraction({ suggestedNodes, fallbackRules, extracted })
```

Responsibility:

* Convert suggested nodes into base rules.
* Populate extracted fields into those rules.
* Return the final populated rules.

This function should become the main function used by `page.tsx`.

---

## Mapping Rules to Preserve

The mapper must preserve the exact existing behavior.

### `HTTP-IN`

Maps:

```text
httpMethod
httpPath
```

Fallbacks:

```text
rule.httpMethod
rule.httpPath
""
```

---

### `BR-002`

Maps:

```text
requiredHeaders
jsonHeaders
```

Fallbacks:

```text
rule.requiredHeaders
rule.jsonHeaders
"content-type, accept"
```

---

### `BR-003`

Maps:

```text
requiredFields
```

Fallback:

```text
rule.requiredFields
""
```

---

### `BR-004`

Maps:

```text
numericFields
```

Fallback:

```text
rule.numericFields
""
```

---

### `BR-005`

Maps:

```text
optionalHeaderDefaults
soapDefaults
```

Fallbacks:

```text
rule.optionalHeaderDefaults
rule.soapDefaults
""
```

---

### `BR-006`

Maps:

```text
outputPayloadMappings
```

Fallback:

```text
rule.outputPayloadMappings
""
```

---

### `BR-007`

Maps:

```text
endpoint
operation
requestHeaderMappings
```

Special behavior:

```text
endpoint uses extracted.httpPath first
operation uses extracted.operation first
operation falls back to extracted.flowName with " Generated Flow" removed
requestHeaderMappings uses default JSON mapping when missing
```

Default request header mapping:

```json
{
  "x-transaction-id": "transactionId",
  "x-source-system": "sourceSystem"
}
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

## Behavior That Must Not Change

The following behavior must remain unchanged:

* Suggested nodes become business rules.
* Invalid suggested node entries are ignored.
* Existing rules are used when suggested nodes are unavailable.
* Extracted values overwrite empty rule fields.
* Rule values are preserved when extracted fields are missing.
* Default `jsonHeaders` remains `content-type, accept`.
* Default `requestHeaderMappings` remains the same JSON string.
* `operation` still falls back to `flowName` without `" Generated Flow"`.
* `setRules` still receives the final populated rules.
* Flow generation payload remains unchanged.
* UI rendering remains unchanged.

---

## Page Logic After Day 34

After Day 34, the extraction success block in `page.tsx` should look cleaner.

Target simplified flow:

```ts
const extracted = data.extracted || {};
setMissingFields(data.missingFields || []);

if (extracted.flowName) {
  setFlowName(extracted.flowName);
}

const populatedRules = buildRulesFromExtraction({
  suggestedNodes: data.suggestedNodes,
  fallbackRules: rules,
  extracted,
});

setRules(populatedRules);
```

This replaces the large inline `baseRules.map(...)` block.

---

## Refactor Strategy

Use this low-risk order:

1. Create shared mapper types.
2. Create suggested node mapper.
3. Create extracted field mapper.
4. Update `page.tsx` to use mapper.
5. Run build.
6. Run full smoke test.

This reduces the chance of breaking working extraction behavior.

---

## Phase Plan

## Phase 1 — Rule Mapping Refactor Plan

Create:

```text
docs/day34/rule_mapping_refactor_plan.md
```

---

## Phase 2 — Shared Mapper Types

Create:

```text
src/types/day34.ts
```

---

## Phase 3 — Extracted Requirement Rule Mapper

Create:

```text
src/mappers/day34/extractedRequirementRuleMapper.ts
```

---

## Phase 4 — Suggested Nodes Helper

Create:

```text
src/mappers/day34/suggestedNodesMapper.ts
```

---

## Phase 5 — Update Page

Update:

```text
src/app/page.tsx
```

Replace inline mapping logic with mapper function calls.

---

## Phase 6 — Mapper Test Notes

Create:

```text
docs/day34/mapper_test_notes.md
```

Document mapper test cases.

---

## Phase 7 — Testing Checklist

Create:

```text
docs/day34/testing_checklist.md
```

---

## Phase 8 — Completion Report

Create:

```text
docs/day34/day34_completion_report.md
```

---

## Acceptance Criteria

Day 34 is complete when:

* Suggested node mapping is moved out of `page.tsx`.
* Extracted field-to-rule mapping is moved out of `page.tsx`.
* `page.tsx` uses `buildRulesFromExtraction`.
* Existing extraction behavior is preserved.
* Existing rule population behavior is preserved.
* Existing generation behavior is preserved.
* `npm run build` passes.
* Full UI smoke test passes.

---

## Final Validation Flow

Run:

```bash
npm run build
```

Then run:

```bash
npm run dev
```

Perform this smoke test:

```text
Upload Confluence export file
→ Extract requirements
→ Confirm suggested nodes populate rules
→ Confirm extracted values populate fields
→ Edit one rule manually
→ Generate flow
→ Review metadata
→ Download generated-flow.json
```

If this works exactly as before, Day 34 is successful.
