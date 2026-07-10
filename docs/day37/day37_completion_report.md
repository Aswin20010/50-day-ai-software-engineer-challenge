# Day 37 — Extraction Workflow Hook Completion Report

## Overview

Day 37 focused on improving the architecture of the API Factory UI by moving the requirement-extraction workflow out of:

```text
src/app/page.tsx
```

and into a reusable custom React hook:

```text
src/hooks/useExtractionWorkflow.ts
```

The refactor reduces the responsibility of the page component while preserving the existing requirement extraction, rule mapping, flow generation, download, and error-handling behavior.

Day 37 builds directly on the extraction-state helpers introduced during Day 36.

---

## Day 37 Goal

The primary goal was to centralize requirement-extraction workflow coordination inside a reusable hook.

The implementation needed to preserve:

* Requirement-file selection
* Requirement-file reading
* Requirement extraction
* Raw extraction output
* Normalized extraction output
* Extraction metadata
* Missing-field handling
* Flow-name population
* Business-rule mapping
* Extraction status messages
* Loading-state behavior
* Extraction reset behavior
* Re-extraction behavior
* Error recovery
* Flow generation
* Generated-flow download

The refactor was architectural and was not intended to change user-visible behavior.

---

## Completed Phases

### Phase 1 — Hook Refactor Plan

Created:

```text
docs/day37/extraction_workflow_hook_plan.md
```

The plan documented:

* The purpose of the hook refactor
* Existing page-component responsibilities
* Responsibilities to move into the hook
* Responsibilities to retain in the page
* Hook design principles
* Reset scenarios
* Loading and error requirements
* Day 36 helper integration
* Validation strategy
* Completion criteria

---

### Phase 2 — Hook Types

Created:

```text
src/types/day37.ts
```

The Day 37 type work documented the expected extraction workflow contracts.

During integration, the implementation was aligned with the project’s existing domain types from:

```text
src/types/day32.ts
```

The active hook implementation uses the existing types:

* `BusinessRule`
* `NormalizedExtraction`
* `ExtractionMetadata`

This avoids duplicating established project models.

---

### Phase 3 — Extraction Workflow Hook

Created:

```text
src/hooks/useExtractionWorkflow.ts
```

The hook now coordinates extraction-specific behavior, including:

* Initial extraction state
* Raw extracted data
* Normalized extraction data
* Extraction metadata
* Missing fields
* Extraction status
* Loading state
* File-content reading
* Empty-file validation
* Requirement-extraction service calls
* Extraction-response mapping
* Flow-name callbacks
* Business-rule mapping callbacks
* Extraction reset behavior
* Error handling
* Recovery after failure

---

### Phase 4 — Page Integration

Updated:

```text
src/app/page.tsx
```

The page now consumes `useExtractionWorkflow` instead of directly managing the complete extraction workflow.

The page continues to manage editable and presentation-level state, including:

* Flow name
* Business rules
* Generated output
* Generation metadata
* SOAP subflow selection
* Requirement-file selection
* Normalized JSON visibility
* Flow generation
* Flow download
* Page rendering

This preserves clear state ownership between extraction logic and page-level UI behavior.

---

### Phase 5 — Hook Test Notes

Created:

```text
docs/day37/extraction_workflow_hook_test_notes.md
```

The test notes cover:

* Initial hook state
* File selection
* Missing-file handling
* Extraction start
* File reading
* Empty-file handling
* Service invocation
* Raw extraction
* Normalized extraction
* Metadata
* Missing fields
* Flow-name handling
* Rule mapping
* Fallback rules
* SOAP subflow behavior
* Extraction success
* Extraction failure
* Recovery after failure
* Re-extraction
* Reset behavior
* File removal
* Flow-generation regression
* Download regression
* Type validation
* Lint validation
* Build validation
* Browser validation

---

### Phase 6 — Testing Checklist

Created:

```text
docs/day37/testing_checklist.md
```

The final acceptance checklist confirms completion of the key Day 37 requirements, including:

* Hook creation
* Page integration
* Extraction-state removal from the page
* Service-call movement into the hook
* Rule-mapping movement into the hook
* Day 36 helper reuse
* Flow-name editing
* Rule editing
* SOAP subflow handling
* Flow generation
* Download
* Error recovery
* Re-extraction
* Lint validation
* Production build validation
* Browser validation
* Regression validation

---

### Phase 7 — Completion Report

Created:

```text
docs/day37/day37_completion_report.md
```

This document records the final Day 37 implementation and completion status.

---

## Files Created

The following files were created for Day 37:

```text
docs/day37/extraction_workflow_hook_plan.md
src/types/day37.ts
src/hooks/useExtractionWorkflow.ts
docs/day37/extraction_workflow_hook_test_notes.md
docs/day37/testing_checklist.md
docs/day37/day37_completion_report.md
```

---

## File Updated

The following existing file was updated:

```text
src/app/page.tsx
```

---

## Existing Files Reused

The Day 37 implementation reused existing project functionality from:

```text
src/lib/day36/extractionStateHelpers.ts
src/services/day33/requirementExtractionService.ts
src/mappers/day34/extractedRequirementRuleMapper.ts
src/services/day33/flowGenerationService.ts
src/lib/day35/downloadHelpers.ts
src/lib/day35/generatedOutputHelpers.ts
src/types/day32.ts
```

No unnecessary replacement of existing extraction, mapping, generation, or download logic was introduced.

---

## Hook Responsibilities

The final hook manages extraction-specific state and workflow behavior.

### State Managed by the Hook

* `rawExtracted`
* `normalizedExtraction`
* `extractionMetadata`
* `missingFields`
* `extractStatus`
* `isExtracting`

### Actions Managed by the Hook

* Resetting extraction state
* Reading the selected file
* Detecting empty file content
* Calling `extractRequirements`
* Mapping the extraction response
* Building extracted rules
* Sending extracted flow names to the page
* Sending mapped rules to the page
* Handling extraction failures
* Restoring loading state after success or failure

---

## Page Responsibilities

The page continues to manage state that must remain editable or presentation-specific.

### State Retained in `page.tsx`

* `flowName`
* `rules`
* `generated`
* `includeSoapSubflow`
* `generationMetadata`
* `requirementFile`
* `showNormalizedJson`

This design is important because:

* Users can manually edit the flow name.
* Users can add, update, and remove business rules.
* Flow generation depends on the current editable values.
* Generated output belongs to the generation workflow.
* JSON visibility is a presentation concern.
* File selection belongs to the page input UI.

---

## Extraction Workflow After Refactor

The resulting extraction workflow is:

1. The user selects a requirement file.
2. The page stores the selected file.
3. Previous extraction and generation output is reset.
4. The user starts requirement extraction.
5. The page passes the selected file to the hook.
6. The hook sets the loading and status state.
7. The hook clears previous extraction output.
8. The hook reads the file using `file.text()`.
9. The hook rejects empty or whitespace-only files.
10. The hook calls `extractRequirements`.
11. The request continues to use `useLLM: false`.
12. The hook maps the response using the Day 36 helper.
13. Raw extraction is stored.
14. Normalized extraction is stored.
15. Extraction metadata is stored.
16. Missing fields are stored.
17. A valid extracted flow name is sent to the page.
18. Business rules are built using the existing mapper.
19. Mapped rules are sent to the page.
20. The completion status is displayed.
21. Loading state is cleared in the `finally` block.

---

## Day 36 Integration

Day 37 reuses the Day 36 extraction-state helpers:

```text
getInitialExtractionWorkflowState
mapExtractionResponseToState
```

This provides consistent extraction defaults and prevents normalization and reset behavior from being duplicated across the page and hook.

The Day 37 hook uses the Day 36 helpers for:

* Initial extraction reset values
* New extraction attempt cleanup
* Normalized extraction mapping
* Consistent state replacement

---

## Extraction Service Preservation

The existing service remains in use:

```text
src/services/day33/requirementExtractionService.ts
```

The extraction request remains:

```ts
await extractRequirements({
  text,
  useLLM: false,
});
```

Day 37 did not change:

* The extraction endpoint
* The request shape
* The extraction mode
* The service behavior
* The returned requirement structure

---

## Rule Mapping Preservation

The existing rule mapper remains in use:

```text
src/mappers/day34/extractedRequirementRuleMapper.ts
```

The hook calls:

```ts
buildRulesFromExtraction({
  suggestedNodes,
  fallbackRules,
  extracted,
});
```

The resulting rules are returned to the page through a callback so they remain fully editable.

The page can still:

* Add rules
* Update rule fields
* Remove rules
* Retain fallback rules
* Synchronize SOAP subflow behavior

---

## Fallback Rule Handling

The hook uses a React ref to retain the latest page-level rule list.

This ensures that:

* The latest editable rules are used during extraction.
* Rule changes do not create unnecessary extraction callback changes.
* The extraction callback does not use stale fallback-rule data.
* Rule mapping remains compatible with manual user edits.

---

## Flow Name Handling

The hook does not permanently own the editable flow name.

Instead:

1. The hook reads `data.extracted.flowName`.
2. The value is checked to ensure it is a string.
3. Leading and trailing whitespace is removed.
4. Empty values are ignored.
5. A valid value is passed to the page callback.
6. The page updates its editable `flowName` state.

This preserves both automatic population and manual editing.

---

## SOAP Subflow Handling

SOAP subflow behavior remains tied to the editable rule list.

The page updates `includeSoapSubflow` when:

* Extracted rules contain `SOAP-SUBFLOW`
* The user manually adds `SOAP-SUBFLOW`
* The user removes the last `SOAP-SUBFLOW` rule
* A selected file is removed

The flow-generation payload continues to receive the current SOAP subflow state.

---

## Empty File Handling

The hook validates file contents before calling the extraction service.

Whitespace-only files produce an error using the format:

```text
The selected file "<filename>" is empty.
```

When an empty file is detected:

* The extraction service is not called.
* Extraction results remain empty.
* The status displays the validation error.
* Loading state returns to `false`.
* The user can retry with another file.

---

## Extraction Failure Handling

Extraction failures are caught inside the hook.

On failure:

* The error is logged for diagnostics.
* Raw extraction is cleared.
* Normalized extraction is cleared.
* Extraction metadata is cleared.
* Missing fields are cleared.
* The user-facing error is stored in `extractStatus`.
* Unknown errors use the fallback message.
* Loading state is reset in `finally`.

Fallback message:

```text
Requirement extraction failed.
```

The application remains usable after a failure.

---

## Re-Extraction Behavior

The refactored workflow supports extracting the same or a different file multiple times.

Before each extraction:

* Previous raw extraction is cleared.
* Previous normalized extraction is cleared.
* Previous metadata is cleared.
* Previous missing fields are cleared.
* Generated output is cleared.
* Generation metadata is cleared.
* Normalized JSON visibility is disabled.

The latest extraction fully replaces the earlier result.

---

## Flow Generation Regression Status

Flow generation remains page-owned and uses:

* Current editable flow name
* Current editable rules
* Current normalized extraction
* Current SOAP subflow state

The existing service remains:

```text
src/services/day33/flowGenerationService.ts
```

The extraction hook does not perform Node-RED flow generation and does not handle generation errors.

---

## Download Regression Status

Generated-flow download remains unchanged.

The page continues to use:

```text
src/lib/day35/downloadHelpers.ts
```

Download is skipped when generated output is empty. Otherwise, the current generated JSON text is passed to `downloadJsonText`.

---

## Type Safety

The final implementation uses established project types:

```text
BusinessRule
NormalizedExtraction
ExtractionMetadata
GenerationMetadata
```

The hook uses explicit nullable values for extraction state and typed callback parameters.

The implementation avoids introducing `any`.

Where service values are broader than the page types, values must be narrowed or validated before assignment.

