# Day 33 — API Service Layer Plan

## Purpose

The purpose of Day 33 is to move frontend API orchestration logic out of `src/app/page.tsx` and into reusable service modules.

Day 32 refactored the UI into smaller components. After that refactor, `page.tsx` became cleaner, but it still owns the API call logic for:

* Requirement extraction
* Flow generation

Day 33 improves the architecture by creating a dedicated frontend service layer.

---

## Day 33 Goal

The main goal of Day 33 is:

```text
Extract API service logic from page.tsx without changing behavior.
```

This means the UI should continue working exactly the same after the refactor.

---

## Current State

After Day 32, the application structure is cleaner:

```text
src/app/page.tsx
  ├── state
  ├── handlers
  ├── extractRequirementsFromFile
  ├── generateFlow
  ├── downloadFlow
  └── component composition
```

The UI rendering was moved into:

```text
src/components/day32/
  ├── RequirementExtractionPanel.tsx
  ├── NormalizedExtractionReview.tsx
  ├── RuleBuilderPanel.tsx
  ├── BusinessRuleCard.tsx
  ├── GenerationOutputPanel.tsx
  └── GenerationMetadataPanel.tsx
```

Helpers were moved into:

```text
src/lib/day32/
  ├── jsonHelpers.ts
  ├── ruleHelpers.ts
  └── generationHelpers.ts
```

Types were moved into:

```text
src/types/day32.ts
```

Day 33 will continue this cleanup by moving API-related logic into service files.

---

## Why an API Service Layer Is Needed

The current page still contains direct `fetch` calls.

Current extraction call:

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

Current generation call:

```ts
fetch("/api/generate", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify(payload),
});
```

These calls work, but keeping them inside `page.tsx` creates several issues:

* The page owns too many responsibilities.
* API request parsing is duplicated.
* Error handling is mixed with UI state updates.
* Future API calls will make the page larger again.
* Testing service logic separately becomes harder.
* Reusing extraction or generation logic from another page becomes harder.

A service layer solves this by separating backend communication from UI orchestration.

---

## Service Layer Responsibility

The service layer should handle:

* Calling internal API routes
* Sending request payloads
* Reading raw response text
* Parsing JSON safely
* Detecting non-JSON responses
* Detecting backend failure responses
* Returning clean typed results to the page

The service layer should not handle:

* React state
* UI rendering
* Button events
* File input state
* Textarea state
* Download logic
* Visual error messages

---

## Proposed Folder Structure

Create the following Day 33 structure:

```text
src/
  services/
    day33/
      requirementExtractionService.ts
      flowGenerationService.ts

  lib/
    day33/
      apiResponseHelpers.ts

  types/
    day33.ts

docs/
  day33/
    api_service_layer_plan.md
    testing_checklist.md
    day33_completion_report.md
```

---

## Service Files

## 1. `requirementExtractionService.ts`

This service will handle the requirement extraction API call.

Target internal route:

```text
/api/extract-requirements
```

Primary function:

```ts
extractRequirements(payload)
```

Expected request:

```ts
{
  text: string;
  useLLM: boolean;
}
```

Expected response should include the same fields currently used by `page.tsx`:

```text
success
error
message
rawExtracted
normalizedExtraction
extractionMetadata
extracted
missingFields
suggestedNodes
```

No response field names should be changed.

---

## 2. `flowGenerationService.ts`

This service will handle the flow generation API call.

Target internal route:

```text
/api/generate
```

Primary function:

```ts
generateNodeRedFlow(payload)
```

Expected request:

```text
flowName
includeSoapSubflow
normalizedExtraction
businessRules
```

Expected response should include the same fields currently used by `page.tsx`:

```text
success
error
flow
generationMetadata
```

No response field names should be changed.

---

## 3. `apiResponseHelpers.ts`

This helper will provide reusable response parsing utilities.

Functions:

```text
parseJsonApiResponse
assertSuccessfulApiResponse
```

The helper should preserve the current behavior:

* Read response as text first.
* Parse JSON only if raw text exists.
* Throw a readable error when response is non-JSON.
* Throw backend error message when response is not OK.
* Support `data.error`, `data.message`, and fallback messages.

---

## Shared Types

Create:

```text
src/types/day33.ts
```

This file should define service-level types for:

```text
RequirementExtractionRequest
RequirementExtractionResponse
FlowGenerationPayload
FlowGenerationResponse
GeneratedBusinessRulePayload
```

It can reuse existing Day 32 types where appropriate.

---

## Page Responsibility After Day 33

After the service layer is introduced, `src/app/page.tsx` should keep:

