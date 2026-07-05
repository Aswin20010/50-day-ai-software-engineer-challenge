# Day 32 — UI Refactor Testing Checklist

## Purpose

This checklist validates the Day 32 UI refactor.

Day 32 split the large `src/app/page.tsx` file into smaller components while preserving the existing API Factory UI behavior.

The goal of this checklist is to confirm that the refactor did not change functionality, API payloads, response handling, UI behavior, or generated output behavior.

---

## Testing Scope

This checklist covers:

* File structure validation
* TypeScript build validation
* Existing page behavior
* Requirement extraction
* Normalized extraction review
* Rule builder behavior
* Flow generation
* Generation metadata display
* Adapter warnings display
* JSON download behavior
* Regression checks

---

## 1. File Structure Validation

Confirm the following new Day 32 files exist:

```text
src/types/day32.ts
src/lib/day32/jsonHelpers.ts
src/lib/day32/ruleHelpers.ts
src/lib/day32/generationHelpers.ts
src/components/day32/RequirementExtractionPanel.tsx
src/components/day32/NormalizedExtractionReview.tsx
src/components/day32/RuleBuilderPanel.tsx
src/components/day32/BusinessRuleCard.tsx
src/components/day32/GenerationOutputPanel.tsx
src/components/day32/GenerationMetadataPanel.tsx
docs/day32/ui_refactor_plan.md
docs/day32/testing_checklist.md
```

Expected result:

```text
All Day 32 refactor files are present.
```

---

## 2. Main Page Structure Check

Open:

```text
src/app/page.tsx
```

Confirm the page now imports and uses:

```tsx
import RequirementExtractionPanel from "@/components/day32/RequirementExtractionPanel";
import RuleBuilderPanel from "@/components/day32/RuleBuilderPanel";
import GenerationOutputPanel from "@/components/day32/GenerationOutputPanel";
```

Confirm the page still keeps the main orchestration logic:

```text
state
addRule
updateRule
removeRule
extractRequirementsFromFile
generateFlow
downloadFlow
```

Expected result:

```text
page.tsx is smaller and acts as an orchestrator page.
```

---

## 3. Behavior Preservation Check

Confirm the following behavior was not changed:

| Area               | Expected Behavior                                |
| ------------------ | ------------------------------------------------ |
| Requirement upload | User can upload `.txt`, `.md`, or `.html` file   |
| Extraction API     | Still calls `/api/extract-requirements`          |
| Generate API       | Still calls `/api/generate`                      |
| Flow name          | Still editable                                   |
| Rule editing       | Still updates the selected rule                  |
| SOAP subflow       | Adding `SOAP-SUBFLOW` still enables SOAP subflow |
| Normalized schema  | Still sent in generate payload when available    |
| Legacy rules       | Still included in generate payload               |
| Metadata           | Still renders after flow generation              |
| Download           | Still downloads `generated-flow.json`            |

Expected result:

```text
The refactor preserved the existing workflow behavior.
```

---

## 4. Build Validation

Run:

```bash
npm run build
```

Expected result:

```text
Build completes successfully.
```

If build fails, check for:

* Missing imports
* Incorrect component paths
* Unused imports blocked by lint
* Type mismatches from `src/types/day32.ts`
* Incorrect prop names
* Missing default exports
* JSX syntax errors

---

## 5. Dev Server Validation

Run:

```bash
npm run dev
```

Open the app in the browser.

Expected result:

```text
The API Factory UI loads without runtime errors.
```

Check browser console for:

* Module not found errors
* Hydration errors
* Undefined property errors
* Event handler errors
* JSON parsing errors

---

## 6. Requirement Extraction Panel Test

In the UI:

1. Click the file upload input.
2. Select a `.txt`, `.md`, or `.html` requirement file.
3. Click `Extract Requirements from Confluence`.

Expected result:

```text
The extract button still triggers requirement extraction.
```

The UI should show:

```text
Extracting requirements...
```

Then after completion:

```text
Requirement extraction completed. Review normalized extraction, gaps, and highlighted missing fields.
```

---

## 7. Missing File Test

Without uploading a file, click:

```text
Extract Requirements from Confluence
```

Expected result:

```text
Please upload a Confluence export file first.
```

This confirms the validation behavior was preserved.

---

## 8. Internal API Route Check

Confirm the extraction call still uses:

```ts
fetch("/api/extract-requirements", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    text,
    useLLM: false,
  }),
});
```

Confirm the generation call still uses:

```ts
fetch("/api/generate", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify(payload),
});
```

Expected result:

```text
Internal Next.js API routes remain relative and were not changed to external backend URLs.
```

---

## 9. Normalized Extraction Review Test

After a successful extraction, confirm the UI displays:

