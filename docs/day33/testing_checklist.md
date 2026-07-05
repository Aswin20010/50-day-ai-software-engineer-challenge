# Day 33 — API Service Layer Testing Checklist

## Purpose

This checklist validates the Day 33 API service layer refactor.

Day 33 moved frontend API communication logic out of `src/app/page.tsx` and into reusable service files.

The goal is to confirm that the refactor did not change behavior, API routes, request payloads, response parsing, error handling, generated flow output, or UI behavior.

---

## Testing Scope

This checklist validates:

* New service-layer file structure
* Shared API response helper
* Requirement extraction service
* Flow generation service
* Updated `page.tsx`
* Build validation
* Runtime validation
* Requirement extraction behavior
* Flow generation behavior
* Error handling behavior
* Regression safety

---

## 1. File Structure Validation

Confirm these files exist:

```text
src/types/day33.ts
src/lib/day33/apiResponseHelpers.ts
src/services/day33/requirementExtractionService.ts
src/services/day33/flowGenerationService.ts
docs/day33/api_service_layer_plan.md
docs/day33/testing_checklist.md
```

Expected result:

```text
All Day 33 service-layer files are present.
```

---

## 2. API Response Helper Validation

Open:

```text
src/lib/day33/apiResponseHelpers.ts
```

Confirm it contains:

```ts
type ApiResponseWithStatus = {
  success?: boolean;
  error?: string;
  message?: string;
};

export async function parseJsonApiResponse<TResponse>(
  response: Response,
  nonJsonFallbackMessage: string,
  failureFallbackMessage: string
): Promise<TResponse> {
  const raw = await response.text();

  let parsedData: unknown;

  try {
    parsedData = raw ? JSON.parse(raw) : {};
  } catch {
    throw new Error(raw || nonJsonFallbackMessage);
  }

  const responseData = parsedData as TResponse;
  const statusData = parsedData as ApiResponseWithStatus;

  if (!response.ok || statusData.success === false) {
    throw new Error(
      statusData.error || statusData.message || failureFallbackMessage
    );
  }

  return responseData;
}
```

Expected result:

```text
The helper parses JSON safely and checks success, error, and message fields without TypeScript generic errors.
```

---

## 3. Requirement Extraction Service Validation

Open:

```text
src/services/day33/requirementExtractionService.ts
```

Confirm it calls the internal API route:

```ts
fetch("/api/extract-requirements", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify(payload),
});
```

Confirm it uses:

```ts
parseJsonApiResponse<RequirementExtractionResponse>(
  response,
  "Extractor returned non-JSON response",
  "Requirement extraction failed"
);
```

Expected result:

```text
Requirement extraction service preserves the same internal route and error behavior.
```

---

## 4. Flow Generation Service Validation

Open:

```text
src/services/day33/flowGenerationService.ts
```

Confirm it exports:

```ts
buildFlowGenerationPayload
generateNodeRedFlow
```

Confirm `generateNodeRedFlow` calls:

```ts
fetch("/api/generate", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify(payload),
});
```

Confirm it uses:

```ts
parseJsonApiResponse<FlowGenerationResponse>(
  response,
  "Generate API returned non-JSON response",
  "Flow generation failed"
);
```

Expected result:

```text
Flow generation service preserves the same internal route and response parsing behavior.
```

---

## 5. `page.tsx` Service Usage Validation

Open:

```text
src/app/page.tsx
```

Confirm it imports:

```ts
import { extractRequirements } from "@/services/day33/requirementExtractionService";
import {
  buildFlowGenerationPayload,
  generateNodeRedFlow,
} from "@/services/day33/flowGenerationService";
```

Confirm it no longer imports:

```ts
import { splitCsv, parseJson } from "@/lib/day32/jsonHelpers";
```

Expected result:

```text
page.tsx uses service functions instead of directly building API payloads.
```

---

## 6. No Direct Fetch Calls in `page.tsx`

Search inside:

```text
src/app/page.tsx
```

Confirm there are no direct calls to:

```ts
fetch("/api/extract-requirements")
fetch("/api/generate")
```

Expected result:

```text
page.tsx no longer directly calls backend API routes.
```

Note:

```text
The internal API routes still exist, but they are now called from service files.
```

---

## 7. TypeScript Build Validation

Run:

```bash
npm run build
```

Expected result:

```text
Build completes successfully.
```

If build fails, check:

* Missing import paths
* Incorrect generic typing in `apiResponseHelpers.ts`
* Missing `src/types/day33.ts`
* Incorrect service export names
* Unused imports blocked by lint
* Type mismatch in `GenerationMetadata`
* Type mismatch in `FlowGenerationPayload`

---

## 8. Dev Server Validation

Run:

```bash
npm run dev
```

Open the UI in the browser.

Expected result:

```text
API Factory UI loads successfully without runtime errors.
```

Check the browser console for:

* Module not found errors
* Hydration errors
* Failed imports
* Undefined service function errors
* JSON parsing errors

---

## 9. Missing File Validation Test

Without uploading a file, click:

```text
Extract Requirements from Confluence
```

Expected result:

```text
Please upload a Confluence export file first.
```

This confirms the page-level validation still works before the service call is triggered.

---

## 10. Requirement Extraction Smoke Test

In the UI:

1. Upload a `.txt`, `.md`, or `.html` Confluence export file.
2. Click `Extract Requirements from Confluence`.

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
Requirement extraction works exactly as before Day 33.
```

---

## 11. Requirement Extraction Response Mapping Test

After extraction, confirm the UI still updates:

* `rawExtracted`
* `normalizedExtraction`
* `extractionMetadata`
* `missingFields`
* `flowName`
* `rules`

Expected result:

```text
The response returned by extractRequirements is mapped into state exactly as before.
```

---

## 12. Normalized Extraction Review Test

After a successful extraction, confirm the UI displays:

* Requirement ID
* API Name
* Endpoint
* Operation
* Schema Version
* Extraction Method
* LLM Used
* Generated At
* Header count
* Path parameter count
* Query parameter count
* Body field count
* Business rule count
* Validation rule count
* Downstream dependency count
* Database dependency count

Expected result:

```text
Normalized extraction review still renders correctly.
```

---

## 13. Show / Hide Normalized JSON Test

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
The normalized JSON panel collapses.
```

---

## 14. Suggested Nodes Mapping Test

After extraction, confirm suggested nodes are still converted into business rules.

Expected behavior:

```text
Each valid string in suggestedNodes becomes a BusinessRule with brId.
```

Example:

```ts
.map((brId: string): BusinessRule => ({ brId }))
```

Expected result:

```text
Suggested node mapping behavior is preserved.
```

---

## 15. Extracted Field Population Test

After extraction, confirm extracted values still populate matching rule fields.

Check examples:

| Rule        | Fields                                                                                                                             |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| HTTP-IN     | httpMethod, httpPath                                                                                                               |
| BR-002      | requiredHeaders, jsonHeaders                                                                                                       |
| BR-003      | requiredFields                                                                                                                     |
| BR-004      | numericFields                                                                                                                      |
| BR-005      | optionalHeaderDefaults, soapDefaults                                                                                               |
| BR-006      | outputPayloadMappings                                                                                                              |
| BR-007      | endpoint, operation, requestHeaderMappings                                                                                         |
| BR-008      | runtimeConfig                                                                                                                      |
| BR-009      | requiredPayloadFields                                                                                                              |
| BR-012      | pathParams                                                                                                                         |
| BR-013      | queryParams                                                                                                                        |
| BR-014      | recordBuilderConfig                                                                                                                |
| MAP-001     | mappingName, method, endpointSource, headers, requestReplacements, successFields, failureRules, xmlTemplate                        |
| MAP-GENERIC | returnCodePaths, messagePaths, successCode, successResponseMapping, failureResponseMapping, requiredSuccessFields, businessCodeMap |

Expected result:

```text
Rule population behavior is unchanged.
```

---

## 16. Rule Builder Regression Test

Confirm these still work:

* Add Nodes buttons
* Editing rule fields
* Removing one rule
* Missing field highlighting
* SOAP subflow button
* Flow name editing

Expected result:

```text
Rule builder behavior was not affected by the service-layer refactor.
```

---

## 17. Flow Generation Payload Builder Test

Open:

```text
src/services/day33/flowGenerationService.ts
```

Confirm `buildFlowGenerationPayload` returns:

```ts
{
  flowName,
  includeSoapSubflow:
    includeSoapSubflow || rules.some((rule) => rule.brId === "SOAP-SUBFLOW"),
  normalizedExtraction,
  businessRules: rules.map(buildBusinessRulePayload),
}
```