---

## Validation Commands

The following commands were used for final validation:

```powershell
npm run lint
npm run build
npm run dev
```

---

## Validation Results

The Day 37 testing checklist records the following completed acceptance items:

* [x] Hook file created
* [x] Page integrated with the hook
* [x] Extraction state removed from the page
* [x] Extraction service call moved into the hook
* [x] Rule mapping moved into the hook
* [x] Day 36 helpers reused
* [x] Flow-name editing preserved
* [x] Rule editing preserved
* [x] SOAP subflow behavior preserved
* [x] Flow generation preserved
* [x] Download behavior preserved
* [x] Error recovery verified
* [x] Re-extraction verified
* [x] Lint validation passed
* [x] Production build passed
* [x] Browser validation passed
* [x] No functional regression identified
* [x] Test notes completed
* [x] Completion report completed

---

## Known Non-Blocking Warning

The Next.js build may continue to report an existing workspace-root warning when multiple lockfiles are present.

Example:

```text
Next.js inferred your workspace root, but it may not be correct.
Multiple lockfiles were detected.
```

This warning is unrelated to the Day 37 hook refactor when the build otherwise succeeds.

It may be addressed separately by:

* Removing an unnecessary lockfile after confirming it is unused, or
* Configuring the correct Turbopack workspace root

---

## Code Quality Improvements

Day 37 improves the codebase through:

### Reduced Page Complexity

The page no longer contains the complete requirement-extraction workflow.

### Clearer State Ownership

Extraction-result state belongs to the hook, while editable and presentation-level state remains in the page.

### Improved Separation of Concerns

Extraction, generation, download, and rendering responsibilities are clearly separated.

### Reusable Workflow Logic

The extraction workflow can now be reused by another component without copying page logic.

### Improved Testability

Hook behavior can be tested independently from the page layout.

### Consistent Reset Behavior

Day 36 reset helpers remain the source of extraction-state defaults.

### Reduced Duplication

Service calls, response mapping, rule building, and failure cleanup no longer need to be repeated in the page.

---

## Risks Reduced

The refactor reduces the risk of:

* Page-component growth
* Duplicate extraction state
* Inconsistent reset logic
* Stale extraction results
* Stale fallback rules
* Loading state remaining active after failure
* Error handling being duplicated
* Extraction changes unintentionally affecting rendering logic
* UI changes unintentionally altering the extraction service workflow

---

## Future Improvements

Potential future improvements include:

* Dedicated unit tests for `useExtractionWorkflow`
* React Testing Library hook and component tests
* Extraction request cancellation
* Prevention of outdated concurrent responses
* File-type validation
* File-size validation
* Reducer-based extraction state
* More specific extraction status types
* Removal or consolidation of unused generic Day 37 types
* Reusable extraction provider or context if multiple pages need the workflow

These improvements are outside the Day 37 scope.

---

## Completion Criteria

The Day 37 completion criteria have been satisfied:

* [x] Hook refactor plan created
* [x] Hook type work created
* [x] Extraction workflow hook implemented
* [x] Page integrated with the hook
* [x] Extraction state removed from the page
* [x] Service call moved into the hook
* [x] Rule mapping moved into the hook
* [x] Day 36 helpers reused
* [x] Existing extraction behavior preserved
* [x] Editable flow-name behavior preserved
* [x] Editable rule behavior preserved
* [x] Generation workflow preserved
* [x] Download behavior preserved
* [x] Error recovery preserved
* [x] Re-extraction preserved
* [x] Test notes created
* [x] Testing checklist completed
* [x] Lint validation passed
* [x] Production build passed
* [x] Browser validation passed
* [x] Completion report created

---

## Final Status

```text
DAY 37 — EXTRACTION WORKFLOW HOOK REFACTOR

STATUS: COMPLETE
```

The requirement-extraction workflow is now centralized inside a reusable React hook while the page retains editable workflow state, generation behavior, download behavior, and rendering responsibilities.

Day 37 is complete and ready for final commit.