* Requirement ID
* API Name
* Endpoint
* Operation
* Schema Version
* Extraction Method
* LLM Used
* Generated At
* Headers count
* Path Params count
* Query Params count
* Body Fields count
* Business Rules count
* Validation Rules count
* Downstreams count
* Databases count

Expected result:

```text
The normalized extraction review displays the same information as before the refactor.
```

---

## 10. Open Questions and Gaps Test

After extraction, check the review section.

Expected result:

```text
Open Questions and Gaps sections render correctly.
```

If there are no open questions:

```text
No open questions found.
```

If there are no gaps:

```text
No gaps found.
```

If open questions or gaps exist, they should render as cards with severity and details.

---

## 11. Show / Hide Normalized JSON Test

Click:

```text
Show Normalized JSON
```

Expected result:

```text
The rawExtracted, extractionMetadata, and normalizedExtraction JSON display.
```

Click:

```text
Hide Normalized JSON
```

Expected result:

```text
The JSON panel collapses.
```

---

## 12. Rule Builder Button Test

Click each Add Nodes button once:

```text
HTTP In
Validate Request Payload
Validate Required Headers
Validate Required Fields
Validate Numeric Fields
Apply Header Defaults
Normalize SOAP Payload
Set Request Metadata
Configure Runtime Settings
Validate SOAP Payload
Advanced Field Validation
Transform Fields
Extract Path Parameters
Extract Query Parameters
Build Repeating Record Payload
Build SOAP Configuration
SOAP Subflow Wrapper
HTTP Response
Output Mapper
```

Expected result:

```text
Each button adds the correct business rule card.
```

---

## 13. Business Rule Card Test

Confirm each rule card renders the correct inputs.

| Rule          | Expected Inputs                                                                                                                                |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| HTTP-IN       | HTTP Method, HTTP Path                                                                                                                         |
| HTTP-RESPONSE | No configuration message                                                                                                                       |
| SOAP-SUBFLOW  | No configuration message                                                                                                                       |
| BR-002        | Required Headers, JSON Headers                                                                                                                 |
| BR-003        | Required Fields                                                                                                                                |
| BR-004        | Numeric Fields                                                                                                                                 |
| BR-005        | Optional Header Defaults, SOAP Defaults                                                                                                        |
| BR-006        | Output Payload Mappings                                                                                                                        |
| BR-007        | Endpoint, Operation, Request Header Mappings                                                                                                   |
| BR-008        | Runtime Config                                                                                                                                 |
| BR-009        | Required Payload Fields                                                                                                                        |
| BR-010        | Field Rules                                                                                                                                    |
| BR-011        | Transform Rules                                                                                                                                |
| BR-012        | Path Parameters JSON                                                                                                                           |
| BR-013        | Query Parameters JSON                                                                                                                          |
| BR-014        | Repeating Record Builder Config                                                                                                                |
| MAP-001       | Mapping Name, Method, Endpoint Source, Headers JSON, SOAP XML Template, Request Replacements, Success Fields, Failure Rules                    |
| MAP-GENERIC   | Return Code Paths, Message Paths, Success Code, Success Response Mapping, Failure Response Mapping, Required Success Fields, Business Code Map |

Expected result:

```text
All rule cards render the same fields as before the refactor.
```

---

## 14. Rule Update Test

For a rule card:

1. Edit a field value.
2. Move to another field.
3. Confirm the value remains.
4. Generate flow.

Expected result:

```text
Manual edits are preserved and included in the generate payload.
```

---

## 15. Remove Rule Test

Click `Remove` on one business rule card.

Expected result:

```text
Only the selected rule is removed.
```

Other rules should remain unchanged.

---

## 16. Missing Field Highlight Test

After extraction, if `missingFields` contains known field names, confirm matching inputs are highlighted.

Expected result:

```text
Missing fields still show red border and red background.
```

This confirms `getMissingFieldClass` still works after moving to:

```text
src/lib/day32/ruleHelpers.ts
```

---

## 17. Generate Flow Test

After extraction and rule review, click:

```text
Generate Flow
```

Expected result:

```text
The UI calls /api/generate and displays generated flow JSON.
```

The generated output textarea should contain formatted JSON.

---

## 18. Generate Payload Preservation Check

Confirm the payload still contains:

```text
flowName
includeSoapSubflow
normalizedExtraction
businessRules
```

Confirm each business rule still maps:

```text
brId
httpPath
httpMethod
recordBuilderConfig
requiredHeaders
jsonHeaders
requiredFields
numericFields
outputPayloadMappings
pathParams
queryParams
optionalHeaderDefaults
soapDefaults
endpoint
operation
requestHeaderMappings
runtimeConfig
requiredPayloadFields
mappingName
soapOperation
soapNamespace
xmlTemplate
requestReplacements
successFields
failureRules
fieldRules
transformRules
method
endpointSource
headers
returnCodePaths
messagePaths
successCode
successResponseMapping
failureResponseMapping
requiredSuccessFields
businessCodeMap
technicalErrorMap
mapperSource
```

