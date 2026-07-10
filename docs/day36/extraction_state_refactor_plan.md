# Day 36 — Extraction State Refactor Plan

## Purpose

Day 36 improves the maintainability of the API Factory UI by moving requirement-extraction reset logic out of `src/app/page.tsx` and into reusable helper functions.

The current page component manages several related extraction state values directly. As additional extraction features are added, keeping all reset and initialization logic inside the page component can make the component difficult to maintain and increase the risk of inconsistent state resets.

This refactor will centralize the extraction-state defaults and reset behavior without changing any existing functionality.

---

## Primary Goal

Create reusable extraction-state helpers that provide consistent default values for the requirement-extraction workflow.

The refactor must preserve the current behavior of:

* Requirement-file selection
* Requirement extraction
* Normalized extraction storage
* Extraction metadata storage
* Missing-field detection
* Flow-name population
* Rule generation
* Error handling
* State reset behavior

---

## Current Problem

The extraction workflow stores multiple related values as separate React state variables inside `src/app/page.tsx`.

Examples include:

* Raw extracted requirement data
* Normalized extraction data
* Extraction metadata
* Missing required fields
* Flow name
* Generated business rules
* Extraction status and error information

When a new requirement file is selected, an extraction fails, or the extraction workflow is restarted, several state setters may need to be called individually.

This creates several risks:

1. A newly added state field may not be reset in every required location.
2. Different reset paths may use different default values.
3. The page component becomes responsible for both UI behavior and state-default construction.
4. Testing extraction-state behavior becomes more difficult.
5. Future extraction workflows may duplicate the same initialization logic.

---

## Refactor Scope

The Day 36 refactor will introduce:

### 1. Shared Extraction-State Types

A new file will define the reusable types needed by the extraction-state helpers:

`src/types/day36.ts`

The types will describe the grouped extraction state and any helper input or result structures required by the implementation.

---

### 2. Extraction-State Helper Functions

A new helper module will be created:

`src/lib/day36/extractionStateHelpers.ts`

The helper module will contain functions responsible for creating consistent extraction-state defaults.

Possible helper responsibilities include:

* Creating the initial extraction state
* Creating an empty extraction-result state
* Creating state values for a successful extraction
* Creating state values after an extraction failure
* Normalizing nullable or missing extraction values

The final helper structure will follow the existing application types and state usage.

---

### 3. Page Component Integration

`src/app/page.tsx` will be updated to use the new helper functions.

The page component will continue to own the React state setters and user-interface behavior.

The helper module will only construct reusable state values. It will not directly update React state and will not contain UI logic.

---

## State Fields Under Review

The following extraction-related values will be reviewed during implementation:

* `rawExtracted`
* `normalizedExtraction`
* `extractionMetadata`
* `missingFields`
* `flowName`
* Generated extraction rules
* Extraction errors
* Extraction completion or loading indicators
* Requirement-file-dependent output
* Any other extraction result stored in `src/app/page.tsx`

Only fields that belong to the requirement-extraction workflow will be moved into the grouped helper model.

Unrelated page state will remain unchanged.

---

## Proposed Design

The refactor will use pure helper functions.

A pure helper function:

* Receives input values
* Returns a new state object
* Does not update React state directly
* Does not modify its input
* Does not access browser APIs
* Does not perform network calls
* Produces predictable output

Example conceptual structure:

```ts
const initialState = createInitialExtractionState();

const successState = createSuccessfulExtractionState({
  rawExtracted,
  normalizedExtraction,
  extractionMetadata,
  missingFields,
  flowName,
  rules,
});
```

The exact field types and function names may be adjusted to match the current codebase.

---

## Behavioral Constraints

The Day 36 implementation must not change:

* The extraction API request
* The `extractRequirements` call
* The `useLLM: false` configuration
* File-reading behavior
* Requirement normalization
* Missing-field validation
* Business-rule generation
* Flow-name generation
* Existing UI labels
* Existing error messages
* Existing button behavior
* Existing generated output

This is an internal state-management refactor only.

---

## Existing Extraction Flow to Preserve

The current extraction flow includes the following general sequence:

1. Validate that a requirement file is available.
2. Read the requirement-file content.
3. Call `extractRequirements`.
4. Store the raw extraction result.
5. Store the normalized extraction result.
6. Store extraction metadata.
7. Store missing required fields.
8. Populate the flow name when available.
9. Build business rules from the normalized requirement.
10. Update the page with the extraction output.
11. Handle extraction errors and reset invalid output when necessary.

The refactored implementation must preserve this sequence and its observable results.

---

## Reset Scenarios

The following reset scenarios must be reviewed:

### Initial Page Load

All extraction-related state must use predictable empty values.

### Requirement File Change

Output belonging to the previously selected requirement file must not remain visible when a new file is selected.

### Requirement File Removal

Extraction output must be cleared when the selected file is removed.

### New Extraction Attempt

State that should not carry across extraction attempts must be reset before processing begins.

### Extraction Failure

Partial or stale extraction results must not remain displayed after a failed extraction unless the current behavior intentionally preserves them.

### Successful Re-Extraction

The previous extraction result must be completely replaced by the new extraction result.

---

## Implementation Rules

The implementation must follow these rules:

1. Keep helper functions small and focused.
2. Use explicit TypeScript types.
3. Avoid `any`.
4. Avoid React imports in the helper module unless strictly required.
5. Do not move network or file-reading logic into the state helper.
6. Do not modify extraction domain models unnecessarily.
7. Reuse existing project types where possible.
8. Return new arrays and objects to avoid shared mutable defaults.
9. Preserve the current page behavior.
10. Keep the refactor easy to test.

---

## Planned Files

### New Files

```text
docs/day36/extraction_state_refactor_plan.md
src/types/day36.ts
src/lib/day36/extractionStateHelpers.ts
docs/day36/extraction_state_test_notes.md
docs/day36/testing_checklist.md
docs/day36/day36_completion_report.md
```

### Existing File to Update

```text
src/app/page.tsx
```

---

## Validation Strategy

The refactor will be validated using:

### Static Validation

* TypeScript compilation
* ESLint validation
* Next.js production build

### Functional Validation

* Select a valid requirement file.
* Run requirement extraction.
* Confirm raw extraction output is unchanged.
* Confirm normalized extraction output is unchanged.
* Confirm metadata is unchanged.
* Confirm missing fields are unchanged.
* Confirm the flow name is unchanged.
* Confirm generated rules are unchanged.
* Select another file and confirm old output is cleared.
* Trigger an invalid extraction and confirm error behavior is unchanged.
* Re-run extraction and confirm the latest result replaces the previous result.

---

## Suggested Commands

```powershell
npm run lint
npm run build
```

If the project includes automated tests, also run the relevant test command defined in `package.json`.

---

## Expected Outcome

After completing Day 36:

* Extraction defaults will be defined in one reusable location.
* Reset behavior will be more consistent.
* `src/app/page.tsx` will contain less repeated state-reset logic.
* Extraction behavior will remain unchanged.
* Future extraction fields can be added with a lower risk of incomplete resets.
* The extraction workflow will be easier to test and maintain.

---

## Completion Criteria

Day 36 is complete when:

* Shared extraction-state types have been created.
* Reusable extraction-state helper functions have been implemented.
* `src/app/page.tsx` uses the helpers.
* Duplicate extraction reset values have been reduced.
* The application passes TypeScript and production-build validation.
* Existing extraction behavior has been manually verified.
* Test notes and the completion report have been documented.