Expected result:

```text
The generate payload structure matches the pre-Day 33 structure.
```

---

## 18. Flow Generation Smoke Test

After extracting requirements and reviewing rules, click:

```text
Generate Flow
```

Expected result:

```text
The UI calls generateNodeRedFlow and displays generated flow JSON.
```

The generated output textarea should contain formatted JSON.

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
* Generated Flow Inspection
* Generated Rule IDs
* Adapter details
* Adapter warnings
* Full metadata JSON

Expected result:

```text
Generation metadata display is unchanged.
```

---

## 20. Flow Generation Error Test

Enter invalid JSON into a JSON textarea such as:

```text
requestReplacements
successFields
failureRules
headers
pathParams
queryParams
businessCodeMap
```

Click:

```text
Generate Flow
```

Expected result in generated output textarea:

```json
{
  "error": "Invalid form input",
  "details": "<specific error message>"
}
```

Expected result:

```text
Generation error behavior is preserved.
```

---

## 21. Non-JSON Response Handling Test

If the backend returns non-JSON text, the service should throw one of these fallback messages:

For extraction:

```text
Extractor returned non-JSON response
```

For generation:

```text
Generate API returned non-JSON response
```

Expected result:

```text
Non-JSON response handling is centralized and readable.
```

---

## 22. Backend Failure Response Handling Test

If the backend returns:

```json
{
  "success": false,
  "error": "Some backend error"
}
```

Expected behavior:

```text
The service throws "Some backend error".
```

If the backend returns:

```json
{
  "success": false,
  "message": "Some backend message"
}
```

Expected behavior:

```text
The service throws "Some backend message".
```

If neither `error` nor `message` exists, fallback messages are used:

```text
Requirement extraction failed
Flow generation failed
```

---

## 23. Download JSON Test

After generating a flow, click:

```text
Download JSON
```

Expected result:

```text
generated-flow.json downloads successfully.
```

Expected behavior:

```text
Download logic still lives in page.tsx and is unchanged.
```

---

## 24. Internal Route Safety Check

Confirm these routes remain relative inside service files:

```text
/api/extract-requirements
/api/generate
```

Expected result:

```text
Internal Next.js API routes were not changed to external backend URLs.
```

---

## 25. Regression Checklist

Use this table during manual validation:

| Feature                              | Pass / Fail |
| ------------------------------------ | ----------- |
| App loads                            |             |
| Build passes                         |             |
| Dev server starts                    |             |
| Missing file validation works        |             |
| Requirement file upload works        |             |
| Extraction service call works        |             |
| Extraction status updates            |             |
| Missing fields render                |             |
| Normalized extraction review renders |             |
| Show / hide normalized JSON works    |             |
| Suggested nodes populate rules       |             |
| Extracted fields populate rule cards |             |
| Flow name updates                    |             |
| Add Nodes buttons work               |             |
| Rule fields update                   |             |
| Rule remove works                    |             |
| Missing field highlighting works     |             |
| Flow payload builder works           |             |
| Generate service call works          |             |
| Generated JSON displays              |             |
| Generation metadata displays         |             |
| Adapter warnings display             |             |
| Full metadata JSON expands           |             |
| Invalid JSON error displays          |             |
| Download JSON works                  |             |

---

## 26. Final Smoke Test

Run this full workflow:

```text
Upload Confluence export file
→ Click Extract Requirements from Confluence
→ Review normalized extraction
→ Show normalized JSON
→ Hide normalized JSON
→ Edit flow name
→ Review generated rule cards
→ Edit one rule field
→ Remove one unnecessary rule
→ Generate flow
→ Review generation metadata
→ Expand full generation metadata JSON
→ Download generated-flow.json
```

Expected result:

```text
The full workflow works the same as before the Day 33 service-layer refactor.
```

---

## 27. Day 33 Phase 7 Completion Criteria

Phase 7 is complete when:

* `npm run build` passes.
* `npm run dev` starts.
* `page.tsx` has no direct fetch calls.
* Requirement extraction works.
* Flow generation works.
* Shared response helper works.
* Generated metadata displays.
* Generated JSON downloads.
* No behavior regression is found.

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

Perform the full smoke test from Section 26.

If all checks pass, Day 33 Phase 7 is complete.
