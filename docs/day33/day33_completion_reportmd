# Day 33 — Completion Report

## Title

API Service Layer Refactor

---

## Purpose

The purpose of Day 33 was to move frontend API communication logic out of `src/app/page.tsx` and into reusable service-layer files.

Day 32 already refactored the UI into smaller components. Day 33 continued that cleanup by separating API orchestration from the page.

The main goal was:

```text
Keep page.tsx focused on state and UI orchestration while moving API calls into services.
```

---

## Day 33 Objective

The Day 33 objective was to extract the following logic from `page.tsx`:

* Requirement extraction API call
* Flow generation API call
* Flow generation payload construction
* API response parsing
* API error handling

This was done without changing the behavior of the existing API Factory UI.

---

## Work Completed

## Phase 1 — API Service Layer Plan

Created:

```text
docs/day33/api_service_layer_plan.md
```

This document defined the service-layer refactor strategy.

It explained:

* Why the API service layer is needed
* Which API calls should move out of `page.tsx`
* What the service files should own
* What should remain in `page.tsx`
* Which internal routes must remain relative
* The planned Day 33 folder structure
* Acceptance criteria for the refactor

---

## Phase 2 — Shared Service Types

Created:

```text
src/types/day33.ts
```

This file defines service-layer request and response types.

Types added:

```text
RequirementExtractionRequest
RequirementExtractionResponse
ExtractedRequirementFields
GeneratedBusinessRulePayload
FlowGenerationPayload
FlowGenerationResponse
RequirementExtractionServiceResult
FlowGenerationServiceResult
BuildFlowPayloadInput
```

The Day 33 types reuse existing Day 32 types where appropriate:

```text
BusinessRule
ExtractionMetadata
GenerationMetadata
NormalizedExtraction
```

This keeps the service layer typed while avoiding duplicate type definitions.

---

## Phase 3 — Requirement Extraction Service

Created:

```text
src/services/day33/requirementExtractionService.ts
```

This service handles the internal requirement extraction API call.

It calls:

```text
/api/extract-requirements
```

The exported function is:

```ts
extractRequirements(payload)
```

The request payload remains unchanged:

```ts
{
  text,
  useLLM: false
}
```

Important behavior preserved:

* The route remains relative.
* The request still uses `POST`.
* The content type remains `application/json`.
* The response fields remain unchanged.
* The page still maps response data into the same state fields.

---

## Phase 4 — Flow Generation Service

Created:

```text
src/services/day33/flowGenerationService.ts
```

This service handles flow generation logic.

It exports:

```ts
buildFlowGenerationPayload
generateNodeRedFlow
```

### `buildFlowGenerationPayload`

This function builds the same generation payload that was previously built directly in `page.tsx`.

The payload still includes:

```text
flowName
includeSoapSubflow
normalizedExtraction
businessRules
```

The service preserves the existing SOAP subflow behavior:

```ts
includeSoapSubflow:
  includeSoapSubflow || rules.some((rule) => rule.brId === "SOAP-SUBFLOW")
```

### `generateNodeRedFlow`

This function calls the internal generation API route:

```text
/api/generate
```

Important behavior preserved:

* The route remains relative.
* The request still uses `POST`.
* The content type remains `application/json`.
* The payload structure is unchanged.
* The generated flow response is still returned to the page.
* Generation metadata is still returned to the page.

---

## Phase 5 — Updated `page.tsx`

Updated:

```text
src/app/page.tsx
```

The page now imports service functions:

```ts
import { extractRequirements } from "@/services/day33/requirementExtractionService";
import {
  buildFlowGenerationPayload,
  generateNodeRedFlow,
} from "@/services/day33/flowGenerationService";
```

The page no longer directly builds the generate payload using `splitCsv` and `parseJson`.

The page also no longer directly calls:

```ts
fetch("/api/extract-requirements")
fetch("/api/generate")
```

The page now focuses on:

* React state
* UI event handling
* Mapping service responses into state
* Error display state updates
* Download JSON behavior
* Top-level component composition

---

## Phase 6 — Shared API Response Helper

Created:

```text
src/lib/day33/apiResponseHelpers.ts
```

This helper centralizes API response parsing and error handling.

Final helper:

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

This helper is used by both:

```text
requirementExtractionService.ts
flowGenerationService.ts
```

Important fix applied:

The helper was updated to parse into `unknown` first and then create separate typed views:

```ts
const responseData = parsedData as TResponse;
const statusData = parsedData as ApiResponseWithStatus;
```

This avoids TypeScript generic errors while still allowing the helper to check:

```text
success
error
message
```

---

## Phase 7 — Testing Checklist

Created:

```text
docs/day33/testing_checklist.md
```

The checklist validates:

* New service-layer files
* Shared response helper
* Requirement extraction service
* Flow generation service
* Updated `page.tsx`
* No direct fetch calls in the page
* Build validation
* Runtime validation
* Requirement extraction behavior
* Flow generation behavior
* Error handling behavior
* Download behavior
* Full smoke test