Expected result:

```text
Generate payload structure is unchanged.
```

---

## 19. Generation Metadata Test

After successful generation, confirm the UI displays:

* Generation Metadata
* Generation Source
* Raw Source Value
* Generation Version
* Rule Count
* SOAP Subflow
* Generated By

Expected result:

```text
Generation metadata still displays correctly.
```

---

## 20. Generated Flow Inspection Test

After successful generation, confirm the UI displays:

* Generation Source
* SOAP Subflow Enabled
* MAP-001 Present
* MAP-GENERIC Present
* Adapter Warning Count
* Adapter Version
* Generated Rule IDs

Expected result:

```text
Generated flow inspection still works after moving to GenerationMetadataPanel.
```

---

## 21. Adapter Details Test

If adapter metadata exists, confirm the UI displays:

* Adapter Version
* Schema Version
* Requirement ID
* Open Questions count
* Gaps count
* Adapter Warnings count
* Generated Rules
* Adapter Generated At

Expected result:

```text
Adapter details render correctly.
```

---

## 22. Adapter Warnings Test

If adapter warnings exist, confirm each warning card displays:

* Warning code
* Severity
* Message
* Source

If no warnings exist, confirm this message displays:

```text
No adapter warnings returned.
```

Expected result:

```text
Adapter warning rendering is unchanged.
```

---

## 23. Full Generation Metadata JSON Test

Click:

```text
Show Full Generation Metadata JSON
```

Expected result:

```text
The full generationMetadata JSON displays in a collapsible details section.
```

---

## 24. Download JSON Test

After generating a flow, click:

```text
Download JSON
```

Expected result:

```text
generated-flow.json downloads successfully.
```

The file should contain the same JSON shown in the generated output textarea.

---

## 25. Error Handling Test — Extraction Failure

Use an invalid or unsupported file content if needed.

Expected result:

```text
The UI shows a meaningful extraction error message.
```

Also confirm:

```text
rawExtracted is cleared
normalizedExtraction is cleared
extractionMetadata is cleared
showNormalizedJson is reset
```

---

## 26. Error Handling Test — Generation Failure

Enter invalid JSON in one of these fields:

```text
requestReplacements
successFields
failureRules
headers
pathParams
queryParams
businessCodeMap
```

Then click:

```text
Generate Flow
```

Expected result:

```json
{
  "error": "Invalid form input",
  "details": "<specific parsing or generation error>"
}
```

The error should appear in the generated output textarea.

---

## 27. Regression Checklist

Confirm the following features still work:

| Feature                                  | Pass / Fail |
| ---------------------------------------- | ----------- |
| App loads                                |             |
| Requirement file upload works            |             |
| Extract button works                     |             |
| Missing file validation works            |             |
| Extraction status updates                |             |
| Missing fields display                   |             |
| Normalized extraction review displays    |             |
| Show / hide normalized JSON works        |             |
| Flow name can be edited                  |             |
| Add Nodes buttons work                   |             |
| Rule fields can be edited                |             |
| Rule remove works                        |             |
| Missing field highlighting works         |             |
| Generate Flow button works               |             |
| Generated JSON displays                  |             |
| Generation metadata displays             |             |
| Adapter warnings display                 |             |
| Full metadata JSON details section works |             |
| Download JSON works                      |             |
| `npm run build` passes                   |             |

---

## 28. Final Smoke Test

Run this complete flow:

```text
Upload Confluence export file
→ Click Extract Requirements from Confluence
→ Review normalized extraction
→ Show normalized JSON
→ Hide normalized JSON
→ Edit flow name
→ Add or review rules
→ Edit one rule field
→ Remove one unnecessary rule
→ Generate flow
→ Review generation metadata
→ Expand full generation metadata JSON
→ Download generated-flow.json
```

Expected result:

```text
The full API Factory UI workflow works exactly as it did before the Day 32 refactor.
```

---

## 29. Day 32 Completion Criteria

Day 32 Phase 8 is complete when:

* `npm run build` passes.
* `npm run dev` starts successfully.
* Existing internal API routes still work.
* Requirement extraction works.
* Normalized extraction review works.
* Rule builder works.
* Flow generation works.
* Metadata display works.
* Adapter warnings display works.
* Download JSON works.
* No behavior regression is found from the UI refactor.

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

Perform the full smoke test from Section 28.

If all checks pass, Day 32 refactor validation is complete.