* React state
* UI event handlers
* State updates after successful extraction
* State updates after failed extraction
* State updates after successful generation
* State updates after failed generation
* Download JSON logic
* Top-level layout composition

The page should no longer directly call:

```text
fetch("/api/extract-requirements")
fetch("/api/generate")
```

Instead, it should call:

```ts
extractRequirements(...)
generateNodeRedFlow(...)
```

---

## Important Behavior to Preserve

Day 33 must not change the current app behavior.

The following must remain unchanged:

* Requirement file upload behavior
* Missing file validation
* Extraction status messages
* Extraction request payload
* Extraction response mapping
* Normalized extraction display
* Missing fields display
* Suggested node mapping
* Rule population behavior
* Flow name update behavior
* Generate flow request payload
* `includeSoapSubflow` behavior
* Normalized extraction included in generation payload
* Legacy business rules included in generation payload
* Generation metadata display
* Generated JSON display
* Generation error display
* Download JSON behavior

---

## Existing Internal API Routes Must Stay Relative

The internal routes should remain:

```text
/api/extract-requirements
/api/generate
```

They should not be changed to:

```text
NEXT_PUBLIC_API_BASE_URL + route
```

Reason:

These are internal Next.js API routes. They should remain relative unless the backend is moved outside the Next.js app in a later phase.

---

## Refactor Strategy

Use a low-risk order:

1. Create service types.
2. Create response helper utilities.
3. Create requirement extraction service.
4. Create flow generation service.
5. Update `page.tsx` to use services.
6. Run build.
7. Run smoke test.

This order allows the service layer to be added before replacing the working page logic.

---

## Phase Plan

## Phase 1 — API Service Layer Plan

Create:

```text
docs/day33/api_service_layer_plan.md
```

This document defines the Day 33 refactor approach.

---

## Phase 2 — Shared Service Types

Create:

```text
src/types/day33.ts
```

This file defines request and response types for the new service layer.

---

## Phase 3 — Requirement Extraction Service

Create:

```text
src/services/day33/requirementExtractionService.ts
```

This service moves extraction API communication out of `page.tsx`.

---

## Phase 4 — Flow Generation Service

Create:

```text
src/services/day33/flowGenerationService.ts
```

This service moves generation API communication out of `page.tsx`.

---

## Phase 5 — Update Page

Update:

```text
src/app/page.tsx
```

Replace direct fetch logic with service calls.

---

## Phase 6 — Error Handling Utility

Create or finalize:

```text
src/lib/day33/apiResponseHelpers.ts
```

Ensure both services use shared parsing and error handling behavior.

---

## Phase 7 — Testing Checklist

Create:

```text
docs/day33/testing_checklist.md
```

This checklist verifies that behavior did not change.

---

## Phase 8 — Completion Report

Create:

```text
docs/day33/day33_completion_report.md
```

This report documents the service-layer refactor.

---

## Expected Architecture After Day 33

After completion:

```text
src/app/page.tsx
  ├── state
  ├── UI event handlers
  ├── calls service functions
  ├── download handler
  └── component composition

src/services/day33/
  ├── requirementExtractionService.ts
  └── flowGenerationService.ts

src/lib/day33/
  └── apiResponseHelpers.ts

src/types/day33.ts
  └── service request/response types
```

---

## Benefits

Day 33 provides these benefits:

* Cleaner `page.tsx`
* Reusable extraction service
* Reusable flow generation service
* Centralized response parsing
* Cleaner error handling
* Better separation of concerns
* Easier future testing
* Easier future backend migration
* Easier addition of new factory APIs

---

## Future Work Enabled

This service layer prepares the project for future features such as:

* Excel generation service
* Gap analysis service
* Node-RED validation service
* Markdown export service
* Factory pipeline orchestration
* External backend migration
* LLM extraction toggle
* Multi-file requirement extraction
* API endpoint sheet generation
* Workflow schema generation

---

## Day 33 Acceptance Criteria

Day 33 is complete when:

* Service-level types are created.
* API response helpers are created.
* Requirement extraction service is created.
* Flow generation service is created.
* `page.tsx` no longer directly calls `/api/extract-requirements`.
* `page.tsx` no longer directly calls `/api/generate`.
* Existing API payloads are unchanged.
* Existing response mapping is unchanged.
* Requirement extraction still works.
* Flow generation still works.
* Generated metadata still displays.
* Generated JSON still downloads.
* `npm run build` passes.

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

Perform the smoke test:

```text
Upload Confluence export file
→ Extract requirements
→ Review normalized extraction
→ Edit flow name
→ Review or edit rules
→ Generate flow
→ Review metadata
→ Download generated-flow.json
```

If this workflow behaves the same as before Day 33, the service-layer refactor is successful.