---

## Final File Summary

The following files were added or updated during Day 33:

```text
docs/day33/api_service_layer_plan.md
docs/day33/testing_checklist.md
docs/day33/day33_completion_report.md

src/types/day33.ts

src/lib/day33/apiResponseHelpers.ts

src/services/day33/requirementExtractionService.ts
src/services/day33/flowGenerationService.ts

src/app/page.tsx
```

---

## Architecture Before Day 33

Before Day 33, `page.tsx` owned both UI state and API communication:

```text
src/app/page.tsx
  ├── React state
  ├── UI event handlers
  ├── Requirement extraction fetch
  ├── Flow generation payload construction
  ├── Flow generation fetch
  ├── Response parsing
  ├── Error handling
  ├── Download handler
  └── Component composition
```

This made the page cleaner than before Day 32, but still too responsible for backend communication.

---

## Architecture After Day 33

After Day 33, the structure is:

```text
src/app/page.tsx
  ├── React state
  ├── UI event handlers
  ├── Service function calls
  ├── Response-to-state mapping
  ├── Download handler
  └── Component composition

src/services/day33/
  ├── requirementExtractionService.ts
  └── flowGenerationService.ts

src/lib/day33/
  └── apiResponseHelpers.ts

src/types/day33.ts
  └── Service-layer request and response types
```

This creates a clearer separation of concerns.

---

## Existing Behavior Preserved

The following behavior was intentionally preserved:

| Behavior                          | Status    |
| --------------------------------- | --------- |
| Requirement file upload           | Preserved |
| Missing file validation           | Preserved |
| `/api/extract-requirements` route | Preserved |
| Extraction request payload        | Preserved |
| Extraction response parsing       | Preserved |
| Extraction status messages        | Preserved |
| Missing fields display            | Preserved |
| Normalized extraction review      | Preserved |
| Show / hide normalized JSON       | Preserved |
| Suggested node mapping            | Preserved |
| Extracted field population        | Preserved |
| Flow name update                  | Preserved |
| Add Nodes behavior                | Preserved |
| Rule editing                      | Preserved |
| Rule removal                      | Preserved |
| Missing field highlighting        | Preserved |
| `/api/generate` route             | Preserved |
| Generation payload structure      | Preserved |
| `includeSoapSubflow` behavior     | Preserved |
| Normalized extraction in payload  | Preserved |
| Legacy business rules in payload  | Preserved |
| Generated JSON display            | Preserved |
| Generation metadata display       | Preserved |
| Adapter warnings display          | Preserved |
| Invalid input error output        | Preserved |
| Download `generated-flow.json`    | Preserved |

---

## Internal Route Decision

The internal API routes remain relative:

```text
/api/extract-requirements
/api/generate
```

They were not changed to use:

```text
NEXT_PUBLIC_API_BASE_URL
```

Reason:

These routes are currently internal Next.js API routes. Keeping them relative preserves local behavior and avoids breaking the existing UI.

---

## Why This Refactor Matters

The API Factory UI will continue growing with more factory capabilities.

Future services may include:

* Excel generation
* Gap analysis
* Markdown export
* Node-RED flow validation
* Workflow schema generation
* API endpoint sheet generation
* LLM extraction
* Factory pipeline execution

Without a service layer, each new feature would add more `fetch`, parsing, and error-handling logic to `page.tsx`.

Day 33 prevents that by creating a reusable service pattern.

---

## Reusable Service Pattern Established

Day 33 established this pattern:

```text
UI event handler
→ Build service request payload
→ Call service function
→ Service calls internal API route
→ Shared helper parses response
→ Service returns typed response
→ Page maps response into state
→ Component renders updated UI
```

This pattern should be reused for future API Factory features.

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
The workflow behaves the same as it did before the Day 33 refactor.
```

---

## Day 33 Completion Criteria

Day 33 is complete when:

* `src/types/day33.ts` exists.
* `src/lib/day33/apiResponseHelpers.ts` exists.
* `src/services/day33/requirementExtractionService.ts` exists.
* `src/services/day33/flowGenerationService.ts` exists.
* `page.tsx` uses service functions.
* `page.tsx` no longer directly calls extraction or generation routes.
* API routes remain relative inside the service files.
* Requirement extraction still works.
* Flow generation still works.
* Metadata display still works.
* Download JSON still works.
* `npm run build` passes.
* Full smoke test passes.

---

## Status

```text
Day 33 completed successfully.
```

---

## Next Step

Day 34 should focus on extracting the rule population logic from `page.tsx`.

Recommended Day 34 focus:

```text
Move extracted requirement-to-business-rule mapping into a dedicated mapper module.
```

Suggested files for Day 34:

```text
src/mappers/day34/extractedRequirementRuleMapper.ts
src/types/day34.ts
docs/day34/rule_mapping_refactor_plan.md
docs/day34/testing_checklist.md
docs/day34/day34_completion_report.md
```

This will make `page.tsx` even smaller and make the rule population logic reusable for future extraction workflows.
